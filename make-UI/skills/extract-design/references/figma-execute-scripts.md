# Figma Execute Scripts for Design Extraction

6 pre-built scripts that run via `figma_execute` to automate what was manual in v5. These are the core of the v6 extraction pipeline.

**Usage**: Copy the script code into a `figma_execute` tool call. Each script returns a JSON string. Parse the result and feed into the normalize step.

**Important**: These scripts use async Figma APIs. The `figma_execute` tool handles async execution automatically.

---

## Script 1: extract-all-variables

**Replaces**: Manual parsing of design-tokens.json (131 variables in SnowUI)
**Output**: Complete token table with all collections, modes, and resolved values including aliases

```javascript
const collections = await figma.variables.getLocalVariableCollectionsAsync();
const result = {};

for (const col of collections) {
  const modeNames = col.modes.map(m => m.name);
  const variables = [];

  for (const varId of col.variableIds) {
    const v = await figma.variables.getVariableByIdAsync(varId);
    if (!v) continue;

    const modeValues = {};
    for (const mode of col.modes) {
      const val = v.valuesByMode[mode.modeId];
      if (val && typeof val === 'object' && 'type' in val && val.type === 'VARIABLE_ALIAS') {
        try {
          const aliasVar = await figma.variables.getVariableByIdAsync(val.id);
          modeValues[mode.name] = {
            type: 'alias',
            aliasName: aliasVar ? aliasVar.name : 'UNRESOLVED',
            aliasCollection: aliasVar ? aliasVar.variableCollectionId : null
          };
        } catch {
          modeValues[mode.name] = { type: 'alias', aliasName: 'UNRESOLVED' };
        }
      } else {
        modeValues[mode.name] = val;
      }
    }

    variables.push({
      name: v.name,
      resolvedType: v.resolvedType,
      description: v.description || '',
      values: modeValues
    });
  }

  result[col.name] = {
    modes: modeNames,
    variableCount: variables.length,
    variables: variables
  };
}

return JSON.stringify(result);
```

### How to normalize output

The result is a JSON object keyed by collection name. Each collection has:
- `modes`: array of mode names (e.g., ["SnowUI-Light", "SnowUI-Dark", "iOS-Light", "iOS-Dark"])
- `variables`: array of `{ name, resolvedType, description, values: { [modeName]: colorValue | alias } }`

Color values come as `{ r, g, b, a }` (0-1 range). Convert to hex:
```
hex = '#' + [r,g,b].map(c => Math.round(c * 255).toString(16).padStart(2, '0')).join('')
```

If `a < 1`, use rgba or 8-digit hex.

---

## Script 2: extract-bound-variables-per-page

**Replaces**: Manual parsing of 34,643 boundVariable references across 14-32MB JSON files. This was the single biggest time sink in v5.
**Output**: Every variable binding on the current page with full node context

```javascript
const page = figma.currentPage;
const bindings = [];
const varCache = {};

async function resolveVar(id) {
  if (varCache[id]) return varCache[id];
  try {
    const v = await figma.variables.getVariableByIdAsync(id);
    varCache[id] = v ? v.name : 'UNRESOLVED';
  } catch {
    varCache[id] = 'UNRESOLVED';
  }
  return varCache[id];
}

async function walk(node, depth) {
  if (depth > 50) return; // safety limit

  if (node.boundVariables) {
    for (const [prop, binding] of Object.entries(node.boundVariables)) {
      const entries = Array.isArray(binding) ? binding : [binding];
      for (const entry of entries) {
        if (entry && entry.id) {
          const varName = await resolveVar(entry.id);
          bindings.push({
            nodeId: node.id,
            nodeName: node.name,
            nodeType: node.type,
            property: prop,
            variableName: varName
          });
        }
      }
    }
  }

  if ('children' in node && node.children) {
    for (const child of node.children) {
      await walk(child, depth + 1);
    }
  }
}

await walk(page, 0);

return JSON.stringify({
  page: page.name,
  totalBindings: bindings.length,
  uniqueVariables: [...new Set(bindings.map(b => b.variableName))].sort(),
  bindings: bindings
});
```

### How to normalize output

The result gives you every variable binding with context. Group by:
- **By variable**: Which nodes use `Black/100%`? → builds the color usage map
- **By node type**: All TEXT nodes → text color rules. All FRAME nodes → background rules.
- **By property**: `fills` = background colors. `strokes` = border colors. `itemSpacing` = gap tokens.

---

## Script 3: extract-component-properties

**Replaces**: Manual component inspection (missed in v5 — instance properties not systematically captured)
**Output**: All component sets with variant axes, default values, instance swap slots

```javascript
const componentSets = figma.currentPage.findAllWithCriteria({ types: ['COMPONENT_SET'] });
const components = figma.currentPage.findAllWithCriteria({ types: ['COMPONENT'] });

// Component sets (multi-variant)
const sets = componentSets.map(cs => ({
  name: cs.name,
  id: cs.id,
  variantCount: cs.children.length,
  variants: cs.children.map(c => c.name),
  properties: Object.fromEntries(
    Object.entries(cs.componentPropertyDefinitions || {}).map(([k, v]) => [k, {
      type: v.type,
      defaultValue: v.defaultValue,
      variantOptions: v.variantOptions || null,
      preferredValues: v.preferredValues ? v.preferredValues.map(pv => pv.key || pv.name || pv) : null
    }])
  ),
  description: cs.description || ''
}));

// Standalone components (single variant)
const standalone = components
  .filter(c => !c.parent || c.parent.type !== 'COMPONENT_SET')
  .map(c => ({
    name: c.name,
    id: c.id,
    properties: Object.fromEntries(
      Object.entries(c.componentPropertyDefinitions || {}).map(([k, v]) => [k, {
        type: v.type,
        defaultValue: v.defaultValue,
        variantOptions: v.variantOptions || null
      }])
    ),
    description: c.description || ''
  }));

return JSON.stringify({
  page: figma.currentPage.name,
  componentSets: sets,
  standaloneComponents: standalone,
  totalSets: sets.length,
  totalStandalone: standalone.length
});
```

### How to normalize output

- `variantOptions` tells you the variant axes (e.g., `["Default", "Hover", "Focus", "Disabled"]`)
- `type: "INSTANCE_SWAP"` indicates a slot that accepts swappable content
- `preferredValues` lists which components are recommended for that slot
- Map variant names to state matrix entries

---

## Script 4: extract-styles-catalog

**Replaces**: Manual style inspection
**Output**: All local paint styles, text styles, and effect styles with full specs

```javascript
const paintStyles = await figma.getLocalPaintStylesAsync();
const textStyles = await figma.getLocalTextStylesAsync();
const effectStyles = await figma.getLocalEffectStylesAsync();

const paints = paintStyles.map(s => ({
  name: s.name,
  type: 'PAINT',
  paints: s.paints.map(p => ({
    type: p.type,
    color: p.color ? { r: p.color.r, g: p.color.g, b: p.color.b } : null,
    opacity: p.opacity,
    visible: p.visible,
    gradientStops: p.gradientStops || null,
    gradientTransform: p.gradientTransform || null
  })),
  description: s.description || ''
}));

const texts = textStyles.map(s => ({
  name: s.name,
  type: 'TEXT',
  fontFamily: s.fontName.family,
  fontStyle: s.fontName.style,
  fontSize: s.fontSize,
  fontWeight: s.fontName.style.includes('Bold') ? 700 :
              s.fontName.style.includes('Semi') ? 600 :
              s.fontName.style.includes('Medium') ? 500 : 400,
  lineHeight: s.lineHeight.unit === 'PIXELS' ? s.lineHeight.value :
              s.lineHeight.unit === 'PERCENT' ? `${s.lineHeight.value}%` : 'AUTO',
  letterSpacing: s.letterSpacing.value || 0,
  textDecoration: s.textDecoration || 'NONE',
  description: s.description || ''
}));

const effects = effectStyles.map(s => ({
  name: s.name,
  type: 'EFFECT',
  effects: s.effects.map(e => ({
    type: e.type,
    color: e.color ? { r: e.color.r, g: e.color.g, b: e.color.b, a: e.color.a } : null,
    offset: e.offset || null,
    radius: e.radius || 0,
    spread: e.spread || 0,
    visible: e.visible
  })),
  description: s.description || ''
}));

return JSON.stringify({
  paintStyles: paints,
  textStyles: texts,
  effectStyles: effects,
  counts: { paint: paints.length, text: texts.length, effect: effects.length }
});
```

---

## Script 5: diff-light-dark-frames

**Replaces**: Manual dark mode comparison
**Input**: Two frame IDs (light and dark variants of the same screen)
**Output**: Property-level diff showing which nodes use inverting tokens, hardcoded overrides, or structural changes

**Before running**: Get two frame IDs from `figma_get_file_data` or `figma_get_selection`. Replace `LIGHT_FRAME_ID` and `DARK_FRAME_ID` in the script.

```javascript
const lightFrame = await figma.getNodeByIdAsync('LIGHT_FRAME_ID');
const darkFrame = await figma.getNodeByIdAsync('DARK_FRAME_ID');

if (!lightFrame || !darkFrame) {
  return JSON.stringify({ error: 'Frame(s) not found. Provide valid frame IDs.' });
}

const diffs = [];

function getFills(node) {
  if (!node.fills || !Array.isArray(node.fills)) return [];
  return node.fills.filter(f => f.visible !== false).map(f => ({
    type: f.type,
    color: f.color ? `rgba(${Math.round(f.color.r*255)},${Math.round(f.color.g*255)},${Math.round(f.color.b*255)},${f.opacity ?? 1})` : null
  }));
}

function getStrokes(node) {
  if (!node.strokes || !Array.isArray(node.strokes)) return [];
  return node.strokes.filter(s => s.visible !== false).map(s => ({
    type: s.type,
    color: s.color ? `rgba(${Math.round(s.color.r*255)},${Math.round(s.color.g*255)},${Math.round(s.color.b*255)},${s.opacity ?? 1})` : null
  }));
}

function compareTrees(lightNode, darkNode, path) {
  if (!lightNode || !darkNode) return;
  if (path.length > 8) return; // depth limit

  const lightFills = JSON.stringify(getFills(lightNode));
  const darkFills = JSON.stringify(getFills(darkNode));
  const lightStrokes = JSON.stringify(getStrokes(lightNode));
  const darkStrokes = JSON.stringify(getStrokes(darkNode));

  if (lightFills !== darkFills || lightStrokes !== darkStrokes) {
    const hasBoundVars = !!(lightNode.boundVariables && Object.keys(lightNode.boundVariables).length > 0);
    diffs.push({
      path: path.join(' > '),
      nodeName: lightNode.name,
      nodeType: lightNode.type,
      hasBoundVariables: hasBoundVars,
      light: { fills: getFills(lightNode), strokes: getStrokes(lightNode) },
      dark: { fills: getFills(darkNode), strokes: getStrokes(darkNode) }
    });
  }

  if ('children' in lightNode && 'children' in darkNode) {
    const lightChildren = lightNode.children || [];
    const darkChildren = darkNode.children || [];
    const maxLen = Math.min(lightChildren.length, darkChildren.length);
    for (let i = 0; i < maxLen; i++) {
      compareTrees(lightChildren[i], darkChildren[i], [...path, lightChildren[i].name]);
    }
    if (lightChildren.length !== darkChildren.length) {
      diffs.push({
        path: path.join(' > '),
        type: 'STRUCTURAL_DIFF',
        lightChildCount: lightChildren.length,
        darkChildCount: darkChildren.length
      });
    }
  }
}

compareTrees(lightFrame, darkFrame, [lightFrame.name]);

return JSON.stringify({
  lightFrame: lightFrame.name,
  darkFrame: darkFrame.name,
  totalDiffs: diffs.length,
  diffs: diffs
});
```

### How to interpret

- `hasBoundVariables: true` + different fills = **correct token binding** (variable resolves differently per mode)
- `hasBoundVariables: false` + different fills = **hardcoded override** (investigate — may be intentional like tooltip bg)
- `STRUCTURAL_DIFF` = **layout change** between modes (rare, flag for review)

---

## Script 6: extract-state-variants

**Replaces**: Manual state matrix construction
**Output**: Property deltas between state variants of component sets

```javascript
const componentSets = figma.currentPage.findAllWithCriteria({ types: ['COMPONENT_SET'] });
const stateMatrices = [];

for (const cs of componentSets) {
  // Look for state-like variant properties
  const props = cs.componentPropertyDefinitions || {};
  const stateProps = Object.entries(props).filter(([k, v]) =>
    v.type === 'VARIANT' && (
      k.toLowerCase().includes('state') ||
      k.toLowerCase().includes('style') ||
      k.toLowerCase().includes('type') ||
      (v.variantOptions && v.variantOptions.some(o =>
        ['default', 'hover', 'focus', 'active', 'disabled', 'pressed', 'selected', 'error'].includes(o.toLowerCase())
      ))
    )
  );

  if (stateProps.length === 0) continue;

  const variants = cs.children.map(variant => {
    const fills = (variant.fills || []).filter(f => f.visible !== false).map(f => ({
      color: f.color ? { r: f.color.r, g: f.color.g, b: f.color.b } : null,
      opacity: f.opacity
    }));
    const strokes = (variant.strokes || []).filter(s => s.visible !== false).map(s => ({
      color: s.color ? { r: s.color.r, g: s.color.g, b: s.color.b } : null,
      opacity: s.opacity,
      weight: variant.strokeWeight
    }));
    const effects = (variant.effects || []).filter(e => e.visible !== false).map(e => ({
      type: e.type,
      radius: e.radius,
      spread: e.spread,
      color: e.color ? { r: e.color.r, g: e.color.g, b: e.color.b, a: e.color.a } : null
    }));

    return {
      name: variant.name,
      opacity: variant.opacity,
      fills, strokes, effects,
      width: variant.width,
      height: variant.height,
      padding: variant.paddingTop !== undefined ? {
        top: variant.paddingTop, right: variant.paddingRight,
        bottom: variant.paddingBottom, left: variant.paddingLeft
      } : null
    };
  });

  stateMatrices.push({
    componentName: cs.name,
    stateProperty: stateProps.map(([k]) => k).join(', '),
    variantCount: variants.length,
    variants: variants
  });
}

return JSON.stringify({
  page: figma.currentPage.name,
  componentSetsWithStates: stateMatrices.length,
  matrices: stateMatrices
});
```

### How to normalize

Compare each variant against the "Default" variant to find deltas:
- Fill color changes = state color rules
- Stroke appearance/changes = hover/focus/selected indicators
- Opacity changes = disabled state (should be 0.4)
- Effect additions = focus ring, shadows
- Size/padding changes = pressed/active state adjustments

Build the state matrix table from these deltas.

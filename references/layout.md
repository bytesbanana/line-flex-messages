# Flex Message layout

Layout follows CSS Flexbox. Box component = flex container; other components = flex items.

## Box orientation

| Box | layout | Main axis | Cross axis | Children placed |
| ----- | -------- | ----------- | ------------ | ----------------- |
| Horizontal box | `horizontal` | Horizontal | Vertical | Horizontally |
| Vertical box | `vertical` | Vertical | Horizontal | Vertically |
| Baseline box | `baseline` | Horizontal | Vertical | Horizontally, aligned on common baseline |

Baseline box: `gravity` and `offsetBottom` are ignored for child components.

## Available child components

| Component | Baseline box | Horizontal box | Vertical box |
| ----------- | ------------- | ---------------- | ------------- |
| Box | No | Yes | Yes |
| Button | No | Yes | Yes |
| Image | No | Yes | Yes |
| Icon | Yes | No | No |
| Text | Yes | Yes | Yes |
| Span | No | No | No |
| Separator | No | Yes | Yes |
| Filler (deprecated) | Yes | Yes | Yes |

## Sizing

### Width allocation (horizontal box)

`flex: N` shares parent width proportionally. `flex: 0` = content-only width.

```json
{
  "type": "box",
  "layout": "horizontal",
  "contents": [
    { "type": "text", "text": "Hello", "flex": 0 },
    { "type": "text", "text": "Lorem...", "flex": 2 },
    { "type": "text", "text": "Lorem...", "flex": 3 }
  ]
}
```

`flex: 0` → `flex: 0 0 auto`; `flex: N` → `flex: N 0 0`.

### Height allocation (vertical box)

`flex: N` shares parent height proportionally.

### Box dimensions

- Width: `width` property (px or %). Sets `flex: 0`.
- Height: `height` property (px or %).
- Max width: `maxWidth` (takes precedence over `width`).
- Max height: `maxHeight` (takes precedence over `height`).
- Bubble width varies by device; prefer `flex` over fixed `width`.

### Image / Icon / Text / Span size

Use `size` keyword (`xxs`, `xs`, `sm`, `md`, `lg`, `xl`, `xxl`, `3xl`, `4xl`, `5xl`, `full`) or pixel value.

### Auto-shrink fonts

Set `adjustMode: "shrink-to-fit"` on button or text. Set `scaling: true` to respect LINE app font-size setting (accessibility).

## Positioning

### Horizontal alignment

`align`: `start` (left), `center`, `end` (right).

### Vertical alignment

`gravity`: `top`, `center`, `bottom`. Ignored for baseline children.

### Padding

`paddingAll`, `paddingTop`, `paddingBottom`, `paddingStart`, `paddingEnd`. Keywords: `none`, `xs`, `sm`, `md`, `lg`, `xl`, `xxl`. Default: `md`.

### Free-space distribution

- `justifyContent` (main axis): `flex-start`, `center`, `flex-end`, `space-between`, `space-around`, `space-evenly`
- `alignItems` (cross axis): `flex-start`, `center`, `flex-end`

Note: `justifyContent` requires all children have `flex: 0`.

### Spacing between components

`spacing` on box: keyword (`none`, `xs`, `sm`, `md`, `lg`, `xl`, `xxl`). Default: `md`. Child `margin` takes precedence.

### Offset

`offsetTop`, `offsetBottom`, `offsetStart`, `offsetEnd` shift from original position (`relative`) or parent edges (`absolute`). For absolute positioning, specify both vertical and horizontal offsets.

## Rendering order

JSON order = render order. First component renders first; later components render on top. Last component = top layer.

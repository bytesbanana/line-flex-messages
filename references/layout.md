# Flex Message layout

Layout follows [CSS Flexible Box (CSS Flexbox)](https://www.w3.org/TR/css-flexbox-1/). The Box component is the flex container; other components are flex items.

## Box orientation

Box components have three orientation modes:

| Box            | `layout` property | Main axis  | Cross axis | Child placement                 |
| -------------- | ----------------- | ---------- | ---------- | ------------------------------- |
| Horizontal box | `"horizontal"`    | Horizontal | Vertical   | Left to right                   |
| Vertical box   | `"vertical"`      | Vertical   | Horizontal | Top to bottom                   |
| Baseline box   | `"baseline"`      | Horizontal | Vertical   | Aligned on common text baseline |

### Baseline box specifics

Baseline boxes behave like horizontal boxes, except:

- Children are vertically aligned on the same text baseline (regardless of font size)
- The baseline of an icon is the bottom of the icon image
- **`gravity`** and **`offsetBottom`** properties are ignored on child components

```json
{
  "type": "box",
  "layout": "baseline",
  "contents": [
    { "type": "icon", "url": "https://example.com/icon.png", "size": "xl" },
    { "type": "text", "text": "The quick brown fox", "size": "sm" },
    { "type": "text", "text": "BIG", "size": "3xl" }
  ]
}
```

## Default flex values

| Box orientation | Default `flex` for children            |
| --------------- | -------------------------------------- |
| Horizontal box  | `1` — children expand to fill width    |
| Vertical box    | `0` — children use content-only height |
| Baseline box    | `0` — children use content-only width  |

Because horizontal box children default to `flex: 1`, set `flex: 0` on all children when using `justifyContent` to distribute free space.

## Width allocation in a horizontal box

`flex: N` allocates proportional space. `flex: 0` = content-only width.

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

In this example: remaining space after "Hello" is divided 2:3 between the second and third text components.

### flex property mapping

| Component `flex` value | CSS equivalent    |
| ---------------------- | ----------------- |
| `0`                    | `flex: 0 0 auto;` |
| `N` (horizontal)       | `flex: N 0 0;`    |
| `N` (vertical)         | `flex: N 0 auto;` |

## Height allocation in a vertical box

`flex: N` allocates proportional space in the parent height.

```json
{
  "type": "bubble",
  "body": {
    "type": "box",
    "layout": "horizontal",
    "contents": [
      {
        "type": "box",
        "layout": "vertical",
        "contents": [
          { "type": "text", "wrap": true, "text": "TEXT\nTEXT\nTEXT\nTEXT\nTEXT" }
        ],
        "backgroundColor": "#c0c0c0"
      },
      {
        "type": "box",
        "layout": "vertical",
        "contents": [
          { "type": "separator", "color": "#ff0000" },
          { "type": "text", "text": "flex=2", "flex": 2 },
          { "type": "separator", "color": "#ff0000" },
          { "type": "text", "text": "flex=3", "flex": 3 },
          { "type": "separator", "color": "#ff0000" }
        ]
      }
    ]
  }
}
```

## Box dimensions

| Property    | Effect                            | Notes                          |
| ----------- | --------------------------------- | ------------------------------ |
| `width`     | Fixed width in px or % of parent  | Sets `flex: 0` on the child    |
| `height`    | Fixed height in px or % of parent | Sets `flex: 0` on the child    |
| `maxWidth`  | Ceiling for width (px or %)       | Takes precedence over `width`  |
| `maxHeight` | Ceiling for height (px or %)      | Takes precedence over `height` |

> Bubble width varies by device. Prefer `flex` over fixed `width` to keep layouts responsive.

## Component size

### Image size

| Unit type  | Values                                                                            | Example           |
| ---------- | --------------------------------------------------------------------------------- | ----------------- |
| Keyword    | `xxs`, `xs`, `sm`, `md` (default), `lg`, `xl`, `xxl`, `3xl`, `4xl`, `5xl`, `full` | `"size": "xl"`    |
| Percentage | % of original image width                                                         | `"size": "50%"`   |
| Pixels     | Positive number with `px`                                                         | `"size": "200px"` |

Height auto-adjusts to retain `aspectRatio`. Percentages are relative to the image's original dimensions.

### Icon / Text / Span size

| Unit type | Values                                                                    | Example          |
| --------- | ------------------------------------------------------------------------- | ---------------- |
| Keyword   | `xxs`, `xs`, `sm`, `md` (default), `lg`, `xl`, `xxl`, `3xl`, `4xl`, `5xl` | `"size": "xl"`   |
| Pixels    | Positive number with `px`                                                 | `"size": "30px"` |

Percentages are **not** supported for Icon, Text, or Span.

### Automatically shrink fonts

`adjustMode: "shrink-to-fit"` on button or text shrinks font to fit the component width. This is a best-effort approach and may behave differently across platforms.

```json
{ "type": "text", "text": "A very long label", "adjustMode": "shrink-to-fit" }
```

### Scaling to LINE font-size setting

`scaling: true` on button, text, or icon scales the font/icon size to match the user's LINE app accessibility font-size setting.

```json
{
  "type": "bubble",
  "body": {
    "type": "box",
    "layout": "vertical",
    "contents": [
      { "type": "text", "text": "hello, world", "size": "30px" },
      { "type": "text", "text": "hello, world", "margin": "10px", "size": "30px", "scaling": true }
    ]
  }
}
```

`scaling` and `adjustMode: "shrink-to-fit"` can be used together on the same component.

## Positioning

### Horizontal alignment (`align`)

Aligns text or image components horizontally. Available on text, image, and span.

| Value    | Meaning                               |
| -------- | ------------------------------------- |
| `start`  | Align to the left (LTR) / right (RTL) |
| `center` | Center (default)                      |
| `end`    | Align to the right (LTR) / left (RTL) |

```json
{
  "type": "box",
  "layout": "vertical",
  "contents": [
    { "type": "text", "text": "align=start", "align": "start" },
    { "type": "separator", "color": "#ff0000" },
    { "type": "text", "text": "align=center", "align": "center" },
    { "type": "separator", "color": "#ff0000" },
    { "type": "text", "text": "align=end", "align": "end" }
  ]
}
```

### Vertical alignment (`gravity`)

Aligns text, image, or button components vertically. Ignored for children of a baseline box.

| Value    | Meaning                    |
| -------- | -------------------------- |
| `top`    | Align to the top (default) |
| `center` | Align to the middle        |
| `bottom` | Align to the bottom        |

```json
{
  "type": "box",
  "layout": "horizontal",
  "contents": [
    { "type": "text", "text": "top", "gravity": "top" },
    { "type": "separator", "color": "#ff0000" },
    { "type": "text", "text": "center", "gravity": "center" },
    { "type": "separator", "color": "#ff0000" },
    { "type": "text", "text": "bottom", "gravity": "bottom" }
  ]
}
```

## Padding

`paddingAll`, `paddingTop`, `paddingBottom`, `paddingStart`, `paddingEnd` allocate space between the parent box border and its children. `paddingStart`/`paddingEnd` respect text direction (LTR/RTL).

| Unit type  | Values                                                | Example                |
| ---------- | ----------------------------------------------------- | ---------------------- |
| Keyword    | `none`, `xs`, `sm`, `md` (default), `lg`, `xl`, `xxl` | `"paddingAll": "lg"`   |
| Percentage | % of parent box width                                 | `"paddingAll": "10%"`  |
| Pixels     | Positive number with `px`                             | `"paddingTop": "20px"` |

`paddingTop` / `paddingBottom` / `paddingStart` / `paddingEnd` take precedence over `paddingAll`.

```json
{
  "type": "box",
  "layout": "horizontal",
  "paddingAll": "80px",
  "paddingTop": "20px",
  "paddingStart": "40px",
  "contents": [
    { "type": "text", "text": "hello, world" }
  ],
  "backgroundColor": "#ffd2d2"
}
```

## Free-space distribution

### `justifyContent` (main axis)

Distributes children along the main axis. All children must have `flex: 0` for this to have effect.

| Value           | Horizontal box                          | Vertical box      |
| --------------- | --------------------------------------- | ----------------- |
| `flex-start`    | Grouped at text start edge              | Grouped at top    |
| `center`        | Grouped at center                       | Grouped at center |
| `flex-end`      | Grouped at text end edge                | Grouped at bottom |
| `space-between` | First/last at edges, equal gaps         | Same              |
| `space-around`  | Equal space on both sides of each child | Same              |
| `space-evenly`  | Equal space between all children        | Same              |

```json
{
  "type": "bubble",
  "direction": "ltr",
  "body": {
    "type": "box",
    "layout": "horizontal",
    "contents": [
      { "type": "box", "layout": "vertical", "width": "40px", "height": "30px", "backgroundColor": "#00aaff", "flex": 0 },
      { "type": "box", "layout": "vertical", "width": "20px", "height": "30px", "backgroundColor": "#00aaff", "flex": 0 },
      { "type": "box", "layout": "vertical", "width": "50px", "height": "30px", "backgroundColor": "#00aaff", "flex": 0 }
    ],
    "justifyContent": "flex-start",
    "spacing": "5px"
  }
}
```

### `alignItems` (cross axis)

Distributes children along the cross axis.

| Value        | Horizontal box    | Vertical box               |
| ------------ | ----------------- | -------------------------- |
| `flex-start` | Aligned at top    | Grouped at text start edge |
| `center`     | Aligned at middle | Grouped at center          |
| `flex-end`   | Aligned at bottom | Grouped at text end edge   |

```json
{
  "type": "bubble",
  "direction": "ltr",
  "body": {
    "type": "box",
    "layout": "horizontal",
    "contents": [
      { "type": "box", "layout": "vertical", "height": "100px", "backgroundColor": "#00aaff", "flex": 0, "width": "85px" },
      { "type": "box", "layout": "vertical", "height": "30px", "backgroundColor": "#00aaff", "flex": 0, "width": "85px" },
      { "type": "box", "layout": "vertical", "height": "60px", "backgroundColor": "#00aaff", "flex": 0, "width": "85px" }
    ],
    "spacing": "5px",
    "alignItems": "flex-start",
    "height": "200px"
  }
}
```

## Spacing between components

`spacing` on a box sets the minimum gap between adjacent children. Child `margin` takes precedence over box `spacing`.

| Unit type | Values                                                | Example             |
| --------- | ----------------------------------------------------- | ------------------- |
| Keyword   | `none`, `xs`, `sm`, `md` (default), `lg`, `xl`, `xxl` | `"spacing": "lg"`   |
| Pixels    | Positive number with `px`                             | `"spacing": "10px"` |

```json
{
  "type": "box",
  "layout": "horizontal",
  "spacing": "md",
  "contents": [
    { "type": "box", "layout": "vertical", "contents": [{ "type": "text", "text": "TEXT1" }], "backgroundColor": "#80ffff" },
    { "type": "box", "layout": "vertical", "contents": [{ "type": "text", "text": "TEXT2" }], "backgroundColor": "#80ffff" },
    { "type": "box", "layout": "vertical", "contents": [{ "type": "text", "text": "TEXT3" }], "backgroundColor": "#80ffff" }
  ]
}
```

## Margin on components

`margin` on a child component sets the minimum space before that component.

| Unit type | Values                                                | Example            |
| --------- | ----------------------------------------------------- | ------------------ |
| Keyword   | `none`, `xs`, `sm`, `md` (default), `lg`, `xl`, `xxl` | `"margin": "xxl"`  |
| Pixels    | Positive number with `px`                             | `"margin": "10px"` |

`margin` takes precedence over the parent box's `spacing`. If set on the first child, space is allocated before it.

```json
{
  "type": "box",
  "layout": "horizontal",
  "spacing": "md",
  "contents": [
    { "type": "box", "layout": "vertical", "contents": [{ "type": "text", "text": "TEXT1" }], "backgroundColor": "#80ffff" },
    { "type": "box", "layout": "vertical", "contents": [{ "type": "text", "text": "TEXT2" }], "backgroundColor": "#80ffff" },
    { "type": "box", "layout": "vertical", "contents": [{ "type": "text", "text": "TEXT3" }], "backgroundColor": "#80ffff", "margin": "xxl" }
  ]
}
```

## Offset (relative vs absolute)

`offsetTop`, `offsetBottom`, `offsetStart`, `offsetEnd` shift a component from its original or parent-edge position.

| Unit type  | Values                                           | Example                |
| ---------- | ------------------------------------------------ | ---------------------- |
| Keyword    | `none`, `xs`, `sm`, `md`, `lg`, `xl`, `xxl`      | `"offsetStart": "xl"`  |
| Percentage | % of box width (horizontal) or height (vertical) | `"offsetStart": "20%"` |
| Pixels     | Positive number with `px`                        | `"offsetTop": "10px"`  |

### Relative positioning (`position: "relative"`)

Shifts the component from its original position. Follows CSS [relative positioning](https://www.w3.org/TR/css-position-3/#relpos-insets).

- `offsetTop` — shifts down from the top edge
- `offsetBottom` — shifts up from the bottom edge
- `offsetStart` — shifts in the text-start direction (right in LTR, left in RTL)
- `offsetEnd` — shifts in the text-end direction (left in LTR, right in RTL)

```json
{
  "type": "box",
  "layout": "vertical",
  "contents": [
    { "type": "box", "layout": "horizontal", "contents": [{ "type": "text", "text": "REFERENCE BOX\n1\n2\n3", "align": "center", "wrap": true }], "backgroundColor": "#80ffff" },
    {
      "type": "box",
      "layout": "horizontal",
      "contents": [{ "type": "text", "text": "TARGET" }],
      "backgroundColor": "#ff8080",
      "offsetTop": "10px",
      "offsetStart": "40px",
      "position": "relative"
    }
  ]
}
```

### Absolute positioning (`position: "absolute"`)

Shifts the component from the parent box's edges. Follows CSS [absolute positioning](https://www.w3.org/TR/css-position-3/#abs-pos).

- `offsetTop` / `offsetBottom` — relative to parent top / bottom edge
- `offsetStart` / `offsetEnd` — relative to parent left / right edge in LTR (reversed in RTL)

> When using absolute positioning, specify both vertical (`offsetTop` or `offsetBottom`) and horizontal (`offsetStart` or `offsetEnd`) offsets. Without both, position may vary by device.

```json
{
  "type": "box",
  "layout": "vertical",
  "contents": [
    { "type": "box", "layout": "horizontal", "contents": [{ "type": "text", "text": "REFERENCE BOX\n1\n2\n3", "align": "center", "wrap": true }], "backgroundColor": "#80ffff" },
    {
      "type": "box",
      "layout": "horizontal",
      "contents": [{ "type": "text", "text": "TARGET" }],
      "backgroundColor": "#ff8080",
      "position": "absolute",
      "offsetStart": "40px",
      "offsetEnd": "80px",
      "offsetTop": "10px",
      "offsetBottom": "20px"
    }
  ]
}
```

An absolutely-positioned child box does not affect parent dimensions — it can overflow the parent (overflowing parts are hidden).

## Linear gradient backgrounds

Set `background.type: "linearGradient"` on a box to fill it with a linear gradient.

> The parent box's text direction (`LTR` / `RTL`) does **not** affect the gradient direction.

### Angle

`angle` accepts `0`–`360` degrees (integer or decimal). Direction rotates clockwise:

| Angle    | Direction               |
| -------- | ----------------------- |
| `0deg`   | Bottom → top            |
| `45deg`  | Bottom-left → top-right |
| `90deg`  | Left → right            |
| `180deg` | Top → bottom            |

```json
{
  "type": "bubble",
  "body": {
    "type": "box",
    "layout": "vertical",
    "contents": [],
    "background": {
      "type": "linearGradient",
      "angle": "90deg",
      "startColor": "#ff0000",
      "endColor": "#0000ff"
    },
    "height": "200px"
  }
}
```

### Color stops (three-color gradient)

Add `centerColor` to introduce a middle color. `centerPosition` sets where the middle color sits (0% = at start, 100% = at end).

```json
{
  "type": "bubble",
  "body": {
    "type": "box",
    "layout": "vertical",
    "contents": [],
    "background": {
      "type": "linearGradient",
      "angle": "0deg",
      "startColor": "#ff0000",
      "centerColor": "#0000ff",
      "endColor": "#00ff00",
      "centerPosition": "10%"
    },
    "height": "200px"
  }
}
```

## Rendering order

JSON order = render order. The first component in the array renders first; later components render on top. The last component in the array is the topmost layer.

To change the visual stacking order, reorder components in the JSON.

## See also

- [Flex Message send](/references/send.md) — Sending via Messaging API, bubble sizes, text direction
- [Flex Message elements](/references/elements.md) — Container/Block/Component taxonomy
- [Flex Message video](/references/video.md) — Video requirements and playback behavior

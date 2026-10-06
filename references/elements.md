# Flex Message elements

Flex Messages have a three-level hierarchy:

```
Container (Bubble | Carousel)
  └── Block (Header | Hero | Body | Footer)
        └── Component (Box | Button | Image | Video | Icon | Text | Span | Separator | Filler)
```

## Container

Container is the top-level building block of a Flex Message.

| Type     | Description                                             |
| -------- | ------------------------------------------------------- |
| Bubble   | Displays a single message bubble                        |
| Carousel | Displays multiple bubbles, browsed by horizontal scroll |

### Bubble

A container that displays one instance of a message bubble.

```json
{
  "type": "bubble",
  "body": {
    "type": "box",
    "layout": "vertical",
    "contents": [
      { "type": "text", "text": "Hello, World!" }
    ]
  }
}
```

### Carousel

A container that displays multiple bubbles side by side. Users scroll horizontally to browse.

```json
{
  "type": "carousel",
  "contents": [
    {
      "type": "bubble",
      "body": {
        "type": "box",
        "layout": "horizontal",
        "contents": [
          {
            "type": "text",
            "text": "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
            "wrap": true
          }
        ]
      },
      "footer": {
        "type": "box",
        "layout": "horizontal",
        "contents": [
          {
            "type": "button",
            "style": "primary",
            "action": {
              "type": "uri",
              "label": "Go",
              "uri": "https://example.com"
            }
          }
        ]
      }
    },
    {
      "type": "bubble",
      "body": {
        "type": "box",
        "layout": "horizontal",
        "contents": [
          { "type": "text", "text": "Hello, World!", "wrap": true }
        ]
      },
      "footer": {
        "type": "box",
        "layout": "horizontal",
        "contents": [
          {
            "type": "button",
            "style": "primary",
            "action": {
              "type": "uri",
              "label": "Go",
              "uri": "https://example.com"
            }
          }
        ]
      }
    }
  ]
}
```

## Block

Blocks compose a bubble. Each block type is used at most once per bubble, in this order:

| Type   | Description                    |
| ------ | ------------------------------ |
| Header | Message subject or header      |
| Hero   | Main image or video            |
| Body   | Main message content           |
| Footer | Buttons and supplementary info |

You do not need to use all four blocks. If used, each can appear only once.

```json
{
  "type": "bubble",
  "styles": {
    "header": { "backgroundColor": "#ffaaaa" },
    "body":   { "backgroundColor": "#aaffaa" },
    "footer":  { "backgroundColor": "#aaaaff" }
  },
  "header": {
    "type": "box",
    "layout": "vertical",
    "contents": [ { "type": "text", "text": "header" } ]
  },
  "hero": {
    "type": "image",
    "url": "https://example.com/flex/images/image.jpg",
    "size": "full",
    "aspectRatio": "2:1"
  },
  "body": {
    "type": "box",
    "layout": "vertical",
    "contents": [ { "type": "text", "text": "body" } ]
  },
  "footer": {
    "type": "box",
    "layout": "vertical",
    "contents": [ { "type": "text", "text": "footer" } ]
  }
}
```

## Component

Components are the UI primitives that live inside blocks.

| Component | Description          | Baseline box | Horizontal box | Vertical box |
| --------- | -------------------- | :----------: | :------------: | :----------: |
| Box       | Layout container     |      No      |      Yes       |     Yes      |
| Button    | Tappable action      |      No      |      Yes       |     Yes      |
| Image     | Raster image         |      No      |      Yes       |     Yes      |
| Icon      | Decorative icon      |     Yes      |       No       |      No      |
| Text      | Single text string   |     Yes      |      Yes       |     Yes      |
| Span      | Styled text segments |      No      |       No       |      No      |
| Separator | Dividing line        |      No      |      Yes       |     Yes      |
| Filler    | Empty space          |     Yes      |      Yes       |     Yes      |

### Box

A layout container that holds other components. Any component can be nested inside a box, including another box.

```json
{
  "type": "bubble",
  "body": {
    "type": "box",
    "layout": "vertical",
    "spacing": "md",
    "contents": [
      { "type": "text", "text": "Item 1" },
      { "type": "separator" },
      { "type": "text", "text": "Item 2" }
    ]
  }
}
```

### Button

A tappable button. Three styles are available:

```json
{
  "type": "bubble",
  "body": {
    "type": "box",
    "layout": "vertical",
    "spacing": "md",
    "contents": [
      {
        "type": "button",
        "style": "primary",
        "action": { "type": "uri", "label": "Primary style button", "uri": "https://example.com" }
      },
      {
        "type": "button",
        "style": "secondary",
        "action": { "type": "uri", "label": "Secondary style button", "uri": "https://example.com" }
      },
      {
        "type": "button",
        "style": "link",
        "action": { "type": "uri", "label": "Link style button", "uri": "https://example.com" }
      }
    ]
  }
}
```

- **`primary`** — dark colored background, for standalone or first action
- **`secondary`** — light colored background, for less prominent actions
- **`link`** — renders like an HTML text link; recommended when stacking multiple buttons vertically

All styles support custom `color`. The `adjustMode: "shrink-to-fit"` property shrinks font to fit the button width. Set `scaling: true` to respect the user's LINE app font-size setting (accessibility).

### Image

Renders a raster image (JPEG or PNG). Image URL must be HTTPS.

```json
{
  "type": "bubble",
  "body": {
    "type": "box",
    "layout": "horizontal",
    "contents": [
      {
        "type": "image",
        "url": "https://example.com/flex/images/image.jpg",
        "size": "md"
      }
    ]
  }
}
```

Image `size` accepts:

| Unit type  | Values                                                                            | Example           |
| ---------- | --------------------------------------------------------------------------------- | ----------------- |
| Keyword    | `xxs`, `xs`, `sm`, `md` (default), `lg`, `xl`, `xxl`, `3xl`, `4xl`, `5xl`, `full` | `"size": "xl"`    |
| Percentage | % of original image width                                                         | `"size": "50%"`   |
| Pixels     | Positive number with `px`                                                         | `"size": "200px"` |

Height is auto-calculated to retain the `aspectRatio`. If no `aspectRatio` is specified, the image uses its natural dimensions.

### Video

Renders a video in the hero block. See [`references/video.md`](./video.md) for full requirements.

```json
{
  "type": "bubble",
  "size": "mega",
  "hero": {
    "type": "video",
    "url": "https://example.com/video.mp4",
    "previewUrl": "https://example.com/video_preview.jpg",
    "altContent": {
      "type": "image",
      "size": "full",
      "aspectRatio": "20:13",
      "aspectMode": "cover",
      "url": "https://example.com/image.jpg"
    },
    "aspectRatio": "20:13"
  }
}
```

Video requires: bubble size `kilo`, `mega`, or `giga`; not inside a Carousel; `altContent` fallback for older LINE versions. See [`references/video.md`](./video.md) for aspect-ratio rules, playback behavior, and URI action.

### Icon

Decorative icon for adjacent text. Can only be used in a **baseline box**.

```json
{
  "type": "bubble",
  "body": {
    "type": "box",
    "layout": "vertical",
    "contents": [
      {
        "type": "box",
        "layout": "baseline",
        "contents": [
          { "type": "icon", "url": "https://example.com/flex/images/icon.png", "size": "md" },
          { "type": "text", "text": "The quick brown fox jumps over the lazy dog", "size": "md" }
        ]
      },
      {
        "type": "box",
        "layout": "baseline",
        "contents": [
          { "type": "icon", "url": "https://example.com/flex/images/icon.png", "size": "lg" },
          { "type": "text", "text": "The quick brown fox jumps over the lazy dog", "size": "lg" }
        ]
      },
      {
        "type": "box",
        "layout": "baseline",
        "contents": [
          { "type": "icon", "url": "https://example.com/flex/images/icon.png", "size": "xl" },
          { "type": "text", "text": "The quick brown fox jumps over the lazy dog", "size": "xl" }
        ]
      },
      {
        "type": "box",
        "layout": "baseline",
        "contents": [
          { "type": "icon", "url": "https://example.com/flex/images/icon.png", "size": "xxl" },
          { "type": "text", "text": "The quick brown fox jumps over the lazy dog", "size": "xxl" }
        ]
      },
      {
        "type": "box",
        "layout": "baseline",
        "contents": [
          { "type": "icon", "url": "https://example.com/flex/images/icon.png", "size": "3xl" },
          { "type": "text", "text": "The quick brown fox jumps over the lazy dog", "size": "3xl" }
        ]
      }
    ]
  }
}
```

Icon `size` accepts keywords `xxs`–`5xl` (no `full`) or a pixel value. The icon's baseline is the bottom of the icon image.

### Text

Renders a single text string. Supports color, size, weight, and alignment.

```json
{
  "type": "bubble",
  "body": {
    "type": "box",
    "layout": "vertical",
    "contents": [
      { "type": "text", "text": "Closing the distance", "size": "md", "align": "center", "color": "#ff0000" },
      { "type": "text", "text": "Closing the distance", "size": "lg", "align": "center", "color": "#00ff00" },
      { "type": "text", "text": "Closing the distance", "size": "xl", "align": "center", "weight": "bold", "color": "#0000ff" }
    ]
  }
}
```

Text `size` accepts keywords `xxs`–`5xl` (no `full`) or a pixel value. `weight` accepts `regular` or `bold`.

#### Text wrapping

By default, overflowing text is truncated with an ellipsis. Set `wrap: true` to enable multi-line wrapping.

```json
{
  "type": "bubble",
  "body": {
    "type": "box",
    "layout": "horizontal",
    "contents": [
      {
        "type": "text",
        "text": "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod\n tempor incididunt ut labore et dolore magna aliqua.",
        "wrap": true,
        "lineSpacing": "20px"
      }
    ]
  }
}
```

Use `\n` in the `text` string to force a line break. Note: `\n` at the end of a text string may render differently across devices.

#### Line spacing

`lineSpacing` specifies the space between lines of wrapped text. **Not applied to the top of the first line or the bottom of the last line.**

```json
{ "type": "text", "text": "Multi-line\ntext here", "wrap": true, "lineSpacing": "20px" }
```

### Span

Renders multiple styled text segments inside a Text component. Each span has independent `color`, `size`, `weight`, and `decoration`.

```json
{
  "type": "bubble",
  "body": {
    "type": "box",
    "layout": "horizontal",
    "contents": [
      {
        "type": "text",
        "text": "hello, world",
        "contents": [
          { "type": "span", "text": "Hello, world!", "decoration": "line-through" },
          { "type": "span", "text": "\nClosing", "color": "#ff0000", "size": "sm", "weight": "bold", "decoration": "none" },
          { "type": "span", "text": " " },
          { "type": "span", "text": "the", "size": "lg", "color": "#00ff00", "decoration": "underline", "weight": "bold" },
          { "type": "span", "text": " " },
          { "type": "span", "text": "distance", "color": "#0000ff", "weight": "bold", "size": "xxl" }
        ],
        "wrap": true,
        "align": "center"
      }
    ]
  }
}
```

Span `decoration` values: `none` (resets decoration), `underline`, `line-through`. Set `decoration: "none"` to explicitly clear a decoration inherited from a parent style.

### Separator

A dividing line inside a box. Vertical in a horizontal box; horizontal in a vertical box.

```json
{
  "type": "bubble",
  "body": {
    "type": "box",
    "layout": "vertical",
    "spacing": "md",
    "contents": [
      {
        "type": "box",
        "layout": "horizontal",
        "spacing": "md",
        "contents": [
          { "type": "text", "text": "orange" },
          { "type": "separator" },
          { "type": "text", "text": "apple" }
        ]
      },
      { "type": "separator" },
      {
        "type": "box",
        "layout": "horizontal",
        "spacing": "md",
        "contents": [
          { "type": "text", "text": "grape" },
          { "type": "separator" },
          { "type": "text", "text": "lemon" }
        ]
      }
    ]
  }
}
```

Set `color` to change the separator line color. Set `margin` on the separator to add space from adjacent components.

### Filler (deprecated)

> **Deprecated.** Use the `margin` property of each component instead.

Filler renders an empty space between components inside a box. Use `margin` on child components to achieve the same result.

```json
{
  "type": "bubble",
  "body": {
    "type": "box",
    "layout": "horizontal",
    "contents": [
      { "type": "image", "url": "https://example.com/flex/images/image.jpg" },
      { "type": "filler" },
      { "type": "image", "url": "https://example.com/flex/images/image.jpg" }
    ]
  }
}
```

## See also

- [Flex Message send](/references/send.md) — Sending via Messaging API, bubble sizes, text direction
- [Flex Message layout](/references/layout.md) — Box orientation, sizing, positioning, linear gradient
- [Flex Message video](/references/video.md) — Video requirements and playback behavior

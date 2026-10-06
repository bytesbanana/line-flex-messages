# Flex Message elements

Flex Messages have a three-level hierarchy:

```text
Container (Bubble | Carousel)
  └── Block (Header | Hero | Body | Footer)
        └── Component (Box | Button | Image | Video | Icon | Text | Span | Separator | Filler)
```

## Container

| Type | Description |
|------|-------------|
| Bubble | Single message bubble |
| Carousel | Multiple bubbles, browsed by horizontal scroll |

## Block

Blocks compose a bubble. Order: header → hero → body → footer. Each used at most once per bubble.

| Type | Description |
| ------ | ------------- |
| Header | Message subject |
| Hero | Main image |
| Body | Main message |
| Footer | Buttons and supplementary info |

## Component

### Box

Horizontal, vertical, or baseline layout container for other components.

### Button

Tappable button. Styles: `primary`, `secondary`, `link`.

```json
{
  "type": "bubble",
  "body": {
    "type": "box",
    "layout": "vertical",
    "spacing": "md",
    "contents": [
      { "type": "button", "style": "primary",   "action": { "type": "uri", "label": "Go",       "uri": "https://example.com" } },
      { "type": "button", "style": "secondary", "action": { "type": "uri", "label": "Details",  "uri": "https://example.com" } },
      { "type": "button", "style": "link",      "action": { "type": "uri", "label": "Learn more","uri": "https://example.com" } }
    ]
  }
}
```

### Image

Renders an image.

### Video

Renders a video in the hero block. See `references/video.md` for requirements, aspect-ratio rules, and `altContent` fallback.

```json
{
  "type": "bubble",
  "size": "mega",
  "hero": {
    "type": "video",
    "url": "https://example.com/video.mp4",
    "previewUrl": "https://example.com/video_preview.jpg",
    "altContent": { "type": "image", "size": "full", "aspectRatio": "20:13", "aspectMode": "cover", "url": "https://example.com/image.jpg" },
    "aspectRatio": "20:13"
  }
}
```

### Icon

Decorative icon for adjacent text. Baseline box only.

### Text

Text string. Set `wrap: true` to wrap long text; use `\n` for newlines. Default overflow is ellipsis.

```json
{
  "type": "text",
  "text": "Lorem ipsum dolor sit amet...",
  "wrap": true,
  "lineSpacing": "20px"
}
```

### Span

Multiple text strings with different styles (color, size, weight, decoration) inside a Text component.

```json
{
  "type": "text",
  "text": "hello, world",
  "contents": [
    { "type": "span", "text": "Hello, world!", "decoration": "line-through" },
    { "type": "span", "text": "\nClosing", "color": "#ff0000", "size": "sm", "weight": "bold" },
    { "type": "span", "text": " " },
    { "type": "span", "text": "the", "size": "lg", "color": "#00ff00", "decoration": "underline", "weight": "bold" },
    { "type": "span", "text": " " },
    { "type": "span", "text": "distance", "color": "#0000ff", "weight": "bold", "size": "xxl" }
  ],
  "wrap": true,
  "align": "center"
}
```

### Separator

Separating line. Vertical in horizontal box; horizontal in vertical box.

### Filler

Deprecated. Use component margin properties instead.

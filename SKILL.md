---
name: line-flex-messages
description: Agent guidance for authoring and sending LINE Flex Messages via the Messaging API. Activate when building Flex Message layouts, deciding between Bubble/Carousel, debugging rendering, or integrating push/reply/multicast endpoints.
---

# line-flex-messages

## Mental model

```
Container (Bubble | Carousel)
  └── Block (Header | Hero | Body | Footer)
        └── Component (Box | Button | Image | Video | Icon | Text | Span | Separator)
```

- **Container**: Bubble (single) or Carousel (multiple, horizontally scrollable)
- **Block**: Header / Hero / Body / Footer — each at most once per bubble, in that order
- **Component**: the actual UI primitives

## Send a Flex Message

Messaging API push request shape:

```sh
curl -v -X POST https://api.line.me/v2/bot/message/push \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {channel_access_token}' \
  -d '{
    "to": "U4af4980629...",
    "messages": [{
      "type": "flex",
      "altText": "this is the fallback text shown in notification",
      "contents": { ... }
    }]
  }'
```

- `altText` is mandatory for every flex message
- `contents` is the Flex Message JSON (Bubble or Carousel root object)
- Supported types: push, reply, multicast — all share the same `contents` shape

## Hello World

```json
{
  "type": "bubble",
  "body": {
    "type": "box",
    "layout": "horizontal",
    "contents": [
      { "type": "text", "text": "Hello," },
      { "type": "text", "text": "World!" }
    ]
  }
}
```

## Layout rules

- **Default orientation**: vertical
- **Sizing keywords**: `xxs` `xs` `sm` `md` `lg` `xl` `xxl` `3xl` `4xl` `5xl` `full` (plus `kilo`/`mega`/`giga` for bubbles)
- **`flex: N`** allocates proportional space in parent box; `flex: 0` = content-only
- **`wrap: true`** on Text enables multi-line wrapping; default overflow = ellipsis
- **`align`** (horizontal): `start` | `center` | `end`
- **`gravity`** (vertical): `top` | `center` | `bottom`
- **Baseline box**: children align on common text baseline; `gravity`/`offsetBottom` ignored
- **JSON order = render order**: later siblings render on top of earlier ones

## Versioned features

| Feature | iOS/Android | LINE for PC |
|---------|-------------|-------------|
| `maxWidth`, `maxHeight`, `lineSpacing` | ≥ 11.22.0 | ≥ 7.7.0 |
| `deca`/`hecto` bubble sizes | ≥ 13.6.0 | ≥ 7.17.0 |
| `scaling` on button/text/icon | ≥ 13.6.0 | ≥ 7.17.0 |
| Video component | ≥ 11.22.0 | ≥ 7.7.0 |

If LINE version is lower than required: video displays `altContent`; unsupported bubble sizes fall back to `kilo`.

## Video

- Must live in **hero block**
- Bubble size must be `kilo`, `mega`, or `giga`
- Not usable inside a Carousel
- `aspectRatio` must match video file AND `previewUrl` image
- Always specify `altContent` (image or box) for older LINE versions
- Auto-play controlled by user settings; not supported on PC LINE

See `references/video.md` for full details.

## Simulator

Flex Message Simulator renders a preview without sending. Use it to validate layout before pushing to a real channel. Video is not playable in the Simulator — `altContent` is shown instead.

## Debugging rendering issues

1. Check LINE version support table above
2. Confirm `altText` is present (required)
3. Verify Bubble size keyword is not `deca`/`hecto` if supporting older LINE versions
4. For video: ensure `altContent` is set and aspect ratios match across `url` / `aspectRatio` / `previewUrl`
5. For layout: confirm box orientation and `flex` values are as intended; baseline box restrictions apply
6. Test via Simulator first, then push to a test user

## References

- `references/elements.md` — Container/Block/Component taxonomy with JSON examples
- `references/layout.md` — Full layout reference: orientation, sizing, positioning, spacing
- `references/video.md` — Video requirements, aspect ratio, altContent, playback behavior

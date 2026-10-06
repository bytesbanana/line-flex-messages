---
name: line-flex-messages-skill
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
- Supported types: push, reply, multicast, narrowcast, broadcast — all share the same `contents` shape

See [`references/send.md`](references/send.md) for bubble size keywords, LTR/RTL text direction, and full send details.

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
- **Sizing keywords**: `xxs` `xs` `sm` `md` `lg` `xl` `xxl` `3xl` `4xl` `5xl` `full` (plus `nano`/`micro`/`kilo`/`mega`/`giga`/`deca`/`hecto` for bubbles)
- **`flex: N`** allocates proportional space in parent box; `flex: 0` = content-only
- **Default flex**: `1` for children of a horizontal box; `0` for vertical box children
- **`wrap: true`** on Text enables multi-line wrapping; default overflow = ellipsis
- **`align`** (horizontal): `start` | `center` | `end`
- **`gravity`** (vertical): `top` | `center` | `bottom`
- **Baseline box**: children align on common text baseline; `gravity`/`offsetBottom` ignored
- **JSON order = render order**: later siblings render on top of earlier ones
- **`direction`** at bubble level: `"ltr"` or `"rtl"` sets text flow for the whole bubble
- **Linear gradient**: `background.type: "linearGradient"` on a box; use `angle`, `startColor`, `endColor`, optionally `centerColor` + `centerPosition`

## Versioned features

| Feature                                | LINE version required | iOS/Android | LINE for PC |
| -------------------------------------- | --------------------- | ----------- | ----------- |
| `maxWidth`, `maxHeight`, `lineSpacing` | ≥                     | 11.22.0     | 7.7.0       |
| Video component                        | ≥                     | 11.22.0     | 7.7.0       |
| `deca` / `hecto` bubble sizes          | ≥                     | 13.6.0      | 7.17.0      |
| `scaling` on button / text / icon      | ≥                     | 13.6.0      | 7.17.0      |

If LINE version is lower than required: video displays `altContent`; unsupported bubble sizes fall back to `kilo`.

## Video

- Must live in **hero block**
- Bubble size must be `kilo`, `mega`, or `giga`
- Not usable inside a Carousel
- `aspectRatio` must match video file AND `previewUrl` image
- Always specify `altContent` (image or box) for older LINE versions
- Auto-play controlled by user settings; not supported on LINE for PC
- URI action label appears in 3 places: chat room after playback, video player during, video player after

See [`references/video.md`](references/video.md) for full details.

## Simulator

Flex Message Simulator renders a preview without sending. Use it to validate layout before pushing to a real channel. Video is not playable in the Simulator — `altContent` is shown instead. Send from the Simulator to your LINE app to test video playback.

See [`references/simulator.md`](references/simulator.md) for the full no-code composition workflow.

## Debugging rendering issues

1. Check the LINE version support table above
2. Confirm `altText` is present (required)
3. Verify bubble size keyword is not `deca`/`hecto` if supporting older LINE versions
4. For video: ensure `altContent` is set and aspect ratios match across `url` / `aspectRatio` / `previewUrl`
5. For layout: confirm box orientation and `flex` values are as intended; baseline box restrictions apply
6. Test via Simulator first, then push to a test user

## References

- [`references/send.md`](references/send.md) — Sending via Messaging API, bubble sizes, LTR/RTL, limitations
- [`references/elements.md`](references/elements.md) — Container/Block/Component taxonomy with full JSON examples
- [`references/layout.md`](references/layout.md) — Box orientation, sizing, positioning, linear gradient
- [`references/video.md`](references/video.md) — Video requirements, aspect ratio, altContent, playback behavior
- [`references/simulator.md`](references/simulator.md) — Flex Message Simulator no-code workflow

(End of file - total 117 lines)

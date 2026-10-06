# Flex Message video

Display video in the hero block.

## Requirements

- Hero block `type`: `video`
- Bubble size: `kilo`, `mega`, or `giga`
- Not a child of a carousel

## Aspect ratio

All three must match:

1. Video file aspect ratio
2. `aspectRatio` property
3. `previewUrl` image aspect ratio

## altContent fallback

If LINE version < required, `altContent` is displayed instead. Use a box or image component.

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

## URI action

`action` property on video renders a button in:

- Chat room (after playback)
- Video player (during and after playback)

## Playback behavior

| Setting | Playback |
| --------- | ---------- |
| Auto-play on mobile + Wi-Fi | Starts automatically |
| Auto-play on Wi-Fi only | Auto-plays on Wi-Fi |
| Never | Manual start only |

PC LINE: auto-play not supported.

After playback finishes: up to two buttons shown — Play (replay) and More information (URI).

## Simulator

Video cannot be previewed in the Flex Message Simulator. `altContent` is shown instead. Send from simulator to your LINE app to test video.

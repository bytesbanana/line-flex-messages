# Flex Message video

Display a video in the hero block of a Flex Message bubble.

## Requirements

A video component requires all of the following:

1. Hero block `type` set to `"video"`
2. Bubble `size` must be `kilo`, `mega`, or `giga`
3. The bubble is **not** a child of a Carousel

## Aspect ratio

All three must match — mismatches cause cropping or incorrect display:

1. The aspect ratio of the video file itself
2. The `aspectRatio` property value
3. The aspect ratio of the image at `previewUrl`

If a preview image has a different aspect ratio than the video, it may appear misaligned behind the playing video.

## altContent fallback

If the LINE version is lower than required (iOS/Android < 11.22.0, PC < 7.7.0), the `altContent` component is displayed instead. `altContent` can be an **image** or a **box**.

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

## Image URL requirements

All image and video URLs in Flex Messages must be HTTPS (TLS 1.2+). Image files must satisfy:

| Requirement           | Value                       |
| --------------------- | --------------------------- |
| Format                | JPEG or PNG                 |
| Max dimensions        | 1024 × 1024 px              |
| Max file size         | 10 MB                       |
| Recommended file size | ≤ 1 MB (for faster display) |

## URI action

Add an `action` property to the video component to let users open a URL after or during playback. The action label appears in three places:

1. **Chat room** — after video playback finishes
2. **Video player** — while video is playing
3. **Video player** — after video playback finishes

```json
{
  "type": "bubble",
  "size": "mega",
  "hero": {
    "type": "video",
    "url": "https://example.com/video.mp4",
    "previewUrl": "https://example.com/video_preview.png",
    "altContent": {
      "type": "image",
      "size": "full",
      "aspectRatio": "20:13",
      "aspectMode": "cover",
      "url": "https://example.com/image.png"
    },
    "action": {
      "type": "uri",
      "label": "More information",
      "uri": "https://example.com/"
    },
    "aspectRatio": "20:13"
  }
}
```

## Full example — cafe business card

This bubble combines a video hero with a body (name, star rating, place info) and footer (call and website buttons).

```json
{
  "type": "bubble",
  "size": "mega",
  "hero": {
    "type": "video",
    "url": "https://example.com/video.mp4",
    "previewUrl": "https://example.com/video_preview.png",
    "altContent": {
      "type": "image",
      "size": "full",
      "aspectRatio": "20:13",
      "aspectMode": "cover",
      "url": "https://example.com/image.png"
    },
    "action": {
      "type": "uri",
      "label": "More information",
      "uri": "https://example.com/"
    },
    "aspectRatio": "20:13"
  },
  "body": {
    "type": "box",
    "layout": "vertical",
    "contents": [
      {
        "type": "text",
        "text": "Brown Cafe",
        "weight": "bold",
        "size": "xl"
      },
      {
        "type": "box",
        "layout": "baseline",
        "margin": "md",
        "contents": [
          { "type": "icon", "size": "sm", "url": "https://example.com/star.png" },
          { "type": "icon", "size": "sm", "url": "https://example.com/star.png" },
          { "type": "icon", "size": "sm", "url": "https://example.com/star.png" },
          { "type": "icon", "size": "sm", "url": "https://example.com/star.png" },
          { "type": "icon", "size": "sm", "url": "https://example.com/gray_star.png" },
          {
            "type": "text",
            "text": "4.0",
            "size": "sm",
            "color": "#999999",
            "margin": "md",
            "flex": 0
          }
        ]
      },
      {
        "type": "box",
        "layout": "vertical",
        "margin": "lg",
        "spacing": "sm",
        "contents": [
          {
            "type": "box",
            "layout": "baseline",
            "spacing": "sm",
            "contents": [
              { "type": "text", "text": "Place", "color": "#aaaaaa", "size": "sm", "flex": 1 },
              { "type": "text", "text": "1-3 Kioicho, Chiyoda-ku, Tokyo", "wrap": true, "color": "#666666", "size": "sm", "flex": 5 }
            ]
          },
          {
            "type": "box",
            "layout": "baseline",
            "spacing": "sm",
            "contents": [
              { "type": "text", "text": "Time", "color": "#aaaaaa", "size": "sm", "flex": 1 },
              { "type": "text", "text": "10:00 - 23:00", "wrap": true, "color": "#666666", "size": "sm", "flex": 5 }
            ]
          }
        ]
      }
    ]
  },
  "footer": {
    "type": "box",
    "layout": "vertical",
    "spacing": "sm",
    "contents": [
      {
        "type": "button",
        "style": "link",
        "height": "sm",
        "action": { "type": "uri", "label": "CALL", "uri": "https://example.com" }
      },
      {
        "type": "button",
        "style": "link",
        "height": "sm",
        "action": { "type": "uri", "label": "WEBSITE", "uri": "https://example.com" }
      },
      {
        "type": "box",
        "layout": "vertical",
        "contents": [],
        "margin": "sm"
      }
    ],
    "flex": 0
  }
}
```

## Playback behavior

### In a chat room

How playback starts depends on the user's **Photos & videos → Auto-play videos** setting in LINE:

| User setting      | Behavior                           |
| ----------------- | ---------------------------------- |
| On mobile & Wi-Fi | Starts automatically               |
| On Wi-Fi only     | Starts automatically on Wi-Fi only |
| Never             | Manual start only (user taps)      |

Auto-play is **not supported on LINE for PC**.

#### After playback finishes (chat room)

Up to two buttons appear over the video:

- **Play** — re-opens the video player and replays
- **More information** — opens the URI action URL (shown only if `action` is defined)

### In a video player

When a user taps the video in a chat room, a dedicated video player launches.

#### During playback

Up to two buttons at the top of the player:

- **Done** — closes the player and returns to the chat room (auto-play continues in chat if conditions are met)
- **More information** — opens the URI action URL (shown only if `action` is defined)

#### After playback finishes (video player)

Up to two buttons over the video:

- **Replay** — plays the video again from the start
- **More information** — opens the URI action URL (shown only if `action` is defined)

## Flex Message Simulator

The Simulator cannot play video — it shows `altContent` instead (same as older LINE versions). To test the actual video, use the **Send...** button in the Simulator to deliver the message to your own LINE app.

## See also

- [Flex Message elements](/references/elements.md) — Component taxonomy and JSON examples
- [Flex Message layout](/references/layout.md) — Box sizing and positioning
- [Flex Message Simulator](/references/simulator.md) — Full no-code composition workflow

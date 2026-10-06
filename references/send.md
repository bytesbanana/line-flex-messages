# Flex Message send

How to send a Flex Message via the Messaging API — JSON structure, message types, bubble sizing, and text direction.

## Messaging API request

Flex Messages are sent with `type: "flex"` inside a message object. The `contents` property holds the Flex Message JSON.

```sh
curl -v -X POST https://api.line.me/v2/bot/message/push \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {channel_access_token}' \
  -d '{
    "to": "U4af4980629...",
    "messages": [{
      "type": "flex",
      "altText": "fallback text shown in notification",
      "contents": { ... }
    }]
  }'
```

- **`altText`** — mandatory fallback text for notifications and LINE for PC.
- **`contents`** — the Flex Message JSON (Bubble or Carousel root object).

## Supported message types

Flex content works with all Messaging API send methods:

| Method     | Endpoint                          | Use case                      |
| ---------- | --------------------------------- | ----------------------------- |
| Push       | `POST /v2/bot/message/push`       | Send to a specific user       |
| Reply      | `POST /v2/bot/message/reply`      | Reply to an event             |
| Multicast  | `POST /v2/bot/message/multicast`  | Send to multiple users        |
| Narrowcast | `POST /v2/bot/message/narrowcast` | Targeted push with conditions |
| Broadcast  | `POST /v2/bot/message/broadcast`  | Send to all subscribers       |

All share the same `contents` JSON shape.

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

1. Set container type to `"bubble"`.
2. Add a `body` block to contain the message contents.
3. Set the body block to a `box` component.
4. Set the box layout to `"horizontal"` to arrange children left-to-right.
5. Insert two `text` components, "Hello," and "World!".

## Bubble size

Bubble size is set at the container level with the `size` property.

| Size keyword | Dimensions (approx.) | Min LINE version                  |
| ------------ | -------------------- | --------------------------------- |
| `nano`       | 71px                 | all                               |
| `micro`      | 91px                 | all                               |
| `kilo`       | 143px                | all                               |
| `xxs`        | 240px                | all                               |
| `xs`         | 360px                | all                               |
| `sm`         | 480px                | all                               |
| `md`         | 540px                | all                               |
| `lg`         | 720px                | all                               |
| `xl`         | 840px                | all                               |
| `xxl`        | 960px                | all                               |
| `3xl`        | 1040px               | all                               |
| `4xl`        | 1200px               | all                               |
| `5xl`        | 1200px               | all                               |
| `full`       | full width           | all                               |
| `mega`       | —                    | all                               |
| `giga`       | —                    | all                               |
| `deca`       | —                    | iOS/Android ≥ 13.6.0, PC ≥ 7.17.0 |
| `hecto`      | —                    | iOS/Android ≥ 13.6.0, PC ≥ 7.17.0 |

If the LINE version is lower than required for `deca` or `hecto`, the bubble renders as `kilo`.

## Text direction

Set `direction: "rtl"` or `direction: "ltr"` at the bubble level to control text flow. Applies to the entire bubble.

```json
{
  "type": "bubble",
  "direction": "rtl",
  "body": {
    "type": "box",
    "layout": "vertical",
    "contents": [
      { "type": "text", "text": "שלום עולם" }
    ]
  }
}
```

The text direction (`LTR` or `RTL`) is always applied horizontally — it does not change the orientation of the box itself. For vertical boxes, RTL shifts the inline axis to the right.

## Flex Message limitations

The same Flex Message may render differently depending on:

- Device OS
- LINE version
- Device resolution
- Language settings
- Font availability

Always test via the [Flex Message Simulator](/flex-simulator/) and on a real device before sending to users.

## Versioned features

| Feature                                | iOS/Android | LINE for PC |
| -------------------------------------- | ----------- | ----------- |
| `maxWidth`, `maxHeight`, `lineSpacing` | ≥ 11.22.0   | ≥ 7.7.0     |
| Video component                        | ≥ 11.22.0   | ≥ 7.7.0     |
| `deca` / `hecto` bubble sizes          | ≥ 13.6.0    | ≥ 7.17.0    |
| `scaling` on button / text / icon      | ≥ 13.6.0    | ≥ 7.17.0    |

If LINE version is lower than required: video displays `altContent`; unsupported bubble sizes fall back to `kilo`.

## See also

- [Flex Message elements](./elements.md) — Container/Block/Component taxonomy
- [Flex Message layout](./layout.md) — Box orientation, sizing, positioning
- [Flex Message Simulator](./simulator.md) — Preview without sending

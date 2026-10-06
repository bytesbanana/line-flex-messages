# Flex Message Simulator

[Flex Message Simulator](https://developers.line.biz/flex-simulator/) is a browser-based tool for composing and previewing Flex Messages without writing code or setting up a development environment.

## UI layout

The Simulator has three panes:

| Pane                        | Purpose                                                  |
| --------------------------- | -------------------------------------------------------- |
| **Preview area** (left)     | Renders the live Flex Message as it would appear in LINE |
| **Tree view area** (center) | Hierarchical data structure — click to select nodes      |
| **Property area** (right)   | Edit properties of the selected node                     |

Hovering over a node in the tree view highlights the corresponding area in the preview.

## Getting started

1. Open [Flex Message Simulator](https://developers.line.biz/flex-simulator/).
2. If not logged in to the [LINE Developers Console](https://developers.line.biz/console/), log in with your LINE Developers account.
3. If you do not have an account, click **Create an account**.

## Showcase (predefined layouts)

Instead of building from scratch, click **Showcase** at the top to choose a predefined layout. Click **Create** to apply it to your canvas. After selection, you can customize any part of it.

## Building a Flex Message step by step

### 1. Select container type

Click **New** and select **bubble** (for a single card) or **carousel** (for multiple cards). A bubble is sufficient for most use cases.

### 2. Add blocks

Select a block node in the tree view (header, hero, body, footer) and click **+** to add a child component. Common components:

| Block  | Typical components                  |
| ------ | ----------------------------------- |
| Header | box + text                          |
| Hero   | image or video                      |
| Body   | box + text, separator, icon, button |
| Footer | box + button                        |

### 3. Style components

With a node selected in the tree view, edit its properties in the property area. Common properties:

| Property          | Component                 | Values                                     |
| ----------------- | ------------------------- | ------------------------------------------ |
| `layout`          | box                       | `"horizontal"`, `"vertical"`, `"baseline"` |
| `backgroundColor` | box, header, body, footer | Hex color e.g. `#00B900`                   |
| `text`            | text                      | Any string                                 |
| `color`           | text                      | Hex color e.g. `#FFFFFF`                   |
| `weight`          | text                      | `"regular"`, `"bold"`                      |
| `size`            | text, icon, image         | Keyword or pixel value                     |
| `align`           | text                      | `"start"`, `"center"`, `"end"`             |
| `wrap`            | text                      | `true` / `false`                           |
| `margin`          | separator, button, box    | Keyword e.g. `"md"`, `"xxl"`               |
| `paddingTop`      | box                       | Pixel value e.g. `"10px"`                  |
| `style`           | button                    | `"primary"`, `"secondary"`, `"link"`       |
| `url`             | image, icon               | HTTPS URL                                  |
| `action`          | button, video             | URI action or postback action              |

After editing any property, press **Enter** to apply the change to the preview.

### 4. Add actions

For buttons, expand the **Action** section in the property area. Default type is `uri`. Set `label` and `uri`. For postback actions, change `type` to `postback` and set `data`.

> **Percent-encode URI:** Domain names, paths, query parameters, and fragments in `uri` must be percent-encoded with UTF-8. Example:
>
> | Part | Value |
> |------|-------|
> | Scheme | `https` |
> | Domain | `example.com` |
> | Path | `/path` |
> | Query | `q=Good%20morning` |
> | Fragment | `Good%20afternoon` |
>
> Final URL: `https://example.com/path?q=Good%20morning#Good%20afternoon`

### 5. Button style guide

| Style       | Best for                                                  |
| ----------- | --------------------------------------------------------- |
| `primary`   | Dark background — standalone first action                 |
| `secondary` | Light background — less prominent secondary action        |
| `link`      | Text-only link — recommended for stacked vertical buttons |

When stacking multiple buttons vertically in the same box, use `link` style to avoid heavy visual weight.

## Image URL requirements

Flex Message Simulator does not support uploading image files. All image and icon URLs must:

| Requirement           | Value            |
| --------------------- | ---------------- |
| Protocol              | HTTPS (TLS 1.2+) |
| Format                | JPEG or PNG      |
| Max dimensions        | 1024 × 1024 px   |
| Max file size         | 10 MB            |
| Recommended file size | ≤ 1 MB           |

## View as JSON

Click **</>View as JSON** to see the full Flex Message JSON. Use the **Copy** button to copy it to the clipboard.

To import a Flex Message from JSON:
1. Click **</>View as JSON**
2. Clear or replace the content in the modal
3. Paste the JSON
4. Click **Apply** — the preview updates immediately

## Send a test message

To test a Flex Message on your own device, click **Send...** at the top right of the Simulator. This pushes the message to your own LINE app via your Messaging API channel. Useful for testing video playback and interactive elements.

## Tutorial shortcut

Instead of building step by step, paste a complete Flex Message JSON:
1. Click **</>View as JSON**
2. Remove the current content
3. Paste the complete JSON (e.g. from the [sample file](/media/code-samples/flex-message-simulator-example.json))
4. Click **Apply**

The preview renders the Flex Message instantly.

## Limitations in the Simulator

- **Video** is not playable — `altContent` is shown instead (same as older LINE versions). Send from the Simulator to your LINE app to test actual video playback.
- Font rendering may differ slightly from actual LINE rendering on different devices.

## See also

- [Flex Message send](/references/send.md) — Sending via Messaging API, bubble sizes, text direction
- [Flex Message elements](/references/elements.md) — Component taxonomy and JSON examples
- [Flex Message layout](/references/layout.md) — Box sizing, positioning, linear gradient
- [Flex Message video](/references/video.md) — Video requirements and playback behavior

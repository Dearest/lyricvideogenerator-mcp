# LyricVideoGenerator MCP server

A hosted remote MCP server for [LyricVideoGenerator](https://lyricvideogenerator.app). Give it a song and its lyrics and it builds a lyric video where each word is timed to the vocal, then proposes three styles to choose from. You can change the look or fix a lyric line by asking in plain English.

This repository holds documentation and the registry manifest only. The server runs on lyricvideogenerator.app; there is nothing to install or self-host.

- Endpoint: `https://lyricvideogenerator.app/api/mcp`
- Transport: streamable HTTP
- Registry name: `app.lyricvideogenerator/lyric-video` ([official MCP Registry](https://registry.modelcontextprotocol.io/v0/servers?search=lyricvideogenerator))
- Auth: none needed to create and edit videos. HD export uses OAuth to link your LyricVideoGenerator account.

## Connect

**ChatGPT.** Works best here: a widget previews the three styles in sync with the song and runs the free export. Add it as a custom connector with the endpoint above, then type `@LyricVideoGenerator` and attach an MP3 or WAV.

**Claude** (Settings → Connectors → Add custom connector) and other clients that take a remote URL:

```
https://lyricvideogenerator.app/api/mcp
```

**Cursor** and other clients configured with `mcp.json`:

```json
{
  "mcpServers": {
    "lyricvideogenerator": {
      "url": "https://lyricvideogenerator.app/api/mcp"
    }
  }
}
```

Outside ChatGPT there is no widget, so the free in-browser export is not available. You can still create a project, pick a style, restyle it and fix lyrics; HD export works once your account is linked.

## Tools

| Tool | What it does |
|---|---|
| `create_lyric_video` | Takes a song (an MP3/WAV URL), optional lyrics and title. Aligns every word to the vocal and proposes three styles. Lyrics are transcribed when you don't pass them. Takes about 10–40 seconds. |
| `get_lyric_video` | Processing status, the three style options, the applied style and the numbered lyric lines. |
| `choose_style` | Applies option 1, 2 or 3. |
| `update_style` | Changes layout, word animation, colors, font, text size, background or chorus emphasis. Unreadable color combinations and unknown fonts are corrected or rejected with a reason. |
| `edit_lyrics` | Replaces the text of one line and keeps its timing. |
| `export_video` | Free export: 720p, 16:9, small watermark and a 2.5-second end card. Renders in the ChatGPT widget. |
| `export_hd_video` | 1080p without a watermark, 16:9 or 9:16. Needs a linked account and uses one credit (refunded if the render fails). |
| `get_export` | Export status and the MP4 download link when it's done. |

A few more tools (`begin_local_export`, `upload_local_export`, `finish_local_export`, `open_in_browser`) are called by the ChatGPT widget itself, not by the model.

## Example

> @LyricVideoGenerator make a lyric video for this song (attached), the lyrics are below. Then make the chorus bigger and use warmer colors.

## Plans

The free tier exports 720p 16:9 with no account. 1080p and vertical 9:16 need a paid plan or a one-off unlock; see [pricing](https://lyricvideogenerator.app/pricing).

## Support

Questions and bug reports: open an issue here, or use the contact details on [lyricvideogenerator.app](https://lyricvideogenerator.app).

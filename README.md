# Canva MCP for OpenClaw Marketing

**Describe your campaign in plain English. Get production-ready Canva designs without touching a browser.**

This is an OpenClaw skill that gives any AI agent deep, practical instructions for using [Canva's official MCP server](https://www.canva.dev/docs/mcp/) to create, export, resize, and organize marketing assets — directly from a chat interface.

---

## The Moment

The [Figma Context MCP](https://github.com/GLips/Figma-Context-MCP) showed developers they could implement designs in one shot just by pasting a Figma link. That changed how engineers think about design handoff.

This is the same unlock — but for marketing.

Instead of:
1. Open Canva
2. Search for a template
3. Customize text and colors
4. Export, download, re-upload

You just say:

> *"Create a LinkedIn post announcing our Series A. Navy and gold, professional tone, headline 'We just raised $8M', brief summary of what we build, logo placeholder top-right."*

And Claude generates it in Canva — ready to share, edit, or export — in seconds.

---

## What's Possible

- **Generate any design** from a text prompt (social posts, pitch decks, logos, banners, thumbnails, flyers, email headers, infographics)
- **Export in any format** — PNG, JPG, PDF, PPTX, GIF, MP4
- **Resize across all platforms** in one go — Instagram, LinkedIn, Twitter, TikTok, YouTube (Canva Pro)
- **Import existing files** — turn a PDF or PowerPoint into an editable Canva design
- **Organize campaign assets** into folders automatically
- **Add review comments** directly to designs for async feedback loops

---

## Setup

### 1. Add the Canva MCP

**Claude Code** — add to `~/.claude/settings.json`:
```json
{
  "mcpServers": {
    "canva": {
      "type": "http",
      "url": "https://mcp.canva.com/mcp"
    }
  }
}
```

Or via CLI:
```bash
claude mcp add --transport http canva https://mcp.canva.com/mcp
```

**Claude Desktop / Cursor** — add to your config file:
```json
{
  "mcpServers": {
    "canva": {
      "type": "streamable-http",
      "url": "https://mcp.canva.com/mcp"
    }
  }
}
```

### 2. Add This Skill

Clone this repo or reference it in your project memory so Claude has access to the full instruction set:

```bash
git clone https://github.com/acwelst/canva-for-openclaw-marketing-
```

Or add as a submodule:
```bash
git submodule add https://github.com/acwelst/canva-for-openclaw-marketing- .canva-skill
```

### 3. Authenticate

On first use, Claude will prompt you to connect your Canva account. One click — then you're live.

---

## What to Say

```
"Create an Instagram post for our product launch — electric blue and white, bold sans-serif,
 headline 'Introducing v2.0', subheadline 'Shipped faster. Built smarter.'"

"Build a 10-slide pitch deck for our Series A. Dark navy theme, professional.
 Slides: cover, problem, solution, market size, traction, team, financials, ask."

"Make social media assets for our summer sale in every format — Instagram, LinkedIn, Twitter."

"Import this deck.pptx and turn it into an editable Canva design."

"Organize all our Q2 campaign designs into a folder called 'Q2 2025 — Summer Launch'."

"Export our homepage banner as PNG and as a PDF for print."
```

---

## File Structure

| File | Purpose |
|------|---------|
| `CLAUDE.md` | Full instruction set for Claude Code — deep workflows, prompting guide, intent recognition |
| `AGENT.md` | Generic instruction set for any AI agent (GPT, Gemini, Cursor, custom pipelines) |
| `skill.json` | Machine-readable manifest for agent routing — trigger keywords, capabilities, workflow index |
| `README.md` | This file |

### For Agent Developers

`skill.json` is designed to be consumed by always-on routing agents to identify when Canva is the right tool. It includes:
- `triggerKeywords` — word-level intent signals
- `intentPatterns` — regex patterns for natural language detection
- `capabilities` — tool index with plan requirements
- `workflows` — named workflows with their tool chains

---

## Plan Requirements

| Feature | Free | Pro | Teams |
|---------|:----:|:---:|:-----:|
| Generate & export designs | Yes | Yes | Yes |
| Import PDFs / PowerPoints | Yes | Yes | Yes |
| Folder organization | Yes | Yes | Yes |
| Resize to new dimensions | — | Yes | Yes |
| Brand Kit integration | — | Yes | Yes |
| Template autofill (bulk) | — | — | Yes |

---

## Canva MCP Tools Reference

| Tool | Description |
|------|-------------|
| `generate-design` | Create a design from a prompt |
| `create-design` | Create a blank canvas at custom dimensions |
| `search-designs` | Search the user's Canva library |
| `list-designs` | List designs filtered by type |
| `get-design` | Get metadata and share link for a design |
| `get-design-pages` | List all pages in a multi-page design |
| `export-design` | Export as PDF, PNG, JPG, GIF, PPTX, MP4 |
| `resize-design` | Resize to any platform preset *(Pro)* |
| `import-design-from-url` | Import external files as editable designs |
| `create-folder` | Create a project folder |
| `get-folder` | Get folder details |
| `list-folder-items` | List items in a folder |
| `move-item-to-folder` | Move designs into folders |
| `add-design-comment` | Add review comments to a design |

Full docs: [canva.dev/docs/mcp/tools](https://www.canva.dev/docs/mcp/tools/)

---

## Contributing

Found a workflow that belongs here? A use case that Claude should detect automatically? A better design prompt formula?

PRs are welcome. Add to `CLAUDE.md` or `AGENT.md` with:
- The trigger phrase(s) that should activate this workflow
- The tools used in sequence
- An example prompt that produces great results

---

## Related

- [Canva MCP Documentation](https://www.canva.dev/docs/mcp/) — official tool reference
- [Canva Connect API](https://www.canva.dev/docs/connect/) — for custom integrations
- [Figma Context MCP](https://github.com/GLips/Figma-Context-MCP) — the design-to-code equivalent

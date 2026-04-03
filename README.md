# Canva for OpenClaw Marketing

**An OpenClaw skill that gives Claude deep, practical instructions for using the Canva MCP to create marketing assets.**

When you ask OpenClaw about marketing — campaigns, social media, presentations, branding, email graphics — this skill tells Claude exactly how to use Canva's official MCP server to get it done without leaving your workflow.

---

## What This Does

This repo provides a `CLAUDE.md` instruction set that Claude reads to understand:

- How to connect and authenticate with the Canva MCP
- Every available Canva MCP tool and what it does
- Step-by-step workflows for common marketing design tasks
- How to write effective design prompts that produce great results
- Export format selection, brand consistency, and error handling
- When to reach for Canva automatically (intent recognition)

Think of it as a design operations guide baked directly into your Claude session.

---

## Quick Start

### 1. Add the Canva MCP to Claude Code

Add this to `~/.claude/settings.json`:

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

Or run in your terminal:
```bash
claude mcp add --transport http canva https://mcp.canva.com/mcp
```

### 2. Add This Skill to Your Project

Reference this repo's `CLAUDE.md` from your project's memory, or clone it locally:

```bash
# Clone alongside your project
git clone https://github.com/acwelst/canva-for-openclaw-marketing-

# Or add as a git submodule
git submodule add https://github.com/acwelst/canva-for-openclaw-marketing- .canva-skills
```

Then tell Claude to read it at session start, or include the path in your `CLAUDE.md` imports.

### 3. Authenticate

On first use, Claude will prompt you to connect your Canva account. Click the link, authorize, and you're done.

---

## Example Prompts

Once set up, you can say things like:

```
"Create an Instagram post for our product launch — blue and white, modern style, headline 'Introducing v2.0'"

"Build a 10-slide pitch deck for our Series A fundraise"

"Make social media assets in all formats for our summer sale campaign"

"Resize our homepage banner for LinkedIn, Twitter, and Instagram"

"Import this PDF and turn it into an editable Canva design"

"Organize all our Q2 campaign designs into a folder"
```

---

## What's Inside

| File | Purpose |
|------|---------|
| `CLAUDE.md` | Full instruction set — Claude reads this to use Canva MCP |
| `README.md` | This file — setup guide for humans |

---

## Canva MCP Capabilities at a Glance

The official Canva MCP server (`https://mcp.canva.com/mcp`) gives Claude access to:

- **Generate designs** from natural language prompts
- **Search and retrieve** designs from your Canva library
- **Export** as PDF, PNG, JPG, GIF, PPTX, or MP4
- **Resize** designs to any platform format (Pro plan)
- **Import** PDFs, PowerPoints, and Google Docs as editable designs
- **Organize** with folders and asset management
- **Comment** on designs for review workflows

---

## Plan Requirements

| Feature | Free | Pro | Teams |
|---------|:----:|:---:|:-----:|
| Generate & export designs | Yes | Yes | Yes |
| Resize to new dimensions | — | Yes | Yes |
| Brand Kit integration | — | Yes | Yes |
| Template autofill (bulk) | — | — | Yes |

---

## Related Resources

- [Canva MCP Documentation](https://www.canva.dev/docs/mcp/)
- [Canva Connect API Reference](https://www.canva.dev/docs/connect/)
- [Canva Help: MCP Setup](https://www.canva.com/help/mcp-agent-setup/)
- [Figma Context MCP](https://github.com/GLips/Figma-Context-MCP) — Inspiration for this repo's structure

---

## Contributing

Found a workflow that works great? Missing a use case? PRs welcome — add to the `CLAUDE.md` with a clear description of the trigger, the tools used, and the expected output.

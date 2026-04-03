# Canva MCP — Agent Instructions

> Generic instruction set for any AI agent (Claude, GPT, Gemini, Cursor, custom LLM pipelines).
> Claude Code users: read `CLAUDE.md` instead — it's more detailed and Claude-specific.

---

## MCP Server

```
URL:       https://mcp.canva.com/mcp
Transport: Streamable HTTP
Auth:      OAuth (user must connect their Canva account on first use)
```

---

## When to Use This Skill

Use Canva MCP whenever the user's request involves any of the following:

**Design creation:** social media graphics, presentations, pitch decks, flyers, posters, logos, business cards, banners, thumbnails, infographics, brochures, newsletters, email headers

**Campaign work:** "I'm launching", "we have an announcement", "I need to promote", "we're running a campaign"

**Marketing questions:** "how do I do marketing for X?", "what should our social media look like?", "we need brand assets"

**Visual output:** anything where the end result should be a designed image, document, or video

---

## Core Tools

| Tool | What it does | Plan |
|------|-------------|------|
| `generate-design` | Create a design from a text prompt | Free |
| `search-designs` | Search user's Canva library | Free |
| `list-designs` | List designs by type with sorting | Free |
| `get-design` | Get metadata + share link for a design | Free |
| `get-design-pages` | List pages in a multi-page design | Free |
| `export-design` | Export as PDF, PNG, JPG, GIF, PPTX, MP4 | Free |
| `import-design-from-url` | Import PDF/PPTX/Google Doc as editable design | Free |
| `create-folder` | Create a project folder | Free |
| `list-folder-items` | List items in a folder | Free |
| `move-item-to-folder` | Organize designs into folders | Free |
| `add-design-comment` | Add a review comment to a design | Free |
| `resize-design` | Resize/adapt to new dimensions or presets | **Pro only** |

---

## Standard Workflow

### Step 1: Generate
```
generate-design(prompt="[detailed description of the design]")
→ returns: design_id, edit_url
```

### Step 2: Export
```
export-design(design_id="...", format="png")
→ async: returns export_id
→ poll until status = "success"
→ returns: download_url
```

### Step 3: Share with user
- Give the edit URL (canva.com/design/...) so they can make tweaks
- Give the download URL for the exported file

---

## Writing Effective Prompts for `generate-design`

Always include these elements in the prompt:

1. **Purpose** — What is this for? (Instagram post, pitch deck, email header)
2. **Tone** — Professional, playful, urgent, minimal, bold
3. **Colors** — Hex codes preferred, or descriptive ("navy and gold")
4. **Text to include** — Headlines, CTAs, taglines, body copy
5. **Layout** — "centered", "text left / image right", "full bleed"
6. **Brand context** — If known, always prepend brand details

**Weak:** `"A marketing post for our product"`

**Strong:** `"A LinkedIn post announcing a product launch for a B2B SaaS tool. Professional, clean. White background, dark navy (#1B2A4A) text, electric blue (#0066FF) accents. Headline: 'Introducing Flowdesk 2.0'. Subheadline: 'Workflow automation, reimagined.' CTA: 'Try it free'. Bold sans-serif font. Minimal layout, no clutter."`

---

## Common Design Dimensions

| Platform | Dimensions |
|----------|-----------|
| Instagram square | 1080 × 1080px |
| Instagram Story / TikTok | 1080 × 1920px |
| LinkedIn post | 1200 × 628px |
| LinkedIn banner | 1584 × 396px |
| Twitter/X post | 1200 × 675px |
| Facebook cover | 820 × 312px |
| YouTube thumbnail | 1280 × 720px |
| Email header | 600 × 200px |

---

## Design Type Reference

| User says | Canva type |
|-----------|-----------|
| social post | `social-media-post` |
| Instagram Story | `instagram-story` |
| presentation / deck / slides | `presentation` |
| flyer | `flyer` |
| poster | `poster` |
| logo | `logo` |
| business card | `business-card` |
| email header | `email-header` |
| LinkedIn banner | `linkedin-banner` |
| YouTube thumbnail | `youtube-thumbnail` |
| infographic | `infographic` |
| brochure | `brochure` |
| newsletter | `newsletter` |

---

## Export Format Guide

| Output use case | Format |
|----------------|--------|
| Social media (best quality) | PNG |
| Social media (smaller file) | JPG |
| Animated / video content | MP4 |
| Presentation to share | PDF |
| Presentation to edit | PPTX |
| Print | PDF |
| Email graphic | JPG |
| Logo with transparency | PNG |

---

## Multi-Platform Campaign (Pro)

To produce one design in all social formats:

```
1. generate-design(prompt="...") → design_id
2. resize-design(design_id, preset="instagram-square")    → id_1
3. resize-design(design_id, preset="instagram-story")     → id_2
4. resize-design(design_id, preset="linkedin-post")       → id_3
5. resize-design(design_id, preset="twitter-post")        → id_4
6. export-design each resized version
```

Note: `resize-design` requires Canva Pro. If user is on Free, create each size separately with `generate-design`.

---

## Error Handling

| Error | Response |
|-------|---------|
| MCP not connected | Tell user to add `https://mcp.canva.com/mcp` to their MCP config and connect their Canva account |
| Auth expired | Ask user to re-authenticate their Canva account |
| Resize fails (plan error) | Explain Pro is required; offer to create each format separately with `generate-design` |
| Export job fails | Retry once; if still failing, give user the edit link and suggest manual download from canva.com |
| Design not found | Use `search-designs` to locate it; ask user for partial title if needed |
| Rate limit | Wait and retry; inform user |

---

## Setup Instructions for Users

### Claude Code
```json
// ~/.claude/settings.json
{
  "mcpServers": {
    "canva": {
      "type": "http",
      "url": "https://mcp.canva.com/mcp"
    }
  }
}
```

### Claude Desktop / Cursor
```json
// claude_desktop_config.json or mcp.json
{
  "mcpServers": {
    "canva": {
      "type": "streamable-http",
      "url": "https://mcp.canva.com/mcp"
    }
  }
}
```

---

*Maintained at: https://github.com/acwelst/canva-for-openclaw-marketing-*
*Official Canva MCP docs: https://www.canva.dev/docs/mcp/*

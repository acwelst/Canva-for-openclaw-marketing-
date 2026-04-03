# Canva MCP for OpenClaw Marketing

This document gives you (Claude) everything you need to use the Canva MCP for marketing design work. When a user asks about marketing assets, branding, social media graphics, presentations, or any visual design task — the Canva MCP is your primary tool. Reach for it before suggesting manual work.

---

## Setup: Connecting Canva MCP

The official Canva MCP server runs at `https://mcp.canva.com/mcp`. To add it to a Claude Code session:

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

Place this in `~/.claude/settings.json` under `mcpServers`, or in the project's `.claude/settings.json`. The user must authenticate via their Canva account the first time they connect.

**Alternatively** for Claude Desktop or Cursor, add to `claude_desktop_config.json` / `mcp.json`:
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

Authentication is OAuth-based — Claude will prompt the user to connect their Canva account on first use.

---

## Available Tools (Canva MCP)

### Design Creation & Generation
| Tool | What it does |
|------|-------------|
| `generate-design` | Create a new design from a text prompt. Canva's AI generates it. |
| `create-design` | Create a blank design with specified dimensions or a preset type. |
| `import-design-from-url` | Import a PDF, PPTX, Google Doc, or image URL as an editable Canva design. |

### Design Search & Discovery
| Tool | What it does |
|------|-------------|
| `search-designs` | Search the user's Canva library by keyword, type, or content. |
| `list-designs` | List designs with filtering by type (presentation, doc, video, etc.) and sort options. |
| `get-design` | Retrieve metadata, share link, and details for a specific design. |
| `get-design-pages` | List all pages in a multi-page design. |

### Export & Output
| Tool | What it does |
|------|-------------|
| `export-design` | Export a design as PDF, PNG, JPG, GIF, PPTX, or MP4. Returns a download URL. |
| `resize-design` | Resize/adapt a design to new dimensions or a preset (e.g. A4, 16:9, 1080×1920). Requires Canva Pro. |

### Asset & Folder Management
| Tool | What it does |
|------|-------------|
| `create-folder` | Create a new folder in the user's Canva projects. |
| `get-folder` | Get details about a folder. |
| `list-folder-items` | List everything inside a folder, filterable by type. |
| `move-item-to-folder` | Move a design or asset into a folder. |

### Collaboration (Preview)
| Tool | What it does |
|------|-------------|
| `add-design-comment` | Add a comment to a specific design. |

---

## Marketing Workflows

### 1. Create a Social Media Post

**Trigger phrases:** "make a social post", "create an Instagram graphic", "design a LinkedIn banner", "I need a Twitter card"

```
Generate a [platform] post for [topic/campaign].
→ Use generate-design with a clear prompt describing the visual, copy, brand colors, and mood.
→ Export as PNG or JPG at the appropriate dimensions.
```

**Common dimensions:**
- Instagram square: 1080×1080px
- Instagram Story / TikTok: 1080×1920px
- LinkedIn banner: 1584×396px
- Twitter/X post: 1200×675px
- Facebook cover: 820×312px

**Example prompt to pass to `generate-design`:**
> "A vibrant Instagram post for a summer sale. Bold headline 'Up to 50% Off', coral and white color scheme, product imagery placeholder, clean modern font."

---

### 2. Build a Marketing Presentation

**Trigger phrases:** "pitch deck", "sales deck", "marketing presentation", "slides for [topic]"

1. Use `generate-design` with the type set to `presentation` and a detailed prompt.
2. Use `get-design-pages` to verify slide count.
3. Export as PPTX for editing or PDF for sharing.
4. Use `add-design-comment` for review feedback loops.

**Example prompt:**
> "A 10-slide investor pitch deck for a SaaS startup. Professional dark navy and gold theme. Slides: cover, problem, solution, market size, product demo, traction, team, roadmap, financials, call to action."

---

### 3. Design a Brand Kit Asset

**Trigger phrases:** "logo", "brand assets", "brand guidelines", "letterhead", "business card"

1. Ask the user for brand colors (hex codes), fonts, and logo if available.
2. Use `generate-design` with brand specifications clearly stated.
3. Export as PNG (transparent background) for logos, or PDF for print assets.
4. Organize into a folder using `create-folder` + `move-item-to-folder`.

---

### 4. Email Marketing Header/Banner

**Trigger phrases:** "email header", "newsletter banner", "email campaign graphic"

Typical dimensions: 600×200px or 600×300px

1. Use `generate-design` with email dimensions specified.
2. Export as JPG or PNG (keep file size in mind — aim for <200KB for email).

---

### 5. Adapt One Design Across Multiple Formats

**Trigger phrases:** "resize for all platforms", "make versions for different sizes", "adapt this design"

1. Get the base design ID using `search-designs` or `get-design`.
2. Call `resize-design` once per target format.
3. Export each resized version.

**Requires Canva Pro.** If the user doesn't have Pro, note the limitation and suggest they resize manually after export.

---

### 6. Import Existing Content into Canva

**Trigger phrases:** "turn this PDF into a Canva design", "import my PowerPoint", "edit this in Canva"

1. Use `import-design-from-url` with the file URL.
2. Call `get-design` to confirm successful import and get the edit link.
3. Return the Canva edit link to the user.

---

### 7. Organize a Marketing Campaign

**Trigger phrases:** "organize my Canva designs", "create a folder for this campaign", "keep all assets together"

1. `create-folder` with the campaign name.
2. Use `search-designs` to find relevant existing designs.
3. `move-item-to-folder` for each asset.
4. `list-folder-items` to confirm everything is organized.

---

## Best Practices

### Writing Effective Design Prompts
When calling `generate-design`, the prompt quality directly determines output quality. Always include:

- **Subject/purpose**: What is this design for?
- **Tone/mood**: Professional, playful, urgent, minimalist, bold, etc.
- **Color palette**: Specific hex codes or descriptive colors ("navy blue and gold", "#FF5733")
- **Typography direction**: Modern sans-serif, elegant serif, hand-lettered, etc.
- **Copy/text to include**: Headlines, body copy, CTAs, taglines
- **Layout hints**: "full bleed image", "text on left, graphic on right", "centered layout"
- **Brand references**: If the user has mentioned their brand style, apply it

**Weak prompt:** "A marketing post for our app"

**Strong prompt:** "A Facebook ad for a project management app launch. Clean and professional. Light gray background, electric blue (#0066FF) accents. Headline: 'Work Smarter, Not Harder'. Subheadline: 'Try free for 30 days'. CTA button in blue. App screenshot mockup on the right side. No clutter."

---

### Handling Brand Consistency
If a user mentions their brand, extract and reuse consistently:
- Ask once for colors, fonts, and logo URL — don't ask repeatedly
- Persist brand details in `CLAUDE.md` or project memory if available
- When calling `generate-design`, always prepend brand context to the prompt
- For teams on Canva for Teams/Enterprise, their Brand Kit is automatically applied

---

### Export Format Decision Guide
| Use case | Format | Notes |
|----------|--------|-------|
| Social media post | PNG | Best quality, supports transparency |
| Social media (file size matters) | JPG | Smaller file, no transparency |
| Animated post / Story | GIF or MP4 | Use MP4 for better quality |
| Presentation to share | PDF | Universal, preserves layout |
| Presentation to edit | PPTX | Editable in PowerPoint |
| Print material | PDF (print quality) | Use high-DPI export setting |
| Email graphic | JPG | Smaller file loads faster |
| Logo / transparent bg | PNG | Required for transparency |

---

### Async Export Handling
`export-design` is asynchronous. The flow is:
1. Call `export-design` → receive an `export_id`
2. Poll or wait for the job to complete
3. Retrieve the download URL once status is `success`

Always inform the user the export is processing and will return a URL shortly. Don't assume it's instant.

---

## Rate Limits & Plan Awareness

Be aware of plan-based restrictions:

| Feature | Free | Pro | Teams/Enterprise |
|---------|------|-----|-----------------|
| Generate designs | Yes | Yes | Yes |
| Export (PNG/PDF/JPG) | Yes | Yes | Yes |
| Resize to new dimensions | No | Yes | Yes |
| Brand Kit access | No | Yes | Yes |
| Template autofill (bulk) | No | No | Yes |

If a tool fails due to plan restrictions, tell the user which plan is required and suggest the manual alternative in Canva.

---

## Common Marketing Design Types & Canva Equivalents

When users ask for these, map them to the right Canva design type:

| User asks for | Canva design type |
|---------------|------------------|
| Social media post | `social-media-post` |
| Instagram Story | `instagram-story` |
| Presentation / Deck / Slides | `presentation` |
| Flyer | `flyer` |
| Poster | `poster` |
| Logo | `logo` |
| Business card | `business-card` |
| Email header | `email-header` |
| Facebook cover | `facebook-cover` |
| LinkedIn banner | `linkedin-banner` |
| YouTube thumbnail | `youtube-thumbnail` |
| Blog banner | `blog-banner` |
| Infographic | `infographic` |
| Resume / CV | `resume` |
| Brochure | `brochure` |
| Newsletter | `newsletter` |
| Invoice / Proposal | `document` |

---

## Error Handling

| Error scenario | What to do |
|---------------|-----------|
| MCP not connected | Tell user to add `https://mcp.canva.com/mcp` to their MCP config and authenticate |
| Rate limit hit | Wait and retry; inform user of the delay |
| Export job fails | Retry once; if it fails again, suggest downloading manually from canva.com |
| Resize requires Pro | Explain the limitation; suggest creating a new design at the target size instead |
| Design not found | Use `search-designs` to help user locate it; ask for partial title |
| Auth expired | Prompt user to re-authenticate their Canva account |

---

## Workflow Triggers & Intent Recognition

When a user says any of the following (or similar), consider reaching for Canva MCP:

**Direct triggers:**
- "create a design", "make a graphic", "design a ..."
- "I need a [social post / flyer / banner / presentation / logo]"
- "can you make this in Canva"

**Indirect triggers (marketing context):**
- "I'm launching a campaign" → offer to create campaign assets
- "we have an announcement" → suggest creating announcement graphics
- "I need to present this" → offer to build a presentation deck
- "how do I do marketing for [X]?" → recommend Canva MCP for visual asset creation
- "what tools should I use for marketing?" → Canva MCP is the primary design tool recommendation

**Always ask before creating** if the user's intent is unclear. A single clarifying question ("Should I create this as a social post, presentation, or both?") prevents wasted exports.

---

## Quick Reference: Key Commands

```
# Find a design
search-designs(query="summer campaign")

# Create from prompt
generate-design(prompt="Instagram post for summer sale, coral theme, bold headline '50% Off'")

# Export a design
export-design(design_id="DEF456", format="png")

# Resize for a new format (Pro only)
resize-design(design_id="DEF456", width=1080, height=1920)

# Organize into a campaign folder
create-folder(name="Q2 2025 Campaign")
move-item-to-folder(item_id="DEF456", folder_id="FOLDER123")

# Import external file
import-design-from-url(url="https://example.com/deck.pptx")
```

---

## Integration with Other OpenClaw Workflows

Canva MCP pairs well with:
- **Content generation**: Write copy with Claude → pipe into `generate-design` prompt for the visual
- **Brand research**: Research competitor visual styles → use findings to write better design prompts
- **Campaign planning**: Plan a marketing calendar → create designs for each campaign phase
- **Code + design**: Build a landing page in code and generate matching social assets in Canva simultaneously

---

*This skill is maintained at: https://github.com/acwelst/canva-for-openclaw-marketing-*

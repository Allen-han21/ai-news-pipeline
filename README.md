# AI News Pipeline

A [Claude Code](https://claude.ai/code) plugin that provides a 3-stage pipeline for staying updated with AI technology trends and turning insights into actionable roadmaps.

```
tech-news (Collect) --> news-dive (Analyze) --> news-apply (Apply)
   7 sources              Multi-source            Ultrathink
   Briefing               Deep Dive               Roadmap + Sentinel
```

## Installation

```
/plugin marketplace add Allen-han21/ai-news-pipeline
```

## Skills

### `/tech-news` - AI News Briefing

Collects AI news from **7 sources** across 3 tiers in parallel:

| Tier | Sources | Method |
|------|---------|--------|
| Tier 1 | GeekNews, Hacker News | WebFetch |
| Tier 2 | Anthropic, OpenAI, Google AI | WebSearch |
| Tier 3 | Reddit, Hugging Face | WebFetch |

Outputs a categorized briefing: Major Announcements, Tech & Research, Community & Trends.

```
/tech-news              # Terminal output only
/tech-news --save       # Terminal + save to Notion (requires setup)
```

### `/news-dive` - Deep Dive Analysis

Takes a news item from the briefing (or any URL/topic) and performs multi-source deep analysis:

- Original source + community reactions (HN, GeekNews)
- Technical details, benchmarks, implementation guides
- Practical application benefits and methods

```
/news-dive 1-2                        # Briefing item reference
/news-dive https://example.com/post   # Direct URL
/news-dive "Claude Agent Teams"       # Keyword search
/news-dive 1-1 --save                 # Save to file
```

### `/news-apply` - Application Roadmap

Creates an actionable roadmap using **ultrathink** (extended thinking) with the **What/How/Well** framework:

- **What**: Priority features to apply (Impact x Effort matrix)
- **How**: 3-phase plan (Quick Win → Integration → Mastery)
- **Well**: Success metrics, anti-patterns, failure signals

Includes **Roadmap Sentinel** for long-term progress tracking across sessions.

```
/news-apply                     # From current session's news-dive
/news-apply --file {name}       # From saved analysis
/news-apply --save              # Save roadmap + sentinel
/news-apply --roadmap {id}      # Restore & check progress
/news-apply --list              # List all roadmaps
/news-apply --update {id} --complete M1.2   # Complete milestone
```

## Optional: Notion Integration

The `tech-news` skill can optionally save briefings to a Notion page. To set this up:

### 1. Create a Notion Integration

1. Go to [Notion Integrations](https://www.notion.so/my-integrations)
2. Create a new integration
3. Copy the API token

### 2. Set the API Token

Set the `NOTION_API_TOKEN` environment variable, or configure your preferred secret manager.

```bash
# Option A: Environment variable
export NOTION_API_TOKEN="ntn_xxxxxxxxxxxxx"

# Option B: 1Password CLI (recommended)
# Update the op:// path in the skill to match your vault
```

### 3. Connect the Page

1. Create a Notion page for storing news briefings
2. Connect your integration to the page (Page → ⋯ → Connections → Add)
3. Copy the page ID from the URL
4. Update `{YOUR_NOTION_PAGE_ID}` in the tech-news skill

## File Storage

| Skill | Save Location | Content |
|-------|--------------|---------|
| news-dive | `~/.claude/news-archive/` | Deep dive analysis (markdown) |
| news-apply | `~/.claude/news-roadmap/` | Roadmap documents (markdown) |
| news-apply | `~/.claude/news-roadmap/sentinel/` | Progress tracking (JSON) |

## Example Workflow

```
# Morning: Check what's new
/tech-news

# Interesting item found? Dive deeper
/news-dive 1-2

# Want to apply it? Create a roadmap
/news-apply --save

# Next week: Check progress
/news-apply --roadmap roadmap-2026-02-07-agent-teams

# Complete a milestone
/news-apply --update roadmap-2026-02-07-agent-teams --complete M1.2
```

## Language

Skills output in **Korean** by default (technical terms kept in English). The pipeline is designed for Korean-speaking developers but the structure is language-agnostic.

## License

MIT

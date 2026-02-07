# AI News Pipeline Plugin

3-stage AI news pipeline for Claude Code.

## Skills

| Skill | Description |
|-------|-------------|
| `tech-news` | AI news briefing from 7 sources (GeekNews, HN, Anthropic, OpenAI, Google, Reddit, HuggingFace) |
| `news-dive` | Deep dive analysis of specific news items with multi-source research |
| `news-apply` | Application roadmap with ultrathink What/How/Well framework and sentinel tracking |

## Pipeline

```
/tech-news  -->  /news-dive {item}  -->  /news-apply
 Collect          Analyze               Apply
```

See the [main README](../../README.md) for full documentation.

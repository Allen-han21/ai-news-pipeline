# AI News Pipeline Plugin

Claude Code용 3-stage AI 뉴스 파이프라인

## Skills

| Skill | Description |
|-------|-------------|
| `tech-news` | 7개 소스 AI 뉴스 브리핑 (GeekNews, HN, Anthropic, OpenAI, Google, Reddit, HuggingFace) |
| `news-dive` | 특정 뉴스 항목 다중 소스 심층 분석 |
| `news-apply` | ultrathink What/How/Well 적용 로드맵 + Sentinel 추적 |

## Pipeline

```
/tech-news  -->  /news-dive {item}  -->  /news-apply
 Collect          Analyze               Apply
```

상세 문서: [README](../../README.md)

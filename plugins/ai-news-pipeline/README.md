# AI News Pipeline 플러그인

Claude Code용 3단계 AI 뉴스 파이프라인입니다.

## 스킬

| 스킬 | 설명 |
|------|------|
| `tech-news` | 7개 소스에서 AI 뉴스 브리핑 (GeekNews, HN, Anthropic, OpenAI, Google, Reddit, HuggingFace) |
| `news-dive` | 특정 뉴스 항목의 다중 소스 심층 분석 |
| `news-apply` | ultrathink What/How/Well 프레임워크 기반 적용 로드맵 + sentinel 추적 |

## 파이프라인

```
/tech-news  -->  /news-dive {항목}  -->  /news-apply
  수집              분석                   적용
```

전체 문서는 [메인 README](../../README.md)를 참조하세요.

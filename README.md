# AI News Pipeline

AI 기술 뉴스 수집 → 심층 분석 → 적용 로드맵 3단계 파이프라인 [Claude Code](https://claude.ai/code) Plugin

```
/tech-news (Collect) --> /news-dive (Analyze) --> /news-apply (Apply)
   7 sources              Multi-source            Ultrathink
   Briefing               Deep Dive               Roadmap + Sentinel
```

## Installation

```
/plugin marketplace add Allen-han21/ai-news-pipeline
```

## Skills

### `/tech-news` - AI News Briefing

7개 소스에서 AI 뉴스를 병렬 수집, 카테고리별 브리핑

| Tier | Sources | Method |
|------|---------|--------|
| Tier 1 (큐레이션) | GeekNews, Hacker News | WebFetch |
| Tier 2 (공식) | Anthropic, OpenAI, Google AI | WebSearch |
| Tier 3 (커뮤니티) | Reddit, Hugging Face | WebFetch |

카테고리: 주요 발표 / 기술 & 연구 / 커뮤니티 & 트렌드

```
/tech-news              # 터미널 출력만
/tech-news --save       # + Notion 저장 (설정 필요)
```

### `/news-dive` - Deep Dive Analysis

브리핑 항목(또는 URL/주제)을 다중 소스에서 심층 분석

- 원본 소스 + 커뮤니티 반응 (HN, GeekNews)
- 기술 상세, 벤치마크, 구현 가이드
- 실무 적용 이점과 방법

```
/news-dive 1-2                        # 브리핑 항목 번호 (주요 발표 2번째)
/news-dive https://example.com/post   # URL 직접 분석
/news-dive "Claude Agent Teams"       # 키워드 검색
/news-dive 1-1 --save                 # 파일 저장
```

### `/news-apply` - Application Roadmap

**ultrathink** 기반 **What/How/Well** 프레임워크로 적용 로드맵 생성

- **What**: 무엇을 적용할 것인가 (Impact × Effort 우선순위)
- **How**: 3-Phase 계획 (Quick Win → Integration → Mastery)
- **Well**: 성공 지표, Anti-patterns, 실패 시그널

**Roadmap Sentinel**로 세션 간 장기 진행 상황 추적

```
/news-apply                                         # 현재 세션의 news-dive 결과 기반
/news-apply --file {name}                           # 저장된 분석 파일 기반
/news-apply --save                                  # 로드맵 + sentinel 저장
/news-apply --roadmap {id}                          # 기존 로드맵 복원
/news-apply --list                                  # 전체 로드맵 목록
/news-apply --update {id} --complete M1.2           # 마일스톤 완료
```

## Example Workflow

```bash
# 아침: 오늘의 AI 뉴스 확인
/tech-news

# 흥미로운 항목? Deep Dive
/news-dive 1-2

# 적용하고 싶다면? 로드맵 생성
/news-apply --save

# 다음 주: 진행 상황 확인
/news-apply --roadmap roadmap-2026-02-07-agent-teams

# 마일스톤 완료
/news-apply --update roadmap-2026-02-07-agent-teams --complete M1.2
```

## File Storage

| Skill | Location | Content |
|-------|----------|---------|
| news-dive | `~/.claude/news-archive/` | Deep dive analysis (markdown) |
| news-apply | `~/.claude/news-roadmap/` | Roadmap documents (markdown) |
| news-apply | `~/.claude/news-roadmap/sentinel/` | Progress tracking (JSON) |

## Optional: Notion Integration

`tech-news --save`로 브리핑을 Notion 페이지에 저장 가능

### Setup

1. [Notion Integrations](https://www.notion.so/my-integrations)에서 Integration 생성
2. API Token 설정:
   ```bash
   # 환경 변수
   export NOTION_API_TOKEN="ntn_xxxxxxxxxxxxx"

   # 또는 1Password CLI
   # 스킬의 op:// 경로를 본인 vault에 맞게 수정
   ```
3. Notion 페이지 생성 → Integration 연결 → Page ID 복사
4. tech-news SKILL.md의 `{YOUR_NOTION_PAGE_ID}`를 실제 ID로 교체

## Language

스킬 출력은 **한국어** 기본 (기술 용어는 영어 유지)

## License

MIT

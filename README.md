# AI News Pipeline

[Claude Code](https://claude.ai/code) 플러그인 — AI 기술 뉴스를 수집하고, 심층 분석하고, 적용 로드맵까지 만드는 3단계 파이프라인입니다.

```
tech-news (수집) --> news-dive (분석) --> news-apply (적용)
  7개 소스            다중 소스            Ultrathink
  브리핑 출력          Deep Dive           로드맵 + Sentinel
```

## 설치

```
/plugin marketplace add Allen-han21/ai-news-pipeline
```

## 스킬 소개

### `/tech-news` - AI 뉴스 브리핑

**7개 소스**에서 AI 뉴스를 병렬 수집하여 카테고리별로 브리핑합니다.

| Tier | 소스 | 수집 방법 |
|------|------|-----------|
| Tier 1 (큐레이션) | GeekNews, Hacker News | WebFetch |
| Tier 2 (공식) | Anthropic, OpenAI, Google AI | WebSearch |
| Tier 3 (커뮤니티) | Reddit, Hugging Face | WebFetch |

출력 카테고리: 주요 발표, 기술 & 연구, 커뮤니티 & 트렌드

```
/tech-news              # 터미널 출력만
/tech-news --save       # 터미널 출력 + Notion에 저장 (설정 필요)
```

### `/news-dive` - 심층 분석

브리핑 항목(또는 URL/주제)을 선택하면 다중 소스에서 심층 분석합니다:

- 원본 소스 + 커뮤니티 반응 (HN, GeekNews)
- 기술 상세, 벤치마크, 구현 가이드
- 실무 적용 이점과 방법

```
/news-dive 1-2                        # 브리핑 항목 번호 (주요 발표 2번째)
/news-dive https://example.com/post   # URL 직접 분석
/news-dive "Claude Agent Teams"       # 키워드 검색
/news-dive 1-1 --save                 # 분석 결과 파일 저장
```

### `/news-apply` - 적용 로드맵

**ultrathink**(확장 사고)로 **What/How/Well** 프레임워크 기반의 적용 로드맵을 생성합니다:

- **What**: 무엇을 적용할 것인가 (Impact x Effort 우선순위)
- **How**: 3단계 계획 (Quick Win → Integration → Mastery)
- **Well**: 성공 지표, Anti-patterns, 실패 시그널

**Roadmap Sentinel**로 세션을 넘어 장기 진행 상황을 추적합니다.

```
/news-apply                     # 현재 세션의 news-dive 결과 기반
/news-apply --file {이름}       # 저장된 분석 파일 기반
/news-apply --save              # 로드맵 + sentinel 저장
/news-apply --roadmap {id}      # 기존 로드맵 복원 및 진행 상황 확인
/news-apply --list              # 전체 로드맵 목록
/news-apply --update {id} --complete M1.2   # 마일스톤 완료 처리
```

## 선택 사항: Notion 연동

`tech-news` 스킬에서 `--save` 옵션으로 브리핑을 Notion 페이지에 저장할 수 있습니다.

### 1. Notion Integration 생성

1. [Notion Integrations](https://www.notion.so/my-integrations)에서 새 통합 생성
2. "Internal Integration" 선택
3. API Token 복사

### 2. API Token 설정

`NOTION_API_TOKEN` 환경 변수를 설정하거나, 원하는 비밀 관리 도구를 사용하세요.

```bash
# 방법 A: 환경 변수
export NOTION_API_TOKEN="ntn_xxxxxxxxxxxxx"

# 방법 B: 1Password CLI (권장)
# 스킬의 op:// 경로를 본인의 vault에 맞게 수정
```

### 3. Notion 페이지 연결

1. Notion에서 뉴스 저장용 페이지 생성
2. 페이지에 통합 연결 (페이지 → ⋯ → Connections → 추가)
3. URL에서 페이지 ID 복사
4. tech-news 스킬의 `{YOUR_NOTION_PAGE_ID}`를 실제 ID로 교체

## 파일 저장 경로

| 스킬 | 저장 위치 | 내용 |
|------|-----------|------|
| news-dive | `~/.claude/news-archive/` | 심층 분석 결과 (markdown) |
| news-apply | `~/.claude/news-roadmap/` | 로드맵 문서 (markdown) |
| news-apply | `~/.claude/news-roadmap/sentinel/` | 진행 상황 추적 (JSON) |

## 사용 예시

```
# 아침: 오늘의 AI 뉴스 확인
/tech-news

# 흥미로운 항목 발견? 심층 분석
/news-dive 1-2

# 적용하고 싶다면? 로드맵 생성
/news-apply --save

# 다음 주: 진행 상황 확인
/news-apply --roadmap roadmap-2026-02-07-agent-teams

# 마일스톤 완료
/news-apply --update roadmap-2026-02-07-agent-teams --complete M1.2
```

## 언어

스킬 출력은 **한국어** 기본입니다 (기술 용어는 영어 유지). 한국어 개발자를 위해 설계되었지만, 구조 자체는 언어에 구애받지 않습니다.

## 라이선스

MIT

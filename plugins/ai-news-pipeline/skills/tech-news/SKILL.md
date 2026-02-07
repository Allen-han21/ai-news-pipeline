---
name: tech-news
description: "AI 기술 뉴스 브리핑. GeekNews 최우선, 7개 소스 병렬 수집. \"기술 뉴스\", \"tech news\", \"AI 뉴스\", \"뉴스 확인\", \"뉴스\" 요청 시 활성화."
version: 1.0.0
---

# Skill: AI Tech News Briefing

매일 확인해야 할 AI 기술 뉴스를 7개 소스에서 한번에 수집하여 카테고리별로 브리핑합니다.

---

## 🎯 목적

분산된 AI 기술 뉴스 소스를 한 번의 명령으로 수집·분류·요약하여 터미널에 브리핑합니다.
GeekNews(news.hada.io)를 최우선 큐레이션 소스로 사용합니다.

## 🔧 사용 시점

다음 키워드나 상황에서 자동 활성화됩니다:

- "기술 뉴스", "tech news", "AI 뉴스"
- "뉴스 확인", "뉴스"
- "오늘 AI 소식", "최신 AI"
- `/tech-news`

---

## ⚙️ 옵션

| 옵션 | 설명 | 예시 |
|------|------|------|
| (기본) | 터미널 출력만 | `/tech-news` |
| `--save` | 터미널 출력 + Notion 페이지에 저장 (선택 사항) | `/tech-news --save` |

---

## 📰 뉴스 소스 (3-Tier 우선순위)

### 🥇 Tier 1: 큐레이션 (품질 최우선)
| 소스 | URL | 방법 | 비고 |
|------|-----|------|------|
| **GeekNews** | `https://news.hada.io/` | WebFetch | 🏆 최우선. AI 관련 글은 무조건 포함 |
| **Hacker News** | `https://news.ycombinator.com/` | WebFetch | 높은 점수 AI 포스트 필터링 |

### 🥈 Tier 2: 공식 소스 (1차 정보)
| 소스 | 검색 쿼리 | 방법 |
|------|-----------|------|
| **Anthropic** | `"Anthropic" OR "Claude" {year} news` | WebSearch |
| **OpenAI** | `"OpenAI" OR "ChatGPT" OR "GPT" {year} latest news` | WebSearch |
| **Google AI** | `"Google Gemini" OR "DeepMind" {year} AI news` | WebSearch |

### 🥉 Tier 3: 커뮤니티 & 오픈소스
| 소스 | URL | 방법 |
|------|-----|------|
| **Reddit** | `https://www.reddit.com/r/MachineLearning+LocalLLaMA+ClaudeAI/hot/` | WebFetch |
| **Hugging Face** | `https://huggingface.co/blog` | WebFetch |

---

## 📋 실행 단계

### Step 1: 병렬 뉴스 수집

**⚠️ 중요: 아래 7개 Tool 호출을 반드시 단일 응답에서 한꺼번에 병렬로 실행하세요. 절대 순차 실행하지 마세요.**

**Tier 1 - WebFetch (2개):**

1. **GeekNews**
   - URL: `https://news.hada.io/`
   - Prompt: "이 페이지의 글 목록에서 AI, LLM, Claude, GPT, 머신러닝, 딥러닝, Anthropic, OpenAI, 인공지능 관련 글을 모두 추출해주세요. 각 글의 제목, 원본 URL, 포인트 수, 댓글 수를 포함해주세요. 최신순으로 정렬."

2. **Hacker News**
   - URL: `https://news.ycombinator.com/`
   - Prompt: "Extract all posts related to AI, ML, LLM, Claude, GPT, machine learning, deep learning, Anthropic, OpenAI from this page. Include: title, URL, score (points), number of comments. Sort by score descending. Only include posts with AI/ML relevance."

**Tier 2 - WebSearch (3개):**

3. **Anthropic/Claude 뉴스**
   - Query: `"Anthropic" OR "Claude AI" OR "Claude Code" {current_year} news announcement`
   - ⚠️ `{current_year}`는 실행 시점의 연도로 치환 (예: 2026)

4. **OpenAI 뉴스**
   - Query: `"OpenAI" OR "ChatGPT" OR "GPT-5" OR "GPT" {current_year} latest news`

5. **Google AI 뉴스**
   - Query: `"Google Gemini" OR "DeepMind" OR "Google AI" {current_year} AI news`

**Tier 3 - WebFetch (2개):**

6. **Reddit AI 커뮤니티**
   - URL: `https://www.reddit.com/r/MachineLearning+LocalLLaMA+ClaudeAI/hot/`
   - Prompt: "Extract the top posts from this page. For each post include: title, subreddit, score (upvotes), number of comments, URL. Focus on posts from today or yesterday. Sort by score."

7. **Hugging Face Blog**
   - URL: `https://huggingface.co/blog`
   - Prompt: "Extract the latest blog posts. Include: title, date, brief description, URL. Focus on posts from the last week about new models, tools, libraries, or research."

---

### Step 2: 필터링 & 분류

수집된 모든 결과를 아래 기준으로 정리합니다.

**GeekNews 앵커 원칙:**
- GeekNews에서 온 AI 관련 글은 **무조건 포함** (필터링하지 않음)
- 다른 소스는 GeekNews에 없는 정보를 보충하는 역할

**카테고리 분류:**
- **주요 발표**: 공식 모델/제품 출시, 중대 업데이트, 기업 발표
- **기술 & 연구**: 논문, 벤치마크, 오픈소스 릴리즈, 깊이 있는 기술 분석
- **커뮤니티 & 트렌드**: 토론, 의견, 튜토리얼, 비교, 사용기

**필터링 규칙:**
1. 최근 24~48시간 이내 (날짜 불분명하면 최신 우선)
2. AI/ML/LLM 관련성 높은 것만 (일반 기술 뉴스 제외)
3. **중복 제거**: 같은 뉴스가 여러 소스에 있으면 하나로 통합, Tier가 높은 소스를 우선 표기하고 다른 소스도 병기
4. 각 카테고리 3~5건, **총 최대 15건**

---

### Step 3: 터미널 출력

아래 형식으로 터미널에 출력합니다. 파일 저장은 하지 않습니다.

```
# 🤖 AI 뉴스 브리핑 ({오늘 날짜})

## 📢 주요 발표
1. **{제목}** - {한줄 요약}
   ↳ {소스} | 🔗 {URL}
2. **{제목}** - {한줄 요약}
   ↳ {소스} | 🔗 {URL}

## 🔬 기술 & 연구
1. **{제목}** - {한줄 요약}
   ↳ {소스} | 🔗 {URL}

## 💬 커뮤니티 & 트렌드
1. **{제목}** - {한줄 요약}
   ↳ {소스} | 🔗 {URL}

---
📊 Tier1: GeekNews · HN | Tier2: Anthropic · OpenAI · Google | Tier3: Reddit · HuggingFace
⏱️ {수집 시각}
```

**소스 표기 형식:**
- GeekNews: `GeekNews 🔥{포인트} 💬{댓글수}`
- Hacker News: `HN ⬆{점수} 💬{댓글수}`
- Reddit: `Reddit r/{서브레딧} ⬆{점수}`
- 공식 소스: `Anthropic 공식`, `OpenAI 공식`, `Google AI`
- HuggingFace: `HuggingFace Blog`
- 복수 소스: `GeekNews 🔥12 | HN ⬆142` (Tier 높은 소스 먼저)

**요약 작성 규칙:**
- 한줄 요약은 한국어로 작성
- 기술 용어는 영어 유지 (LLM, GPT, fine-tuning 등)
- 핵심 포인트만 간결하게 (15자~30자)

---

### Step 4: Notion 저장 (--save 옵션 시) [선택 사항]

`--save` 옵션이 있을 때만 실행합니다. Step 3 터미널 출력 후에 진행합니다.

**⚠️ 이 기능은 선택 사항이며 Notion 통합 설정이 필요합니다. 아래 "Setup (Optional)" 섹션을 참조하세요.**

**4.1 Notion 블록 JSON 구성**

브리핑 결과를 Notion 블록으로 변환합니다.
scratchpad 디렉토리에 JSON 파일을 생성합니다.

```json
{
  "children": [
    {
      "object": "block",
      "type": "heading_2",
      "heading_2": {
        "rich_text": [{"type": "text", "text": {"content": "🤖 AI 뉴스 브리핑 ({오늘 날짜})"}}]
      }
    },
    {
      "object": "block",
      "type": "divider",
      "divider": {}
    },
    {
      "object": "block",
      "type": "heading_3",
      "heading_3": {
        "rich_text": [{"type": "text", "text": {"content": "📢 주요 발표"}}]
      }
    },
    {
      "object": "block",
      "type": "bulleted_list_item",
      "bulleted_list_item": {
        "rich_text": [
          {
            "type": "text",
            "text": {"content": "{제목}", "link": {"url": "{원본URL}"}},
            "annotations": {"bold": true}
          },
          {
            "type": "text",
            "text": {"content": " - {한줄 요약}"}
          },
          {
            "type": "text",
            "text": {"content": " | {소스}"},
            "annotations": {"color": "gray"}
          }
        ]
      }
    },
    {
      "object": "block",
      "type": "heading_3",
      "heading_3": {
        "rich_text": [{"type": "text", "text": {"content": "🔬 기술 & 연구"}}]
      }
    },
    {
      "object": "block",
      "type": "heading_3",
      "heading_3": {
        "rich_text": [{"type": "text", "text": {"content": "💬 커뮤니티 & 트렌드"}}]
      }
    },
    {
      "object": "block",
      "type": "divider",
      "divider": {}
    },
    {
      "object": "block",
      "type": "paragraph",
      "paragraph": {
        "rich_text": [
          {
            "type": "text",
            "text": {"content": "📊 Tier1: GeekNews · HN | Tier2: Anthropic · OpenAI · Google | Tier3: Reddit · HuggingFace ⏱️ {시각}"},
            "annotations": {"color": "gray"}
          }
        ]
      }
    }
  ]
}
```

**블록 변환 규칙:**
- 각 카테고리(주요 발표, 기술 & 연구, 커뮤니티 & 트렌드) → `heading_3`
- 각 뉴스 항목 → `bulleted_list_item` (제목에 원본 URL 링크, 볼드 처리)
- 카테고리에 항목이 없으면 해당 `heading_3` 생략
- 날짜 헤딩 → `heading_2` (매일 구분용)
- 소스 표기 → `gray` 색상 annotation

**4.2 API 호출**

```bash
curl -s -X PATCH "https://api.notion.com/v1/blocks/{YOUR_NOTION_PAGE_ID}/children" \
  -H "Authorization: Bearer ${NOTION_API_TOKEN}" \
  -H "Content-Type: application/json" \
  -H "Notion-Version: 2022-06-28" \
  -d @{scratchpad}/tech_news_notion.json
```

**4.3 결과 확인**

API 응답 확인 후 출력:

```
✅ Notion 저장 완료!
📄 페이지: https://www.notion.so/{YOUR_PAGE}
```

API 에러 시:
```
⚠️ Notion 저장 실패: {에러 메시지}
터미널 출력은 정상 완료되었습니다.
```

---

## ⚙️ Setup (Optional)

Notion 저장 기능(`--save` 옵션)을 사용하려면 다음 설정이 필요합니다.

### 1. Notion Integration 생성

1. [Notion Integrations](https://www.notion.so/my-integrations)에서 새 통합 생성
2. "Internal Integration" 선택
3. 권한 설정:
   - ✅ Read content
   - ✅ Update content
   - ✅ Insert content
4. API Token 복사 (예: `secret_xxxxxxxxxxxx`)

### 2. 뉴스 페이지 생성 및 통합 연결

1. Notion에서 새 페이지 생성 (예: "Tech News")
2. 페이지 우측 상단 `···` → `Connections` → 위에서 만든 통합 선택
3. 페이지 URL에서 페이지 ID 추출:
   - URL 형식: `https://www.notion.so/Tech-News-{PAGE_ID}`
   - 예: `abc123def456789012345678abcdef01`

### 3. 환경 변수 설정

```bash
# ~/.bashrc 또는 ~/.zshrc에 추가
export NOTION_API_TOKEN="secret_xxxxxxxxxxxx"

# 적용
source ~/.bashrc  # 또는 source ~/.zshrc
```

### 4. SKILL.md 수정

Step 4의 API 호출 부분에서:
- `{YOUR_NOTION_PAGE_ID}`를 실제 페이지 ID로 교체
- `https://www.notion.so/{YOUR_PAGE}`를 실제 페이지 URL로 교체

### 5. 테스트

```bash
/tech-news --save
```

성공 시 Notion 페이지에 브리핑이 추가됩니다.

---

## 🚫 주의사항

- WebFetch 실패 시 해당 소스만 건너뛰고 나머지 결과로 브리핑 생성
- 수집 결과가 전혀 없으면 "현재 수집 가능한 AI 뉴스가 없습니다" 메시지 출력
- `--save` 없이 실행하면 터미널 출력만 (Notion 저장 안 함)
- GeekNews 원문이 한국어가 아닐 수 있음 (영어 원문 + 한국어 요약 조합)
- Notion API 실패 시 터미널 브리핑은 정상 표시 (저장만 실패 안내)
- Notion 블록은 페이지 하단에 누적 추가됨 (최신이 아래쪽)

---

## 📌 예제

### 예제 1: 일반 실행

```
User: "AI 뉴스"

Claude: [tech-news 스킬 활성화]

# 🤖 AI 뉴스 브리핑 (2026-02-06)

## 📢 주요 발표
1. **Claude 4.5 Opus 출시** - Anthropic이 최신 플래그십 모델 발표, 코딩·수학 벤치마크 SOTA
   ↳ GeekNews 🔥24 💬15 | Anthropic 공식 | 🔗 https://...
2. **GPT-5 Turbo 공개** - OpenAI의 차세대 모델, 멀티모달 성능 대폭 향상
   ↳ OpenAI 공식 | HN ⬆342 | 🔗 https://...

## 🔬 기술 & 연구
1. **Llama 4 오픈소스 공개** - Meta의 400B 파라미터 모델, 상업적 사용 가능
   ↳ GeekNews 🔥18 | HuggingFace Blog | 🔗 https://...
2. **LoRA 학습 효율 3배 향상 연구** - 새로운 어댑터 아키텍처 제안
   ↳ HN ⬆89 💬47 | 🔗 https://...

## 💬 커뮤니티 & 트렌드
1. **로컬 LLM으로 코드 리뷰 자동화 구축기** - Ollama + CodeReview 파이프라인 튜토리얼
   ↳ Reddit r/LocalLLaMA ⬆523 | 🔗 https://...

---
📊 Tier1: GeekNews · HN | Tier2: Anthropic · OpenAI · Google | Tier3: Reddit · HuggingFace
⏱️ 09:30
```

### 예제 2: Notion 저장

```
User: /tech-news --save

Claude: [tech-news 스킬 활성화]

# 🤖 AI 뉴스 브리핑 (2026-02-07)

## 📢 주요 발표
1. **Claude Opus 4.6 출시** - Agent Teams와 100만 토큰 컨텍스트 지원
   ↳ GeekNews 🔥32 💬21 | Anthropic 공식 | 🔗 https://...

## 🔬 기술 & 연구
1. **Qwen3 오픈소스 공개** - Alibaba의 최신 LLM, 코딩 벤치마크 상위권
   ↳ HN ⬆256 💬89 | 🔗 https://...

## 💬 커뮤니티 & 트렌드
1. **MCP 서버 구축 완전 가이드** - 실무 개발자의 단계별 튜토리얼
   ↳ Reddit r/ClaudeAI ⬆412 | 🔗 https://...

---
📊 Tier1: GeekNews · HN | Tier2: Anthropic · OpenAI · Google | Tier3: Reddit · HuggingFace
⏱️ 09:30

Notion 저장 중...

✅ Notion 저장 완료!
📄 페이지: https://www.notion.so/{YOUR_PAGE}
```

---

## 연계 스킬

| 스킬 | 역할 |
|------|------|
| `/news-dive` | 브리핑 항목 심층 분석 → tech-news 출력을 입력으로 사용 |
| `/news-apply` | 적용 로드맵 생성 → news-dive 분석 결과를 실행 계획으로 전환 |

```
tech-news (수집) → news-dive (분석) → news-apply (적용)
```

---

**Created:** 2026-02-06
**Updated:** 2026-02-07
**Version:** 1.0.0

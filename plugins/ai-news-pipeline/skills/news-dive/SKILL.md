---
name: news-dive
version: 1.0.0
description: "AI 뉴스 심층 분석. tech-news 브리핑 항목 또는 URL/주제를 다중 소스에서 연구하여 적용 이점·방법·인사이트 제공. \"뉴스 분석\", \"뉴스 자세히\", \"news dive\", \"deep dive\", \"자세히\" 요청 시 활성화."
---

# Skill: AI News Deep-Dive

tech-news 브리핑의 특정 항목 또는 직접 제공한 AI 뉴스 URL/주제를 다중 소스에서 심층 연구하여, 개발자의 AI 활용 관점에서 실무 적용 분석을 제공합니다.

---

## 🎯 목적

- tech-news 브리핑에서 흥미로운 항목을 선택하여 깊이 있는 분석
- 원본 소스 + 커뮤니티 반응 + 관련 자료를 종합 수집
- **내 워크플로우에 어떻게 적용할 수 있는가**에 초점을 둔 실무 분석
- 구체적인 적용 방법, 코드/설정 예시, 핵심 인사이트 제공

## 🔧 사용 시점

다음 키워드나 상황에서 자동 활성화됩니다:

- "뉴스 분석", "뉴스 자세히", "news dive", "deep dive"
- "1번 자세히", "뉴스 2번 분석", "기사 분석"
- `/news-dive`, `/news-dive {번호}`, `/news-dive {URL}`

---

## 📰 입력 형식

3가지 입력 형식을 지원합니다. 자동 판별 후 처리합니다.

### A. tech-news 브리핑 번호 참조

현재 세션의 tech-news 브리핑 결과에서 항목을 참조합니다.

| 형식 | 의미 | 예시 |
|------|------|------|
| `{카테고리}-{항목}` | 특정 카테고리의 N번째 | `/news-dive 1-2` → 주요 발표 2번째 |
| `{번호}` | 전체 브리핑에서 N번째 | `/news-dive 3` → 전체 3번째 항목 |

**카테고리 번호:**
- 1 = 📢 주요 발표
- 2 = 🔬 기술 & 연구
- 3 = 💬 커뮤니티 & 트렌드

**세션에 tech-news 결과가 없을 때:**
- "현재 세션에 tech-news 브리핑이 없습니다. 주제나 URL을 직접 입력해주세요." 메시지 출력
- AskUserQuestion으로 대안 제시: tech-news 먼저 실행 / 직접 주제 입력

### B. 직접 URL

```bash
/news-dive https://www.anthropic.com/news/claude-opus-4-6
/news-dive https://openai.com/index/introducing-gpt-5-3-codex/
```

### C. 직접 주제/키워드

```bash
/news-dive Claude Opus 4.6
/news-dive "Agent Teams Anthropic"
```

키워드 입력 시 WebSearch로 최적 기사를 탐색한 후 분석을 시작합니다.
주제가 너무 광범위하면 AskUserQuestion으로 구체화를 요청합니다.

---

## ⚙️ 옵션

| 옵션 | 설명 | 예시 |
|------|------|------|
| `--save` | 분석 결과를 파일로 저장 | `/news-dive 1-2 --save` |
| `help` | 도움말 표시 | `/news-dive help` |
| (기본) | 터미널 출력만 | `/news-dive 1-2` |

**`--save` 저장 경로:** `~/.claude/news-archive/`
**파일명 규칙:** `news-dive-{날짜}-{제목-slug}.md`

---

## Help (사용 방법)

`/news-dive help` 실행 시 다음 내용을 표시합니다:

~~~markdown
## news-dive v1.0 사용 방법

AI 뉴스를 심층 분석하여 적용 이점·방법·인사이트를 제공합니다.

### 사용법
```
/news-dive 1-2                              # tech-news 주요 발표 2번째
/news-dive 3                                # tech-news 전체 3번째
/news-dive https://example.com/article      # URL 직접 분석
/news-dive Claude Opus 4.6                  # 키워드로 검색 후 분석
/news-dive 1-1 --save                       # 분석 후 파일 저장
```

### 출력 섹션
- 📝 상세 요약: 무엇이, 왜 중요한지, 기술적 세부사항
- 💡 적용 이점: 즉시/중장기 활용 가능성
- 🛠 적용 방법: 단계별 행동, 코드/설정 예시
- 🎯 핵심 인사이트: 시사점, 업계 영향, 주목 포인트

### 연계 스킬
- `/tech-news` → AI 뉴스 브리핑 (입력 소스)
- `/news-apply` → 적용 로드맵 생성 (실행 계획 전환)
~~~

---

## 📋 실행 단계

### Step 1: 입력 파싱

입력 타입을 자동 판별합니다:

```
입력 → 판별 기준:
  숫자 패턴 (N 또는 N-M)   → tech-news 브리핑 참조 (Case A)
  URL 패턴 (http/https)    → 직접 URL 분석 (Case B)
  일반 텍스트              → 주제 키워드 검색 (Case C)
```

**Case A: 브리핑 번호**
1. 현재 세션의 tech-news 브리핑 결과에서 해당 항목을 찾습니다
2. 항목의 제목, URL, 소스 정보를 추출합니다
3. 세션에 브리핑이 없으면 안내 메시지를 출력합니다

**Case B: 직접 URL**
1. URL 유효성을 확인합니다
2. 해당 URL을 원본 소스로 사용합니다

**Case C: 키워드**
1. WebSearch로 `"{키워드}" {current_year} AI news` 검색
2. 상위 결과에서 가장 관련성 높은 기사를 선택합니다
3. 결과가 모호하면 AskUserQuestion으로 선택지를 제시합니다

---

### Step 2: 원본 소스 병렬 수집

**⚠️ 중요: 아래 WebFetch 호출을 반드시 단일 응답에서 한꺼번에 병렬로 실행하세요. 절대 순차 실행하지 마세요.**

```
[WebFetch 병렬 실행 - 최대 4개]

1. 원본 URL (필수)
   - Prompt: "이 페이지의 전체 내용을 상세히 추출해주세요.
     다음을 포함해주세요:
     - 핵심 발표/변경 내용
     - 기술적 세부사항 (아키텍처, 성능 지표, API 변경 등)
     - 제약 사항 및 한계점
     - 가격/라이선스 정보 (있으면)
     - 관련 링크 (GitHub, 문서, 논문 등)"

2. Hacker News 댓글 (tech-news 소스에 HN URL이 있으면)
   - URL: https://news.ycombinator.com/item?id={id}
   - Prompt: "이 스레드의 상위 15개 댓글을 추출해주세요.
     각 댓글의 점수(upvotes)와 내용을 포함하고,
     특히 실무 경험 공유, 기술적 비평, 대안 제안에 해당하는 댓글을 우선해주세요."

3. GeekNews 댓글 (tech-news 소스에 GeekNews URL이 있으면)
   - URL: https://news.hada.io/topic?id={id}
   - Prompt: "이 글의 댓글과 토론 내용을 모두 추출해주세요.
     한국어 댓글은 그대로 보존해주세요."

4. 관련 GitHub README (원본에서 GitHub 링크 발견 시)
   - URL: https://github.com/{org}/{repo}
   - Prompt: "이 레포지토리의 README 전체 내용과 최신 릴리즈 정보를 추출해주세요.
     설치 방법, 주요 기능, 사용 예시를 포함해주세요."
```

---

### Step 3: 추가 컨텍스트 병렬 수집

**⚠️ 중요: 아래 WebSearch 호출을 반드시 단일 응답에서 한꺼번에 병렬로 실행하세요. 절대 순차 실행하지 마세요.**

```
[WebSearch 병렬 실행 - 4개]

쿼리 생성 규칙:
- {topic}: 뉴스 제목에서 핵심 고유명사 추출 (예: "Claude Opus 4.6", "GPT-5.3-Codex")
- {year}: 현재 연도 (예: 2026)

1. 기술 상세 & 아키텍처
   - Query: "{topic} technical details architecture {year}"
   - 목적: 내부 구현, 아키텍처 결정, 기술 스택

2. 벤치마크 & 비교
   - Query: "{topic} benchmark comparison vs alternatives {year}"
   - 목적: 성능 지표, 경쟁 제품 대비 장단점

3. 구현 가이드 & 튜토리얼
   - Query: "{topic} implementation guide tutorial getting started {year}"
   - 목적: 실제 사용법, 코드 예시, 설정 방법

4. 커뮤니티 반응 & 실무 경험
   - Query: "{topic} developer experience review discussion {year}"
   - 목적: 얼리 어답터 피드백, 실무 적용 사례, 알려진 이슈
```

---

### Step 4: 종합 분석 및 출력

수집된 모든 자료를 종합하여 아래 4개 핵심 섹션을 작성합니다.

**분석 관점: 개발자의 AI 활용**
- 이 기술/도구/모델이 내 개발 워크플로우에 어떤 영향을 주는가?
- 실제로 써보려면 무엇부터 해야 하는가?
- 업계 흐름에서 이것의 의미는 무엇인가?

**개인화 규칙:**
- 사용자의 CLAUDE.md 컨텍스트가 있으면 자연스럽게 참조 (iOS 개발자면 Swift/Xcode 관련 적용, 웹 개발자면 프론트엔드 관련 적용 등)
- 특정 직무에 하드코딩하지 않음
- AI 모델 발표 → "이 모델을 내 프로젝트에 적용하면?", AI 도구 발표 → "내 워크플로우에 통합하면?", 연구 논문 → "이 기술이 실용화되면?"

---

## 📄 출력 템플릿

아래 형식으로 터미널에 출력합니다.

```markdown
# 🔍 {뉴스 제목} - Deep Dive

**분석일**: {날짜 시간} | **원본**: {소스명} | 🔗 {원본 URL}

---

## 📝 상세 요약

### 무엇이 발표/공개되었나?
{핵심 내용 3-5문장. 객관적 사실 중심.}

### 왜 중요한가?
{의미와 영향 2-3문장. 기존 대비 무엇이 달라졌는지.}

### 기술적 세부사항
- **{핵심 기술 1}**: {설명}
- **{핵심 기술 2}**: {설명}
- **성능 지표**: {벤치마크, 속도, 정확도 등}
- **제약 사항**: {한계점, 알려진 이슈}
- **가격/접근성**: {가격, API 제한, 라이선스 등}

---

## 💡 적용 이점 (내 워크플로우에 미치는 영향)

### 즉시 활용 가능
1. **{이점 1}** - {설명}
   - 적용 시나리오: {구체적인 상황}
   - 예상 효과: {어떤 개선이 되는지}

2. **{이점 2}** - {설명}
   - 적용 시나리오: {구체적인 상황}

### 중장기 적용 가능
- **{이점 3}**: {설명. 왜 중장기인지.}
- **{이점 4}**: {설명}

---

## 🛠 적용 방법 (실제로 써보려면)

### 1단계: 준비
{필요한 사전 조건 - 계정, API 키, 도구 설치 등}

```bash
# 예시: 설치/설정 명령어
```

### 2단계: 실험
{처음 시도해볼 수 있는 최소 단위}

```
# 예시: API 호출, 프롬프트, 설정 코드
```

### 3단계: 통합
{본격적으로 워크플로우에 녹이는 방법}
- {구체적 단계}
- {주의사항}

### 참고 자료
- 📖 공식 문서: {URL}
- 💻 GitHub: {URL}
- 📚 튜토리얼: {URL}

---

## 🎯 핵심 인사이트

### 주요 시사점
1. **{인사이트 1}** - {설명}
2. **{인사이트 2}** - {설명}
3. **{인사이트 3}** - {설명}

### 업계 영향 & 관련 동향
- {이 발표가 업계에 미치는 영향}
- {관련된 다른 기술/트렌드와의 연결}

### 주목할 포인트
- 🔥 {향후 주목할 발전 방향}
- ⚠️ {주의 깊게 봐야 할 리스크/이슈}

---

## 💬 커뮤니티 반응

### Hacker News (⬆{점수} | 💬{댓글수})
- **{핵심 댓글 1}**: "{요약}" — {저자}
- **{핵심 댓글 2}**: "{요약}" — {저자}
- **{핵심 댓글 3}**: "{요약}" — {저자}

### GeekNews (🔥{포인트} | 💬{댓글수})
- **{핵심 논의}**: {요약}

---

## 🔗 참고 링크

| 유형 | 링크 |
|------|------|
| 원본 | {URL} |
| 공식 문서 | {URL} |
| GitHub | {URL} |
| 관련 기사 | {URL} |
| 커뮤니티 토론 | {URL} |

---
📊 수집 소스: {사용된 소스 나열}
⏱️ {수집 시각}
```

---

## 💾 파일 저장 (--save 옵션)

`--save` 옵션 사용 시 분석 결과를 파일로 저장합니다.

**저장 경로:** `~/.claude/news-archive/`

**파일명 규칙:** `news-dive-{YYYY-MM-DD}-{title-slug}.md`
- title-slug: 영어 소문자, 공백을 하이픈으로 변환, 특수문자 제거
- 예: `news-dive-2026-02-07-claude-opus-4-6.md`

**저장 시 동작:**
1. 디렉토리가 없으면 자동 생성
2. 터미널에도 동일하게 출력
3. 저장 완료 후 경로 표시: "💾 저장: {파일 경로}"

---

## 🚫 주의사항

### WebFetch/WebSearch 실패 시
- 해당 소스만 건너뛰고 나머지 결과로 분석 계속
- 출력에 "⚠️ {소스명} 접근 실패" 명시
- 원본 URL마저 실패하면: WebSearch 결과만으로 축약 분석 제공

### Reddit 접근 불가
- www.reddit.com은 WebFetch가 차단됨
- 대신 WebSearch의 커뮤니티 반응 쿼리로 Reddit 내용 간접 수집

### 컨텐츠 부족 시
- 커뮤니티 반응이 없으면: "아직 커뮤니티 반응이 축적되지 않았습니다" 표시
- 기술 상세가 없으면: "공개된 기술 상세 없음 - 추후 업데이트 예상" 명시
- 적용 방법이 불분명하면: 일반적인 방향만 제시하고 "공식 문서 공개 후 업데이트 필요" 명시

### 분석 품질 규칙
- 한줄 요약이 아닌 **충분한 깊이**의 분석 제공
- 추측보다 사실 기반 (출처 명시)
- "적용 방법"에는 반드시 구체적인 첫 단계를 포함 (추상적 조언 금지)
- 한국어 작성, 기술 용어는 영어 유지

---

## 📌 예제

### 예제 1: tech-news 브리핑 번호 참조

```
User: /news-dive 1-1

Claude: [news-dive 활성화]

tech-news 브리핑 "📢 주요 발표" 카테고리의 1번째 항목:
→ **Claude Opus 4.6 출시** | 🔗 https://www.anthropic.com/news/claude-opus-4-6

원본 소스 수집 중...
[WebFetch 병렬: 원본 URL, HN 댓글, GeekNews 댓글]

추가 컨텍스트 수집 중...
[WebSearch 병렬: 기술 상세, 벤치마크, 가이드, 커뮤니티]

# 🔍 Claude Opus 4.6 출시 - Deep Dive

**분석일**: 2026-02-07 14:30 | **원본**: Anthropic 공식 | 🔗 https://www.anthropic.com/news/claude-opus-4-6

---

## 📝 상세 요약

### 무엇이 발표되었나?
Anthropic이 2026년 2월 5일 Claude Opus 4.6을 발표했다. Opus 4.5 대비 코딩 능력이 대폭 향상되었으며,
100만 토큰 컨텍스트 윈도우를 최초로 Opus 모델에서 지원한다. Agent Teams 기능이 함께 공개되어
여러 Claude Code 인스턴스를 팀으로 구성해 병렬 작업이 가능해졌다.

### 왜 중요한가?
기존 Opus 모델의 약점이었던 장시간 에이전틱 태스크 지속 능력이 크게 개선되었다.
Agent Teams는 복잡한 소프트웨어 프로젝트를 분할-병렬 처리하는 새로운 패러다임을 제시한다.

### 기술적 세부사항
- **Agent Teams**: 여러 AI 에이전트가 순차가 아닌 병렬로 작업, 서로 직접 조율
- **Effort Controls**: 4단계 설정으로 토큰 사용량 최적화
- **Context Compaction**: 오래된 컨텍스트 자동 요약으로 긴 대화 지속
- **성능 지표**: SWE-Bench Verified SOTA, 500+ 오픈소스 제로데이 발견
- **제약 사항**: Agent Teams는 research preview 단계
- **가격**: Claude Code Max 구독 $100/월 (무제한), API는 기존 Opus 가격

---

## 💡 적용 이점

### 즉시 활용 가능
1. **Agent Teams로 대규모 리팩토링** - 모듈별로 에이전트를 분배하여 병렬 작업
   - 적용 시나리오: 프로젝트 전체 아키텍처 변경 시 여러 모듈 동시 수정
   - 예상 효과: 단일 에이전트 대비 2-3배 빠른 작업 완료

2. **100만 토큰 컨텍스트로 전체 코드베이스 분석** - 대규모 프로젝트도 한번에 분석 가능
   - 적용 시나리오: 레거시 코드 이해, 의존성 분석

### 중장기 적용 가능
- **Effort Controls 기반 비용 최적화**: 간단한 작업은 low effort, 복잡한 작업만 high effort로 API 비용 절감

---

## 🛠 적용 방법

### 1단계: 준비
Claude Code 최신 버전 업데이트

```bash
claude update
```

### 2단계: Agent Teams 실험
CLAUDE.md에 에이전트 역할 정의 후 실행

```
# 예시: 3개 에이전트로 리팩토링
claude --agent-team "frontend:UI 컴포넌트 수정" "backend:API 레이어 수정" "test:테스트 업데이트"
```

### 참고 자료
- 📖 공식 문서: https://code.claude.com/docs/en/agent-teams
- 💻 활용 가이드: https://claude.com/resources/tutorials/get-the-most-from-claude-opus-4-6

---

## 🎯 핵심 인사이트

### 주요 시사점
1. **에이전트 협업이 새 표준** - 단일 AI → 멀티 에이전트 팀이 복잡한 작업의 기본 패턴으로 자리잡는 중
2. **AI 보안 감사의 실용화** - 500개 제로데이 발견은 AI가 보안 도구로서 실질적 가치를 증명
3. **비용 제어가 핵심 경쟁력** - Effort Controls는 기업 도입의 가장 큰 장벽인 비용 문제를 정면 해결

### 주목할 포인트
- 🔥 Agent Teams가 GA 되면 CI/CD 파이프라인에 통합하는 사례 급증 예상
- ⚠️ 멀티 에이전트 환경에서의 충돌 관리와 일관성 유지가 새로운 과제

---

## 💬 커뮤니티 반응

### Hacker News (⬆2,138 | 💬918)
- **"C 컴파일러 구축 데모가 인상적"**: Agent Teams의 실용성을 보여주는 좋은 사례
- **"100만 토큰이 진짜 게임체인저"**: 대규모 코드베이스 작업에서 컨텍스트 부족 문제 해결

---

📊 수집 소스: Anthropic 공식 · HN · GeekNews · WebSearch x4
⏱️ 14:30
```

### 예제 2: 직접 URL

```
User: /news-dive https://openai.com/index/introducing-gpt-5-3-codex/

Claude: [news-dive 활성화]

URL 분석 중...
→ GPT-5.3-Codex 공개

[WebFetch 병렬: 원본 URL, 관련 HN 스레드]
[WebSearch 병렬: 기술 상세, 벤치마크, 가이드, 커뮤니티]

# 🔍 GPT-5.3-Codex 공개 - Deep Dive
...
(동일 출력 템플릿)
```

### 예제 3: 키워드 검색

```
User: /news-dive Agent Teams

Claude: [news-dive 활성화]

"Agent Teams"로 검색 중...
→ 최적 기사 발견: "Anthropic releases Opus 4.6 with new 'agent teams'" (TechCrunch)

[수집 및 분석 실행]

# 🔍 Agent Teams - Claude Code 멀티 에이전트 오케스트레이션 - Deep Dive
...
```

---

## 연계 스킬

| 스킬 | 역할 |
|------|------|
| `/tech-news` | AI 뉴스 브리핑 수집 → news-dive의 입력 소스 |
| `/news-apply` | 적용 로드맵 생성 → news-dive 분석 결과를 실행 계획으로 전환 |

---

**Created:** 2026-02-07
**Version:** 1.0.0

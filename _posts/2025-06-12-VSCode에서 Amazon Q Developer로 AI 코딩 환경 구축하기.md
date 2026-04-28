---
permalink: /vscode-amazon-q-ai-coding/
published: true
title: "[AI Agent] Amazon Q Developer로 VS Code AI 코딩 환경 구축하기"
date: 2025-06-12 20:00:00
toc: true
toc_sticky: true
toc_label: "Amazon Q 세팅 가이드"
description: "VS Code에서 Amazon Q Developer로 AI 코딩 환경을 구축하는 방법을 정리한다. MCP 서버 연동, Rule 설정, Memory Bank 활용까지 실전 가이드."
categories:
- AI Agent
tags:
- Amazon Q
- VS Code
- MCP
- AI 코딩
- Model Context Protocol
- Cursor
- 개발환경
- Memory Bank
last_modified_at: 2025-06-12
---

요즘 주변 개발자들 사이에서 Cursor 얘기가 정말 많이 나온다. 써본 사람들은 하나같이 "코딩이 달라진다"고 하는데, 막상 도입하려니 월 $20 비용도 그렇고, 지금까지 세팅해둔 VS Code 환경을 버리기가 아까웠다.

그러다 **Amazon Q Developer**를 알게 됐는데, 써보니 생각보다 훨씬 괜찮았다. Free Tier로 무료 사용이 가능하고, 내가 쓰던 VS Code를 그대로 유지하면서 AI 기능만 얹을 수 있다는 게 결정적이었다. 특히 이 블로그를 관리하면서 Amazon Q에 MCP를 연동해 파일 정리, 이미지 최적화, 빌드 테스트까지 자동화한 경험이 꽤 만족스러워서 그 세팅 과정을 공유하려 한다.

> 📚 **관련 포스팅**
> - [MCP(Model Context Protocol)란? AI 에이전트의 표준 인터페이스 완벽 가이드](/mcp-model-context-protocol/)


## ✅ 왜 Cursor 대신 Amazon Q를 선택했나

Cursor를 안 써본 건 아니다. 확실히 UI가 깔끔하고 코드 이해력도 좋다. 하지만 몇 가지 이유로 Amazon Q 쪽에 손을 들어줬다.

**Cursor가 아쉬웠던 점:**
- VS Code 포크라서 기존에 쓰던 확장 프로그램이 일부 호환 안 됨
- 무료 플랜은 기능 제한이 심하고, Pro는 월 $20
- 메모리 사용량이 꽤 높아서 노트북에서 버거울 때가 있음

**Amazon Q가 좋았던 점:**
- VS Code 확장 프로그램이라 기존 환경 100% 유지
- AWS Builder ID만 있으면 Free Tier로 바로 시작
- MCP 연동이 자유로워서 내가 원하는 도구를 직접 붙일 수 있음
- AWS 서비스 관련 질문에 특히 강함 (공식 문서 기반 답변)

결론적으로 Cursor는 "설치하면 바로 쓸 수 있는 완성품"이고, Amazon Q는 "내가 직접 조립하는 커스텀 PC" 같은 느낌이다. 조립하는 과정이 좀 있지만, 완성되면 내 워크플로우에 딱 맞는 환경이 된다.


## ✅ Step 1: Amazon Q 설치 및 기본 세팅

설치는 간단하다.

1. VS Code 확장 프로그램 마켓플레이스에서 **"Amazon Q"** 검색 후 설치
2. 좌측 사이드바에 Amazon Q 아이콘이 생기면 클릭
3. **AWS Builder ID**로 로그인 (없으면 무료 생성)
4. 채팅창이 열리면 세팅 완료

설치 후 바로 채팅창에서 코드 관련 질문을 할 수 있고, `@workspace`를 붙이면 현재 프로젝트 파일들을 컨텍스트로 참조해서 답변해준다.


## ✅ Step 2: MCP 서버 연동으로 기능 확장

Amazon Q의 진짜 힘은 **MCP(Model Context Protocol)**를 연동했을 때 나온다. MCP가 뭔지 모른다면 [이전 포스팅](/mcp-model-context-protocol/)을 먼저 읽어보길 추천한다.

간단히 말하면, MCP 서버를 연결하면 AI가 **파일 읽기/쓰기, 터미널 명령 실행, DB 조회** 같은 작업을 직접 수행할 수 있게 된다. 필자는 이 블로그를 관리하면서 desktop-commander라는 MCP 서버를 연동해 사용하고 있다.

### 📌 연동 방법
<hr>

프로젝트 루트에 `.amazonq/agents/default.json` 파일을 만들고 아래 내용을 작성한다.

```json
{
  "mcpServers": {
    "desktop-commander": {
      "command": "npx",
      "args": [
        "-y",
        "@anthropic-ai/desktop-commander"
      ],
      "timeout": 0
    }
  }
}
```

저장 후 Amazon Q 채팅창을 새로고침하면 MCP 서버가 자동으로 로드된다.

또는 Amazon Q 채팅창 우측 상단의 **MCP 아이콘(🛠️) → Add** 버튼을 통해 UI에서 직접 추가할 수도 있다.

### 📌 실제로 써본 예시
<hr>

이 블로그 작업하면서 실제로 사용한 프롬프트 몇 가지를 공유한다.

```
"_posts 폴더에서 description이 없는 포스트를 전부 찾아서 title 기반으로 description을 자동 생성해줘"
```

```
"img 폴더에서 500KB 넘는 이미지를 찾아서 1200px로 리사이즈해줘"
```

```
"_sass 폴더의 SCSS 파일 중 한글 주석이 있는 파일에 @charset utf-8을 추가해줘"
```

MCP가 연동되어 있으니 이런 요청을 하면 AI가 직접 파일을 탐색하고, 수정하고, 결과를 보여준다. 수동으로 하면 한 시간 걸릴 작업이 몇 분이면 끝난다.

<p class="notice--info">
💡 <a href="https://smithery.ai">Smithery.ai</a>에서 GitHub, PostgreSQL, Slack 등 다양한 MCP 서버를 찾을 수 있다. 필요한 도구를 검색해서 바로 연동하면 된다.
</p>


## ✅ Step 3: 자동 승인으로 워크플로우 가속

MCP 도구를 실행할 때마다 "이거 실행해도 돼?" 하고 확인 팝업이 뜨는데, 자주 쓰는 도구는 이게 꽤 번거롭다.

**설정 방법:**
1. MCP 아이콘(🛠️) 클릭 → 연동된 서버의 도구 목록 진입
2. 자주 쓰는 도구(파일 읽기, 검색 등)를 **Ask → Always Allow**로 변경

<p class="notice--warning">
⚠️ 파일 삭제나 DB 수정 같은 위험한 작업은 반드시 <strong>Ask 상태를 유지</strong>하자. 실수로 중요한 파일을 날릴 수 있다.
</p>


## ✅ Step 4: Rule로 AI 답변 스타일 커스터마이징

AI 코딩 도우미를 쓰다 보면 이런 경험이 있을 것이다. "프로젝트는 TypeScript인데 자꾸 JavaScript로 답변한다", "우리 팀 컨벤션이랑 다른 스타일로 코드를 쓴다", "매번 같은 주의사항을 반복해서 말해줘야 한다". Rule은 이런 문제를 근본적으로 해결해준다.

### 📌 Rule이란?
<hr>

**Rule은 AI가 답변할 때 항상 지켜야 할 규칙을 마크다운 파일로 정의해두는 기능**이다. 한번 작성해두면 매 대화마다 자동으로 적용되기 때문에, 매번 "한국어로 답변해줘", "TypeScript로 작성해줘" 같은 지시를 반복할 필요가 없다.

Cursor에서 `.cursorrules` 파일로 제공하는 기능과 동일한 개념이고, Amazon Q에서는 `.amazonq/rules/` 폴더에 마크다운 파일로 관리한다.

### 📌 Rule을 쓰면 뭐가 좋은가?
<hr>

**1. 반복 지시 제거**

Rule 없이 AI를 쓰면 매번 이렇게 된다.

```
"한국어로 답변해줘. TypeScript로 작성해줘. 함수명은 camelCase로 해줘.
코드 변경 시 이유를 설명해줘. 불확실한 건 추측하지 말고..."
```

Rule을 설정해두면 이런 지시 없이 바로 본론으로 들어갈 수 있다.

**2. 팀 컨벤션 일관성 유지**

Rule 파일을 git에 커밋해두면 팀원 모두가 동일한 규칙으로 AI를 사용하게 된다. 누군가는 함수명을 snake_case로, 누군가는 camelCase로 쓰는 문제가 사라진다.

**3. 프로젝트별 맞춤 설정**

Rule은 프로젝트 루트의 `.amazonq/rules/` 폴더에 저장되기 때문에, 프로젝트마다 다른 규칙을 적용할 수 있다. 백엔드 프로젝트와 프론트엔드 프로젝트에 서로 다른 Rule을 두는 식이다.

**4. 실수 방지**

"파일 수정 전에 반드시 백업해라", "테스트 코드를 삭제하지 마라" 같은 안전장치를 Rule에 넣어두면 AI가 실수로 중요한 코드를 날리는 것을 예방할 수 있다.

### 📌 Rule 생성 방법
<hr>

1. Amazon Q 채팅창 상단 **'Rules'** 클릭
2. **'Create a new rule'** → 이름 입력 → Create
3. `.amazonq/rules/` 폴더에 마크다운 파일이 생성됨
4. 규칙 작성

### 📌 필자가 쓰는 Rule
<hr>

필자는 이 블로그 프로젝트에서 실제로 아래와 같은 Rule을 사용하고 있다. `.amazonq/rules/Github_Blog.md` 파일에 작성해두었다.

```markdown
# Jekyll 블로그 프로젝트 Rule

## 기본 규칙
- 한국어로 답변한다.
- 파일 수정 전에 반드시 현재 내용을 먼저 읽고 확인한다.
- 불확실한 내용은 추측하지 않고 "확인이 필요하다"고 명시한다.
- 사용자의 기존 코드를 임의로 삭제하지 않는다.
- 변경 사항이 생기면 `블로그_저장규칙.md`의 변경 이력 섹션에 추가한다.

## 포스트 작성 규칙
- 새 포스트 작성 시 front matter에 다음 필드를 반드시 포함한다:
  - `permalink`: 영문으로 작성 (한글 URL 금지)
  - `title`: `[카테고리] 제목` 형식
  - `description`: 50~160자, 핵심 키워드 포함
  - `tags`: 관련 키워드 3~8개
  - `toc: true`, `toc_sticky: true`
  - `last_modified_at`: 작성/수정 날짜
- 본문 제목은 `## ✅` (대제목), `### 📌` + `<hr>` (소제목) 형식을 사용한다.
- 문체는 `~이다/~한다` 체를 사용한다.
- 여백은 `<br>` 태그 대신 CSS margin으로 처리한다.
- 이미지는 `/img/` 폴더에 저장하고 상대경로로 참조한다 (GitHub raw URL 금지).
- 관련 포스트가 있으면 `> 📚 관련 포스팅` 블록으로 내부 링크를 추가한다.

## SCSS/CSS 규칙
- SCSS 파일에 한글 주석 사용 시 파일 상단에 `@charset "utf-8";` 필수.
- 반응형 스타일 수정 시 모바일(600px 이하)을 반드시 고려한다.

## 빌드/배포 규칙
- 빌드 명령어: `RUBYOPT="-EUTF-8" bundle exec jekyll serve`
- 파일 수정 후 빌드 테스트를 실행하여 경고/에러가 0개인지 확인한다.

## Git 규칙
- 커밋 메시지는 한국어로 작성한다.
```

이 Rule 하나만 있어도 AI가 포스트를 작성할 때 front matter를 빠뜨리지 않고, 문체를 일관되게 유지하며, 빌드 에러를 사전에 방지해준다.

프로젝트 성격에 따라 `backend-rule.md`, `frontend-rule.md` 등으로 파일을 분리해서 관리하면 더 체계적이다. 예를 들어 Spring Boot 프로젝트라면 아래와 같은 Rule을 별도로 만들 수 있다.

```markdown
# Spring Boot 프로젝트 Rule

## 코드 규칙
- Java 17 이상 문법을 사용한다.
- 컨트롤러는 @RestController, 서비스는 @Service 어노테이션을 사용한다.
- DTO와 Entity를 반드시 분리한다.
- API 응답은 ResponseEntity로 감싸서 반환한다.

## 테스트 규칙
- 서비스 레이어는 반드시 단위 테스트를 작성한다.
- 테스트 메서드명은 한국어로 작성한다. (예: `로그인_성공_테스트()`)

## 보안 규칙
- 비밀번호, API 키 등 민감 정보는 코드에 하드코딩하지 않는다.
- application.yml의 민감 정보는 환경변수로 대체한다.
```

### 📌 Rule 작성 시 주의할 점
<hr>

**1. 너무 많은 규칙을 넣지 말 것**

Rule이 너무 길어지면 AI가 모든 규칙을 동시에 지키려다 오히려 답변 품질이 떨어질 수 있다. **핵심 규칙 10~15개 이내**로 유지하는 것을 권장한다.

**2. 서로 충돌하는 규칙을 피할 것**

"코드를 최대한 간결하게 작성해라"와 "모든 함수에 상세한 주석을 달아라"를 동시에 넣으면 AI가 혼란스러워한다. 규칙 간 우선순위가 명확해야 한다.

**3. 구체적으로 작성할 것**

"코드를 잘 작성해라" 같은 모호한 규칙은 효과가 없다. "함수명은 camelCase, 컨테이너 컴포넌트는 PascalCase로 작성한다" 처럼 구체적일수록 AI가 정확하게 따른다.

**4. 주기적으로 리뷰할 것**

Rule은 한번 작성하고 끝이 아니다. 프로젝트가 진행되면서 불필요한 규칙은 제거하고, 새로운 컨벤션이 생기면 추가하는 식으로 관리해야 한다.


## ✅ Step 5: Memory Bank로 AI의 기억력 보완

AI 코딩 도우미의 가장 큰 약점은 **대화가 길어지면 앞에서 한 얘기를 까먹는다**는 것이다. 또한 프로젝트 맥락을 모르고 엉뚱한 답변을 하는 할루시네이션도 문제다.

이걸 해결하기 위해 **Memory Bank** 기법을 적용했다. 원리는 단순하다.

1. 작업 내용을 특정 폴더에 마크다운으로 기록
2. AI가 답변하기 전에 항상 그 폴더를 먼저 읽도록 Rule에 명시

### 📌 구축 방법
<hr>

**1) 메모리 폴더 생성**

```bash
mkdir .context-memory
```

**2) Rule에 참조 규칙 추가**

```markdown
## 컨텍스트 확인
- 답변 전에 반드시 `.context-memory` 폴더의 모든 파일을 읽고
  프로젝트 맥락을 파악한 후 답변한다.

## 작업 기록
- 기능 단위 작업이 완료되면 `.context-memory`에 요약 문서를 생성한다.
- 파일명: `YYYY-MM-DD_작업요약.md`
```

**3) 실제 메모리 파일 예시**

```markdown
# 2025-06-12 블로그 SEO 개선

## 작업 내용
- 138개 포스트에 description 필드 추가
- sitemap.xml에 정적 페이지 포함하도록 수정
- og_image, title_separator 설정

## 주의사항
- SCSS 파일에 한글 주석 시 @charset "utf-8" 필수
- 빌드 시 RUBYOPT="-EUTF-8" 환경변수 필요
```

이렇게 해두면 새 대화를 시작해도 AI가 이전 작업 맥락을 파악한 상태에서 답변하기 때문에 **일관성 있는 작업**이 가능하다. 실제로 이 블로그의 모바일 반응형 수정, SEO 개선, 이미지 최적화 등 여러 작업을 진행하면서 Memory Bank 덕분에 맥락이 끊기지 않고 연속적으로 작업할 수 있었다.


## ✅ 마무리

지금까지 세팅한 내용을 정리하면 다음과 같다.

| 단계 | 내용 | 설정 위치 |
|------|------|-----------|
| Step 1 | Amazon Q 설치 + 로그인 | VS Code 마켓플레이스 |
| Step 2 | MCP 서버 연동 | `.amazonq/agents/default.json` |
| Step 3 | 자동 승인 설정 | MCP 도구 관리 UI |
| Step 4 | Rule 커스터마이징 | `.amazonq/rules/*.md` |
| Step 5 | Memory Bank 구축 | `.context-memory/*.md` |

솔직히 Cursor의 완성도 높은 UX를 100% 대체한다고 말하기는 어렵다. 하지만 **무료로, 기존 VS Code를 유지하면서, 내 워크플로우에 맞게 커스터마이징**할 수 있다는 점에서 Amazon Q는 충분히 매력적인 선택지다. 특히 AWS 환경에서 개발하는 사람이라면 더더욱 추천한다.

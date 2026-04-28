---
permalink: /mcp-model-context-protocol/
published: true
title: "[AI Agent] MCP(Model Context Protocol)란? 개념부터 실습까지 총정리"
date: 2025-06-12 18:00:00
toc: true
toc_sticky: true
toc_label: "MCP 완벽 가이드"
description: "MCP(Model Context Protocol)란 무엇인지 개념부터 아키텍처, 핵심 기능(Resources, Tools, Prompts), Amazon Q Developer CLI 연동 실습까지 총정리한다."
categories:
- AI Agent
tags:
- MCP
- Model Context Protocol
- AI 에이전트
- Amazon Q
- Claude
- Anthropic
- LLM
- JSON-RPC
last_modified_at: 2025-06-12
---

> 📚 **관련 포스팅**
> - [Amazon Q Developer로 VS Code AI 코딩 환경 구축하기](/vscode-amazon-q-ai-coding/) - MCP, Rule, Memory Bank 활용 실전 가이드

최근 AI 모델은 단순한 대화를 넘어 직접 코드를 수정하고, 데이터베이스를 조회하며, API를 호출하는 **'AI 에이전트'**로 진화하고 있다. 하지만 지금까지는 각 AI 서비스(Claude, Cursor, Amazon Q 등)마다 외부 도구를 연결하는 방식이 제각각이어서 개발 효율이 떨어졌다.

예를 들어 PostgreSQL에 접근하는 기능을 만들었다고 하자. Claude에서 쓰려면 Claude용 플러그인을 따로 만들고, Amazon Q에서 쓰려면 또 Amazon Q용으로 만들어야 했다. 같은 기능인데 AI 서비스가 바뀔 때마다 처음부터 다시 개발해야 하는 비효율이 반복됐다.

이 문제를 해결하기 위해 **Anthropic**이 제안한 새로운 표준이 바로 **MCP(Model Context Protocol)**이다.


## ✅ MCP(Model Context Protocol)란?

MCP는 **AI 애플리케이션(Host)이 LLM에게 컨텍스트(데이터, 도구, 프롬프트)를 제공하는 방식을 표준화하는 개방형 프로토콜**이다. 2024년 11월 Anthropic이 오픈소스로 공개했으며, 현재 Claude, Amazon Q, Cursor, Zed 등 주요 AI 도구들이 지원하고 있다.

### 📌 핵심 개념: "AI계의 USB-C 포트"
<hr>

과거에는 스마트폰마다 충전기 규격이 달랐지만, 이제는 USB-C 하나로 모든 기기를 연결한다. MCP도 마찬가지이다.

- **기존 방식**: 서비스별로(Claude용, Amazon Q용) 커스텀 연동 코드를 각각 개발해야 함
- **MCP 도입 후**: **하나의 MCP 서버만 구축하면**, MCP를 지원하는 모든 AI 호스트에서 즉시 데이터를 읽고 도구를 사용할 수 있음

예를 들어, PostgreSQL 데이터베이스에 접근하는 MCP 서버를 하나 만들어두면, Claude Desktop에서도, Amazon Q Developer에서도, Cursor에서도 동일한 서버를 통해 DB를 조회할 수 있다. AI 서비스가 바뀌어도 MCP 서버는 그대로 재사용된다.

### 📌 MCP가 없었다면?
<hr>

MCP 이전에는 AI에게 외부 데이터나 도구를 연결하려면 각 서비스의 플러그인 시스템이나 Function Calling을 직접 구현해야 했다. 문제는 이 방식이 서비스마다 달랐다는 것이다.

| 상황 | MCP 이전 | MCP 이후 |
|------|----------|----------|
| DB 연동 기능 개발 | Claude용, GPT용, Q용 각각 개발 | MCP 서버 1개로 모두 지원 |
| AI 서비스 교체 | 연동 코드 전면 재작성 | MCP 서버 그대로 유지 |
| 새 도구 추가 | 각 서비스 API 문서 별도 학습 | MCP 표준 스펙 하나만 학습 |
| 보안 인증 | 서비스마다 다른 방식 | OAuth 2.1 표준 통일 |


## ✅ MCP 아키텍처 및 통신 방식

MCP는 **JSON-RPC 2.0** 기반의 클라이언트-서버 아키텍처를 따른다.

### 📌 주요 구성 요소
<hr>

| 구성 요소 | 역할 | 예시 |
|-----------|------|------|
| **MCP Host** | 사용자가 사용하는 실제 AI 서비스 | Amazon Q Developer, Claude Desktop, Cursor |
| **MCP Client** | 호스트 내부에서 서버와 통신을 담당하는 인터페이스 | 호스트에 내장됨 |
| **MCP Server** | 실제 데이터 소스를 가지고 있으며, 모델이 쓸 수 있게 제공하는 프로그램 | DB 서버, 파일 시스템 서버, API 서버 |

동작 흐름을 정리하면 다음과 같다.

1. **사용자**가 Host(예: Amazon Q)에 질문을 입력
2. **Host 내부의 Client**가 질문을 분석하여 필요한 MCP Server를 판단
3. **MCP Server**에 도구 실행을 요청 (JSON-RPC)
4. **Server**가 실제 데이터 소스(DB, API 등)에서 결과를 가져와 반환
5. **모델**이 반환된 컨텍스트를 바탕으로 최종 응답을 생성

이 구조에서 핵심은 **모델이 직접 DB나 API에 접근하지 않는다**는 점이다. 모델은 MCP Server에 "이 작업을 해줘"라고 요청할 뿐이고, 실제 실행은 MCP Server가 담당한다. 덕분에 보안 정책을 MCP Server 레벨에서 일괄 관리할 수 있다.

### 📌 전송 계층 (Transport Layer)
<hr>

서버와 클라이언트가 연결되는 방식은 크게 세 가지로 발전하고 있다.

| 방식 | 설명 | 사용 환경 | 특징 |
|------|------|-----------|------|
| **STDIO** | 클라이언트가 서버를 하위 프로세스로 실행하여 표준 입출력으로 통신 | 로컬 환경 | 설정 간단, 로컬 전용 |
| **HTTP SSE** | 서버가 클라이언트로 데이터를 스트리밍 (Server-Sent Events) | 원격 서버 | 단방향 스트리밍 |
| **Streamable HTTP** (최신) | 단일 엔드포인트를 통해 양방향 통신 | 원격 서버 (권장) | 세션 관리, 재연결 지원 |

<p class="notice--info">
💡 최신 <strong>Streamable HTTP</strong> 방식이 도입되면서 기존 SSE 방식의 단점(단방향 통신, 세션 유지 어려움)이 크게 개선되었다. 새로운 MCP 서버를 구축한다면 Streamable HTTP 방식을 권장한다.
</p>


## ✅ MCP 서버의 3가지 핵심 기능

MCP 서버는 모델에게 세 가지 형태의 능력을 부여한다.

| 기능 | 설명 | 예시 |
|------|------|------|
| **Resources** (리소스) | 모델이 읽을 수 있는 정적/동적 데이터 | 로컬 로그 파일, DB 테이블 내용, API 응답 데이터 |
| **Prompts** (프롬프트) | 특정 작업을 위해 미리 정의된 템플릿 | "코드 리뷰를 위한 프롬프트", "로그 분석용 가이드" |
| **Tools** (툴즈) | 모델이 직접 실행을 요청할 수 있는 함수 | "파일 읽기/쓰기", "AWS 문서 검색", "SQL 쿼리 실행" |

### 📌 Resources (리소스)
<hr>

리소스는 MCP 서버가 클라이언트에게 제공하는 **데이터**이다. 모델이 직접 읽어올 수 있는 정보를 제공하는 역할로, 크게 두 가지로 나뉜다.

- **정적 리소스**: 파일 내용, 설정 정보처럼 변하지 않는 데이터
- **동적 리소스**: DB 쿼리 결과, API 응답처럼 요청할 때마다 달라지는 데이터

예를 들어 서버 로그 파일을 리소스로 등록해두면, 모델이 "최근 에러 로그를 분석해줘"라는 요청을 받았을 때 MCP Server를 통해 로그 파일을 직접 읽어와 분석할 수 있다. 개발자가 로그를 복사해서 붙여넣을 필요가 없다.

### 📌 Prompts (프롬프트)
<hr>

프롬프트는 **재사용 가능한 프롬프트 템플릿**이다. 특정 작업에 최적화된 지시문을 미리 정의해두고, 필요할 때 불러와 사용하는 방식이다.

예를 들어 코드 리뷰 프롬프트를 이렇게 정의해둘 수 있다.

```
코드 리뷰 시 다음 항목을 반드시 확인한다:
1. 보안 취약점 (SQL Injection, XSS 등)
2. 성능 이슈 (N+1 쿼리, 불필요한 반복 등)
3. 코드 가독성 및 네이밍 컨벤션
4. 예외 처리 누락 여부
5. 테스트 커버리지
```

이 프롬프트를 MCP Server에 등록해두면, "이 코드 리뷰해줘"라고 요청할 때마다 위 기준을 자동으로 적용한 리뷰를 받을 수 있다. 팀 전체가 동일한 리뷰 기준을 공유하는 효과도 있다.

### 📌 Tools (툴즈)
<hr>

툴즈는 모델이 **직접 실행을 요청할 수 있는 함수**이다. 세 가지 기능 중 가장 강력하고 많이 사용된다. 모델이 단순히 데이터를 읽는 것을 넘어, 실제 작업을 수행할 수 있게 해준다.

대표적인 툴즈 예시:

```
- 파일 시스템: 파일 읽기, 쓰기, 이동, 삭제
- 터미널: 쉘 명령어 실행
- 데이터베이스: SQL 쿼리 실행
- 웹 검색: 실시간 정보 검색
- API 호출: 외부 서비스 연동
```

예를 들어 "이 프로젝트의 모든 TODO 주석을 찾아서 이슈 목록으로 정리해줘"라고 요청하면, 모델이 파일 시스템 툴을 사용해 프로젝트 전체를 탐색하고, 결과를 마크다운 파일로 저장하는 것까지 자동으로 처리한다.

<p class="notice--warning">
⚠️ Tools는 모델이 <strong>자동으로 실행</strong>할 수 있기 때문에, 보안에 주의해야 한다. 민감한 작업(파일 삭제, DB 수정 등)에는 사용자 승인 단계를 추가하는 것이 좋다.
</p>


## ✅ MCP 서버 연동 실습 (Amazon Q Developer CLI)

실제로 Amazon Q Developer CLI에 **AWS 공식 문서 검색 MCP 서버**를 연동하는 방법을 알아보겠다.

### 📌 사전 준비
<hr>

- Python 3.10 이상 설치
- uv 설치 (빠른 파이썬 패키지 관리자)

```bash
brew install uv
```

- Amazon Q Developer CLI 설치 및 로그인

```bash
# Amazon Q CLI 설치 (Mac)
brew install amazon-q

# 로그인
q login
```

### 📌 단계 1: MCP 설정 파일 작성
<hr>

Amazon Q는 설정된 경로의 `mcp.json` 파일을 읽어 서버를 자동 로드한다.

글로벌 설정 경로: `~/.aws/amazonq/mcp.json` (Mac/Linux 기준)

```json
{
  "mcpServers": {
    "aws-docs": {
      "command": "uvx",
      "args": ["@awslabs/aws-documentation-mcp-server"]
    }
  }
}
```

`uvx`는 패키지를 설치하지 않고도 즉석에서 실행해주는 명령어로, 최신 버전의 MCP 서버를 항상 유지하기 좋다. `npm`의 `npx`와 비슷한 개념이다.

### 📌 단계 2: 연동 확인
<hr>

1. 터미널에서 `q chat`을 입력하여 대화창을 실행
2. 상단에 **"MCP server initialized"** 메시지가 뜨는지 확인
3. `/tools` 명령어를 입력하여 사용 가능한 도구 목록을 확인

```
/tools

Available tools:
  aws-docs/search_documentation
  aws-docs/read_documentation
```

이렇게 나오면 성공이다.

### 📌 단계 3: 실제 활용
<hr>

모델에게 구체적으로 질문해 보자.

```
"AWS 문서를 검색해서 Amazon ECS Service Connect의 최신 보안 권장 사항을 정리해줘."
```

**작동 원리:**

1. **모델 분석**: 질문을 이해하고 `aws-docs` 서버의 `search_documentation` 도구가 필요하다고 판단
2. **도구 호출**: Amazon Q가 MCP 서버에 검색 명령을 전송
3. **결과 수신**: 서버가 AWS 공식 문서를 마크다운 형태로 변환하여 모델에게 전달
4. **최종 응답**: 모델이 수집된 최신 컨텍스트를 바탕으로 정확한 답변을 생성

MCP 없이 같은 질문을 하면 모델이 학습 데이터 기반으로 답변하기 때문에 최신 정보가 반영되지 않거나 할루시네이션이 발생할 수 있다. MCP를 통해 실시간으로 공식 문서를 참조하면 훨씬 정확한 답변을 얻을 수 있다.


## ✅ 다양한 MCP 서버 예시

MCP 생태계에는 이미 다양한 오픈소스 서버가 존재한다.

| MCP 서버 | 기능 | 설정 |
|----------|------|------|
| **aws-documentation** | AWS 공식 문서 검색 | `uvx @awslabs/aws-documentation-mcp-server` |
| **postgres** | PostgreSQL DB 조회 | `uvx @modelcontextprotocol/server-postgres` |
| **filesystem** | 로컬 파일 읽기/쓰기 | `npx @modelcontextprotocol/server-filesystem` |
| **github** | GitHub 이슈/PR 관리 | `uvx @modelcontextprotocol/server-github` |
| **slack** | Slack 메시지 검색/전송 | `uvx @modelcontextprotocol/server-slack` |
| **brave-search** | 실시간 웹 검색 | `npx @modelcontextprotocol/server-brave-search` |

여러 MCP 서버를 동시에 연동하는 것도 가능하다. 예를 들어 AWS 문서 검색과 파일 시스템을 함께 연동하면, AWS 공식 문서를 참조해서 코드를 작성하고 파일로 저장하는 것까지 한 번에 처리할 수 있다.

```json
{
  "mcpServers": {
    "aws-docs": {
      "command": "uvx",
      "args": ["@awslabs/aws-documentation-mcp-server"]
    },
    "filesystem": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "/Users/myname/projects"]
    }
  }
}
```

<p class="notice--info">
💡 <a href="https://smithery.ai">Smithery.ai</a>에서 다른 개발자들이 만든 다양한 MCP 서버를 검색하고 바로 사용할 수 있다. 현재 수백 개 이상의 MCP 서버가 등록되어 있다.
</p>


## ✅ 기존 방식 vs MCP 비교

| 구분 | 기존 방식 | MCP |
|------|-----------|-----|
| 연동 방식 | AI 서비스마다 커스텀 코드 개발 | 표준 프로토콜로 한 번만 구축 |
| 재사용성 | 서비스 변경 시 재개발 필요 | 모든 MCP 호스트에서 즉시 사용 |
| 보안 | 서비스별 인증 방식 상이 | OAuth 2.1 기반 표준 인증 |
| 확장성 | 로컬 도구 위주 | 로컬 + 원격 인프라 통합 |
| 생태계 | 폐쇄적 | 오픈소스 커뮤니티 활발 |


## ✅ 결론 및 향후 전망

MCP는 **"모델과 데이터의 분리"**를 가능하게 한다. 개발자는 모델이 바뀔 때마다 연동 코드를 고칠 필요 없이, 표준화된 MCP 서버 하나만 잘 관리하면 된다.

**주요 이점 요약:**

- **보안**: OAuth 2.1 기반 인증으로 안전한 데이터 접근
- **재사용성**: Claude에서 쓰던 툴을 Amazon Q에서도 그대로 사용
- **확장성**: 로컬뿐만 아니라 원격 인프라까지 하나의 인터페이스로 통합

앞으로 더 많은 오픈소스 MCP 서버들이 등장할 것이며, 이는 AI 에이전트 생태계를 폭발적으로 성장시키는 촉매제가 될 것이다.

---
permalink: /2023-02-22-HTTP 상태코드(1) - 2xx ( Successful )/
title: "[HTTP] HTTP 상태코드(1) - 2xx ( Successful )"
date: 2023-02-22 15:00:00
toc: true
toc_sticky: true
toc_label: "HTTP"
categories:
- HTTP
tags:
- HTTP
- HTTP 상태코드
---
<br><br>

<br><br>

## ✅ HTTP 상태코드
**HTTP 상태코드는 클라이언트가 보낸 요청의 처리 상태를 응답에서 알려주는 기능을 의미합니다.** 1xx 부터 5xx 까지 존재하고 각 상태코드 마다 처리 상태에 대한 결과를 알려줍니다.

- 1xx ( Informational ) : 요청이 수신되어 처리중 ( 거의 사용 x )
- 2xx ( Successful ) : 요청 정상 처리
- 3xx ( Redirection ) : 요청을 완료하면 추가 행동이 필요
- 4xx ( Client Error ) : 클라이언트 오류, 잘못된 문법등으로 서버가 요청을 수행할 수 없음
- 5xx ( Server Error ) : 서버 오류, 서버가 정상 요청을 처리하지 못함.

이번 포스팅은 2xx 상태코드에 대해서 알아볼텐데, 그전에 실무에서 거의 사용되지 않는 1xx 상태코드에 대해 간단하게 먼저 알아보고 2xx 상태코드를 이해해보도록 하겠습니다.

<br><br><br><br><br><br>

## ✅ 1xx 상태 코드 ( Informational )

1xx 번대의 상태 코드들은 **요청이 수신되어 처리중** 이라는 의미를 가집니다.

- 100 : Continue -> 처리가 되었으니 다음으로 진행하라
- 101 : Switching Protocols -> 서버가 프로토콜을 전환중
- 102 : Processing -> 서버가 요청을 수신하였으며 이를 처리중
- 103 : Early Hints -> 리소스에 대한 힌트를 제공하여 리소스를 사전 로드하게 함

이러한 상태코드들이 있지만 실무에서는 거의 사용되지 않으므로 한번 보고 넘어가도록 하자.

<br><br><br><br><br><br>

## ✅ 2xx 상태 코드 ( Successful )
2xx 상태코드들은 **클라이언트의 요청을 성공적으로 처리했음**을 의미합니다. 단순히 요청에 대한 성공을 의미하지만 어떠한 행위에 대한 성공인지, 응답을 받고 클라이언트가 어떤 행동을 취할것인지 등 결정하기에 따라 다른 상태코드를 가집니다.

- 200 : OK
- 201 : Created
- 202 : Accepted
- 204 : No Content
- ...


<br><br><br><br><br><br>
## ✅ 200 OK
**상태코드 200은 클라이언트의 요청을 서버가 정상적으로 처리했음을 의미합니다.**


<p align="middle">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/2xx1.png?raw=true">
</p>

<br><br><br><br><br><br>
## ✅ 201 Created
**상태코드 201은 요청에 성공해서 새로운 리소스가 생성됐음을 의미합니다.**

<br><br><br><br><br><br>

## ✅ 202 Accepted
**상태코드 202는 요청이 접수되었으나 처리가 완료되지 않았음을 의미합니다.**

- Batch 처리 같은 곳에서 사용
- 예) 요청 접수 후 1시간 뒤에 배치 프로세스가 요청을 처리한다.

<br><br><br><br><br><br>

## ✅ 204 No Content
**상태코드 204는 서버가 요청을 성공적으로 수행했지만, 응답 페이로드 본문에 보낼 데이터가 없음을 의미합니다.**

- 예) 웹 문서 편집기에서 save 버튼
- save 버튼의 결과로 아무 내용이 없어도 된다.
- save 버튼을 눌러도 같은 화면을 유지해야 한다.
- 결과 내용이 없어도 204 메시지만으로 성공을 인식할 수 있다.


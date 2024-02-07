---
permalink: /2023-02-24-HTTP 상태코드(3) - 4xx ( Client Error ), 5xx ( Server Error )
title: "[HTTP] HTTP 상태코드(3) - 4xx ( Client Error ), 5xx ( Server Error )"
date: 2023-02-24 15:00:00
toc: true
toc_sticky: true
toc_label: "HTTP"
categories:
- HTTP
tags:
- HTTP
- HTTP 상태코드
---
<br><br><br><br>

## ✅ HTTP 상태코드
**HTTP 상태코드는 클라이언트가 보낸 요청의 처리 상태를 응답에서 알려주는 기능을 의미합니다.** 1xx 부터 5xx 까지 존재하고 각 상태코드 마다 처리 상태에 대한 결과를 알려줍니다.

- 1xx ( Informational ) : 요청이 수신되어 처리중 ( 거의 사용 x )
- 2xx ( Successful ) : 요청 정상 처리
- 3xx ( Redirection ) : 요청을 완료하면 추가 행동이 필요
- 4xx ( Client Error ) : 클라이언트 오류, 잘못된 문법등으로 서버가 요청을 수행할 수 없음
- 5xx ( Server Error ) : 서버 오류, 서버가 정상 요청을 처리하지 못함.

이번 포스팅은 **4xx, 5xx 상태코드**에 대해서 알아보도록 하겠습니다.

<br><br><br><br><br><br>

## ✅ 4xx 상태 코드 ( Client Error )

- 클라이언트가 잘못된 요청을 보내서 서버가 이를 수행할 수 없음.
- 오류의 원인이 클라이언트에게 있음.
- 클라이언트가 잘못된 요청 및 데이터를 보내고 있기 때문에 똑같은 재시도는 계속 실패함.


<br><br><br><br><br><br>

## ✅ 400 Bad Request
- 클라이언트가 잘못된 요청을 해서 서버가 이를 처리할 수 없음
- 요청 구문, 메시지 등의 오류
- 예 ) 요청파라미터가 잘못되었거나, API스펙이 맞지 않을때 발생

<br><br><br><br><br><br>

## ✅ 401 Unauthorized
- 클라이언트가 해당 리소스에 대한 인증이 필요함
- 인증(Authentication) 되지 않음
- 401 오류 발생시 응답에 WWW-Authenticate 헤더와 함께 인증방법 설명

<br><br><br><br><br><br>

## ✅ 403 Forbidden
- 서버가 요청을 이해했지만 승인을 거부함
- 주로 인증 자격 증명은 있지만, 접근 권한이 불충분한 경우
- 예 ) admin 등급이 아닌 사용자가 로그인은 했지만, admin 리소스에 접근하는 경우

<br><br><br><br><br><br>

## ✅ 404 Not Found
- 요청 리소스를 찾을 수 없음
- 요청 리소스가 서버에 없음
- 또는 클라이언트가 권한이 부족한 리소스에 접근할 때 해당 리소스를 숨기고 싶을때


<br><br><br><br><br><br>

## ✅ 5xx 상태 코드 ( Server Error )

- 서버 문제로 오류가 발생한 경우 5xx 에러를 반환한다.
- 서버 문제기 때문에 재시도하면 성공할 수도 있다. ( 복구가 된 경우 )

<br><br><br><br><br><br>

## ✅ 500 Internal Server Error
- 서버 내부 문제로 오류 발생
- 보통 애매하면 500 오류를 반환한다.


<br><br><br><br><br><br>

## ✅ 503 Service Unavailable
- 서비스 이용 불가
- 서버가 일시적인 과부하 또는 예정된 작업으로 잠시 요청을 처리할 수 없음
- Retry-After 헤더필드로 얼마뒤에 복구되는지 보낼 수 있음.

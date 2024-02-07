---
permalink: /2023-02-23-HTTP 상태코드(2) - 3xx ( Redirection )
title: "[HTTP] HTTP 상태코드(2) - 3xx ( Redirection )"
date: 2023-02-23 15:00:00
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

이번 포스팅은 **3xx 상태코드**에 대해서 알아보도록 하겠습니다.

<br><br><br><br><br><br>

## ✅ 3xx 상태 코드 ( Redirection )

3xx 번대의 상태 코드들은 **요청을 완료하기 위해 유저 에이전트의 추가적인 조치가 필요함을 의미**합니다.

- 300 Multiple Choices
- 301 Moved Permanently
- 302 Found
- 303 See Other
- 304 Not modified
- 307 Temporary Redirect
- 308 Permanent Redirect


<br><br><br><br><br><br>

## ✅ Redirect란 ?
요청에 대한 응답 결과에 Location 헤더가 있으면, **Location 위치로 자동 이동**

<p align="left">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/redirect.png?raw=true">
</p>

<br>

- **영구 리다이렉션** : 특정 리소스의 URI가 영구적으로 이동
	- 예) /members -> /users
	- 예) /event -> /new-event

- **일시 리다이렉션** : 일시적인 변경
	- 주문 완료 후 주문 내역 화면으로 이동
	- PRG : Post/Redirect/Get

- **특수 리다이렉션**
	- 결과 대신 캐시를 사용


<br><br><br><br><br><br>

## ✅ 영구 리다이렉션 - 301, 308

- 리소스의 URI가 영구적으로 이동
- 원래의 URL을 사용하지 않고 검색엔진에서도 이를 인지한다.
- **301 Moved Permanently**
	- **리다이렉트시 요청 메서드가 GET으로 변하고, 본문이 제거될 수 있음**
- **308 Permanent Redirect**
	- 301과 기능은 같음
	- **리다이렉트시 요청 메서드와 본문을 그대로 유지함**


<br><br><br><br><br><br>

## ✅ 301 Moved Permanently

- 리다이렉트 요청시 요청메서드가 **GET**으로 변하고, **본문이 제거될 수 있다.**
- **301 Moved Permanently는 안전하지 않은 HTTP URL을 HTTPS로 변경하는데 자주 사용된다.**

<br>

<p align="left">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/redirect2.png?raw=true">
</p>

<br>

위 그림을 보면 HTTP 요청시 POST요청과 본문이 존재한다. 그리고 요청에 대한 응답은 301 Moved Permanently로 /new-event로 리다이렉트 하는 응답을 반환했다.

때문에 HTTP 요청은 GET 메서드로 변경되었고, 본문은 제거 되었다.

<br><br><br><br><br><br>

## ✅ 308 Permanent Redirect

- 301 Moved Permanently와 기능적으로는 같다.
- 리다이렉트시 요청 메서드와 본문이 그대로 유지된다.

<br>

<p align="left">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/redirect3.png?raw=true">
</p>


<br><br><br><br><br><br>

## ✅ 일시적인 리다이렉션 - 302, 303, 307

**리소스의 URI가 일시적으로 변경된다.**


- **302 Found**
	- 리다이렉트시 요청 메서드가 GET으로 변하고, 본문이 제거될 수 있다.
- **303 See Other**
	- 302와 기능은 같고 요청메서드가 GET으로 변경된다.
- **307 Temporary Redirect**
	- 302와 기능은 같으나 요청메서드와 본문을 반드시 유지해야 한다.


<br><br><br><br><br><br>

## ✅ RPG : Post/Redirect/Get

**만약 POST요청으로 물건을 주문한뒤에 웹 브라우저를 새로고침하면 어떻게 될까? 새로고침은 POST요청을 다시 요청하게 되고 중복 주문이 발생한다.**

**때문에 POST요청 후에 GET요청으로 변경해주는 Redirect 작업이 필요하다!**

<br>

### 📌 RPG 적용 전
<p align="left">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/rpg1.png?raw=true">
</p>

1. POST요청으로 주문작업 실행
2. 주문 데이터 1개 저장
3. 그냥 200 OK로 성공 반환
4. 결과 화면에서 웹페이지 새로고침
5. 다시 POST요청으로 주문작업 실행
6. 같은 주문 데이터 1개 또 저장
7. 주문이 중복되는 문제 발생!

<br><br>
### 📌 RPG 적용 후
<p align="left">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/rpg2.png?raw=true">
</p>

1. POST 요청으로 주문작업 실행
2. 주문 데이터 1개 저장
3. 302 Found로 임시적인 리다이렉션 반환
4. 결과화면에서 웹페이지 새로고침
5. 기존 POST요청에서 GET요청으로 HTTP 메서드 변경
6. 주문 데이터 조회 요청
7. 주문 데이터 조회 반환

<br><br><br><br><br><br>

## ✅ 실무 적용 방법
- **임시 리다이렉션 정리**
	- 302 Found -> GET으로 변할 수 있음
	- 303 See Other -> 메서드가 GET으로 변경
	- 307 Temporart Redirect -> 메서드가 변경될 수 없음

- **역사**
	- 처음 302 스펙의 의도는 HTTP 메서드를 유지하는것
	- 그러나 웹브라우저들이 대부분 GET으로 바꿔버림
	- 그래서 모호한 302대신 명확한 303,307이 등장

- **현실**
	- 307, 303을 권장하지만 현실적으로 이미 많은 애플리케이션 라이브러리들이 302를 기본값으로 사용중
	- 자동 리다이렉션시에 GET으로 변해도 되면 그냥 302 사용해도 문제없음

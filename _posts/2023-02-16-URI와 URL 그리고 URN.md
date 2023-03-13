---
permalink: /2023-02-16-URI와 URL 그리고 URN/
title: "[HTTP] URI와 URL 그리고 URN"
date: 2023-02-16 13:00:00
toc: true
toc_sticky: true
toc_label: "HTTP"
categories:
- HTTP
tags:
- HTTP
- URI
- URL
- URN
---
<br><br><br>

## ✅ URI (Uniform Resource Identifier)

**인터넷 자원을 나타내는 고유 식별자**.

**URI는 로케이터(Locator), 이름(Name) 또는 둘다로 분류될 수 있다.**

<p align="left">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/uri1.png?raw=true">
</p>


<br><br>

### 📌 **URI 의미**
- **Uniform : 리소스를 식별하는 통일된 방식**
- **Resource : 자원, URI로 식별할 수 있는 모든 것(제한없음)**
- **Identifier : 다른 항목과 구분하는데 필요한 정보**

<br><br>


### 📌 **URL과 URN의 구조**
<p align="left">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/uri2.png?raw=true">
</p>

- **URL : 리소스가 있는 위치를 지정**

- **URN : 리소스에 이름을 부여**

- **위치는 변할 수 있지만 이름은 변하지 않는다.**

- **그러나 URN으로 실제 리소스를 찾을 수 있는 방법이 보편화 되지 않아 URL을 주로 사용한다.**

- **때문에 URI와 URL을 혼용해서 부르기도 한다.**


<br><br><br><br><br><br>

## ✅ URL 분석

### 📌 전체 문법

> **scheme://[userinfo@]host[:port][/path][?query][#fragment]**
> 
> **https://www.google.com:443/search?q=hello&hl=ko**

<br>

- **프로토콜 (https)**

- **호스트명 (www.google.com)**

- **포트번호 (443)**

- **경로 (/search)**

- **쿼리 파라미터 (q=hello&hl=ko)**


<br><br>

### 📌 프로토콜 ( scheme )
> **scheme**://[userinfo@]host[:port][/path][?query][#fragment]
> 
> **https:**//www.google.com:443/search?q=hello&hl=ko

- 프로토콜 : 어떤 방식으로 자원에 접근할 것인가 하는 약속 규칙 ex) http, https, ftp
- http는 80포트, https는 443포트를 주로 사용한다.

<br><br>

### 📌 사용자정보 ( userinfo )
> scheme://**[userinfo@]**host[:port][/path][?query][#fragment]
> 
> https://www.google.com:443/search?q=hello&hl=ko

- URL에 사용자정보를 포함해서 인증할때 사용하나 거의 쓰이지 않는다.

<br><br>

### 📌 호스트 ( host )

> scheme://[userinfo@]**host**[:port][/path][?query][#fragment]
> 
> https://**www.google.com**:443/search?q=hello&hl=ko

- 호스트명
- 도메인명 또는 IP주소를 직접 사용가능하다.

<br><br>

### 📌 포트번호 ( port )
> scheme://[userinfo@]host**[:port]**[/path][?query][#fragment]
> 
> https://www.google.com:**443**/search?q=hello&hl=ko

- 포트 번호를 의미한다.
- 일반적으로는 생략하고, http는 80, https는 443을 사용한다.

<br><br>

### 📌 경로 ( path )
> scheme://[userinfo@]host[:port]**[/path]**[?query][#fragment]
> 
> https://www.google.com:443 **/search**?q=hello&hl=ko

- 리소스 경로를 의미하며 계층적 구조를 가진다.
- ex ) /home/file1.jpg


<br><br>

### 📌 쿼리파라미터 ( query )
> scheme://[userinfo@]host[:port][/path]**[?query]**[#fragment]
> 
> https://www.google.com:443/search?**q=hello&hl=ko**

- key=value 형태를 가지는 쿼리파라미터이다.
- ?로 시작하고, &로 추가가 가능하다.


<br><br><br>

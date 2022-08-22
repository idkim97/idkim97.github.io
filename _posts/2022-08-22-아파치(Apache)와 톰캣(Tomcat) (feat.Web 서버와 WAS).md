---
permalink: /2022-08-22-아파치(Apache)와 톰캣(Tomcat)/
published: true
title: "[Web & Server] 아파치(Apache)와 톰캣(Tomcat) (feat. Web 서버와 WAS)"
date: 2022-08-22 18:00:00
toc: true
toc_sticky: true
toc_label: "아파치(Apache)와 톰캣(Tomcat)"
categories:
- Web & Server
tags:
- Apache
- Tomcat
- WAS
- Web 서버
- 개발상식
- Server
- Web
---
<br><br><br>

오늘은 **아파치와 톰캣**이 무엇인지, 그 차이점은 뭐가있는지, 그리고 아파치가 속해있는 **Web 서버**와 톰캣이 속해있는 **WAS(Web Application Server)** 에 대해 알아보자.

<br><br><br>

## ⭐ 아파치 톰캣 (Apache Tomcat)
<p align="left">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/apache1.png?raw=true">
</p>
스프링 뿐만 아니라 웹개발을 공부하는 사람이라면 **아파치 톰캣(Apache Tomcat)** 이라는 서버를 많이 들어봤을 것이다. 우리가 사용하는 많은 웹페이지는 아파치 톰캣을 서버로 사용하고, 세계에서 가장 많이 사용중인 WAS중 하나이다. 

**정적페이지를 처리하는 아파치(Web Server)** 와 **동적 페이지를 처리하는 톰캣 (WAS)** 이 합쳐진 구조로 각각의 기능을 일부 가져와서 제공하는 형태이기 때문에 합쳐서 부른다. **아파치, 톰캣**과 함께 **웹서버**와 **WAS**에 대해 알아보자.

<br><br><br>

## ⭐ Web 서버
Web서버는 보통 **HTTP 서버**를 의미한다. HTTP 서버는 **URL(웹주소)** 및 **HTTP(프로토콜 주소)** 를 이해하는 소프트웨어 이다. HTTP 서버는 저장하는 웹 사이트의 **도메인 이름**을 통해 액세스 할 수 있으며 호스팅 된 웹 사이트의 콘텐츠를 사용자의 장치로 전달한다. 

<p align="left">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/servlet2.png?raw=true">
</p>

브라우저에 Web 서버에서 호스팅되는 파일이 필요할 때마다 브라우저는 **HTTP**를 통해 파일을 요청한다. 요청이 올바른 Web 서버에 도달하면 HTTP서버가 요청을 수락하고 요청된 문서를 찾은 다음 HTTP를 통해 브라우저로 다시 보낸다. ( 이때, 요청된 문서를 찾지 못하면 우리가 많이 접해본 **'404 Not Found'** 를 반환한다! )

<br><br><br>

이러한 Web 서버는 **정적인 자료**를 처리하는 서버이다. **HTML, CSS, IMAGE** 등의 정적인 파일들을 **서버에 저장하고, 요청이 들어올때 마다 서버에 저장된 파일을 사용자에게 HTML파일로 뿌려주기 때문에** **서버 자원의 한계가 생기고 리소스를 많이 차지하게 되는 단점**이 있다! 이를 보완하기 위해 생긴 것이 바로 **동적으로 파일을 처리하는 WAS 서버** 이다..!

<br><br>
### 🔍 Web 서버 예시
- Apache 재단의 Apache
- Microsoft사의 IIS, NGINX
- Google Web Server
- Node.js ( 자체 웹 서버 내장 )

<br><br><br>

## ⭐ 아파치 (Apache)

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/apache2.png?raw=true">
</p>
결국 **아파치 서버**는 클라이언트에서 요청한 **HTTP Request**를 처리하는 **웹서버** 이다. 아파치는 **정적 타입 (HTML, CSS, IMAGE)** 의 데이터만을 처리 한다!

<br><br><br>



## ⭐ WAS (Web Application Server) 서버
WAS 서버는 **동적인 자료**를 처리하는 서버이다. 기존 WEB서버의 단점을 보완하여 **Servlet Container** 추가되었다. 클라이언트에서 웹 페이지를 요청하면 Servlet Container가 요청정보를 파악하여 실시간으로 페이지에 필요한 파일을 생성한다. 요청이 올 때마다 페이지에 필요한 정보를 그때그때 생성하므로 서버의 리소스의 부하를 줄일 수 있다는 장점이 있다.

<br><br>
### 🔍 WAS 예시
- Tomcat
- JBoss
- Jeus

<br><br>

## ⭐ 톰캣(Tomcat)

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/tomcat1.png?raw=true">
</p>
톰캣은 **동적인 웹을 만들기 위한 웹 컨테이너** ,  **JSP와 Servlet을 구동하기 위한 서블릿 컨테이너** 역할을 수행한다. 한마디로 정적페이지를 제외한 JSP, ASP, PHP 등은 톰캣에서 처리한다. 또한 **DB연결, 데이터 조작, 다른 응용 프로그램과 상호작용**이 가능하다.

<br><br><br>



## ⭐ 아파치 톰캣 구조
<p align="left">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/apa1.png?raw=true">
</p>
<br>

<p align="left">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/apa2.png?raw=true">
</p>
<br>

<p align="left">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/apa3.png?raw=true">
</p>

Apache Tomcat 구조의 쉬운 이해를 위해 그림 3개를 가져왔다. 이번 포스팅은 아파치 톰캣의 구조를 이해하는 것이 목적이기 때문에 서버 내부에서 일어나는 데이터 처리 방식은 잠시 제쳐두고 살펴보자.

<br>


- 사용자 (Client)가 HTTP Request를 던졌을 때 필요한 데이터가 **정적데이터** 라면 **Web 서버(Apache)** 에서는 바로 **HTTP Response**를 통해 **정적 HTML을 반환**하고 **동적 데이터**라면 이를 **Web Container(Servlet Container)** 로 보내 동적 데이터 처리를 한 뒤 Web 서버를 통해 사용자에게 반환한다.

- 기본적으로 아파치와 톰캣의 기능은 나뉘어져 있지만, 톰캣 안의 컨테이너를 통해 일부 아파치 기능을 발휘하기 때문에 아파치톰캣 이라고 합쳐서 부른다.

- 아파치만 쓰면 정적 웹페이지만 처리가 가능하다

- 톰캣만 쓰면 정적 웹페이지 + 동적 웹페이지 처리가 모두 가능하지만, 아파치에서 필요한 기능을 가져올 수 없고, 과부하가 걸릴 가능성이 높다.

- 따라서 **아파치와 톰캣을 같이 사용하여 아파치는 정적 데이터 처리, 톰캣은 JSP, ASP, PHP 등 동적 데이터 처리를 분담한다.**




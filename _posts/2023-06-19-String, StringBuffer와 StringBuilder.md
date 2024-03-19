---
permalink: /2023-06-19-String, StringBuffer와 StringBuilder/
published: true
title: "[JAVA] String, StringBuffer와 StringBuilder "
date: 2023-06-19 02:00:00
toc: true
toc_sticky: true
toc_label: "객체지향 한방에 정리하기"
categories:
- JAVA
tags:
- JAVA
- 객체지향
- 객체지향 한방에 정리하기
---

<br><br>

## ✅ String VS StringBuffer/ StringBuilder

JAVA에서 문자열을 처리할 때 대표적으로 많이 쓰이는게 String, StringBuffer 그리고 StringBuilder 이다. 셋다 비슷한 기능을 수행하지만 엄연히 다르고 장단점이 존재하는데 무엇인지 알아보자.

<br><br><Br><br>
## ✅ String의 불변성
**한번 생성된 String 인스턴스는 읽어올 수는있지만, 변경할 수는 없다.** 그런데 우리는 실제로 String 클래스를 사용하면서 String이 변경되는것을 자주봤을 것이다. 다음 코드를 보자.

```java
String str = "hello ";
str = str + "world";

>>> str
>>> hello world
```
이처럼 String은 불변한다고 했지만 막상 String에 새로운 문자열을 갖다 붙일 수 있다. 때문에 불변하지 않는다고 생각할 수 있지만 **실제 메모리 영역을 살펴보면 그 이유를 알 수 있다.**

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/java1.png?raw=true">
</p>

처음 String 인스턴스를 생성하고 "hello"라는 값을 부여하면 메모리에 할당되고 임의의 주소값을 부여받는다. 이때 **기존 String 인스턴스에 다른 문자열을 추가하면 기존에 존재하던 주소값에 이를 할당하는게 아니라 아예 새로운 메모리에 할당해서 새로운 값을 만들어내는 구조이다.** 그렇기 때문에 String인스턴스가 **'변한게'** 아니라 **'새로 생성'** 된것이다. 기존의 String 인스턴스는 전혀 변화가 없기 때문에 **String은 불변하다** 라고 보는것이다.

이런식으로 '+' 연산자를 이용해서 문자열을 결합하는 것은 매 연산시마다 새로운 문자열을 가진 String 인스턴스가 생성되어 메모리 공간을 차지하기 때문에 가능한 결합 횟수를 줄이는것이 좋다. 그러나 문자열 간의 결합이 추출 등 문자열을 다루는 작업이 많이 필요한 경우에는 메모리 낭비가 심해질 수 밖에 없다. 그래서 나온게 바로 StringBuffer/StringBuilder 이다! 

<br><br><Br><br>
## ✅ StringBuffer 클래스
String은 인스턴스 생성할 때 지정된 문자열을 수정할 수 없지만, StringBuffer는 가능하다. 내부적으로 **문자열 편집을 위한 버퍼(buffer)를 가지고 있고, StringBuffer 인스턴스를 생성할 때 크기를 지정할 수도 있다.**

크기를 지정하지 않은 경우 **16개의 문자**를 저장할 수 있는 크기의 버퍼를 생성하고 이를 초과하는 문자열이 들어오는 경우 **자동으로 버퍼의 길이를 늘려주는 작업이 수행되기 때문에 작업 효율이 떨어질 수 있다.** 

<br><br>

### 📌 StringBuffer의 변경
```java
StringBuffer sb = new StringBuffer("hello");
sb.append("world");
```

StringBuffer는 다양한 API를 사용해 변경할 수 있다. 위의 코드는 앞서 String 클래스에서 설명한 '+' 연산자와 동일한 기능을 수행하는 코드이다. StringBuffer 인스턴스를 생성하고 초기값을 "hello"라고 부여한 뒤 StringBuffer 인스턴스에 "world"를 추가한 것이다.

<br>

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/stringbuffer1.png?raw=true">
</p>

<Br>
그림을 보면 StringBuffer는 String과 달리 메모리를 새로 할당하지 않고 기존 메모리에 접근하여 값을 변경하는것을 볼 수 있다. 때문에 String 보다 메모리 낭비가 발생하지 않아 문자열의 추가, 수정, 삭제가 빈번하게 발생되는 경우 많이 사용한다.

또 하나 기억해야할 점은 **`append()`는 반환타입이 StringBuffer이고 자기 자신의 주소를 반환한다**는 점이다. 때문에 다음과 같은 코드작성도 가능하다.

```java
StringBuffer sb = new StringBuffer("안녕하세요 ");
StringBuffer sb2 = sb.append("반갑습니다");
System.out.println(sb);	// 안녕하세요 반갑습니다
System.out.println(sb2);	// 안녕하세요 반갑습니다
```

```java
StringBuffer sb = new StringBuffer("안녕하세요 ");
sb.append("반갑습니다 ").append("잘부탁드려요!");
System.out.println(sb)	// 안녕하세요 반갑습니다 잘부탁드려요!
```
`append()`의 반환타입이 자기자신 StringBuffer이기 때문에 위와같은 코드작성이 가능하다.

<br><br><Br><br>
## ✅ StringBuffer 메서드

전부다 알면 좋겠지만 주로 많이 쓰는걸 위주로 기억해두도록 하자. 빨간 박스 친 메서드는 코딩테스트에서도 용이하게 쓰일수 있는 메서드 이기때문에 코딩테스트를 준비하는 상황이라면 기억해두도록 하자!
<br>

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/stringbuffer2.png?raw=true">
</p>

<br>

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/stringbuffer3.png?raw=true">
</p>

<br><br><Br><br>
## ✅ StringBuilder 클래스

StringBuffer는 멀티쓰레드에 안전하도록 동기화되어 있다. 동기화는 StringBuffer의 성능을 떨어뜨리기 때문에 멀티쓰레드로 작성된 프로그램이 아닌경우, StringBuffer의 동기화는 불필요하게 성능만 떨어뜨린다.

그래서 StringBuffer에서 쓰레드의 동기화만 뺀 StringBuilder가 추가되었다. StringBuffer와 기능적으로 완전히 동일해서 StringBuffer대신 StringBuilder라고 선언해주기만 하면된다. 

```java
StringBuilder sb = new StringBuilder();
```
<br><br>

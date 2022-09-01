---
permalink: /2022-08-25-팩토리 메소드(Factory Method)패턴/
published: true
title: "[Design Pattern] 팩토리 메소드(Factory Method) 패턴 "
date: 2022-08-25 18:00:00
toc: true
toc_sticky: true
toc_label: "팩토리 메소드(Factory Method)패턴"
categories:
- 디자인 패턴
tags:
- 디자인 패턴
- Design Pattern
- 생성 패턴
---
<br><br><br>

## ✅ 팩토리 메소드 패턴 (Factory Method)


- **객체 생성 처리를 서브 클래스로 분리 해 처리하도록 캡슐화 하는 패턴**

- **객체의 생성코드를 별도의 클래스/메소드로 분리**함으로써 객체 생성의 변화 대비에 유용

- 객체 생성 기능처럼 특정 기능을 개별 클래스를 통해 구현하는 것은 바람직한 객체 지향 설계 방법이다.

- 생성 (Creational) 패턴의 하나이다.

- **기존 코드 변경 없이 확장하기 위한 디자인 패턴!**

<br><br>


<p align="left">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/factory1.png?raw=true">
</p>

<br><br>

📌 **Product** 
 　　　　팩토리 메소드로 생성 될 객체의 공통 인터페이스
 　　　　　　
📌 **ConcreteProduct** 
　　　　구체적으로 객체가 생성되는 클래스
　　　
📌 **Creator** 
　　　　팩토리 메소드를 갖는 클래스
　　　
📌 **ConcreteCreator** 
　　　　팩토리 메소드를 구현하는 클래스, ConcreteProduct 객체 생성


<br><br><br><br>

## ✅ 팩토리 메소드 예제
<h5>⭐ 사용자 관리 프로그램이 있고 네이버 계정으로 가입할 수 있다.</h5>

<br>

### 📌 Product ( User )

```java
public  interface User { 
	void signup(); 
}
```
-``` User``` 인터페이스 정의

<br>

```java
public  class NaverUser implements User { 
	@Override  
	public void signup() { 
		System.out.println("네이버 아이디로 가입"); 
	} 
}
```
- ```User``` 인터페이스를 구현하는 ```NaverUser``` 클래스
- 오버라이드한 메소드에서는 네이버 유저 전용 로직 추가

<br><br>

### 📌 Creator ( UserFactory )
<h5>⭐ 실제로 객체를 생성하는 Factory</h5>

```java
public abstract class UserFactory { 
	public User newInstance() { 
		User user = createUser(); 
		user.signup(); 
		return user;
	} 
	protected abstract User createUser(); 
}
```

- 추상클래스로 UserFactory를 정의
- 실제 객체를 생성하는 곳 ( 일종의 팩토리 )
- 외부에서 User 객체를 생성 할 때는 ```newInstance()``` 메서드 호출
- 어떤 객체를 생성할 지는 하위 클래스에서 결정

<br>

```java
public class NaverUserFactory extends UserFactory {
	@Override  
	protected User createUser() { 
		return new NaverUser(); 
	} 
}
```

- ```UserFactory```를 상속받는 ```NaverUserFactory```



<br><br>



### 📌 Client
```java
UserFactory userFactory = new NaverUserFactory(); 
User user = userFactory.newInstance();
```
- 클라이언트 코드에서 ```NaverUser``` 클래스에 대한 의존성 없이 사용


<br><br><br><br>

## ✅ 팩토리 메소드 확장

팩토리 메소드의 가장 큰 장점은 확장 시 기존 코드의 변경 없이도 확장이 가능하다는 것이다. 따라서 위 예시에서 네이버 외에 다른 회사가 추가 되어도 기존 코드 변경 없이 추가할 수 있다. 카카오 서비스가 새로 추가된다고 가정해보자.

```java
public class NaverUser implements User { 
	@Override  
	public void signup() { 
		System.out.println("네이버 아이디로 가입"); 
	} 
}

public class KakaoUser implements User { 
	@Override  
	public void signup() { 
		System.out.println("카카오 아이디로 가입"); 
	} 
}
```

- ```KakaoUser``` 클래스를 추가해준다.

<br><br>

```java
public class NaverUserFactory extends UserFactory {
	@Override  
	protected User createUser() { 
		return new NaverUser(); 
	} 
}

public class KakaoUserFactory extends UserFactory {
	@Override  
	protected User createUser() { 
		return new KakaoUser(); 
	} 
}
```
- ```NaverUserFactory``` 와 동일하게 ```KakaoUserFactory``` 정의

<br><br>

```java
UserFactory userFactory = new NaverUserFactory(); 
User user = userFactory.newInstance();

UserFactory userFactory = new KakaoUserFactory(); 
User user = userFactory.newInstance();
```
- 클라이언트에 새로운 Kakao객체 선언해주면서 기존 코드 변경없이 확장 가능!

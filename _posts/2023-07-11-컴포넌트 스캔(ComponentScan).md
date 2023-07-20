---
permalink: /2023-07-11-컴포넌트 스캔(@ComponentScan)/
published: true
title: "[스프링] 컴포넌트 스캔(ComponentScan)"
date: 2023-07-11 18:00:00
toc: true
toc_sticky: true
toc_label: "컴포넌트 스캔(@ComponentScan)"
categories:
- Spring
tags:
- Spring
- 카카오 클라우드 스쿨
---

<br><br>

## ✅ 컴포넌트 스캔이란?

**의존성 주입을 위해 스프링은 어플리케이션 컨텍스트를 생성한다. 스프링은 객체를 인스턴스화 하고 이를 어플리케이션 컨텍스트에 추가하는데 이때 인스턴스화 된 객체를 우리는 스프링 빈 혹은 컴포넌트 라고 부른다.**

**이때, 어플리케이션 컨텍스트에 스프링 빈(컴포넌트)으로 등록될 클래스들을 스캔하여 빈으로 등록해주는 과정을 우리는 컴포넌트 스캔 이라고 부른다.**

**다른말로 스프링 빈으로 등록될 클래스패스(Classpath)를 스캔하여 빈으로 등록해주는 과정을 컴포넌트 스캔이라고 한다.**

컴포넌트 스캔이 무엇인지 모르는 상태라면 기존에 스프링 빈을 등록할 때는 자바 코드의 `@Bean`이나 XML의 `<bean>`을 통해서 설정 정보를 직접 등록했을 것이다. 그러나 이렇게 등록해야 할 **스프링 빈의 수가 엄청 많아지게 되면 일일이 등록하기도 힘들뿐더러, 설정정보의 양도 많아지고 실수가 생길 수 있다.**

그래서 스프링에서는 **설정정보 없이 자동으로 스프링 빈을 등록하는 컴포넌트 스캔**을 제공한다. 추가적으로 의존관계를 자동으로 주입해주는 `@Autowired` 까지 함께 알아보도록 하겠다.

먼저 비교를 위해 기존 `@Bean`을 사용한 스프링 빈 등록방법과 DI를 적용한 코드를 살펴보고 컴포넌트 스캔과의 차이를 살펴보도록 하자. 

<br><br><br>

###  📌 `@Bean` 을 사용한 스프링 빈 등록

```java
package hello.core;  
  
import hello.core.discount.DiscountPolicy;  
import hello.core.discount.FixDiscountPolicy;  
import hello.core.member.MemberService;  
import hello.core.member.MemberServiceImpl;  
import hello.core.member.MemoryMemberRepository;  
import hello.core.order.OrderService;  
import hello.core.order.OrderServiceImpl;  
import org.springframework.context.annotation.Bean;  
import org.springframework.context.annotation.Configuration;  
  
@Configuration  // 설정 정보 등록
public class AppConfig {  
  
	@Bean  // 스프링 빈으로 등록
	public MemberService memberService(){  
		return new MemberServiceImpl(memoryMemberRepository());  // 의존성 주입
	}  
  
	@Bean  // 스프링 빈으로 등록
	public OrderService orderService(){  
		return new OrderServiceImpl(memoryMemberRepository(), discountPolicy());  // 의존성 주입
	}  
  
	@Bean  // 스프링 빈으로 등록
	public MemoryMemberRepository memoryMemberRepository(){  
		return new MemoryMemberRepository();  
	}  
  
	@Bean  // 스프링 빈으로 등록
	public DiscountPolicy discountPolicy(){  
		return new FixDiscountPolicy();  
	}  
}
```
<br>
기존 스프링 빈을 등록하는 방식의 코드이다.

1. 설정정보를 담고있는 클래스에 `@Configuration` 태그 사용
2. 스프링 빈으로 등록할 객체에 `@Bean` 태그 사용
3. 스프링 빈에 생성자 형태로 의존성 주입

일반적으로는 이러한 방식으로 스프링 빈을 등록했을 것이다. 그러나 앞서 언급한 것처럼 위와 같은 방식은 편의성이 많이 떨어지기 때문에 스프링에서는 `@ComponentScan` 이라는 기능을 제공한다.

<br><Br><Br>

###  📌 `@ComponentScan` 을 사용한 스프링 빈 등록

```java
package hello.core; 

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.FilterType; 

import static org.springframework.context.annotation.ComponentScan.*; 

@Configuration 
@ComponentScan
public class AutoAppConfig { 

}
```

컴포넌트 스캔을 사용하기 위해서는 `@ComponentScan`을 설정정보에 붙여주기만 하면 된다. 기존에는 `@Bean`을 사용해 스프링 빈을 등록해줬지만 여기에는 하나도 없는것을 볼 수 있다. 컴포넌트 스캔을 사용할 때 스프링 빈 등록은 `@Component` 태그를 붙여줌으로써 가능해진다.  

<br><br>

####  📌 MemoryMemberRepository에  `@Component` 추가
```java
@Component 
public class MemoryMemberRepository implements MemberRepository {}
```
<br>

####  📌 RateDiscountPolicy 에  `@Component` 추가
```java
@Component 
public class RateDiscountPolicy implements DiscountPolicy {}
```
<br>

####  📌 MemberServiceImpl 에  `@Component` 추가 + `@Autowired` DI
```java
@Component 
public class MemberServiceImpl implements MemberService { 
	private final MemberRepository memberRepository; 

	@Autowired 
	public MemberServiceImpl(MemberRepository memberRepository) {
		this.memberRepository = memberRepository; 
	}
}
```
<br>

####  📌 OrderServiceImpl 에  `@Component` 추가 + `@Autowired` DI
```java
@Component 
public class OrderServiceImpl implements OrderService { 

	private final MemberRepository memberRepository; 
	private final DiscountPolicy discountPolicy; 

	@Autowired 
	public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) { 
		this.memberRepository = memberRepository; 
		this.discountPolicy = discountPolicy; 
	} 
}
```

<br>

빈으로 등록하려는 클래스에 `@Component` 를 붙여줌으로써 컴포넌트 스캔을 사용한 스프링 빈 등록이 가능해진다. 또한 `@Autowired` 태그를 붙이면 자동으로 의존관계를 주입해주는 기능을 한다. 이를 통해 더이상 `@Bean`을 사용한 스프링 빈 등록과 의존성 주입을 할 필요 없이 편하게 스프링 빈을 등록할 수 있게 되었다. 이 과정을 정리하면 다음과 같다.

1. 설정정보 클래스에 `@ComponentScan` 어노테이션을 추가해준다.
2. 스프링 빈으로 등록할 클래스에 `@Component` 어노테이션을 추가하여 `@ComponentScan`의 대상이 되도록 지정하고 스프링 빈으로 등록해준다.
3. 스프링 빈으로 등록할 때 필요한 DI는 `@Autowired`를 이용해 처리한다.

<br><br><br>

###  📌 `@ComponentScan`의 스캔 대상


컴포넌트 스캔은 기본적으로 `@Component` 어노테이션이 붙은 클래스를 대상으로 스캔한다. 이름그대로 컴포넌트스캔 이기때문에 컴포넌트를 스캔한다고 생각하면 쉽다.

그러나 `@Component`만 스캔하는 것은 아니다. 다음 내용들도 스캔 대상에 포함된다.
- `@Component` : 컴포넌트 스캔에 사용
- `@Controller` : 스프링 MVC 컨트롤러로 지정
- `@Service` : 스프링 비즈니스 로직으로 지정
- `@Repository` : 스프링 데이터 접근 계층으로 지정
- `@Configuration` : 스프링 설정 정보로 지정

위 어노테이션들을 스캔하는 이유가 있다. 각 어노테이션을 뜯어보면 모두 `@Component` 를 포함하고 있다. 때문에 컴포넌트 스캔 대상에 포함되는 것이다.

<br><Br><br>

###  📌 `@ComponentScan`의 스캔 시작 위치

컴포넌트 스캔은 시작위치를 지정할 수 있다.

```java
// 하나의 패키지를 시작위치로 지정
@ComponentScan( basePackages ="hello.core" )

// 여러개의 패키지를 시작위치로 지정
@ComponentScan( basePackages = {"hello.core", "hello.service"} )
```

- `basePackages` : 탐색할 패키지의 시작위치를 지정한다. 이 패키지를 포함해서 하위 패키지를 모두 탐색한다.
- `basePackageClasses` : 지정한 클래스의 패키지를 탐색 시작 위치로 지정한다.
- 만약 지정하지 않으면 `@ComponentScan` 이 붙은 설정정보 클래스의 패키지가 시작위치가 된다.

<br><Br>

####  📌 `@ComponentScan`의 스캔 시작 위치 예제

다음과 같은 패키지 구조를 가진 스프링 프로젝트가 있다고 하자.

```
com.example.myapp
├── controllers
│   └── UserController.java
├── services
│   └── UserService.java
└── repositories
    └── UserRepository.java
```

controllers 패키지와 services 패키지를 컴포넌트 스캔의 시작 위치로 지정하고자 하면 다음과 같이 코드를 작성할 수 있다.

<br>

```java
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan(basePackages = {"com.example.myapp.controllers", "com.example.myapp.services"})
public class AppConfig {
    // AppConfig configuration
}
```
이러면 `com.example.myapp.controllers` 패키지와 `com.example.myapp.services` 패키지를 컴포넌트 스캔의 시작위치로 지정하여 수행하고 하위 패키지가 따로 없기때문에 두개의 패키지만 컴포넌트 스캔이 진행된다.

<br><Br><br>

###  📌 컴포넌트 스캔 시작위치 권장 방법

**요즘은 컴포넌트 스캔의 시작위치를 따로 지정해주지 않는 방식을 주로 택한다.** 시작위치를 지정해주지 않으면 디폴트값은 `@ComponentScan` 어노테이션이 달려있는 설정정보 클래스의 패키지를 기준으로 하위 모든 패키지로 지정된다.

이를 잘 활용해 설정정보 클래스를 프로젝트의 시작 루트에 생성하고 `@ComponentScan` 을 붙임으로써 모든 패키지가 컴포넌트 스캔의 대상이 될 수 있도록 설정한다.
<br>

```java
com.example.myapp
│   
├── AppConfig.java
│   
├── controllers
│   └── UserController.java
│   
├── services
│   └── UserService.java
│   
└── repositories
    └── UserRepository.java
```
위 패키지 구조처럼 프로젝트의 시작 루트에 `AppConfig.java` 라는 설정정보를 위치하고 해당 클래스에 `@ComponentScan`을 붙이는게 요즘 권장사항이다..!


<br><Br><br>


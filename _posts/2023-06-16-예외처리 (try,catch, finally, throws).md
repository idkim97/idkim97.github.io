---
permalink: /2023-06-16-7)예외처리 (try,catch, finally, throws)/
published: true
title: "[JAVA] [객체지향] 예외처리 ( try, catch, finally, throws ) "
date: 2023-06-16 03:00:00
toc: true
toc_sticky: true
toc_label: "객체지향 한방에 정리하기"
categories:
- 객체지향
tags:
- JAVA
- 객체지향
- 객체지향 한방에 정리하기
---

<br><br>

## ✅ 프로그램 오류

프로그램이 실행 중 어떤 원인에 의해서 오작동을 하거나 비정상적으로 종료되는 경우가 있는데, 이러한 결과를 초래하는 원인을 프로그램 에러 또는 오류라고 한다.

<br><br>

### 📌 발생시점에 따른 에러 분류

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/exception2.png?raw=true">
</p>

프로그램 오류는 발생시점에 따라 **컴파일 에러**와 **런타임 에러**로 나눌수 있다. 추가적으로 **논리적 에러**도 존재한다.

**컴파일 에러는 글자 그대로 컴파일 할 때 발생하는 에러이고 소스의 오타나 잘못된 구문, 자료형 체크등 검사를 수행하면서 발생한다.**
```java
@Test  
void compileError() {
    STring helloStr = "hello";   // String이 아닌 STring으로 작성해서 오류발생
    System.out.println("helloStr = "  + helloStr);
}
```

<br>

**런타임 에러는 프로그램의 실행 시점에서 발생하는 에러로 컴파일 이후 프로그램이 실행되고 실행도중 의도치 않은 동작에 대처하지 못해 에러가 발생한다.**
```java
// 문자가 숫자형태가 아닌 문자 타입인 경우 
// parseInt()메서드에서 NumberFormatException 발생
void stringToInt(String str) {
    int i = Integer.parseInt(str);
    System.out.println("str : " + i);
```

<br>
논리적 에러는 컴파일도 잘되고 실행도 잘되지만 의도와는 다르게 동작하는 것을 말한다. 버튼을 뜨게 만들었는데 페이지가 뜬다거나, 아무 동작도 안하게 만들었는데 무언가 출력된다던가 하는 에러를 말한다.


<br><br><br><br>

## ✅ 예외 클래스의 계층 구조
<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/exception1.png?raw=true">
</p>

자바에서는 실행 시 발생할 수 있는 오류를 클래스로 정의했다. 모든 클래스의 조상은 Object 클래스이므로 Exception과 Error클래스 역시 Object클래스의 자손들이다.

모든 예외의 최고 조상인 Exception 클래스는 크게 두가지로 나뉜다. **Exception 클래스와 자손들, RuntimeException클래스와 자손들** 두가지가 있다. 

RuntimeException클래스와 하위 예외들을 **선택적 예외(unchecked Exception)**로 개발자가 상황에 맞춰 대응해줘야 하는 예외이고 그외 나머지 예외클래스와 하위객체들을 **필수 예외(checked Exception)**라 하여 반드시 확인해줘야 하는 예외이다.

**RuntimeException 클래스들은 주로 프로그래머의 실수에 의해 발생될 수 있는 예외들로 자바의 프로그래밍 요소들과 관계가 깊다.** 배열의 범위를 벗어난 ArrayIndexOutOfBoundsException, 값이 null인 참조변수를 호출할때 발생하는 NullPointerException, 클래스간의 형변환을 잘못할때 발생하는 ClassCastException, 정수를 0으로 나눌 때 발생하는 ArithmeticException 등이 있다.

**Exception 클래스들은 외부의 영향으로 발생할 수 있는 예외들로 프로그램의 사용자들의 동작에 의해 발생하는 경우가 많다.** 존재하지 않는 파일의 이름을 입력하는 FileNotFoundException, 클래스의 이름을 잘못 입력한 ClassNotFoundException, 입력 데이터 형식이 잘못된 DataFormatException 등이 있다.

<br><br><br><br>

## ✅ 예외 처리하기 :: try-catch

프로그램의 실행 도중에 발생하는 오류는 어쩔 수 없지만, **프로그램 실행 시 발생할 수 있는 예기치 못한 예외의 발생은 예외처리**를 통해 해결할 수 있다.

예외처리를 위한 첫번째 방법은 **try-catch 구문**을 사용하는 것이다.

```java
try {
	// 예외가 발생할 가능성이 있는 문장을 넣는다.
} catch ( Exception1 e1 ) {
	// Exception1이 발생했을 경우, 이를 처리하기 위한 문장을 넣는다.
} catch ( Exception2 e2 ) {
	// Exception1이 발생했을 경우, 이를 처리하기 위한 문장을 넣는다.
} catch ( Exception3 e3 ) {
	// Exception1이 발생했을 경우, 이를 처리하기 위한 문장을 넣는다.
} finally {
	deleteTempFiles() // 프로그램 설치에 사용된 임시파일 삭제
	// try-catch구문이 종료되고 무조건 실행할 문장을 넣는다.
}
```

하나의 try 블럭 다음에는 여러개의 catch블럭이 나올 수 있고, 발생한 예외의 종류와 일치하는 catch 블럭이 수행되고 try-catch 구문을 통째로 빠져나온다. 이때, 발생한 예외의 종류과 일치하는 catch블럭이 없는경우에는 예외가 처리되지 않는다.

**`finally`** 는 예외 발생여부와 상관없이 실행되어야할 코드를 포함시킬 목적으로 사용된다. try-catch 구문 끝에 선택적을 사용할 수 있다. 보통 임시파일 삭제나 DB연결 해제 등 불필요한 메모리를 지우거나 기존 연결을 해제하는 등 반드시 실행되어야 할 코드를 작성한다. 

또한 **catch문에 들어갈 예외의 순서는 작은 범위부터 차례대로 넣어줘야 한다.** 예를 들어 Exception 예외처리를 하는경우에 ,Exception 예외 클래스가 가장 최상의 부모클래스이므로 마지막 catch 구문에 작성해줘야만 한다.
```java
public class Ex07_Exception {  
	public static void main(String[] args) {  
		int i = 10 ;  
		String str = "aa";  
		try {  
			int result = 10 / i 
			return;  
		} catch(ArithmeticException e) {  
			System.out.println("ArithmeticException 발생!");  //ArithmeticException 이 발생 했을 때만 실행  
		} catch(NullPointerException e) {  
			System.out.println("NullPointerException 발생!");  //NullPointerException 이 발생 했을 때만 실행  
		} catch(Exception e){  
			// 그 외의 Exception 이 발생 했을 때 실행  
		} finally{  
			System.out.println("항상 실행");  
		}  
			System.out.println( "정상 종료");  
	}  
}
```

위의 코드를 보면 `ArithmeticException`, `NullPointerException`, 그리고 `Exception` 예외처리를 한것을 볼 수 있는데 `Exception` 예외처리를 가장 마지막 catch문에 작성한 것을 볼 수 있다.

만일 `Exception` 예외처리를 상위 catch문에 작성한다면 `ArithmeticException`이나 `NullPointerException`이 발생한 경우에도 `Exception` 예외처리를 진행하게 된다. 그렇게 되면 각각의 예외에 맞는 catch문을 실행할 수 없어지기 때문에 올바른 예외처리를 못하게 된다. 따라서 `Exception` 예외처리는 가장 마지막 catch문에 작성해줘야만 한다.


<br><br><br>
### 📌 try-catch문에서의 흐름
try-catch문에서, 예외가 발생한 경우와 발생하지 않은 경우에 흐름은 달라진다.

- **try 블럭 내에서 예외가 발생한 경우**
	1. 발생한 예외와 일치하는 catch문이 있는지 확인한다.
	2. 일치하는 catch문을 찾으면, 해당 catch블럭 내의 문장들을 수행하고, **try-catch문을 통째로 빠져나간다.**

```java
public class Ex07_Exception {  
	public static void main(String[] args) {  
		System.out.println(1);
		try {
			System.out.println(2);
			System.out.println(3);
		} catch(Exception e) {
			System.out.println(4);
		}
		System.out.println(5);
	}  
} // 1 2 3 5 가 출력된다
```

위 코드를 보면 어떤 오류도 발생하지 않는다. 때문에 try문은 그대로 출력되고 catch문은 실행되지 않는다. 이후 try-catch 구문을 빠져나간뒤 5를 출력한다. 결과적으로는 1, 2, 3, 5가 출력될 것이다.
<br><br>


- **try 블럭 내에서 예외가 발생하지 않은 경우**
	1. catch문을 거치지 않고 try-catch문을 통째로 빠져나간다.

```java
public class Ex07_Exception {  
	public static void main(String[] args) {  
		System.out.println(1);
		try {
			System.out.println(2);
			System.out.println(0/0);
			System.out.println(3);
		} catch(ArithmeticException ae) {
			System.out.println(4);
		}
		System.out.println(5);
	}  
} // 1 2 4 5 가 출력된다
```

위의 코드는 0으로 어떤수를 나눴기 때문에 `ArithmeticException`이 발생한다. 때문에 try문의 `System.out.println(0/0);` 문장 이전까지는 정상 실행되다가 `System.out.println(0/0);` 문장에서 오류가 발생하고 catch 구문으로 넘어가 실행되고 이후 try-catch 구문을 통째로 빠져나간다. 때문에 1, 2, 4, 5가 출력될 것이다.

<Br><Br><Br><Br>

## ✅ 예외 처리하기 :: throws

예외를 처리하는 방법에는 try-catch문을 사용하는것 외에, 예외를 메서드에 선언하는 throws를 사용하는 방법이 있다. 메서드에 예외를 선언하려면, 메서드 선언부에 throws를 사용해서 발생 할 수 있는 예외를 적어주기만 하면 된다.

```java
	void method() throws Exception1, Exception2, ... ExceptionN {
		// 메서드 내용
	}
```

예외를 선언하면, 해당 예외뿐만 아니라 **자손타입의 예외까지도 발생할 수 있다** 라고 알리는 것이다. 또한 메서드를 통한 예외처리는 실제 예외를 어떻게 처리하겠다 라는 로직을 포함하지 않기때문에 단순히 이러이러한 오류가 발생할 수 있고, **해당 오류가 발생한다면 오류에 대한 처리를 메서드를 호출한 쪽으로 넘기겠다!** 라는 의미로 사용된다. 때문에 사용자쪽은 오류에 대한 `try-catch` 구문을 반드시 사용해 처리해야만 한다.

기존의 다른 언어는 메서드에 예외선언을 하지않기 때문에, 경험이 많은 프로그래머가 아니라면 어떤 상황에서 어떤 종류의 오류가 발생할 가능성이 있는지 예측하기 어려웠다. 그러나 자바에서는 메서드 내에서 발생할 가능성이 있는 예외를 선언부에 명시하여 메서드를 사용하는 쪽에 이에 대한 처리를 강요하기 때문에, 보다 견고한 코드를 작성할 수 있다.

<br><br><br>
### 📌 메서드에 예외 선언하기 흐름

```java
class EX {
	public static void main(String[] args) throws Exception {
		method1();
	}

	static void method1() throws Exception {
		method2();
	}

	static void method2() throws Exception {
		throw new Exception();
	}
}
```
<br>


위의 코드를 보면 `method2()` 에서 `throw new Exception();` 을 통해 Exception() 예외를 발생시켰다. `method2()`에서는 try-catch를 통한 예외처리를 하지 않았기 때문에 `method2()`를 호출한 `method1()`로 예외를 넘긴다(throws). `method1()` 에서도 마찬가지로 예외처리를 하지않았으므로 `method1()`을 호출한 `main` 으로 예외를 넘긴다. main에서도 마찬가지로 예외처리를 하지 않았으므로 위의 코드는 오류가 난다.

<Br><br><br>


### 📌 메서드에 예외 선언하기 예제
```java
class EX {
	public static void main(String[] args) {
		try{
			File f = createFile(args[0]);
			System.out.println(f.getName()+"파일이 성공적으로 생성되었습니다.");
		} catch (Exception e) {
			System.out.println(e.getMessage()+"다시 입력해주세요");
		}
	}

	static File createFile(String fileName) throws Exception {
		if(fileName==null || fileName.equals(""))
			throw new Exception("파일이름이 유효하지 않습니다");
		File f = new File(fileName);
		
		f.createNewFile();
		return f;
	}
}
		
```

위의 코드는 사용자로부터 파일이름을 입력받아서 파일을 생성하는 예제이다. 파일을 생성하는것은 `createFile` 메소드인데 파일이름이 유효하지 않으면 `Exception`을 발생시킨다. 이때 `createFile` 메소드에 예외가 선언되어 있으므로 `createFile` 메소드를 호출한 `main` 으로 예외처리를 넘겨준다. `main` 에서는 넘어온 예외를 try-catch 구문을 통해 해결하고 있다. 

그런데 이와 반대로 만약 `createFile` 메소드에서 try-catch 문을 작성해서 오류를 처리하면 어떻게 될까? **`createFile` 메소드에서 처리하게 되면 `main`에서는 오류가 발생했는지 여부도 알지 못하게 된다.**

이를 잘 활용하여 예외가 발생한 메서드 내에서 자체적으로 처리해도 되는 경우 메서드 내에 try-catch문을 넣어서 처리하고, 위 예제처럼 메서드 내에서 자체적으로 해결이 안되는 경우(파일 이름을 다시 받아와야 하는 경우)에는 예외를 선언하여 호출한 메서드가 오류를 처리하도록 해야한다.

 
<br><br><br><Br>

## ✅ 예외 발생시키기 :: throw

키워드 throw를 사용해서 프로그래머가 고의로 예외를 발생시킬 수 있다. 

1. 연산자 new를 이용해 발생시키려는 예외클래스의 객체 생성
	`Exception e = new Exception("고의로 예외발생");`

2. 키워드 throw를 이용해 예외 발생
	`throw e;`

3. `throw new Exception("고의로 예외발생")` 같은 형태로도 사용가능 

<br>

```java
class EX {
	public static void main(String args[]) {
		try {
			Exception e = new Exception("고의로 발생");
			throw e;
			// throw new Exception("고의로 발생");
		} catch (Exception e) {
			System.out.println("에러");
			e.printStackTrace();
		}
		System.out.println("정상종료");
	}
}
```

위 코드처럼 고의로 예외를 발생시켜 catch문 처리를 하도록 만들 수도 있다.

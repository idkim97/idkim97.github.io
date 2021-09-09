---
title: "[컴퓨터 구조] 1. Instruction Set"
date: 2021-09-09 22:00:00
categories:
- Computer Architecture
tags:
- Number representations
- The Von-Neumann Model
- Instruction
- Instruction Set Architecture
---

# Number Representations
인간은 기본적으로 모든 수를 10진수에 기반해 생각한다. 그러나 컴퓨터는 모든 연산을 0과 1로 처리를 하는 2진수에 기반한다. 이때문에 컴퓨터 구조를 이해하기 위해서는 기본적으로 컴퓨터가 어떻게 2진수로 연산을 하고 처리하는지 알 필요가 있다. 먼저 컴퓨터가 어떻게 음수와 양수를 2진수로 표현하는지 알아보자.

<br><br><br><br><br><br>

## 2's Complement( = 2의 보수 )
<hr>
먼저 양수는 기본적인 산수실력만 갖추고 있다면 이진법으로 어떻게 나타내는지 잘 알고 있을 것이다.

예를들어 32비트 수가 주어진다고 했을때 11을 이진수로 나타낸다면 다음과 같다.

> **0000 0000 0000 0000 0000 0000 0000 1011<sub>2</sub>**

그러나 이를 음수, 즉 -11로 나타내려면 어떻게 표기해야 할까?

정답은 **2의 보수**를 사용하는 것이다. 그리 어렵지 않다.

**1로 나타낸 수는 0으로**, **0으로 나타낸 수는 1로 바꿔주고** **1을 더하면 된다.**

<br><br><br><br><br><br>

앞선 11을 다시 예로 들어보겠다.

> **+11 : 0000 0000 0000 0000 0000 0000 0000 1011**  
> **-11 : 1111 1111 1111 1111 1111 1111 1111 0101**  

이렇게 0과 1을 변경해주고 마지막에 1만 더해주면 음수로 바꿀수 있다.

그렇다면 X + (-X) 는 어떻게 표현될까?

> **(1) 0000 0000 0000 0000 0000 0000 0000 0000**

모든 자릿수에서 올림이 들어가고 결국 1이 overflow되어 나타난다.

<br><br><br><br><br><br>

이를 통해 우리는 몇가지 공식을 뽑아낼 수 있다.

> **A + A's(A의 보수) = 1111 1111 <sub>2</sub>**
>**-A = A's + 1**

이를 통해 우리는 컴퓨터 연산에서 뺄셈을 덧셈으로 변경할 수 있다.

이를 잘 적용할 수 있는 연습문제를 하나 풀어보자.

<br><br><br><br><br><br>

**[ 연습문제 ]**
<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/2's.jpg?raw=true">
</p>

1111 1111 1111 1111 1111 1111 1111 1000<sub>2</sub>의 값을 구하라는 문제다.

2의 보수를 노골적으로 사용하라는게 보이지 않는가?

-A = A's +1 을 활용한다면 정답은

 -0000 0000 0000 0000 0000 0000 0000 1000 <sub>2</sub> 즉, -8이 된다.

<br><br><br><br><br><br>

# The Von-Nuemann Model ( 폰 노이만 구조 )

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/Von.png?raw=true">
</p>

<br>

**CPU ( 산술논리장치 )** : Instructions(명령어)를 Fetch, Interpret, Execute 하는 작업을 한다.

	- Fetch : 메모리상에 존재하는 명령어를 CPU로 가져오는 작업
	- Interpret : 가져온 명령어를 CPU가 해석하는 작업
	- Execute : 해석된 명령어대로 CPU가 실행하는 작업


**Memory** : W비트 만큼의 N words를 저장한다.

<br><br><br><br><br><br>

# Instructions ( 명령어 ) 란?

컴퓨터의 언어, 그중에서도 **단어** 라고 볼 수 있다.

CPU는 Instruction 사이클을 반복해서 프로그램을 실행한다.


<br><br><br><br><br><br>

## Instruction Set Architecture (ISA)
<hr>
**Instruction Set Architecture (ISA)**는 **문장**이라고 볼 수 있다.

**ISA**는 **HW와 SW간의 추상적인 인터페이스**로 Machine Language Program을 작성하기 위해 필요한 **모든 정보**를 말하며 단순히 Instructions의 집합뿐만 아니라 **register, memory, access** 등을 포함한 모든 정보를 일컫는다.

<br><br><br><br><br><br>

## Two types of Instruction Set
<hr>
1. **Complex Instruction Set Computer**  ( CISC )

- 1000개가 넘는 Instructions를 가진다.
- 명령어를 해석하는데 시간이 오래걸리며 명령어 해석에 필요한 회로도 복잡하다.
- 10여개가 넘는 addressing mode를 가진다.
- 대표적으로 x86 이있다.

**2. Reduced Instruction Set Computer ( CISC )**
- 비교적 적은 명령어를 가져 구조가 좀더 간단하다.
- 적은수의 명령어로 Instruction Set을 이루므로 속도가 빠르다.
- 일반적으로 많이 쓰이는 명령어를 주로 가진다.
- **메모리에 접근하는 명령어**로 **load와 store**만을 가진다!
- 대표적으로 MIPS 가 있다.

<br><br><br><br><br><br>

## RISC vs CISC 정리
<hr>
<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/RISC.jpg?raw=true">
</p>

<br><br><br><br><br><br>



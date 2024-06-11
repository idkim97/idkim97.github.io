---
published: true
title: "[컴퓨터 구조] Instruction Set Architecture (ISA) 이란?"
date: 2021-09-12 12:00:00
toc: true
toc_sticky: true
toc_label: "ISA란?"
description: "Instruction Set(명령어 집합)이란 무엇이고 ISA의 구성요소 및 특징을 알아보자. RISC와 CISC의 특징과 차이점을 알아보자."
categories:
- Computer Architecture
tags:
- Computer Architecture
---

<br><br><br>

## ✅ Instruction Set (명령어 집합)

**Instruction Set**은 프로세서가 이해하고 실행할 수 있는 모든 명령어의 목록을 말한다. 이 목록에는 산술 연산, 논리 연산, 데이터 전송, 제어 명령 등의 다양한 명령어가 포함된다. 간단히 말해, Instruction Set는 프로세서가 수행할 수 있는 모든 명령어들의 집합이다.

-   `ADD`: 두 레지스터 값을 더하는 명령어.
-   `SUB`: 두 레지스터 값을 빼는 명령어.
-   `LW`: 메모리에서 레지스터로 데이터를 로드하는 명령어.
-   `SW`: 레지스터에서 메모리로 데이터를 저장하는 명령어.
-   `BEQ`: 두 레지스터 값이 같으면 분기하는 명령어.


<br><br><br><Br><Br><Br>

## ✅ Instruction Set Architecture (ISA)

**Instruction Set Architecture (ISA)는 Instruction Set을 포함한 더 포괄적인 개념으로,  컴퓨터의 하드웨어와 소프트웨어 간의 인터페이스를 정의하는 개념이다.** ISA는 프로세서가 실행할 수 있는 명령어의 집합과 이 명령어들이 실행되는 방식을 정의한다. 이는 컴퓨터 아키텍처의 핵심 요소 중 하나로, 특정 프로세서 설계와 소프트웨어 개발에 중요한 역할을 한다.

좀더 쉽게 풀어쓰면 **ISA**는 **HW와 SW간의 추상적인 인터페이스**로 기계어를 작성하기 위해 필요한 **모든 정보**를 말하며 단순히 Instructions의 집합뿐만 아니라 **register, memory, access** 등을 포함한 모든 정보를 의미한다. 


<BR><BR>

### 📌 ISA는 다양하다
<hr>

ISA(Instruction Set Architecture)는 회사마다 다양할 수 있다. 이는 각 회사가 목표로 하는 설계 철학, 성능 요구 사항, 전력 소비, 응용 분야 등에 따라 서로 다른 ISA를 개발하기 때문이다.

ISA를 개발하는 대표적인 회사는 Intel과 AMD, ARM이 있다. Intel과 AMD는 고성능 데스크탑과 서버용 프로세서를 위해 x86 ISA를 개발했으며, ARM은 모바일 장치와 임베디드 시스템을 위한 저전력, 고효율 프로세서를 위해 ARM ISA를 개발했다.

또한 다양한 응용 분야에 맞춘 ISA가 필요하다. 예를 들어, 서버용 프로세서는 높은 연산 성능과 멀티스레드 처리를 중시하고, 모바일 프로세서는 전력 효율성과 낮은 발열을 중시한다. 때문에 각 응용 분야에 맞춘 최적화된 ISA를 필요로 한다.

특정 ISA는 소프트웨어와의 호환성 및 생태계를 형성한다. 예를 들어, x86 ISA는 오랫동안 사용되어 온 소프트웨어 생태계와의 호환성을 유지하기 위해 지속적으로 발전하고 있다. 새로운 ISA를 도입하면 기존 소프트웨어와의 호환성을 유지하는 데 어려움이 있을 수 있다.

<BR><BR><BR><BR><BR><BR>

## ✅ ISA 구성 요소
앞서 언급한 ISA의 구성 요소를 살펴보면 다음과 같다.

-   **명령어 형식(Instruction Format)** : 각 명령어의 길이와 각 필드(Opcode, 오퍼랜드 등)의 위치를 정의한다.
-   **명령어 집합(Instruction Set)** : 프로세서가 이해하고 실행할 수 있는 모든 명령어의 목록을 정의한다. 산술 연산, 논리 연산, 데이터 전송, 제어 명령 등이 포함된다.
-   **레지스터 집합(Register Set)** : 프로세서가 사용할 수 있는 레지스터의 목록을 정의한다. 레지스터는 연산 중간 결과나 중요한 데이터를 일시적으로 저장하는 데 사용된다.
-   **주소 지정 모드(Addressing Modes)** : 명령어가 오퍼랜드를 참조하는 다양한 방식을 정의한다. 직접 주소 지정, 간접 주소 지정, 즉시 값, 레지스터 주소 지정 등이 포함된다.
-   **데이터 타입(Data Types)** : 프로세서가 처리할 수 있는 데이터의 종류를 정의한다. 정수, 실수, 문자, 논리값 등을 포함한다.
-   **인터럽트와 예외(Interrupts and Exceptions)** : 프로세서가 외부 신호나 오류 조건을 처리하는 방식을 정의한다.


<br><br><Br><Br><Br><Br>




## ✅ RISC와 CISC

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/RISC.jpg?raw=true">
</p>

**RISC**(Reduced Instruction Set Computer)와 **CISC**(Complex Instruction Set Computer)는 두 가지 주요 프로세서 설계 철학을 나타내는 용어이다. 각 설계 철학은 명령어 집합(Instruction Set)과 프로세서 아키텍처에 대한 다른 접근 방식을 특징으로 한다.

<BR><BR>


### 📌 RISC ( Reduced Instruction Set Computer )
<hr>

- 비교적 적은 명령어를 가져 구조가 좀더 간단하다.
- 적은수의 명령어로 Instruction Set을 이루므로 속도가 빠르다.
- 일반적으로 많이 쓰이는 명령어를 주로 가진다.
- **메모리에 접근하는 명령어**로 **load와 store**만을 가진다!
- 대표적으로 MIPS 가 있다.



### 📌 CISC (Complex Instruction Set Computer )
<hr>

- 1000개가 넘는 Instructions를 가진다.
- 명령어를 해석하는데 시간이 오래걸리며 명령어 해석에 필요한 회로도 복잡하다.
- 10여개가 넘는 addressing mode를 가진다.
- 대표적으로 x86 이있다.



<br><br>

### 📌주요 차이점 
<hr>

-   **명령어 수**: RISC는 적은 수의 명령어를, CISC는 많은 수의 명령어를 가진다.
-   **명령어 실행 시간**: RISC는 대부분의 명령어가 동일한 시간에 실행되도록 설계되었지만, CISC는 명령어마다 실행 시간이 다를 수 있다.
-   **파이프라이닝**: RISC는 파이프라이닝에 최적화되어 있어 고속 처리가 가능하다. CISC는 명령어의 복잡성 때문에 파이프라이닝이 어렵다.
-   **메모리 접근**: RISC는 load와 store 명령어를 통해서만 메모리에 접근할 수 있다. CISC는 다양한 명령어를 통해 메모리에 접근할 수 있다.

이러한 차이점 때문에, RISC는 높은 성능과 효율성을, CISC는 복잡한 명령어 처리를 목표로 한다. 각 아키텍처는 특정 응용 분야와 목적에 따라 장단점이 있다.

<br><br><br><br><br><br>

## ✅ 결론

이번 포스팅에서는 Instruction Set과 Instruction Set Architecture(ISA)의 개념을 살펴보고, RISC와 CISC의 차이점을 알아보았다. ISA는 컴퓨터 시스템의 중요한 구성 요소로, 프로세서가 실행할 수 있는 명령어 집합과 이 명령어들이 실행되는 방식을 정의한다. ISA는 하드웨어와 소프트웨어 간의 인터페이스 역할을 하며, 다양한 요소를 포함한다.

RISC와 CISC는 서로 다른 설계 철학을 바탕으로 명령어 집합을 구성한다. RISC는 단순하고 효율적인 명령어 세트를 통해 고속 처리를 목표로 하며, 파이프라이닝에 최적화되어 있다. 반면, CISC는 복잡한 명령어 세트와 다양한 주소 지정 모드를 통해 복잡한 작업을 효율적으로 처리할 수 있도록 설계되었다.

각 아키텍처는 특정 응용 분야와 목적에 따라 장단점이 있으며, 최적의 성능을 달성하기 위해 적합한 ISA를 선택하고 설계하는 것이 중요하다. 컴퓨터 아키텍처를 이해하고 설계하는 데 있어 ISA와 명령어 집합의 개념을 잘 이해하는 것이 필수적이다.




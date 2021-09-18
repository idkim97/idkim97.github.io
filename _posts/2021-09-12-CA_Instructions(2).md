---
title: "[컴퓨터 구조] 2. Instruction Set (2)"
date: 2021-09-12 03:00:00
categories:
- Computer Architecture
tags:
- Instruction
- Instruction Set Architecture
- ARMv8
---

지난 포스팅에 이어 ISA에 대해 작성하겠다. 마이크로프로세서마다 기계어 코드의 길이와 숫자 코드가 다르기 때문에 그 양이 방대하므로 본 포스팅에서는 **ARMv8**의 ISA에 대해서 다루도록 하겠다.

<br><br><br><br><br><br>

# Arithmetic Operations ( 산술 연산 )

add와 sub, 그리고 3개의 operands(피연산자)가 존재한다.
<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/ari.jpg?raw=true">
</p>
operands에는 두개의 source(b,c)와 한개의 destination(a)이 존재한다.

모든 산술연산은 이러한 형태를 가지고있다.

### Design Principle 1 : Simplicity favors regularity
여기서 첫번째 하드웨어 설계원칙을 살펴볼 수 있다. 모든 명령어가 operand를 3개씩 갖도록 제한함으로써 하드웨어를 단순하게 할 수 있다.


<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/ari2.jpg?raw=true">
</p>

위 그림에서 Arithmetic Operation의 산술 과정을 살펴보자.

**C 코드 :** 
```c
f = ( g + h ) - ( i + j )
```
컴퓨터는 위의 코드를 다음과 같이 처리한다.

t0 라는 임의의 변수에 g + h 값을 저장하고 t1이라는 변수에 i + j를 저장한다.

그후 sub operation은 t0 - t1을 수행해 결과를 출력한다.

<br><br><br><br><br><br>

## The Stored-Program Concept

위의 산술 연산자 뿐만아니라 다른 연산자를 실행할때 컴퓨터가 데이터를 저장하는 방법에 대해 알아보자.

> **Instructions와 Data는 Binary(2진법)으로 Main Memory에 저장된다.**

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/ari3.jpg?raw=true">
</p>

<br><br><br><br><br><br>

본 노이만 컴퓨터 구조에서 반드시 알아둬야할 몇가지 note를 보자
<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/ari4.jpg?raw=true">
</p>

<br><br><br><br><br><br>

## Register Operands
**레지스터는 프로세서 내부적으로 관리하는 저장공간**으로, 데이터를 담을 수 있는 가장 기본적인 단위이다. 일반적으로 레지스터의 크기는 **32bit**이다.

그러나 본 포스팅에서 다룰 LEGv8같은 경우는 32,64bit의 레지스터를 둘다 가지며

**64-bit 데이터는 "double word" 라고 불리우며
32-bit 데이터는 "word" 라고 불린다.**

**이때 레지스터는 0~31로 번호가 매겨져 있음을 유의해야 한다.**

<br><br><br><br><br><br>

### Design Principle 2 : Smaller is Faster
LEGv8에서 레지스터의 수는 32개로 제한 됐으며 이는 더 빠른 연산을 위함이다.  
cf ) 메인메모리는 수백만개의 저장공간을 가진다.

<br><br><br><br><br><br>

LEGv8에서 제공하는 레지스터의 역할은 다음과 같다.

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/re1.jpg?raw=true">
</p>

**X0 ~ X7 : Procedure arguments / results**  

**X9 ~ X15 : 임시적으로 값을 저장**  

**X19 ~ X27 : 레지스터 저장**  

**XZR ( = X31 ) : 절대상수 0을 항상 갖고있음**  

<br><br><br><br><br><br>

레지스터 피연산자의 예시를 보자.

**C코드 :**
```c
f = ( g + h ) - ( i + j )
```

이때 f, ... , j 는 X19, X20, X21, X22, X23 에 각각 저장된다.
이를 LEGv8 내부에서 인식하는 코드로 살펴보면 다음과 같다.



> **ADD X9, X20, X21**   
> **ADD X10, X22, X23**   
> **SUB X19, X9, X10**  

<BR><BR><BR><BR><BR><BR>

## Register vs Main Memory

레지스터는 32비트 혹은 64비트의 저장소를 32개 가지므로 128B / 256B 의 크기를 가진다.

메모리는 훨씬 큰 데이터를 저장할 수 있다.

레지스터는 CPU내에 존재하는데 더 빠른 데이터 처리를 위해 존재한다. CPU가 메모리에 접근할 때마다 **BUS를 통해 이동하는 시간 + 메모리 처리 시간**이 소요되는데 이를 절약하기 위해 레지스터에 필요한 데이터를 미리 저장해두고 필요할 때마다 메모리에 접근하지 않고 레지스터에서 데이터를 빼가는 구조이다.

이런 구조속에서 메모리와 레지스터 간에 데이터를 전달하기 위해서 우리는 **data transfer instruction**가 반드시 필요하다. 이때 사용되는 명령어는 단 두개! **load와 store**가 있다.

> **load : memory -> register**   
> **store : register -> memory**  

이때 load와 store를 하기 위해선 반드시 **Memory Operands(Address)**가 필요하다.

<br><br><br><br><br><br>

## Memory Operands

 - **보통 메모리는 혼합된 데이터 ( 배열, 구조체, 동적할당 등 )을 위해 사용된다.** 
 - **Arithmetic Operation을 위해 Register Operand를 사용하려면,** 
 - **가장 먼저 Memory의 데이터를 Register로 Load 해야한다.**
 - **그리고 Register에서 연산하고, 결과 값을 다시 Memory에 Store한다.** 
 - **이런 구조를 Load - Store 구조라고 한다.**

<br><br><br><br><br><br>

## Memory Operand Example

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/mo1.jpg?raw=true">
</p>

 **1. h는 X21 레지스터에 저장되어 있고, 배열 A의 base address는 X22에 있다.**  

 **2. 먼저 A[8]을 가져오기 위해 Load 명령어를 실행한다.**  

 **3. A의 base address인 X22 레지스터부터**  

 **4. 배열의 8번째 값을 load하기 위해 8byte x 8번째 = 64의 byte offset을  X9라는 임시레지스터에서 Load한다.**  

 **5. X9에 저장된 A[8]과 X21에 저장된 h를 더해 X9에 다시 저장한다.**  
 
 **6. X9에 h + A[8]의 값인 A[12]를 Store 한다.**  

<br><br><br><br><br><br>

## Immediate Operands
우리가 상수를 더하고자 할때는 Instruction을 Load하는 과정을 거치지 않고 직접 더해버린다.

**C코드 :** 
```c
x = x + 4
```
이를 LEGv8에서는 다음과 같이 컴파일한다.

> **ADDI X22, X22, #4**

<br><br><br><br><br><br>

### Design Principle 3 : Make the common case fast
-> 작은 상수는 common에 해당한다.  
-> Immediate Operand는 불필요한 Load를 피한다!  

<br><br><br><br><br><br>

## The Constant Zero
LEGv8의 31번째 레지스터는 XZR, 즉 절대상수 0이다.
이는 Common Operations에 굉장히 유용하다.

**C코드 :** 
```c
y = z
```

위와 같은 연산을 처리할때 또 다른 Instruction을 만들지말고 XZR을 활용할 수 있다.

> **ADD X10, X11, XZR 또는 ORR X10, X11, XZR**


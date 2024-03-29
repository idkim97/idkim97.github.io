﻿---
title: "[머신러닝] [Python] 5. Expectation-Maximization (EM)"
date: 2021-09-25 06:00:00
categories:
- Machine Learning
tags:
- Machine Learning
- Classification
- EM
---

# EM 알고리즘 이란
<hr>
Expectation-Maximization 알고리즘이란, 관측되지 않은 잠재변수에 의존하는 확률 모델에서 Maximum Likelihood ( 최대가능도 ) 나 Maximum A Posteriori ( 최대사후확률)을 갖는 모수의 추정값을 찾는 반복적인 알고리즘이다. 

EM 알고리즘은 모수에 관한 추정값으로 로그가능도 ( Log Likelihood )의 기댓값을 계산하는 **(E) 단계**와 이 기댓값을 최대화하는 모수 추정값들을 구하는 최대화 **(M) 단계**를 번갈아가면서 적용한다. 

처음보는 단어가 많아 그 정의를 정확하게 이해가기가 어려울 것이다. 예시를 들어 쉽게 설명해 보겠다.

<br><br><br><br><br><br>

> **상태를 아는 경우 (State Known)**

동전 A와 동전 B가 주어진다.  우리는 각 동전의 앞/뒷면이 나오는 확률을 모르고 이 확률을 구하고자 한다. 이때 두개의 동전 중 하나를 랜덤하게 선택한다. 동전 B가 먼저 선택되었다면 이를 열번 던져 앞면(H)과 뒷면(T)이 나오는 경우의 수를 차례대로 적는다. 이렇게 동전을 선택하고 열번 던지는 과정을 5번 반복한다. 

여기서 우리는 각 차수마다 동전 A를 선택했는지, B를 선택했는지 알고 있다. 이는 우리가 상태를 아는 경우에 해당한다. 이때 우리는 **각 동전의 앞/뒷면이 나올 확률**을 구하고 싶다. 이를 구하기 위해서는 Maximum Likelihood (최대 가능도) 를 사용한다. 다음과 같이 동전 A를 선택하였을 때 앞면이 나온 총 횟수 24와 뒷면이 나온 총 횟수 6을 사용하여 앞면이 나올 최대 가능도는 24/24+6 = 0.8을 구할 수 있다. 마찬가지로 동전 B의 앞면이 나올 확률 0.45를 구할 수 있다. 각 차수마다 어떤 동전을 선택했는지 아는 상태이기에, 한 차수에는 한 동전에 대한 가능성만을 계산한다.
<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/em1.png?raw=true">
</p>

<br><br><br><br><br><br>

> **상태를 모르는 경우 (State Unknown)**

위의 예시와 동일하지만 단 한가지가 다른 경우를 살펴보자. 똑같이 각 동전A와 동전 B의 앞/뒷면이 나올 확률을 구하고자 하지만 어떤 동전이 선택된지 모른채 동전을 던진다고 가정해보자. 우리는 **각 차수에 어떤 동전이 선택되었는지 추가로 추측**을 해야하며 각 동전의 확률을 구해야만 한다.

이를 해결할 수 있는 방법이 바로 **EM Algorithm**이다.
<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/em2.png?raw=true">
</p>

<br><br><br><br><br><br>

# EM 알고리즘

계속해서 위와 동일한 예시로 EM알고리즘을 구현하는 과정을 설명해보겠다.
5개의 동전이 있고 이를 랜덤으로 선택해 각각 열번씩 던진다고 가정하자. 이때 우리는 각 차수마다 어떤 동전을 선택했는지 모르는 상황(State Unknown)이다.

<br><br><br>

## 초기값
EM알고리즘의 시작은 초기값 설정이다. 동전 A의 앞면이 나올 확률을 p=0.6, 동전 B의 앞면이 나올 확률을 q=0.5라고 임의로 설정한다. 이때 초기값은 **랜덤하게 결정**해주는 것이다.

<br><br><br>

## E-Step
주어진 추정값( 초기는 랜덤값 )을 사용하여 각 차수마다 동전의 앞/뒷면이 나올 확률을 구해본다. 그림에서 1번 차수는 HTTTHHTHTH로 주어졌으므로 이를 활용한다. 이때 **베이즈 정리**를 활용해 계산을 해야하는데 베이즈 정리에 대해 간단히 정리해보겠다.

 <br><br><br>

동전 A를 사용한 경우 *a = p<sup>5</sup> x (1-p)<sup>5</sup>= 0.6<sup>5</sup> x 0.4<sup>5</sup>*

동전 B를 사용한 경우 *b = q<sup>5</sup> x (1-q)<sup>5</sup>= 0.6<sup>5</sup> x 0.4<sup>5</sup>*

이때 H가 5번, T가 5번이 나왔으므로 각각 5제곱을 해준것이다.

1차수에서 동전 A를 사용했을 비율 : *a / ( a + b ) = 0.45*

1차수에서 동전 B를 사용했을 비율 : *b / ( a + b ) = 0.55* 

<br><br><br>

## M-Step
1차수에서 A를 사용했을 확률은 0.45, B를 사용했을 확률은 0.55로 측정됐다.

이때 1차수에서는 H가 5번, T가 5번 나왔으므로 

 - 동전 A인 경우 H 개수 : 0.45 x 5 = 2.2   
 - 동전 A인 경우 T 개수 : 0.45 x 5 = 2.2   
 - 동전 B인 경우 H 개수 : 0.55 x 5 = 2.8   
 - 동전 B인 경우 T 개수 : 0.55 x 5 = 2.8  

이에 더해 1차수부터 5차수까지 같은 과정을 반복하면 아래와 같은 결과가 나온다.


 - 동전 A인 경우 전체 H 개수 : 21.3   
 - 동전 A인 경우 전체 T 개수 : 8.6   
 - 동전 B인 경우 전체 H 개수 : 11.7  
 - 동전 B인 경우 전체 T 개수 : 8.4  

동전 A의 H가 나올 확률 p : 21.3 / (21.3 + 8.6) = 0.71
동전 B의 H가 나올 확률 q : 11.7 / (11.7 + 8.4) = 0.58

처음 추정한 p : 0.6 -> p : 0.71
처음 추정한 q : 0.5 -> q : 0.58로 변화된 것을 알 수 있다.

<br><br><br><br><br><br>

## E-M Step의 반복

E-M 과정을 한번 거쳤을 때 p : 0.71 q : 0.58이 나왔다.
이 과정을최종적으로 p와 q값이 크게 변하지 않을 때까지 반복한다.

<br><br><br><br><br><br>



---
title: "[머신러닝] [Python] 2. Decision Trees Regression (결정 트리 회기)"
date: 2021-09-09 02:00:00
categories:
- Machine Learning
tags:
- Machine Learning
- Decision Trees
- 
---

## Decision Tree For Regression

 - **ID3 알고리즘**
	 Categorical 속성값을 갖는 데이터에 대한 결정트리 학습
	 
 - **C4.5 알고리즘**
	 Categorical 속성값과 numerical 속성값을 갖는 데이터에 대한 결정트리 학습. ID3를 개선한 알고리즘이다.
	 
 - **CART 알고리즘**
	 Numerical 속성값을 갖는 데이터에 대한 결정트리 학습
	 
<br><br><br><br><br><br>

## ID3 알고리즘

1986년 로스 퀸란에 의해 개발된 ID3는 결정 트리의 가장 초창기 모델 중 하나이다. 결정 트리의 핵심은 변별력이 좋은 질문을 위에서 부터 하나하나 세팅하는 것인데, 이를 위해 ID3는 Information Gain과 엔트로피 그리고 Standard Deviation Reduction(표준편차)를 활용한다.
이 포스팅에서는 일반적으로 쓰이는 **엔트로피를 활용한 ID3가 아닌 Standard Deviation Reduction을 활용한 ID3를 소개하겠다.**

<br><br><br><br><br><br>

## Building a Decision Tree for Regression ( STEP )
데이터셋이 주어지고 해당 데이터셋을 SDR을 활용해 Decision Tree로 변환하는 과정을 보면서 설명해보겠다.
<br>

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/ex1.jpg?raw=true">
</p>

Outlook, Temp, Humidity, Windy가 Predictors이고 Hours Played가 Target 데이터이다. Target 데이터가 Numerical 데이터라는걸 잘 염두해 둬야한다.

<br><br><br><br><br><br>

### STEP 1
<hr>
**Root 노드를 어떤 predictor로 할지 결정해야만 한다.**

이를 위해선 **각 predictor의 SD(Standard Deviation)를 계산**해준뒤 

**SDR(Standard Deviation Reduction)이 가장 큰 요소**를 선택하면 된다.

이때 **SDR = (Target의 SD - 각 Predictor의 SD)**를 말한다.

<br><br><br><br><br><br>

### STEP 2
<hr>
**Target 데이터의 SD값을 계산해준다.**
<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/step2.jpg?raw=true">
</p>
주어진 데이터의 **SD = 9.32**이다.

이때 **SD는 Decision Tree의 Branching, 즉 분기를 위해 사용하는 값**이고,

**CV와 Count는 Branching을 계속 할지 말지를 결정하기 위해 사용하는 값,**

**Average는 Tree가 완성되면 Leaf Node에 적어줄 값**을 의미한다.

이 부분에 대해서는 STEP을 넘어가면서 차차 설명 하겠다.

<br><br><br><br><br><br>

### STEP 3
<hr>
**하나의 Predictor에 대한 SD값을 구한다. 이때 Predictor에 속한 모든 요소들의 SD를 각각 구한뒤 Count에 따라 가중치를 주고 Predictor의 SD를 구한다.**

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/step3.jpg?raw=true">
</p>

위의 그림처럼 하나의 Predictor (Outlook) 에서 Overcast, Rainy, Sunny 요소의 SD를 각각 구해준 뒤 가중치를 이용해 Outlook의 SD를 구해줘야 한다.

**SD(Outlook) = (4/14) x 3.49 + (5/14) x 7.78 + (5/14) x 10.87 = 7.66**

<br><br><br><br><br><br>

### STEP 4
<hr>
STEP3의 방식으로 다른 모든 Predictor들에 대한 SD를 구한뒤 Target과의 SDR을 구해서 SDR이 가장 큰값을 Root Node로 선정한다!

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/step4.jpg?raw=true">
</p>

Target인 Played Hours 의 SD가 9.32이고 Overcast 의 SD가 7.66이므로

Outlook의 SDR이 1.66으로 가장 크다.

이때문에 Outlook을 Root Node로 선정한다.
<br><br><br><br><br><br>

### STEP 5
<hr>
Outlook을 Root Node로 한 초기 Decision Tree는 다음과 같다.

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/step5.jpg?raw=true">
</p>

<br><br><br><br><br><br>

### STEP 6
<hr>
이제 똑같은 과정을 반복해서 Branching을 할건데 여기에 termination criteria( 가지치기 종료 기준)를 설정해주겠다.

1. **Coefficient of Deviation (CV)** 가 10%보다 작아지는 경우
2. 데이터의 수가 **Threshold(3)**보다 작거나 같아지는 경우

위의 기준은 사용자에 따라 다르게 설정할 수 있다.

<br><br><br><br><br><br>

### STEP 7
이제 각각의 요소에 대해서 Termination criteria를 비교하면서
 branching을 더 할지 말지 결정해 보는 과정이다.

**[ Overcast ]**

**SD(Hours Played) = 3.49**

**Average(Hours Played) = (43 + 43 + 52 + 44) / 4 = 46.3**

**CV(Hours Played) = SD / AVG x 100 = 3.49 / 46.3 x 100 = 7.53% ( 약 8 % )**

CV가 10%보다 작아지는 경우 Branching을 멈추는것으로 설정했으므로
Outlook의 Overcast같은 경우는 더이상 Branching을 멈추고
**Target value의 평균값인 46.3을 Leaf node로 가진다.**

Rainy와 Sunny는 CV가 10보다 크고 Count도 3보다 크므로 Branching을 계속한다.

<br><br><br><br><br><br>

### STEP 8
<hr>
나머지 Predictor인 Temp, Humidity, Windy에도 위와같은 과정을 거쳐 Branching 해준다.

<br><br><br><br><br><br>

### STEP 9
<hr>
최종 **Decision Tree**를 구현한다!
<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/step9.jpg?raw=true">
</p>


---
published: true
title: "[머신러닝] [Python] Support Vector Machine (SVM) 이란?"
date: 2021-09-24 06:00:00
toc: true
toc_sticky: true
toc_label: "SVM이란?"
categories:
- Machine Learning
tags:
- Machine Learning
- Classification
---

<br><br><br>


## ✅ SVM이란?

**서포트 벡터 머신(이하 SVM)**은 **결정 경계(Decision Boundary)**, 즉 분류를 위한 기준 선을 정의하는 모델이다. 그래서 분류되지 않은 새로운 점이 나타나면 경계의 어느 쪽에 속하는지 확인해서 분류 과제를 수행할 수 있게 된다.

결국 이  **결정 경계**라는 걸 어떻게 정의하고 계산하는지 이해하는 게 중요하다는 뜻이다. 예시를 보자.

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/svm1.png?raw=true">
</p>

데이터에 2개의 속성만 존재한다면 위와같은 분포가 나타날 것이다. 이때 두 데이터 분포를 완벽하게 나누는 선, 즉 **결정 경계**는 무수히 많이 존재할 수 있다.

<br><br><br><br><br><br>

## ✅ Decision Boundary ( 최적의 결정경계 )

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/svm2.png?raw=true">
</p>

그렇다면 **어떤 결정 경계가 가장 최적의 결정 경계일까**를 판단하는것이 관건이 된다. 위의 그림에서는 오른쪽 분포가 최적의 결정 경계를 나타낸다. 그 이유는 **경계를 기준으로 데이터가 더 멀리 분포되어 있기 때문이다**.
<br><br><br>

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/svm3.png?raw=true">
</p>
이제 간단히 용어정리를 하고 넘어가겠다. 두개의 cluster를 나누는 선을 **Separating line 혹은 Decision boundary 혹은 Classifier**
Separating line의 가장 경계부분에 있는 데이터를 **Support Vectors**
Separating line부터 Support Vectors까지의 영역을 **Margin** 이라고 부른다.

<br><br><br>

여기서 하나의 결론을 얻을 수 있다. **최적의 결정 경계는 마진을 최대화한다.**

또한 SVM알고리즘의 장점을 하나 알수 있다.

대부분의 머신러닝 지도 학습 알고리즘은 학습 데이터 모두를 사용하여 모델을 학습한다. 그런데 **SVM에서는 결정 경계를 정의하는 게 결국 서포트 벡터이기 때문에 데이터 포인트 중에서 서포트 벡터만 잘 골라내면 나머지 쓸 데 없는 수많은 데이터 포인트들을 무시할 수 있다. 그래서 매우 빠르다.**

<br><br><br><br><br><br>



앞서 들은 예시는 매우 이상적인 형태의 데이터 분포로 명확하게 Decision Boundary를 구할 수 있었다. 그러나 대부분의 데이터들은 명확하게 분류 할 수 없는 형태를 띈다.

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/svm4.png?raw=true">
</p>

이런 경우에는 두가지 방법을 사용해 분류 할 수 있다.

**1) Soft Margin
2) Kernel Trick**

<br><br><br><br><br><br>

## ✅ Soft Margin
소프트 마진은 두가지만 기억하면 된다.

**1. 마진을 크게  
2. 약간의 error를 허용하는것**  

이를 위해선 **"C"**라고 불리는 penalty parameter를 사용한다.

**C값이 클수록 하드마진( 오류 허용안함), 작을수록 소프트마진(오류를 허용함)을 의미한다.**

C값의 최적 값은 데이터마다 다르므로 여러가지 C값을 넣어보면서 모델을 검증하는 수밖에 없다.

scikit-learn에서는 SVM모델이 오류를 어느정도 허용할 것인지를 파라미터 C를 통해 지정할 수 있다. (기본 값은 1이다.)

```python
classifier = SVC(C=0.01)
```

<br><br><br>

## ✅ "C" 의 Tradeoffs
C를 사용해 마진의 너비를 조절하면 그 장단점이 분명하다.
<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/svm5.png?raw=true">
</p>

**C값을 줄이면 ( 소프트마진 ) 마진은 커지고 Error도 커진다.
C값을 늘리면 ( 하드마진 ) 마진은 작아지고 Error도 작아진다.**


<br><br><br><br><br><br>

## ✅ Kernel
<hr>
앞서 설명한 것들은 모두 Linear하게 분리가 가능한 데이터셋이였지만 만약 선형으로 분리 할 수 없는 데이터셋이 있다면 어떻게 해야할까?

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/svm6.png?raw=true">
</p>

위와같은 데이터는 어떤식으로는 선형으로 분류할 수 없는 데이터이다.
이때 사용 할 수 있는 개념이 **Kernel**이다.

**Kernel**이란 **데이터셋을 현재차원보다 고차원으로 변형시켜 선형으로 분류가 가능하게끔 만드는 것을 말한다**.

위의 데이터를 Kernel을 사용해 선형 분류한 최종 결과는 아래 그림과 같다.

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/svm7.png?raw=true">
</p>

<br><br><br>

scikit-learn에서 Kernel로 사용할 수 있는 함수는 대표적으로 4가지가 있다.

 - **Linear kernel**   
 - **Polynomial kernel**   
 - **RBF kernel (=Gaussian kernel)**   
 - **Sigmoid kernel**  

Linear역시 커널의 한종류이며 RBF커널에 대해 좀더 자세히 알아보자.

<br><br><br><br><br><br>

## ✅ RBF kernel

**RBF 커널** 혹은 **가우시안 커널**이라고 부른다.

RBF커널은 2차원의 점을 무한한 차원의 점으로 변환한다. 이 과정에서 상당히 복잡한 선형대수학이 사용되지만 생략하도록 하겠다.

이를 scikit-learn으로 구현한 코드는 아래와 같다.
```python
from sklearn.svm import SVC

classifier = SVC(kernel='rbf')
```
모델을 불러올 때 kernel의 기본값은 `'rbf'`로 설정되어 있고 이 외에도 `'linear'`, `'poly'`, `'sigmoid'` 같은 걸로 지정해줄 수도 있다.

<br><br><br><br><br><br>

## ✅ Gamma
커널을 사용할 때 C말고도 사용할 수 있는 파라미터가 하나 더 있는데 그게 바로 **감마(gamma)**이다.  `'rbf'`,  `'poly'`,  `'sigmoid'` 커널을 사용할 때 `gamma`를 설정해줘야 한다.
```python
classifier = SVC(kernel = "rbf", C = 2, gamma = 0.5)
```

`gamma`는 결정 경계를 얼마나 유연하게 그을지 정해주는 것이다. 학습 데이터에 얼마나 민감하게 반응할 것인지 모델을 조정하는 것이므로 C와 비슷하다고 볼 수 있다.

<br><br><br>

-   `gamma`값을  **높이면**  학습 데이터에 많이 의존해서  **결정 경계를 구불구불**  긋게 된다. 이는  **오버피팅**을 초래할 수 있다.

-   반대로  `gamma`를  **낮추면**  학습 데이터에 별로 의존하지 않고  **결정 경계를 직선에 가깝게**  긋게 된다. 이러면  **언더피팅**이 발생할 수 있다.


<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/svm8.png?raw=true">
</p>

위의 그림을 보면 감마에 대한 이해가 쉽다.
gamma = 3.0 인경우가 적당한 경우의 모습이라고 보면되고
gamma = 0.008인 경우 너무 낮아 경계가 직선형을 띄는 모습
gamma = 11.0인 경우 너무 높아 경계가 제대로 이루어지지 못한 모습이다.

<br><br><br><br><br><br>

## ✅ Multi-Class SVM
SVM은 Binary Classifier로 이진분류만 가능하지만 SVM을 이용해 다중 Class의 분류도 가능하다. 간단하게 예시를 들어 맛만보자.

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/svm9.png?raw=true">
</p>

원리는 간단하다. 세개의 클래스중 한개를 제외한 나머지를 하나의 클래스로 분류한뒤 이진분류를 진행해주면 된다.

<br><br><br><br><br><br>

## ✅ SVM의 장/단점

### 📌 Advantages

 - 마진이 명확하게 구분될때 잘 작동한다. 
 - 고차원 데이터 ( Feature가 많은 경우) 일때 효과적이다. 
 - Support Vector만 사용해 경계를 계산하므로 메모리를 효율적으로 쓴다.

### 📌 Disadvantages
- 데이터셋이 많은 경우 Training time이 오래걸린다.
- 데이터에 noise가 많은 경우 잘 작동하지 않는다
- 정확도를 직접적으로 보여주진 않는다.

<br><br><br><br><br><br>

## ✅ SVM을 사용할 때 유의점
1. 어떤 커널을 사용할 지 결정해야만 한다. 
	- `rgf`가 기본값으로 설정되어있다.
2. 커널 파라미터를 결정해야 한다.
	- gamma, 가우시안 커널의 σ
3. 하드마진 vs 소프트마진 어떤걸 사용할지 C를 이용해 결정해야 한다.


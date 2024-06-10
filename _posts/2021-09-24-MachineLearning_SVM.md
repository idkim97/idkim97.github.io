---
published: true
title: "[머신러닝] [Python] Support Vector Machine (SVM) 이란?"
date: 2021-09-24 06:00:00
toc: true
toc_sticky: true
toc_label: "SVM이란?"
use_math: true
categories:
- Machine Learning
tags:
- Machine Learning
- Classification
---

<br><br><br>


## ✅ SVM이란?

**서포트 벡터 머신(이하 SVM)**은 **결정 경계, 즉 분류를 위한 기준 선**을 정의하는 모델이다. SVM의 궁극적인 목표는 한 클래스의 데이터 점을 다른 클래스의 데이터 점과 가능한 한 가장 잘 구분해내는 초평면을 찾는 것이다. "가장 잘 구분한다" 라는 기준은 두 클래스 사이의 가장 큰 마진(공백)을 갖는 초평면으로 정의된다. 따라서 분류되지 않은 새로운 점이 나타나면 경계의 어느 쪽에 속하는지 확인해서 분류 과제를 수행할 수 있게 된다. 

따라서 SVM을 정확히 이해하기 위해서는 먼저  **결정 경계**를 확실히 짚고 넘어가야만 한다.

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/svm1.png?raw=true">
</p>

데이터에 2개의 속성만 존재한다면 위와 같은 분포가 나타날 것이다. 이때 두 데이터 분포를 완벽하게 나누는 선, 즉 **결정 경계**는 무수히 많이 존재할 수 있다. 때문에 우리는 두 데이터 분포를 가장 정확하게 분류할 수 있는 단 하나의 결정 경계를 파악할 필요가 있다. 이를 **Decision Boundary(최적의 결정경계)** 라고 부른다.

<br><br><br><br><br><br>

## ✅ Decision Boundary ( 최적의 결정경계 )

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/svm2.png?raw=true">
</p>

그렇다면 **어떤 결정 경계가 가장 최적의 결정 경계일까**를 판단하는 것이 관건이 된다. 위의 그림에서는 오른쪽 분포가 최적의 결정 경계를 나타낸다. 가장 직관적으로 받아들일 수 있는 이유는 **경계를 기준으로 데이터가 더 멀리 분포되어 있기 때문이다**.

<br><br><br>

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/svm3.png?raw=true">
</p>
원활한 이해를 돕기위해 간단하게 용어정리를 하고 넘어가겠다. 

- Cluster : 데이터가 모여있는 집합 또는 데이터군
- Decision boundry (Separating line 또는 Classifier) : 결정 경계
- Support Vectors : 결정 경계에서 가장 가까운 데이터 점
- Margin : 결정 경계로부터 Support Vectors 까지의 거리

<br><br><br>

여기서 하나의 결론을 얻을 수 있다. **최적의 결정 경계는 마진을 최대화한다.**

또한 SVM알고리즘의 장점을 하나 알수 있다. 대부분의 머신러닝 지도 학습 알고리즘은 학습 데이터 모두를 사용하여 모델을 학습한다. 그런데 **SVM에서는 결정 경계를 정의하는 게 결국 Support Vectors이기 때문에 데이터 포인트 중에서 서포트 벡터만 잘 골라내면 나머지 쓸 데 없는 수많은 데이터 포인트들을 무시할 수 있다. 그래서 매우 빠르다.**

<br><br><br><br><br><br>


## ✅ Soft Margin

앞서 살펴본 예시는 매우 이상적인 형태의 데이터 분포로 명확하게 Decision Boundary를 구할 수 있었다. 그러나 대부분의 데이터들은 명확하게 분류 할 수 없는 형태를 띈다.

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/svm4.png?raw=true">
</p>

위와 같은 cluster를 형성한 경우에는 직관적으로 Support Vectors를 구한다거나 Decision Boundry를 정의하기 어렵다. 이럴 때 SVM에서는 두가지 방법을 사용해 데이터를 분류 할 수 있다. 첫번째 방법은 **Soft Margin**을 사용하는 것이다.

**Soft Margin은 SVM에서 Margin은 최대화 하면서 약간의 오류를 허용하는 방법이다.** Soft Margin의 핵심 아이디어는 두가지이다.

1. **마진 최대화** : SVM의 기본 목표는 두 클래스의 마진을 최대화 하는 것이다. 즉, 결정 경계에서 가장 가까운 데이터(Supprot Vectors)와 결정 경계 사이의 거리를 최대화 한다.
  
3. **약간의 Error 허용**  : 모든 데이터가 완벽히 올바르게 분류되지 않아도 된다. 일부 데이터는 경계를 넘어서거나 마진 내에 위치할 수 있으며, 이를 통해 모델이 더 유연하게 데이터에 적응할 수 있게 한다.

위의 두가지 아이디어를 충족시키기 위해 SVM에서는 **"C"**라고 불리는 penalty parameter를 사용한다.

<BR><BR>

### 📌 C Parameter
<hr>

**C는 결정 경계에서 Error를 얼마나 허용할지를 결정하는 파라미터이다.** C의 값에 따라 모델의 유연성을 조절할 수 있다. 

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/svm5.png?raw=true">
</p>

- **C값이 클수록** : Hard margin(Error 허용 안 함). 즉 오류를 거의 허용하지 않는다. 모델은 데이터를 최대한 올바르게 분류하려고 하기 때문에 margin은 작아지고 overfitting(과적합)의 위험이 있다.

- **C값이 작을수록** : Soft maring(Error를 허용함). 즉 더 많은 오류를 허용한다. 모델은 margin을 최대화 하면서 일부 데이터가 잘 못 분류되는 것을 허용한다. 때문에 underfitting(과소적합)의 위험을 줄여준다.

<BR><BR>

### 📌 Soft Margin의 구현
<hr>

scikit-learn에서 Soft Maring을 구현하는 방법은 매우 간단하다. SVM 모델을 정의할 때 C 파라미터 값을 설정해주면 된다. 이때 C의 기본값은 1이다.

```python
from sklearn.svm import SVC 

# C 값을 0.01로 설정하여 소프트 마진을 사용 
classifier = SVC(C=0.01)
```

위 처럼 설정하면 모델은 일부 오류를 허용하면서도 margin을 최대화 하려고 한다. C 값의 최적 값은 주어진 데이터마다 다르기 때문에 여러가지 C값을 시도하면서 모델을 검증하는 것이 중요하다.



<br><br><br><br><br><br>

## ✅ Kernel

SVM의 기본 아이디어는 데이터를 고차원 공간으로 매핑하여 선형적으로 분리할 수 있는 초평면을 찾는 것이다. 앞서 설명한 것들은 모두 Linear하게 분리가 가능한 데이터셋이였지만 만약 선형으로 분리 할 수 없는 데이터셋이 있다면 어떻게 해야할까? 이때 **Kernel**이 중요한 역할을 수행한다.

<BR><BR>

### 📌 Kernel이란?
<hr>
**Kernel**은 데이터를 현재 차원보다 더 높은 차원으로 매핑하여 선형적으로 분리 가능하게 만드는 함수이다. Kernel 함수는 두 벡터의 내적을 계산하여 새로운 고차원 공간으로 변환된 결과를 나타내는데, 이를 통해 비선형적인 데이터도 선형적으로 분리할 수 있다.


<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/svm6.png?raw=true">
</p>

위와같은 데이터는 어떤식으로도 선형으로는 분류할 수 없는 데이터이다. 이때 우리는 SVM을 적용하기 위해 Kernel을 활용하여 2차원 데이터를 3차원 데이터로 변경할 수 있다. 그 결과는 다음과 같다.

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/svm7.png?raw=true">
</p>

위 그림처럼 2차원 데이터를 3차원 데이터로 변경한 뒤 데이터군을 분리할 수 있는 초평면을 구성함으로써 SVM을 사용할 수 있게되었다.

그러나 저차원 데이터를 분류하기 위해 고차원 데이터 형태로 실제 변환하는 작업은 효율적이지 못하고 데이터 분류 작업보다 어려울 수 있다. 데이터 분류를 위해 수행하는 변환 작업이 오히려 데이터 분류 작업 자체 효율을 낮출 수 있는 문제가 생긴다. 때문에 SVM에서는 이러한 고차원 공간으로의 변환 작업을 직접 수행하지 않고 효율적으로 계산할 수 있는 **Kernel Trick**을 제시한다.


<BR><BR>

### 📌 Kernel Trick이란?
<hr>

**Kernel Trick**은 데이터를 명시적으로 고차원 공간으로 변환하지 않고, Kernel 함수를 통해 변환된 고차원 공간에서의 내적을 효율적으로 계산하는 방법이다. Kernel Trick을 사용하면 계산 비용을 크게 줄이면서도 고차원 공간에서의 분리를 가능하게 한다.

즉, 고차원 공간의 특징을 계산하지 않고도 고차원 공간에서의 내적을 직접 계산할 수 있게 해주는 기법이다. 이를 통해 SVM은 실제로 고차원 공간으로 데이터를 변환하지 않고도 비선형 분류를 수행할 수 있다.

<BR><BR>

### 📌 대표적인 kernel 함수
<hr>

scikit-learn에서 지원하는 대표적인 kernel 함수는 크게 4가지가 있다.

1. **Linear Kernel**: 선형 커널은 원래 공간에서 선형 분리를 수행한다.
	
	$K(x,y)=x⋅y$

 2. **Polynomial kernel** : 다항식 커널은 입력 벡터의 다항식 형태로 매핑한다.

	$K(x,y)=(x⋅y+c)^d$ 


 3. **RBF kernel (=Gaussian kernel)** : 가우시안 커널이라고도 하며, 무한 차원 공간으로 매핑한다.  ($γ$는 커널의 폭을 조절하는 파라미터)

	$K(x,y)=exp(−γ∥x−y∥^2)$

 4. **Sigmoid kernel** : 시그모이드 커널은 신경망에서 사용하는 활성화 함수와 유사한 형태이다.

	$K(x,y)=tanh(αx⋅y+c)$


<BR><BR>

### 📌 RBF kernel
<hr>

kernel 함수 중 Default로 지정되어 가장 많이 사용되는 함수는 **RBF 커널**이다. RBF 커널 함수는 두 데이터 포인트 $x$와 $y$ 사이의 거리에 기초한다. 구체적으로, RBF 커널 함수는 다음과 같이 정의된다.

$K(x,y)=exp(−γ∥x−y∥^2)$

$∥x−y∥$는 두 벡터 $x$와 $y$사이의 유클리드 거리이고, $γ$는 커널의 폭을 조절하는 매개변수로 사용된다. 이때 $γ$는 반드시 양수값이다.


<BR><BR>

### 📌 RBF kernel의 파라미터 : $γ$
<hr>

RBF kernel을 사용할 때 매개변수로 사용되는 **$γ$(gamma)**는 데이터 포인트 사이의 유사성을 측정하는데 사용된다. 

`gamma`는 쉽게 표현하면 결정 경계를 얼마나 유연하게 그을지 정해주는 매개변수이다. 학습 데이터에 얼마나 민감하게 반응할 것인지 모델을 조정하는 것이므로 SVM의 C파라미터와 비슷하다고 볼 수 있다.

-   `gamma`값을  **높이면**  학습 데이터에 많이 의존하면서 데이터 포인트의 가까운 이웃을 위주로 같은 집단으로 분류하려고 한다. 한마디로 데이터에 매우 민감하게 반응한다. 결과적으로는 **결정 경계를 구불구불**  긋게 된다. 이는  모델의 **과적합(Overfitting)**을 초래할 수 있다.

-   `gamma`를  **낮추면**  학습 데이터에 별로 의존하지 않고  더 넓은 영역의 데이터 포인트를 같은 집단으로 분류하려고 한다. 한마디로 데이터에 민감하게 반응하지 않는다. 결과적으로는 **결정 경계를 직선에 가깝게**  긋게 된다. 이는 모델의  **과소적합(Underfitting)**을 발생 시킬 수 있다.


<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/svm8.png?raw=true">
</p>

위의 그림을 통해 감마 값에 대한 분류 추이를 살펴볼 수 있다.

gamma = 3.0 인 경우가 가장 이상적으로 분류된 정도라고 보이고,  
gamma = 0.008인 경우 너무 낮아 경계가 직선형을 띄는 모습,  
gamma = 11.0인 경우 너무 높아 경계가 제대로 이루어지지 못한 모습이다.


<BR><BR>

### 📌 RBF kernel 구현 - python
<hr>

이를 scikit-learn으로 구현한 코드는 아래와 같다. 먼저 scikit-learn 라이브러리를 설치해준다.

```python
pip install numpy matplotlib scikit-learn
```

SVM 모델 적용방법은 다음과 같다.

```python
from sklearn.svm import SVC

classifier = SVC(kernel='rbf')
```

모델을 불러올 때 kernel의 Default는 `'rbf'`이고, 이 외에도 `'linear'`, `'poly'`, `'sigmoid'` 와 같은 다른 함수로 지정해줄 수도 있다.

 `'rbf'`,  `'poly'`,  `'sigmoid'` 커널을 사용할 때는 `gamma`값을 설정해줘야 한다.
 
```python
classifier = SVC(kernel = "rbf", C = 2, gamma = 0.5)
```

이를 실제로 데이터와 연동하여 적용한 예제는 다음과 같다. scikit-learn의 datasets 모듈에서 테스트용 데이터셋인 Iris를 불러오고 전처리 후 SVM 모델을 적용해준다.

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn import svm, datasets
from sklearn.model_selection import train_test_split
from sklearn.metrics import classification_report, accuracy_score

# 데이터셋 생성
# iris 데이터셋을 사용합니다.
iris = datasets.load_iris()
X = iris.data[:, :2]  # 꽃잎 길이와 폭만 사용
y = iris.target

# 데이터셋을 훈련 세트와 테스트 세트로 나눔
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)

# SVM 모델 생성 및 학습 (RBF 커널 사용)
model = svm.SVC(kernel='rbf', gamma='scale')
model.fit(X_train, y_train)

# 테스트 데이터로 예측 수행
y_pred = model.predict(X_test)

# 모델 성능 평가
print("Accuracy:", accuracy_score(y_test, y_pred))
print("Classification Report:\n", classification_report(y_test, y_pred))

# 학습 데이터와 결정 경계 시각화
def plot_decision_boundary(X, y, model):
    h = .02  # 결정 경계의 해상도
    x_min, x_max = X[:, 0].min() - 1, X[:, 0].max() + 1
    y_min, y_max = X[:, 1].min() - 1, X[:, 1].max() + 1
    xx, yy = np.meshgrid(np.arange(x_min, x_max, h),
                         np.arange(y_min, y_max, h))
    Z = model.predict(np.c_[xx.ravel(), yy.ravel()])
    Z = Z.reshape(xx.shape)

    plt.contourf(xx, yy, Z, alpha=0.8)
    plt.scatter(X[:, 0], X[:, 1], c=y, edgecolors='k', marker='o')
    plt.xlabel('Sepal length')
    plt.ylabel('Sepal width')
    plt.title('SVM with RBF Kernel')
    plt.show()

# 결정 경계 시각화
plot_decision_boundary(X_train, y_train, model)

```
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


---
title: "[머신러닝] [Python] 1. Decision Trees (결정 트리)"
date: 2021-09-06 02:00:00
categories:
- 머신러닝
tags:
- 머신러닝
- 
---

## Decision Tree란

**Decision Tree (결정 트리, 의사결정트리, 의사결정나무)** 는 **분류(Classification)와 회귀(Regression)**이 모두 가능한 지도 학습 모델(Supervised Learning) 중 하나이다. 이러한 특성 때문에 **CART(Classification and Regression Tree) 알고리즘**의 하나이기도 하다. 결정 트리는 예/아니오 질문을 이어가며 학습하는데 예를 들어 매, 펭귄, 돌고래, 곰을 구분한다고 보자. 매와 펭귄은 날개가 있고, 돌고래와 곰은 날개가 없다. 날개가 있나요? 라는 질문을 통해 매, 펭귄 / 돌고래, 곰 으로 집단을 나눌 수 있다. 매와 펭귄은 날 수 있나요? 라는 질문을 통해 매 / 펭귄으로 나눌 수 있고 돌고래와 곰은 지느러미가 있나요? 라는 질문을 통해 나눌 수 있다.

<center>
![DecisionTree](https://github.com/idkim97/idkim97.github.io/blob/master/img/%EA%B2%B0%EC%A0%95%ED%8A%B8%EB%A6%AC.png?raw=true)</center>


이렇게 특정 기준에 따라 데이터를 구분하는 모델을 트리 모델이라고 한다. 한번의 분기마다 영역을 여러개로 나누고 이때 기준 하나하나를 노드(Node)라고 한다. 맨 처음 분류 기준을 Root Node라고 하며 맨 마지막 노드를 Leaf Node라고 한다.

<br><br><br><br><br><br>

## 트리를 어떻게 나눌까?
트리구조를 만들기 위해서는 고려해야할 사항이 몇가지 있다.
<br>

**1. 데이터를 나누는 기준**
		
**2. 언제 분기를 멈출 것인가?**

크게 보면 이렇게 두가지 인데 먼저 첫번째 고려사항부터 살펴보자.

<br><br><br><br><br><br>

### 데이터를 나누는 기준
<hr>
데이터를 나누는 기준은 **1. 데이터 타입**과 **2. 나눌 가짓수** 에 따라 나눈다.


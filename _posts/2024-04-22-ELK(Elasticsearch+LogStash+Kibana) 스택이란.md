---
permalink: /2024-04-22-ELK(Elasticsearch+LogStash+Kibana) 스택이란/
published: true
title: "[ELK] ELK(Elasticsearch+LogStash+Kibana) 스택이란? "
date: 2024-04-17 12:00:00
toc: true
toc_sticky: true
toc_label: "ELK(Elasticsearch+LogStash+Kibana) 스택이란"
categories:
- ELK
tags:
- ELK
- ElasticSearch
- LogStash
- Kibana
- Solr
---

<br><br><br>

## ✅ ELK 스택이란 ?

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/elk1.jpg?raw=true">
</p>


ELK 스택은 **Elasticsearch, LogStash, Kibana**의 로그 수집 및 시각화를 위한 세가지 오픈소스 프로젝트를 의미하는 약어이다. ELK 스택은 원래 독립적으로 개발되던 오픈소스 프로젝트였지만 각각의 구성 요소가 개별적으로 갖는 강점을 결합하여 효과적인 데이터 분석 및 모니터링 솔루션으로 거듭날 수 있었기에 함께 사용하게 됐다.


보통 메인은 **Elasticsearch**이다. **Elasticsearch**를 사용하고자 할 때, 다양한 소스에서 데이터를 편하게 수집하기 위해 **LogStash**를 함께 사용하고, 저장된 데이터를 편하게 시각화하고 분석하기 위해 **Kibana**를 함께 사용하는 순서를 띄긴한다. 굳이 이 3가지 프로젝트를 항상 함께 사용할 필요는 없다. 그러나 ELK 스택은 **Elastic**이라는 회사에서 함께 개발하고 세가지 프로젝트를 같이 사용하기에 편리하게끔 최적화 시켜놨기 때문에 다른 특별한 이유가 없다면 함께 사용하는게 좋다.

<br><br><br>

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/elk2.jpg?raw=true">
</p>

ELK 스택은 독립된 모듈로 사용할 수도 있고, 다른 오픈소스 프로젝트와 연동해도 무방하다. **Beats**라는 경량화된 데이터 수집 모듈과 함께 사용할 수도 있다. 또한 데이터를 안정적으로 버퍼링하고 전달하는 **Redis, Kafka, RabbitMQ** 등과 같이 사용할 수도 있다.

각각의 요소에 대한 포스팅은 따로 작성해뒀으니 여기서는 간략하게만 설명하고 넘어가도록 하겠다. 자세한 포스팅은 [ELK 카테고리](https://idkim97.github.io/categories/#elk)에 작성되어 있으니 참고하면 좋겠다.

<br>

### [📌 E : Elasticsearch](https://idkim97.github.io/2024-04-18-ElasticSearch%28%EC%97%98%EB%9D%BC%EC%8A%A4%ED%8B%B1%EC%84%9C%EC%B9%98%29%EB%9E%80/)
<hr>

**Elasticsearch는 실시간 분산형 검색엔진이다.** 주로 데이터 검색 및 분석을 위해 사용되고 데규모 데이터를 저장하고 실시간으로 검색할 수 있도록 설계되었다. Java 기반으로 제작되었고, 스키마가 없는 NoSQL 데이터베이스 계열에 속한다.


<br>

### [📌 L : LogStash](https://idkim97.github.io/2024-04-17-LogStash%28%EB%A1%9C%EA%B7%B8%EC%8A%A4%ED%83%9C%EC%8B%9C%29%EB%9E%80/)
<hr>

**LogStash는 다양한 소스에서 데이터를 수집하고, 변환하며, ElasticSearch 또는 다른 저장소에 전달하는 데이터 처리 파이프 라인 도구이다.** 로그파일, 이벤트로그, 메트릭 등 다양한 소스에서 데이터를 수집할 수 있고, 데이터를 필터링 및 정규화하여 Elasticsearch에 인덱싱 할 수 있다.


<br>

### [📌 K : Kibana](https://idkim97.github.io/2024-04-19-Kibana%28%ED%82%A4%EB%B0%94%EB%82%98%29%EB%9E%80/)
<hr>

**Kibana는 Elasticsearch에서 저장된 데이터를 시각화하고 분석하는 데 사용된다.** 사용자는 대시보드, 차트, 그래프 및 지도를 생성하여 데이터를 탐색하고 시각적으로 표현할 수 있다. Elasticsearch와 연동하여 거의 실시간으로 데이터를 시각화 할 수 있다는 큰 장점이 있다.

<br>

### 📌 Beats
<hr>
ELK스택이 더욱 널리 사용될 수 있었던 큰 요인중 하나가 Beats의 연동이다. 기존 LogStash는 데이터 수집과 데이터 입출력 및 필터 그리고 변환까지 담당하고 있었기에 오버헤드가 굉장히 크다. 그러나 데이터 수집만을 담당하는 Beats를 도입함으로써 애플리케이션의 성능에 영향을 미치지 않고 필요한 이벤트를 수집할 수 있다.

엘라스틱서치에서는 공식적으로 파일비트, 메트릭비트, 패킷비트, 윈로그비트, 오딧비트, 하트비트, 펑션비트, 카프카비트 등 상당히 많은 비트를 지원한다. 또한 온프레미스와 가상 머신뿐만 아니라 컨테이너와 쿠버네티스 환경에서도 사용이 가능하다.

<br>



<br><br><br><br><Br><br>

## ✅ ELK 스택을 왜 사용할까?

ELK스택은 Elasticsearch + Logstash + Kibana를 함께 사용하는 기술로 로그 수집 + 검색엔진 + 대시보드의 기능을 수행한다는 것을 알았다. 그럼 우리는 왜 굳이 ELK스택을 활용하는지 알아야 사용해야 할 필요성을 느끼고 당위성을 체감할 수 있다. 왜 많은 사람들이 ELK스택을 사용하는 것일까?

**왜 ELK스택을 사용할까**를 파악하는 가장 쉬운 방법은 ELK를 대체할 다른 기술은 어떠한지를 살펴보면 된다. ELK보다 더 좋고 편한 기술이 있다면 그걸 사용하지 ELK를 사용하진 않을 것이기 때문이다.

일단 ELK스택에서 가장 핵심기술은 **Elasticsearch**이다. 검색엔진으로 Elasticsearch를 선정한 뒤, 이에 대한 데이터를 편하게 수집하고, 대시보드를 보여줄 수 있는 편리한 툴을 찾은 결과 ELK스택이 탄생하게 된 것이다.


그렇다면 Elasticsearch를 대체할 수 있는 검색엔진을 살펴보면 된다. 현재 Elasticsearch와 견줄만한 검색엔진은 **Solr**과 **Splunk**가 있다.

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/elk3.jpg?raw=true">
</p>

Splunk는 유료로 제공되는 검색엔진이기 때문에 제외하고 Solr와 Elasticsearch에 대한 비교를 해보도록 하겠다. 

<br><br><br>

### 📌 Elasticsearch VS Solr
<hr>

 **Elasticsearch 장점**
 
 1. 사용 편의성 : RESTful API를 제공하여 쉽게 데이터를 쿼리하고 관리할 수 있다. 진입 장벽이 낮다.
 2. 확장성 : Elasticsearch는 scale-out이 가능하다. 클러스터에 노드를 추가함으로써 트래픽이 증가해도 성능을 유지할 수 있다.
 3. 생태계 : 오픈소스이기 때문에 활발한 커뮤니티와 다양한 플러그인, 도구가 제공된다.


**Elasticsearch 단점**
1. 메모리 사용 : Elasticsearch는 많은 메모리를 사용하므로, 메모리 제약이 있는 환경에서 조심해야 한다.
2. 복잡성 : 초기설정이 복잡하다. 클러스터링, 보안 설정등을 구성하는데 어려울 수 있다.



**Solr 장점**
1. 성능 : 일부 케이스에서 Elasticsearch보다 빠른속도를 보이는 경우가 있음
2. 기능 완성도 : 텍스트 분석, 튜닝 옵션 등 검색과 관련된 다양한 기능 지원

**Solr 단점**
1. 복잡성 : 초기 설정이 어렵고 러닝 커브가 높을 수 있다.
2. 생태계 : Elasticsearch보다 커뮤니티가 작고 플러그인 및 도구가 적다.


<br>

장단점을 보면 Elastiscsearch가 성능이 항상 더 좋아보이지는 않는다. 본인이 안정적인 환경속에서 빠른 검색 속도를 원한다면 오히려 Solr를 사용하는게 더 좋아보인다. 그럼에도 불구하고 Elasticsearch가 더 많은 사람들이 사용하는 검색엔진인 가장 큰 이유는 **생태계의 차이**라고 생각한다.

Elasticsearch는 이미 사용하기 너무 편리하게 생태계가 잘 갖춰져 있다. 

로그를 수집하고 가공하는 **[logstash](https://idkim97.github.io/2024-04-17-LogStash%28%EB%A1%9C%EA%B7%B8%EC%8A%A4%ED%83%9C%EC%8B%9C%29%EB%9E%80/)**
logstash가 무겁다면 로그 수집만을 목적으로 설계된 **Beats**
데이터 처리 이벤트에 대한 버퍼를 처리하는 **Kafka**
검색엔진의 결과를 다양한 UI로 제공하는 **[Kibana](https://idkim97.github.io/2024-04-19-Kibana%28%ED%82%A4%EB%B0%94%EB%82%98%29%EB%9E%80/)**
그리고 이 모두와 연동해서 사용할 수 있는 **[Elasticsearch](https://idkim97.github.io/2024-04-18-ElasticSearch%28%EC%97%98%EB%9D%BC%EC%8A%A4%ED%8B%B1%EC%84%9C%EC%B9%98%29%EB%9E%80/)**

이렇게 안정적인 생태계가 구축된 것이 Elasticsearch를 많은 사람들이 이용하는 가장 큰 이유가 아닐까 싶다.





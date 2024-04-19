---
permalink: /2024-04-18-ElasticSearch(엘라스틱서치)란/
published: true
title: "[ELK] ElasticSearch (엘라스틱서치)란? "
date: 2024-04-17 12:00:00
toc: true
toc_sticky: true
toc_label: "ElasticSearch (엘라스틱서치)란?"
categories:
- ELK
tags:
- ELK
- ElasticSearch
---

<br><br><br>



## ✅ Elasticsearch

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/elastic1.png?raw=true">
</p>

**Elasticsearch는 모든 데이터를 색인하여 저장하고 검색, 집계 등을 수행하며 결과를 클라이언트 또는 다른프로그램으로 전달하여 동작하는 검색엔진이다. 아파치 루씬(Apache Lucene)** 기반 루씬 개발자들을 중심으로 개발이 시작되었으며,  뛰어난 검색 능력과 대규모 분산 시스템을 구축할 수 있는 다양한 기능을 제공하며 설치 과정도 비교적 간편하다.

기존 관계데이터베이스 시스템에서는 다루기 어려운 전문검색(Full Text Search) 기능과 점수 기반의 다양한 정확도 알고리즘, 실시간 분석 등의 구현이 가능하다.

또한 다양한 플러그인을 통해 AWS, MS Azure와 같은 클라우드 서비스 그리고 Hadoop 플랫폼들과의 연동도 가능하다.

<br><br><br>
<br><br><br>
<br>

## ✅ Elasticsearch의 특징
ElasticSearch는 단독 검색엔진으로 쓰이기도 하지만, ELK 스택이라는 집합으로 **ElasticSearch + LogStash + Kibana** 와 함께 쓰이기도 한다. ELK는 접근성과 용이성이 좋기때문에 최근 많은 기업과 프로젝트에서 사용되고 있는데 이는 ElasticSearch의 다음과 같은 특징 덕분이다.

<br><br>

### 📌 오픈소스 기반
<hr>
Elasticsearch의 모든 제품은 Github([https://github.com/elastic](https://github.com/elastic))에 공개되어 있다. 누구나 자유롭게 사용 가능하고 개발할 수 있기 때문에 기술적으로 빠르게 성장해왔다. 또한 루씬이 자바로 만들어졌기 때문에 ElasticSearch 역시 자바로 코딩이 되어있다.

<br><br>

### 📌 실시간 분석 (real-time)
<hr>
Elasticsearch의 가장 큰 특징 중 하나는 실시간 분석 시스템이다. Elasticsearch 클러스터가 실행되고 있는 동안에는 계속해서 데이터가 입력(인덱싱)되고, 이와 동시에 실시간에 가까운 속도로 색인된 데이터의 검색 및 집계가 가능하다.

<br><br>

### 📌 전문(full text) 검색 엔진
<hr>
루씬은 기본적으로 역파일 색인(inverted file index)이라는 구조로 데이터를 저장한다. 때문에 루씬을 사용하는 **ElasticSearch도 역파일 색인 구조로 데이터를 저장하여 검색하는데 이를 전문 검색** 이라고 한다.

 <p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/elastic2.png?raw=true">
</p>

JSON 문서 기반 Elasticsearch는 내부적으로는 역파일 색인 구조로 데이터를 저장하고 있으나, 사용자의 관점에서는 JSON 형식으로 데이터를 전달한다. JSON형식은 간결하고 개발자들이 다루기 편한 구조로 되어 있어 색인 할 대상 문서를 가공 하거나 다른 클라이언트 프로그램과 연동하기에 용이하다.

또한 key-value 형식이 아닌 문서 기반으로 되어 있기에 복합적인 정보를 포함하는 형식의 문서를 있는 그대로 저장이 가능하며 사용자가 직관적으로 이해하고 사용할 수 있다. Elasticsearch에서 질의에 사용되는 쿼리문이나 쿼리에 대한 결과도 모두 JSON 형식으로 전달되고 리턴된다.

다만 JSON이 Elasticsearch가 지원하는 유일한 형식이기 사전에 입력할 데이터를 JSON 형식으로 가공하는 것이 필요한데, 대부분의 형식들은 LogStash에서 변환을 지원하고 있다.


<br><br>

### 📌 RESTful API
<hr>

현재 대규모 시스템들은 대부분 MSA를 기본으로 설계된다. MSA구조는 거의 대부분 REST API를 사용해 데이터를 주고받는데, ElasticSearch 역시 REST API를 기본으로 지원하여 REST API로 CRUD를 처리한다.


<br><br>

### 📌 멀티테넌시 (multitenancy)
<hr>
Elasticsearch의 데이터들은 인덱스(Index) 라는 논리적인 집합 단위로 구성되며 서로 다른 저장소에 분산되어 저장된다. 서로 다른 인덱스들을 별도의 커넥션 없이 하나의 질의로 묶어서 검색하고, 검색 결과들을 하나의 출력으로 도출할 수 있는데, Elasticsearch의 이러한 특징을 멀티테넌시 라고 한다.


<br><br><br>
<br><br><br>
<br>

## ✅ Elasticsearch의 클러스터링

Elasticsearch는 **대용량 데이터의 증가에 따른 스케일 아웃과 데이터 무결성을 유지하기 위한 클러스터링을 지원**한다. 항상 클러스터를 기본으로 동작하고 1개의 노드만 있어도 클러스터로 구성된다.

<br><br>

### 📌 여러 서버에 하나의 클러스터로 실행
<hr>

Elasticsearch의 노드들은 클라이언트와의 통신을 위한 HTTP 포트 (9200 ~ 9299), 노드 간의 데이터 교환을 위한 TCP 포트 ( 9300 ~ 9399 ) 총 2개의 네트워크 통신을 열어두고 있다. 일반적으로 1개의 물리 서버마다 하나의 노드를 실행하는 것을 권장한다. 3개의 다른 물리 서버에서 각각 1개씩의 노드를 실행한 클러스터는 다음 그림과 같다.

 <p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/elastic1.avif?raw=true">
</p>

<br><br><br>

하나의 물리적인 서버 안에서 여러개의 노드를 실행하는 것도 가능하다. 이런 경우 각 노드들은 차례대로 9200,9201... 순으로 포트를 사용하게 된다. 클라이언트는 9200, 9201 등의 포트를 통해 원하는 노드와 통신을 할 수 있다. 만약 서버1에서 두개의 노드를 실행하고, 또 다른 서버에서 한개의 노드를 실행하면 클러스터는 다음과 같이 구성된다.
 <p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/elastic3.png?raw=true">
</p>


<br><br><br>

**물리적인 구성과 상관 없이 여러 노드가 하나의 클러스터로 묶이기 위해서는 클러스터명 `cluster.name` 설정이 묶여질 노드들 모두 동일해야 한다. 같은 서버나 네트워크망 내부에 있다 하더라도 `cluster.name`이 동일하지 않으면 논리적으로 서로 다른 클러스터로 실행이 되고, 각각 별개의 시스템으로 인식된다.**


<br><br>

### 📌 하나의 서버에서 여러 클러스터 실행
<hr>

 <p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/elastic4.png?raw=true">
</p>

하나의 물리 서버에 3개의 노드를 실행시키는 상황이다. node-1과 node-2는 es-cluster-1이라는 이름의 클러스터에서 실행되고, node-3은 es-cluster-2라는 이름의 클러스터에서 실행된다. 설정은 `config/elasticsearch.yml` 파일에서 아래와 같이 입력한다.

```yml
cluster.name: es-cluster-1
node.name: "node-1"

cluster.name: es-cluster-1
node.name: "node-2"

cluster.name: es-cluster-2
node.name: "node-3"
```

node-1과 node-2는 es-cluster-1이라는 클러스터에 묶여있기 때문에 데이터 교환이 일어난다. node-1로 입력된 데이터는 node-2에서도 읽을 수 있고 반대도 가능하다. 그러나 node-3은 클러스터가 다르기 때문에 node-1,2에 입력된 데이터를 node-3에서는 읽을 수 없다. 

<br><br><br>
<br><br><br>
<br>



## ✅ 인덱스와 샤드 - Index & Shards
Elasticsearch 에서 단일 데이터 단위는 **도큐먼트(document)** 라고 한다. 그리고 이 도큐먼트를 모아놓은 집합을 **인덱스(Index)** 라고 한다. 인덱스는 **샤드(Shard)** 라는 단위로 분리되고 각 노드에 분산되어 저장된다. 다음 그림은 하나의 인덱스가 5개의 샤드로 저장되도록 설정한 예시이다.

 <p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/elastic5.png?raw=true">
</p>

<p class="notice--info">
⚠️ 샤드란 데이터를 분리하기 용이한 기준을 잡고 데이터를 분산저장 하는것
</p>

<br><br>

### 📌 프라이머리 샤드(Primary Shard)와 복제본(Replica)
<hr>

Elasticsearch는 샤드를 사용해 데이터를 저장하는 방식을 채택했기 때문에 샤드의 문제점이 그대로 발생한다. 샤드는 데이터를 분산 저장하기 때문에 **분산저장된 데이터중 일부가 유실되거나 사라질수 있어 복제본(Replica)을 생성해서 보관한다.** 때문에 Elasticsearch에서도 샤드에 대한 Replica를 항상 생성하여 데이터가 완전히 유실될 위험을 줄이고 있다.

Elasticsearch는 별도의 설정을 하지 않으면 **7.0버전부터는 Default로 1개의 샤드로 인덱스를 구성**하고, **6.x이하 버전에서는 Default로 5개의 샤드로 인덱스를 구성**한다. 클러스터에 노드를 추가하면 샤드들이 각 노드들로 분산되고 **Default로 1개의 Replica를 생성**한다. 이때 처음 생성된 샤드들을 **프라이머리 샤드(Primary Shard)** 라고 한다.

 <p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/elastic6.avif?raw=true">
</p>

<p class="notice--danger">
⚠️ 노드가 1개만 있는 경우 프라이머리 샤드만 존재하고 레플리카는 생성되지 않기 때문에 Elasticsearch에서는 데이터 가용성과 무결성을 위해 최소 3개의 노드로 구성할 것을 권장한다!
</p>

같은 쌍의 샤드와 레플리카는 동일한 데이터를 담고있고, **당연하겠지만 반드시 서로 다른 노드에 저장된다.** 데이터 무결성을 위해 레플리카를 생성한 것이기 때문에 당연히 서로 다른 노드에 저장함으로써 데이터를 항상 유지할 수 있어야 한다.

만약 예를들어 위 그림에서 시스템 다운이나 네트워크 단절로 인해 Node-3이 유실되면 이 클러스터는 Node-3의 0,4번 샤드를 잃어버린다. 그러나 여전히 Node-1의 0번 샤드, Node-2의 4번 샤드가 존재하기 때문에 데이터 유실 없이 클러스터의 사용이 가능하다. 

 <p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/elastic7.webp?raw=true">
</p>

**노드가 죽어 샤드를 잃으면 클러스터는 처음에 유실된 노드가 복구되기를 기다린다. 그러나 타임아웃이 지나 노드가 복구되지 않을거라 판단되면 유실된 샤드를 복제한다. 프라이머리 샤드가 유실되었다면 레플리카를 복제후 기존 레플리카로 취급당하던 샤드가 프라이머리 샤드가 되고, 레플리카가 유실된 경우 프라이머리 샤드를 복제해 레플리카를 다시 만들어낸다.** 

이렇게 Elasticsearch는 프라이머리 샤드와 레플리카를 통해 노드가 유실되어도 데이터의 가용성과 무결성을 보장한다.


<br><br><br>
<br><br><br>
<br>



## ✅ 마스터 노드와 데이터 노드 






<br><br><br><br><br><br><br>
> ✅ 참고 : https://esbook.kimjmin.net









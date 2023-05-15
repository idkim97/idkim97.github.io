---
permalink: /2023-05-11-CSS 기본 선택자, 캐스케이딩(Cascading), 박스 모델, 레이아웃/
title: "[TIL] [CSS] CSS 기본 선택자, 캐스케이딩(Cascading), 박스 모델, 레이아웃"
date: 2023-05-11 15:00:00
toc: true
toc_sticky: true
toc_label: "CSS 기본 선택자, 캐스케이딩(Cascading), 박스 모델, 레이아웃"
categories:
- TIL
tags:
- TIL
- CSS
- HTML
---
<br><br>


## ✅ CSS 기본 선택자 (Selector)

### 📌 1. 전체 선택자
**HTML페이지 내부의 모든 태그를 선택합니다.**

```css
* { margin : 0;
	padding : 0;
  }
```

전체선택자는 HTML 페이지 내부의 모든 요소에 같은 CSS 속성을 적용합니다. 때문에 margin이나 padding값을 초기화하는 등 기본값을 정해둘 때 사용합니다. 

<br><br>

### 📌 2. 태그 선택자 ( Type Selector )
**HTML요소를 직접 지칭하는 가장 간단한 선택자**
```css
/* css */
p { background : yellow;
	color : darkgreen;
	}
/* HTML */
<p> 태그 선택자 </p>
```

<BR><BR>

### 📌 3. 클래스 선택자 ( Class Selector )
**class 속성값을 가진 HTML요소를 찾아 CSS를 적용하는 선택자**
```css
/* CSS */
.class1 { background: yellow; color : darkgreen; }
div.class2 { background : darkgreen; color : yellowgreen; }

<!-- HTML -->
<p class="class1"> 클래스 선택자 </p>
<div class="class2"> 클래스 선택자 </div>
```

<Br><br>

### 📌 4. ID 선택자 ( ID Selector )
**id 속성값을 가진 HTML요소를 찾아 CSS를 적용하는 선택자**
```css
/* CSS */
#id1 { background: yellow; color : darkgreen; }
div#id2 { background : darkgreen; color : yellowgreen; }

<!-- HTML -->
<p id="id1"> 클래스 선택자 </p>
<div id="id2"> 클래스 선택자 </div>
```

<BR><BR>


### 🔔 클래스 선택자와 ID 선택자를 어떻게 구분해서 쓸까?

그냥 딱 결론만 말하면

**한 페이지내에서 여러번 반복되는 스타일은 클래스 선택자!**
**한 페이지내에서 단한번 쓰이는 스타일은 ID 선택자!**

클래스 선택자는 글자색이나 굵기 등 나중에 다른곳에서도 쓰일수 있는 스타일을 지정하고, ID선택자는 요소의 배치 방법을 지정할 때 자주 사용합니다.

class 속성은 두개이상의 속성값을 가질 수 있고, ID 선택자는 하나의 속성값만 가질 수 있습니다.

또한 ID 선택자의 우선순위가 클래스 선택자의 우선순위보다 높기때문에, 우선으로 적용되어야 할 스타일을 ID선택자를 사용하여 정의하는게 좋습니다.

<br><br>

### 📌 5. 그룹 선택자
**같은 스타일을 사용하는 선택자를 한꺼번에 정의하는 선택자**
**쉼표(,)로 구분해 여러 선택자를 나열**

```css
h1{ text-align : center; }
p { text-align : center; }
```

```css
/* 그룹 선택자로 변환 */
h1, p { texxt-align : center; }
```

<BR><BR><Br><br>



## ✅ 캐스케이딩 ( Cascading )


### 📌 캐스케이딩이란 ?
위에서 아래로 흐른다, 즉 계단식으로 적용된다는 의미로 사용됩니다.
선택자에 따라 여러 스타일이 적용될 때 스타일 충돌을 막기위해 우선순위에 따라 스타일을 결정할 수 있습니다.

<br><br>

### 📌 스타일 충돌을 막는 캐스케이딩의 원칙
1. **스타일 우선순위** : 스타일 규칙의 중요도와 적용 범위에 따라 우선순위가 결정되고 그 우선순위에 따라 위에서 아래로 스타일 적용

	🚩 `!important` 태그를 붙여주면 어떤 스타일보다 우선적용 된다! 

2. **스타일 상속** - 태그들의 포함 관계에 따라 부모 요소의 스타일을 자식 요소로, 위에서 아래로 전달


<BR><BR><br><br>



## ✅ CSS의 레벨 요소

### 📌 블록 레벨 요소 (Block Element)
요소를 삽입했을 때 혼자 한줄을 차지하는 요소.
요소의 너비가 100% -> 줄바꿈이 됨
ex ) `<div>, <p>` 등

```html
<div> div태그는 블록레벨 요소입니다. </div>
<p> p태그는 블록레벨 요소입니다.</p>
```

<br><br>

### 📌 인라인 레벨 요소 (Inline Element)
줄을 차지하지 않는 요소.
화면에 표시되는 콘텐츠 만큼만 영역을 차지하고 나머지 공간에는 다른 요소가 올 수 있음
ex ) `<img>, <string>, <span>` 등
```html
<div> div태그는 블록레벨 요소입니다. </div>
<p> p태그는 블록레벨 요소입니다. 
<span>그러나 span 태그는 인라인 레벨 요소이므로 블록 레벨 요소 중간에 넣을 수 있습니다.</span>
</p>
```

<br><br><br><br>

## ✅ CSS와 박스 모델

<p align="left">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/box-model.png?raw=true">
</p>

웹페이지에 배치하는 모든 HTML 요소는 3개의 층을 가진 사각형 구조를 띄고 있습니다. 
가장 외곽의 층을 `margin` 영역이라고 부르고 보통 주변에 위치한 다른 요소와의 상하좌우 간격을 두기위해 사용합니다.
그 바로 아래층을 `border` 영역이라고 부르고 요소의 테두리를 의미합니다. 색깔, 두께, 모양 등을 설정 가능합니다.
그 아래층은 `padding` 영역이라고 부르고 컨텐츠와 `border` 사이의 간격을 지정하기 위해 사용합니다.
마지막으로 `content` 영역은 컨텐츠의 `width`와 `height`를 지정할 수 있습니다. 


{% include codepen.html hash="wvYxOvX" title="css" %}


<br><br><br><br>

## ✅ 레이아웃

### 📌 display 속성
 요소의 배치 방법을 결정합니다.
 
|종류|설명  |
|--|--|
|block  | 인라인 레벨 요소를 블록 레벨 요소로 변경 |
|inline  | 블록 레벨 요소를 인라인 레벨 요소로 변경 |
|inline-block  |인라인 레벨 요소와 블록 레벨 요소의 속성을 모두 가지고 있고, 마진과 패딩 지정 가능  |
|none  |  해당 요소를 화면에 표시하지 않음  |

<br><br>

### 📌 float 속성
요소를 왼쪽이나 오른쪽에 떠있게 만듦

기본형 - ` float : left || right || none `

| 속성값  | 설명 |
|--|--|
| left | 해당 요소를 문서의 왼쪽으로 배치합니다. |
| right | 해당 요소를 문서의 오른쪽으로 배치합니다. |
| none | 좌우 어느쪽으로도 배치하지 않습니다. |

<br><br>
### 📌 position 속성
웹 문서 안에 요소들을 배치하기 위한 속성

기본형 - ` position : static || relative || absolute || fixed `

| 속성 값 | 설명 |
|--|--|
| *static | *요소를 문서의 흐름에 맞춰 배치합니다 |
| relative | 이전 요소에 자연스럽게 연결해 배치하되 위치를 지정할 수 있습니다. |
| absolute | 원하는 위치를 지정해 배치합니다. |
| fixed | 지정한 위치에 고정해 배치합니다. 화면에서 요소가 잘릴 수도 있습니다. |


---
permalink: /trumpnews-2/
title: "[Android] [TrumpNews] Jetpack Compose란?"
date: 2025-02-05 15:00:00
toc: true
toc_sticky: true
toc_label: "Jetpack Compose란?"
description: "Jetpack Compose에 대해 알아보자."
categories:
- Android
tags:
- Android
- TrumpNews
- Jetpack Compose
---
<br><br>


## ✅ XML vs Jetpack Compose

트럼프 뉴스 앱을 만들기 시작하면서 UI를 먼저 제작해보기 위해 방법을 찾아보았다. 널리 알려진 방법은 **XML 기반 UI**제작이었는데, 일전에 학부시절 경험을 떠올려보면 XML로 UI를 만드는 과정이 되게 고역이었던 기억이 떠오른다.

최근에는 이런 XML기반 UI의 단점을 보완하기 위해 **Jetpack Compose**라는 최신 UI 프레임워크를 사용한다고 한다. Kotlin 코드만으로 UI를 설계할수 있고, 재사용 가능한 컴포넌트 형태로 UI를 제작할수 있다하여 관련 자료를 찾아보았다.

같은 화면을 XML방식과 Jetpack Compose 방식, 두가지 버전으로 만든 예시가 있어서 이를 통해 객관적인 비교를 할 수 있었다.

먼저 XML로 작성한 코드를 살펴보자. 

<br>

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/jetpack1.webp?raw=true">
</p>

<br>

위의 코드는 UI 제작만을 위해 작성된 XML코드이고 안에 데이터를 매핑해주기 위해 또 아래와 같은 코드를 작성해야한다.

<br>

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/jetpack2.webp?raw=true">
</p>

<br>

다음은 Compose 방식으로 구현한 코드이다.

<br>

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/jetpack3.webp?raw=true">
</p>

<br>

끝이다. 그냥 위 이미지 3장을 보자마자 Jetpack Compose를 사용하기로 결정했다. 코드가 너무 간결해져서 안드로이드 문외한인 내가 선택하지 않을 이유가 없었다.. 좀더 구체적인 차이점을 표로 정리해보았다.

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/jetpack4.png?raw=true">
</p>



<br><br><br><br><br><br>
## ✅ Jetpack Compose란?

**Jetpack Compose**는 **안드로이드 UI를 선언형 방식(Declarative UI)으로 구성할 수 있도록 지원하는 최신 UI 프레임워크**이다.  기존의 **XML 기반 UI(View 시스템)** 대신 **Kotlin 코드만으로 UI를 설계**할 수 있다.

<br><br>

### 📌 선언형(Declarative) 방식이란?

선언형 UI 방식은 "어떻게 그릴지"가 아닌 "어떤 상태여야 하는지"를 선언하는 방식이다. 즉, 현재 UI의 상태를 선언하면, 프레임워크가 알아서 UI를 그려주는 방식이다.
- 명령형(Imperative) 방식 → "이 UI를 직접 변경하라"
- 선언형(Declarative) 방식 → "이 UI의 상태를 정의하면 자동으로 업데이트됨"

정확한 이해를 위해 예시를 함께 보자.

<br>

🔹 **명령형 방식**
```xml
<TextView android:id="@+id/textView" 
android:layout_width="wrap_content" 
android:layout_height="wrap_content"
android:text="Hello, World!" /> 

<Button android:id="@+id/button"
android:layout_width="wrap_content"
android:layout_height="wrap_content"
android:text="클릭하세요" />
```

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val textView = findViewById<TextView>(R.id.textView)
        val button = findViewById<Button>(R.id.button)

        button.setOnClickListener {
            textView.text = "버튼이 클릭됨!"
        }
    }
}
```

- `findViewById()`를 사용해 UI 요소를 가져온 후 **직접 변경해야 함**
- `button.setOnClickListener { ... }`에서 **명령을 내리는 방식**
- UI 요소가 많아지면 **수동으로 상태를 관리해야 함 → 코드가 복잡해짐**


<br><br>

🔹 **선언형 방식**
```kotlin
@Composable
fun MyScreen() {
    var text by remember { mutableStateOf("Hello, World!") } // 상태 정의

    Column {
        Text(text = text, fontSize = 24.sp) // 상태가 변경되면 UI가 자동 업데이트됨
        Button(onClick = { text = "버튼이 클릭됨!" }) { // 상태 변경
            Text("클릭하세요")
        }
    }
}
```
-   `text` 상태(`State`)만 변경하면 **자동으로 UI가 업데이트됨**
-   `Text(text = text)`에서 **UI를 "현재 상태(State) 기준"으로 선언**
-   **클릭 이벤트에서 text 값만 변경하면 알아서 화면이 갱신됨**
-   UI 상태를 따로 `setText()` 같은 함수로 변경할 필요가 없음


<br><br><br><br><br><br>

## ✅ Jetpack Compose의 특징

### 1️⃣ **선언형 UI (Declarative UI)**

기존 **XML + `findViewById()`** 방식과 달리 **UI를 코드로 직접 작성**가능!

<br>

### 2️⃣ **XML 없이 UI 구성 가능**
기존에는 UI를 **XML에서 작성한 후** Kotlin/Java에서 조작해야 했지만,  
Compose에서는 UI를 **Kotlin 코드 내에서 바로 작성**할 수 있다. 때문에 UI와 로직을 한 곳에서 관리가 가능하고 가독성이 좋아지며 코드가 간결해진다.

<br>

### 3️⃣ **상태 관리가 편리 (State Management)**

Compose는 `State`를 활용한 **반응형 UI (Reactive UI)** 를 지원한다.

```kotlin
@Composable
fun Counter() {
    var count by remember { mutableStateOf(0) }

    Column {
        Text("카운트: $count", fontSize = 24.sp)
        Button(onClick = { count++ }) {
            Text("증가")
        }
    }
}
```
- `remember`와 `mutableStateOf()`를 사용하면 상태 변화에 따라 자동으로 UI 업데이트
- 기존 `setText()` 같은 명령형 방식보다 더 직관적

<br>

### 4️⃣ **뷰 재활용 & 코드 간결화**

Compose에서는 **Composable 함수**를 사용하여 UI를 모듈화할 수 있다.

```kotlin
@Composable
fun Greeting(name: String) {
    Text(text = "Hello, $name!", fontSize = 20.sp)
}

@Composable
fun MainScreen() {
    Column {
        Greeting("Alice")
        Greeting("Bob")
    }
}
```

- **재사용이 가능** → `Greeting(name: String)`을 여러 곳에서 호출 가능
- **가독성이 좋아짐** → XML처럼 레이아웃을 따로 관리할 필요 없음


<br>

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/jetpack5.png?raw=true">
</p>

<br><br><br><br>

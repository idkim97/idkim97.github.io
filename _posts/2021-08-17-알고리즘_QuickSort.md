---
title: "[알고리즘] 퀵 정렬(Quick Sort) "
date: 2021-08-18 01:00:00
categories:
- Algorithm
tags:
- 알고리즘
- 정렬
---

## Goal
 
> Quick Sort 알고리즘을 이해한다.  
> Quick Sort 알고리즘의 특징   
> Quick Sort 알고리즘을 C언어로 구현한다.  

<br><br><br><br><br><br>

## Merge Sort 알고리즘 개념 요약

 - **분할 정복 (Divide and Conquer)** 알고리즘의 하나이다.

 - 분할 정복 알고리즘은 대게 **Recursion**을 활용하여 구현한다.

 - 간단한 과정 설명
	 - 배열의 한 요소를 아무거나 선택한다. 이를 **피벗(pivot)**이라고 한다.
	 - 피벗을 기준으로 피벗보다 작은 요소들은 모두 피벗의 왼쪽으로 옮기고 피벗보다 큰 요소들은 오른쪽으로 옮긴다.
	 - 피벗을 제외한 왼쪽 배열과 오른쪽 배열을 다시 정렬한다.
	 - 부분 배열들이 더이상 분할이 불가능 할때까지 이를 반복한다.

<br><br><br><br><br><br>

## Quick Sort 알고리즘 구체적 개념

- 하나의 배열을 **피벗(pivot)**을 기준으로 두 개의 배열로 분할하고 분할된 부분 배열을 정렬한 다음, 두 개의 정렬된 부분 배열을 합하여 전체가 정렬된 배열이 되게 하는 방법이다.


- Quick Sort는 다음과 같은 단계들로 이루어진다.
	- **분할(Divide)** : 입력 배열을 피벗을 기준으로 두개의 부분 배열로 분할한다.
	- **정복(Conquer)** : 부분 배열을 정렬한다. 이때 부분 배열의 크기가 0 또는 1이 될 때까지 **Recursive**하게 **Divide & Conquer**를 반복한다.
	- **결합(Combine)** : 정렬된 모든 배열들을 하나의 배열에 합병한다.

![enter image description here](https://github.com/idkim97/idkim97.github.io/blob/master/img/quicksort1.png?raw=true)


<br> <br> <br><br> <br> <br>

## Quick Sort 알고리즘 예제
![enter image description here](https://github.com/idkim97/idkim97.github.io/blob/master/img/quicksort2.png?raw=true)

<br>

**Ⅰ**. 피벗값을 입력 배열의 첫번째 데이터로 하자. ( 다른 값이여도 상관없다)  
**Ⅱ**.  2개의 인덱스 변수 (low, high)를 이용해 두개의 배열로 나눠준다.  
**Ⅲ**. 1회전 : 피벗이 5인 경우  

> ⅰ. low는 왼쪽에서 오른쪽으로 탐색하다가 피벗보다 큰 데이터(8)를 찾으면 멈춘다.  
> ⅱ. high는 오른쪽에서 왼쪽으로
> 탐색하다가 피벗보다 작은 데이터(2)를 찾으면 멈춘다.  
> ⅲ. low와 high가 가리키는 데이터를 서로 교환한다.  
> ⅳ. 이 과정은 low와 high가 서로 엇갈릴 때까지 반복한다.  

**Ⅳ**. 2회전 : 피벗(1회전의 왼쪽 배열의 첫번째 데이터)이 1인 경우

> 위의 과정(ⅰ~ⅳ)을 반복한다.

**Ⅴ**. 3회전 : 피벗(1회전의 오른쪽 배열의 첫번째 데이터)이 9인 경우

> 위의 과정(ⅰ~ⅳ)을 반복한다.


<br><br><br><br><br><br>

## Quick Sort C언어 코드
```c
# include <stdio.h>
# define MAX_SIZE 9
# define SWAP(x, y, temp) ( (temp)=(x), (x)=(y), (y)=(temp) )

// 1. 피벗을 기준으로 2개의 부분 리스트로 나눈다.
// 2. 피벗보다 작은 값은 모두 왼쪽 부분 리스트로, 큰 값은 오른쪽 부분 리스트로 옮긴다.
/* 2개의 비균등 배열 list[left...pivot-1]와 list[pivot+1...right]의 합병 과정 */
/* (실제로 숫자들이 정렬되는 과정) */
int partition(int list[], int left, int right){
  int pivot, temp;
  int low, high;

  low = left;
  high = right + 1;
  pivot = list[left]; // 정렬할 리스트의 가장 왼쪽 데이터를 피벗으로 선택(임의의 값을 피벗으로 선택)

  /* low와 high가 교차할 때까지 반복(low<high) */
  do{
    /* list[low]가 피벗보다 작으면 계속 low를 증가 */
    do {
      low++; // low는 left+1 에서 시작
    } while (low<=right && list[low]<pivot);

    /* list[high]가 피벗보다 크면 계속 high를 감소 */
    do {
      high--; //high는 right 에서 시작
    } while (high>=left && list[high]>pivot);

    // 만약 low와 high가 교차하지 않았으면 list[low]를 list[high] 교환
    if(low<high){
      SWAP(list[low], list[high], temp);
    }
  } while (low<high);

  // low와 high가 교차했으면 반복문을 빠져나와 list[left]와 list[high]를 교환
  SWAP(list[left], list[high], temp);

  // 피벗의 위치인 high를 반환
  return high;
}

// 퀵 정렬
void quick_sort(int list[], int left, int right){

  /* 정렬할 범위가 2개 이상의 데이터이면(리스트의 크기가 0이나 1이 아니면) */
  if(left<right){
    // partition 함수를 호출하여 피벗을 기준으로 리스트를 비균등 분할 -분할(Divide)
    int q = partition(list, left, right); // q: 피벗의 위치

    // 피벗은 제외한 2개의 부분 리스트를 대상으로 순환 호출
    quick_sort(list, left, q-1); // (left ~ 피벗 바로 앞) 앞쪽 부분 리스트 정렬 -정복(Conquer)
    quick_sort(list, q+1, right); // (피벗 바로 뒤 ~ right) 뒤쪽 부분 리스트 정렬 -정복(Conquer)
  }

}

void main(){
  int i;
  int n = MAX_SIZE;
  int list[n] = {5, 3, 8, 4, 9, 1, 6, 2, 7};

  // 퀵 정렬 수행(left: 배열의 시작 = 0, right: 배열의 끝 = 8)
  quick_sort(list, 0, n-1);

  // 정렬 결과 출력
  for(i=0; i<n; i++){
    printf("%d\n", list[i]);
  }
}
https://gmlwjd9405.github.io/2018/05/10/algorithm-quick-sort.html
```
코드 출처 : [https://gmlwjd9405.github.io/2018/05/10/algorithm-quick-sort.html](https://gmlwjd9405.github.io/2018/05/10/algorithm-quick-sort.html)

<br><br><br><br><br><br>


## Quick Sort 라이브러리를 활용한 C언어 코드
```c
#include <stdio.h>
#include <stdlib.h>    // qsort 함수가 선언된 헤더 파일

int compare(const void *a, const void *b)    // 오름차순 비교 함수 구현
{
    int num1 = *(int *)a; // void 포인터를 int 포인터로 변환한 뒤 역참조하여 값을 가져옴
    int num2 = *(int *)b; // void 포인터를 int 포인터로 변환한 뒤 역참조하여 값을 가져옴

    if (num1 < num2) // a가 b보다 작을 때는
        return -1;      // -1 반환
    
    if (num1 > num2)    // a가 b보다 클 때는
        return 1;       // 1 반환
    
    return 0; // a와 b가 같을 때는 0 반환
}

int main()
{
    int numArr[10] = { 8, 4, 2, 5, 3, 7, 10, 1, 6, 9 };    // 정렬되지 않은 배열

    // 정렬할 배열, 요소 개수, 요소 크기, 비교 함수를 넣어줌
    qsort(numArr, sizeof(numArr) / sizeof(int), sizeof(int), compare);

    for (int i = 0; i < 10; i++)
    {
        printf("%d ", numArr[i]); // 1 2 3 4 5 6 7 8 9 10
    }

    printf("\n");

    return 0;
}
```
코드 출처 : [https://dojang.io/mod/page/view.php?id=638](https://dojang.io/mod/page/view.php?id=638)

그때 그때 정렬 코드를 직접 짜는것은 공부 단계에서는 적절하겠지만 실제 코딩 테스트나 실무에서 적용하는데는 라이브러리를 활용하는것이 좋겠다.

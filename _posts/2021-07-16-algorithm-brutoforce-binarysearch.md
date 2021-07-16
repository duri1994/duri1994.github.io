---
title: "알고리즘 특강 Week2: 완전탐색과 이분탐색"
categories:	
  - Algorithm
tags:
  - Concept
---

## 탐색

#### 탐색이란

탐색이란, 많은 데이터 속에서 원하는 데이터를 찾는 행위이다.



#### 탐색의 종류

완전탐색, 이분탐색, 깊이우선탐색, 너비우선탐색, 문자열탐색, KMP, BM



## 완전탐색(Bruto Force)

#### 정의

- 컴퓨터의 성능을 이용하여 **가능한 모든 경우의 수를 탐색**하는 방법

- **효율성 관점에선 최악**의 방법이나 무조건 원하는 값을 탐색할 수 있다는 장점



#### 완전탐색 구현방법

[문제] 8장의 숫자카드 중 숫자가 8인 카드의 위치값을 반환하라.

###### 반복문

```python
def solution(card):
    for i in range(len(card)):
        if card[i] == 8:
            return i
        return -1
```



###### 재귀함수

```python
def solution(card, loc):
    if trump[loc] == 8:
        return loc
   	else:
        return(trump, loc+1)
```



## 이분탐색(Binary Search)

#### 정의

- 이진검색이라고도 표현하며, **오름차순으로 정렬된 리스트**에서 **특정 값의 위치를 찾는 알고리즘**
- **중간값을 선택**하여 **타겟 값과의 대소를 비교**하는 과정을 반복한다.



#### 이분탐색 구현방법

[문제] 오름차순으로 정렬된 9장의 숫자카드 중 숫자가 8인 카드의 위치값을 반환하라.

###### 예시코드

```python
def solution(card):
    left = 0
    right = len(card)-1
    while(left<=right):
        mid = (left+right)//2 # 중간 인덱스
        if card[mid] == 8:
            return mid
        elif card[mid] < 8:
            left = mid + 1
        elif card[mid] > 8:
            right = mid - 1
    return mid
```

- 중간 인덱스의 값을 target값과 비교한 뒤 일치하지 않으면 새로운 구간을 설정하는 방식
- 중간값과 target값의 크기를 비교하여 구간을 정하기 때문에 **반드시 정렬된 데이터**여야만 한다.


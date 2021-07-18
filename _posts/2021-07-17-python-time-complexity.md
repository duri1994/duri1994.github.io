---
title: "파이썬: 자료형 별 연산자의 시간복잡도(Big-O) 정리"
categories:	
  - Python
  - Algorithm
tags:
  - Python3
  - Concept
---

## 시간복잡도

 컴퓨터 과학에서 시간복잡도(Time complexity)란 프로그램의 입력값과 연산 수행 시간의 상관관계를 나타내는 척도이다. 일반적으로 알고리즘의 시간복잡도는 Big-O 표기법을 사용한다.

  Big-O 표기법의 형태들을 보면 다음과 같다.

| 표기 예시  | 형태             |
| ---------- | ---------------- |
| O(1)       | 상수             |
| O(log n)   | 로그             |
| O(n)       | 선형             |
| O(n log n) | 선형로그         |
| O(n^3)     | 다차(polynomial) |
| O(3^n)     | 지수             |
| O(n!)      | 팩토리얼         |

- 아래로 갈수록 더 복잡한 알고리즘이며 수행시간이 오래 걸린다.
- n의 크기가 커질 수록 시간복잡도 형태 간 수행시간 차이는 더욱 커지게 된다.



## 왜 시간복잡도를 정리하는가?

##### 효율적인 알고리즘 구현을 위해

- 지금까지 코딩테스트 문제를 풀 때, 단순하게 결과를 내기 위하여 효율성 측면을 하나도 고려하지 않았다.
- 따라서, 답은 나오지만 시간이 비효율적이거나 시간초과가 나오는 경우가 많았다.
- 시간복잡도를 고려한 적절한 자료형 및 함수를 선택하여 효율적 코드를 짤 필요가 있다.



##### 다양한 자료형 및 함수 활용을 위해

- 익숙한 자료형인 List를 제외하고는 잘 알지 못해 알고리즘 구현에 활용하지 못했다.
- 딕셔너리나 set 등 목표에 맞는 적절한 자료형을 자유자재로 활용할 필요가 있다.



## 자료형 별 함수들의 시간복잡도

#### 자료형 정리

| 자료형               | 특징                                        | 표현                       |
| -------------------- | ------------------------------------------- | -------------------------- |
| 리스트(List)         | - 순서 O, 수정 가능(mutable)                | list = [v1, v2, ...]       |
| 세트(Set)            | - 순서 X, unique, mutable                   | set = {v1, v2, ...}        |
| 딕셔너리(Dictionary) | - 순서 X, 키(immutable), 값(mutable)이 맵핑 | dict = {k1:v1, k2:v2, ...} |



#### 리스트의 메소드 별 시간복잡도

|      | Operation     | Example         | Big-O       | Notes                    |
| ---- | ------------- | --------------- | ----------- | ------------------------ |
| 1    | Index         | l[i]            | O(1)        | 인덱스로 값 찾기         |
| 2    | Store         | l[i] = val      | O(1)        | 인덱스로 데이터 저장     |
| 3    | Length        | len(l)          | O(1)        | 리스트 길이              |
| 4    | Append        | l.append(val)   | O(1)        | 리스트 뒤에 데이터 추가  |
| 5    | Pop           | l.pop()         | O(1)        | 리스트 마지막 데이터 pop |
| 6    | Clear         | l.clear()       | O(1)        | 빈 리스트로 만듬         |
| 7    | Slice         | l[a:b]          | O(b-a)      | 슬라이싱                 |
| 8    | Extend        | l.extend(l2)    | O(len(l2))  | 리스트 확장              |
| 9    | Construction  | list(...)       | O(len(...)) | 리스트 생성              |
| 10   | check ==, !=  | l1 == l2        | O(N)        |                          |
| 11   | Insert        | l[a:b] = ...    | O(N)        | 데이터 삽입              |
| 12   | Delete        | del l[i]        | O(N)        | 데이터 삭제              |
| 13   | Containment   | x in/not in l   | O(N)        | x값 포함 여부 확인       |
| 14   | Copy          | l.copy()        | O(N)        | 리스트 복제              |
| 15   | Remove        | l.remove(val)   | O(N)        | 리스트에서 val값 제거    |
| 16   | Pop 2         | l.pop(i)        | O(N)        | i번째 인덱스 값 pop      |
| 17   | Extreme value | min(l) / max(l) | O(N)        | 전체 데이터를 체크해야함 |
| 18   | Reverse       | l.reverse()     | O(N)        | 뒤집기                   |
| 19   | Iteration     | for v in l:     | O(N)        |                          |
| 20   | Sort          | l.sort()        | O(N log N)  | 파이썬 기본 정렬         |
| 21   | Multiply      | k*l             | O(k N)      |                          |



#### 세트의 메소드별 시간복잡도

|      | Operation      | Example        | Big-O            | Notes                   |
| ---- | -------------- | -------------- | ---------------- | ----------------------- |
| 1    | Add            | s.add(val)     | O(1)             | 집합 요소 추가          |
| 2    | Containment    | x in/not in s  | O(1)             | 포함 여부 확인          |
| 3    | Remove         | s.remove(val)  | O(1)             | 요소 제거(없으면 에러)  |
| 4    | Discard        | s.discard(val) | O(1)             | 요소 제거(없어도 에러x) |
| 5    | Pop            | s.pop()        | O(1)             | 랜덤하게 하나 pop       |
| 6    | Clear          | s.clear()      | O(1)             |                         |
| 7    | Construction   | set(...)       | O(len(...))      | 길이만큼                |
| 8    | check ==, !=   | s != t         | O(len(s))        |                         |
| 9    | <= or <        | s <= t         | O(len(s))        | 부분집합 여부           |
| 10   | Union          | s, t           | O(len(s)+len(t)) | 합집합                  |
| 11   | Intersection   | s & t          | O(len(s)+len(t)) | 교집합                  |
| 12   | Difference     | s - t          | O(len(s)+len(t)) | 차집합                  |
| 13   | Symmetric Diff | s ^ t          | O(len(s)+len(t)) | 여집합                  |
| 14   | Iteration      | for v in s:    | O(N)             | 전체 요소 순회          |
| 15   | Copy           | s.copy()       | O(N)             |                         |



#### 딕셔너리의 메소드별 시간복잡도

|      | Operation      | Example     | Big-O       | Notes                       |
| ---- | -------------- | ----------- | ----------- | --------------------------- |
| 1    | Store          | d[k] = v    | O(1)        | 집합 요소 추가              |
| 2    | Length         | len(d)      | O(1)        |                             |
| 3    | Delete         | del d[k]    | O(1)        | 해당 key 제거               |
| 4    | get/setdefault | d.get(k)    | O(1)        | key에 따른 value 확인       |
| 5    | Pop            | d.pop(k)    | O(1)        | pop                         |
| 6    | Pop item       | d.popitem() | O(1)        | 랜덤 데이터(key:value) pop  |
| 7    | Clear          | d.clear()   | O(1)        |                             |
| 8    | View           | d.keys()    | O(1)        | key 전체 확인               |
| 9    | Values         | d.values()  | O(1)        | value 전체 확인             |
| 10   | Construction   | dict(...)   | O(len(...)) | (key, value) tuple 갯수만큼 |
| 11   | Iteration      | for k in d: | O(N)        | 딕셔너리 전체 순회          |



## 정리

자료형 별로 동일한 기능을 하는 메소드라도 시간복잡도가 다르다는 것을 확인할 수 있다. 

따라서 문제상황에 따라 적절한 자료형을 선택하는 것이 중요하다.

예를 들어 **세트/딕셔너리**는 **삽입,제거,탐색,포함여부 확인 메소드**가 O(1)이라 O(N)인 리스트보다 유리하다.

반면 **리스트**는 **순서가 있거나 index로 요소에 접근할 필요가 있을 때** 활용하면 좋다.


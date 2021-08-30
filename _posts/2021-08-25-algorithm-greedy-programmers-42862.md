---
title: "알고리즘 특강 Week9: 탐욕법 + [프로그래머스] #42862 체육복"
categories:	
  - Algorithm
  - CodingTest
tags:
  - Concept
  - Python3
---

## 탐욕법

- 그리디 알고리즘이라고도 불리며, 매 순간 최적의 선택을 하며 적합한 결과를 도출하는 알고리즘이다.
- 동적계획법이 지나치게 많은 작업을 수행하는 단점이 있어, 이를 보완하는 개념으로 보기도 한다.
- 주의할 점은, 매 순간의 최적의 선택이 곧바로 전체적인 최적의 선택이 되는 것은 아니라는 점이다.
  - 따라서, 현재의 선택이 미래의 선택지에 영향을 미치는 경우에는 부적합한 알고리즘이다.
- 문제의 성격에 따라, 탐욕법이 적절한지, 동적계획법이 적절한지 판단하는 것이 중요하다.
  - 이것은 문제를 많이 접해보는 것이 가장 좋은 방법일 것이다.



## 문제

그러면, 실전문제를 통해 곧바로 탐욕법을 적용해보자.

[문제보기](https://programmers.co.kr/learn/courses/30/lessons/42862)

##### 문제설명

점심시간에 도둑이 들어, 일부 학생이 체육복을 도난당했습니다. 다행히 여벌 체육복이 있는 학생이 이들에게 체육복을 빌려주려 합니다. 학생들의 번호는 체격 순으로 매겨져 있어, 바로 앞번호의 학생이나 바로 뒷번호의 학생에게만 체육복을 빌려줄 수 있습니다. 예를 들어, 4번 학생은 3번 학생이나 5번 학생에게만 체육복을 빌려줄 수 있습니다. 체육복이 없으면 수업을 들을 수 없기 때문에 체육복을 적절히 빌려 최대한 많은 학생이 체육수업을 들어야 합니다.

전체 학생의 수 n, 체육복을 도난당한 학생들의 번호가 담긴 배열 lost, 여벌의 체육복을 가져온 학생들의 번호가 담긴 배열 reserve가 매개변수로 주어질 때, 체육수업을 들을 수 있는 학생의 최댓값을 return 하도록 solution 함수를 작성해주세요.

<br>

##### 제한사항

- 전체 학생의 수는 2명 이상 30명 이하입니다.
- 체육복을 도난당한 학생의 수는 1명 이상 n명 이하이고 중복되는 번호는 없습니다.
- 여벌의 체육복을 가져온 학생의 수는 1명 이상 n명 이하이고 중복되는 번호는 없습니다.
- 여벌 체육복이 있는 학생만 다른 학생에게 체육복을 빌려줄 수 있습니다.
- 여벌 체육복을 가져온 학생이 체육복을 도난당했을 수 있습니다. 이때 이 학생은 체육복을 하나만 도난당했다고 가정하며, 남은 체육복이 하나이기에 다른 학생에게는 체육복을 빌려줄 수 없습니다.

<br>

##### 입출력 예

| n    | lost   | reserve   | return |
| ---- | ------ | --------- | ------ |
| 5    | [2, 4] | [1, 3, 5] | 5      |
| 5    | [2, 4] | [3]       | 4      |
| 3    | [3]    | [1]       | 2      |

<br>

##### 입출력 예 설명

예제 #1
1번 학생이 2번 학생에게 체육복을 빌려주고, 3번 학생이나 5번 학생이 4번 학생에게 체육복을 빌려주면 학생 5명이 체육수업을 들을 수 있습니다.

예제 #2
3번 학생이 2번 학생이나 4번 학생에게 체육복을 빌려주면 학생 4명이 체육수업을 들을 수 있습니다.

<br>



## 풀이

```
1. lost와 reserve를 이용하여 최종적으로 여분의 체육복을 갖는 학생들의 리스트(finReserve)와 체육복을 갖고 있지 못한 학생들의 리스트(finLost)를 도출한다.
2. finLost의 학생들을 한명씩 꺼내와 인접한 번호의 학생들이 여분을 갖고 있는지 점검한다.
	- 만약, 인접번호 학생들이 여분이 있다면 해당 학생을 finLost에서 제외시킨다.
3. 최종적으로 finLost에 남은 학생들이 바로 체육수업을 들을 수 없는 학생들이다.
```

```python
def solution(n, lost, reserve):
    # 여분을 갖고 있는 학생과, 체육복을 잃어버린 학생들 정리
    # 참고: set끼리는 연산이 가능하며, 자연스럽게 중복된 학생들은 -연산으로 걸러진다.
    finReserve = set(reserve) - set(lost)
    finLost = set(lost) - set(reserve)

    # 인접 학생이 여분을 갖고 있는지 체크. 있다면 해당 학생을 finLost에서 제거
    for i in finReserve:
        if i - 1 in finLost:
            finLost.remove(i - 1)
        elif i + 1 in finLost:
            finLost.remove(i + 1)
	
    # 체육수업을 들을 수 있는 학생들 수 반환
    return n - len(finLost)
```

- set끼리는 +, - 연산이 가능한데 -연산 결과 중복된 원소들이 자연스럽게 제거된다.
  - 즉, 여분을 갖고 있는 학생이 도난당한 경우를 제외하겠다는 의미이다.(반대도 해당)



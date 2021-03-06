---
title: "[프로그래머스] #42577 전화번호 목록"
categories:	
  - CodingTest  
tags:
  - Programmers  
  - Python3
---

## 문제

[문제 보기](https://programmers.co.kr/learn/courses/30/lessons/42577)

##### 문제 설명

전화번호부에 적힌 전화번호 중, 한 번호가 다른 번호의 접두어인 경우가 있는지 확인하려 합니다.
전화번호가 다음과 같을 경우, 구조대 전화번호는 영석이의 전화번호의 접두사입니다.

- 구조대 : 119
- 박준영 : 97 674 223
- 지영석 : 11 9552 4421

전화번호부에 적힌 전화번호를 담은 배열 phone_book 이 solution 함수의 매개변수로 주어질 때, 어떤 번호가 다른 번호의 접두어인 경우가 있으면 false를 그렇지 않으면 true를 return 하도록 solution 함수를 작성해주세요.

<br>

##### 제한 사항

- phone_book의 길이는 1 이상 1,000,000 이하입니다.
  - 각 전화번호의 길이는 1 이상 20 이하입니다.
  - 같은 전화번호가 중복해서 들어있지 않습니다.

<br>

##### 입출력 예제

| phone_book                        | return |
| --------------------------------- | ------ |
| ["119", "97674223", "1195524421"] | false  |
| ["123","456","789"]               | true   |
| ["12","123","1235","567","88"]    | false  |

<br>

##### 입출력 예 설명

입출력 예 #1
앞에서 설명한 예와 같습니다.

입출력 예 #2
한 번호가 다른 번호의 접두사인 경우가 없으므로, 답은 true입니다.

입출력 예 #3
첫 번째 전화번호, “12”가 두 번째 전화번호 “123”의 접두사입니다. 따라서 답은 false입니다.

<br>

## 풀이

```python
# Idea: 전화번호를 sorted하면 문제 조건에 해당하는 두 번호(접두어, 접두어를 갖는 번호)는 반드시 붙어있을 수 밖에 없다.

def solution(phone_book):
    answer = True
    a = sorted(phone_book)
    print(a)
    for i, j in zip(a, a[1:]): # zip(sorted한 리스트의 인접한 두 값들을 tuple로 묶어낸다.)
        if j[:len(i)] == i: # j번호의 접두어로 i번호가 나타났다면
            answer = False
            break

    return answer
```

- 정렬을 통해 전화번호들을 정렬한다.
  - Why? 같은 접두어를 갖는 번호들은 정렬할 때 붙어있을 수 밖에 없으므로
- 정렬된 전화번호 목록에서 인접한 번호들 i,j를 zip함수를 통해 tuple로 묶어 for문을 돌린다.
  - 뒤에 나타나는 j번호에서 앞의 i번호가 접두어로 나타나는 경우 문제 요구대로 False를 반환한다.


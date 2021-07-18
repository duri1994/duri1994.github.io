---
title: "알고리즘 특강 Week2: 완전탐색 연습문제 - 소수찾기"
categories:	
  - Algorithm
tags:
  - 연습문제
  - Programmers
---

## 문제

https://programmers.co.kr/learn/courses/30/lessons/42839

##### 문제 설명

한자리 숫자가 적힌 종이 조각이 흩어져있습니다. 흩어진 종이 조각을 붙여 소수를 몇 개 만들 수 있는지 알아내려 합니다.

각 종이 조각에 적힌 숫자가 적힌 문자열 numbers가 주어졌을 때, 종이 조각으로 만들 수 있는 소수가 몇 개인지 return 하도록 solution 함수를 완성해주세요.

##### 제한사항

- numbers는 길이 1 이상 7 이하인 문자열입니다.
- numbers는 0~9까지 숫자만으로 이루어져 있습니다.
- "013"은 0, 1, 3 숫자가 적힌 종이 조각이 흩어져있다는 의미입니다.

##### 입출력 예

| numbers | return |
| ------- | ------ |
| "17"    | 3      |
| "011"   | 2      |

##### 입출력 예 설명

예제 #1
[1, 7]으로는 소수 [7, 17, 71]를 만들 수 있습니다.

예제 #2
[0, 1, 1]으로는 소수 [11, 101]를 만들 수 있습니다.

- 11과 011은 같은 숫자로 취급합니다.



## 풀이

- itertools 라이브러리의 permutations 메서드를 이용하여 만들 수 있는 모든 숫자 리스트를 만든다.
- 자연수가 자신의 제곱근 이하의 소수들로 나누어 떨어지지 않으면 소수라는 성질을 활용한다. 

```python
import math
from itertools import permutations

def chkPrime(num):
    if num < 2:
        return False

    a = int(math.sqrt(num))
    for i in range(2, a+1):
        if num % i == 0:
            return False
    return True

def solution(numbers):
    answer = 0
    nums = []
    for i in range(len(numbers)):
        a = permutations(numbers, i+1) # 반환값: tuple들로 이루어진 object
        							   # ex) {(1,2,3), (1,3,2), ...}
        for j in a:
            aa = "".join(j) # 각 tuple들의 문자들을 이어서 하나의 숫자 문자열로 만듬
            nums.append(int(aa))

    nums = list(set(nums)) # 중복된 숫자를 제거
    for i in nums:
        if chkPrime(i) == True:
            answer += 1
    return answer
```

- `chkPrime(num)`: 입력된 숫자가 소수이면 True, 아니라면 False를 반환하는 함수
  - 자신의 제곱근 이하의 모든 수에 대하여 완전탐색을 통해 나누어 떨어지지 않으면 소수



- `solution(numbers)`
  - 입력받은 문자열로 만들 수 있는 길이 1 ~ 문자열 최대길이의 숫자들을 nums list에 추가한다.
    - `itertools.permutation`의 반환값은 tuple들로 이루어진 object이다.
    - 따라서, for문을 통해 하나의 숫자로 이어붙이고  int형으로 바꿔준 뒤 list에 추가한다.
  - nums list의 중복된 숫자를 제거한다.
    - set는 중복을 허용하지 않는 자료형이므로 `set()`을 통해 중복을 제거한 뒤 다시 list로 바꾼다.
  - 정제된 list 안의 숫자들을 하나하나 소수 체크를 해준다.




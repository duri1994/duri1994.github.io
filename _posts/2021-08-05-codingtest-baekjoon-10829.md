---
title: "[Baekjoon] # 10829 이진수 변환"
categories:	
  - CodingTest  
tags:
  - Baekjoon
  - Python3
---

## 문제

[문제 보기](https://www.acmicpc.net/problem/10829)

##### 문제

자연수 N이 주어진다. N을 이진수로 바꿔서 출력하는 프로그램을 작성하시오.

##### 입력

첫째 줄에 자연수 N이 주어진다. (1 ≤ N ≤ 100,000,000,000,000)

##### 출력

N을 이진수로 바꿔서 출력한다. 이진수는 0으로 시작하면 안 된다.

##### 예제 입력 1 복사

```
53
```

##### 예제 출력 1 복사

```
110101
```

<br>

## 풀이

##### 재귀함수를 이용한 풀이

```python
# 재귀함수 풀이
def bina(n):
    if n == 0:
        return '0'
    elif n == 1:
        return '1'
    else:
        return bina(int(n/2)) + str(n%2)

n = int(input())
print(bina(n))
```

- 10진수를 이진화하는 알고리즘을 그래도 따라가며 재귀함수로 구현하면 된다.
  - 숫자를 계속해서 2로 나누는데, 나누어 떨어지면 0, 나누어 떨어지지 않으면 1을 뒤에서부터 추가한다.

<br>

##### 간단한 풀이

파이썬에선 내장함수 bin()을 이용하여 10진수를 간단히 이진화할 수 있다.

```python
# 내장함수 bin()을 이용한 풀이
n = int(input())
print(bin(n)[2:])
```


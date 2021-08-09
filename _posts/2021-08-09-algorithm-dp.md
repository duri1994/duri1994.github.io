---
title: "알고리즘 특강 Week7: 동적계획법"
categories:	
  - Algorithm
tags:
  - Concept
  - Python3
  - Dynamic Programming
---

## 동적계획법

Dynamic Programming(DP)

하나의 큰 문제를 **여러 개의 공통된 작은 문제로 나눈 뒤**에 그 **정답들을 결합하여 알고리즘을 푸는 과정**

- 쉽게 말하면 **규칙을 찾는 문제**
- 전체 문제를 작은 문제로 단순화, 이를 **점화식** 등 **재귀적인 구조**를 만들어 전체문제를 해결



##### 동적계획법 접근방법

1. Bottom-up
   - **작은 문제에서 시작**하여 큰 문제로 **확장하여 분석**하는 사고방식
   - **반복문**을 이용한 호출
2. Top-Down
   - **큰 문제로부터 거꾸로 거슬러 올라가** 작은 문제로 **분해하여 분석**하는 사고방식
   - **재귀함수**를 이용한 호출

<br>

##### 예제: 피보나치 수열

(1) **Bottom-up 방식**

- 우선 a1, a2 = 1,1 이므로 이를 list에 담는다.
- 차례차례 다음 수열값을 list에 담는다.
  - 초기값부터 천천히 큰 문제로 끌어가는 Bottom-up사고방식

```python
# Bottom-up(for문)
def fib(n):
	flist = [1,1]
    for i in range(2, n+1):
        flist.append(flist[i-2]+flist[i-1])
    return flist[-1] # 마지막 항 반환

    
```

(2) **Top-Down 방식**

- 최종결과 fib(n)의 값을 구하기 위하여 fib(n-1)과 fib(n-2)가 필요하다.
- 마찬가지로 fib(n-2)를 구하기 위하여 그 전 두 항이 필요하다.
  - 이 과정이 반복되므로, 결국 근본적으로는 처음 두 항이 필요하다. 
  - 결론적으로 fib(n) 결과값은 fib(1)과 fib(2)의 선형결합으로 이루어진다.

```python
# Top-Down(재귀함수)
def fib(n):
	if n <= 1:
		return 1
	else:
        return fib(n-1) + fib(n-2)
```

- 그러나 위의 경우 치명적인 문제점이 **중복되는 연산이 존재하여 효율성이 떨어진다는 것**이다.
  - 이를 보완하기 위해 나온 개념이 **Memoization**이다.

(3) **Memoization의 활용(딕셔너리)**

```python
def fib(n):
	if n in memo: # memo = 해시(딕셔너리)
        return memo[n]
    else:
        res = fib(n-1) + fib(n-2)
        memo[n] = res
        return res
```

<br>

##### 예제2: 이웃하지 않은 숫자들의 최대합 구하기

Q. data = [3,4,5,6,1,2,5] 에서 이웃하지 않는 숫자들의 합의 최대값은?

###### Bottom-up 사고방식 과정

- data = [3]인 경우
  - 최대값 3, 이를 배열의 0번째 index에 추가
- data = [3, 4]인 경우
  - 최대값 4, 이를 배열의 1번째 index에 추가
- data = [3, 4, 5]인 경우
  - 3+5 vs 4, 8을 배열의 2번째 index에 추가
- data = [3,4,5,6]인 경우
  - 4+6 vs 8(기존 최대값), 10을 3번째 index에 추가
- data = [3,4,5,6,1]인 경우
  - 1+8(전전 최대값) vs 10(기존 최대값), 10을 4번째 index에 추가
- data = [3,4,5,6,1,2]인 경우
  - 2+10(전전 최대값) vs 10(전 최대값), 12를 5번째 index에 추가

- data =  [3,4,5,6,1,2,5]
  - 5+10(전전 최대값) vs 12(전 최대값), 15를 6번째 index에 추가

<br>

위의 일련의 과정을 통해 작은 문제로부터 규칙을 파악하고 전체 문제를 풀어낼 수 있었다.

- M(n) = max(M(n-1), M(n-2)+a_n)
- 이 규칙을 파악하고 나면 쉽게 코딩을 할 수 있다.

```python
def solution(data):
	if len(data) == 1:
		return data[0]
    res = [data[0], max(data[0], data[1])] # Memoization List
    for i in range(2, len(data)):
        res.append(max(res[i-1], res[i-2]+data[i]))
   	return res[-1]   
```

<br>

## 나가며

동적계획법은 기존의 알고리즘 유형과 달리 고도의 사고력을 요구한다.

온라인상에 출제되어 있는 많은 코딩테스트 문제를 풀며 **로직이나 규칙을 찾아내는 연습이 필수적**일 것이다.


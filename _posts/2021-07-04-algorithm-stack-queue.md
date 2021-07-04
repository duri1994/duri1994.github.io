---
title: "알고리즘 특강 Week1: 스택과 큐 개념"
categories:	
  - Algorithm
tags:
  - Concept
  - Stack
  - Queue
---

# 스택(Stack)과 큐(Queue)

 알고리즘 자료구조의 기본이 되는 구조



## 1. 스택(Stack)

##### 개념

- 프로그래밍에서 목록 or 리스트에서 **접근이 한 쪽에서만 가능**한 구조
- 마치 '<u>책을 쌓는 것</u>'처럼 **맨 위에서만 입력, 확인, 추출**이 가능
- 기본원리: LIFO(Last-In, First-Out) - **후입선출**



##### 주요 함수

1. **push**: 스택 가장 **마지막 부분**에 자료 **추가**
2. **peek**: 스택 가장 **마지막 부분**의 자료 **확인**(<u>스택은 그대로 유지</u>)
3. **pop**: 스택 가장 **마지막 부분**의 자료 **추출**(<u>스택에서 그 자료가 제거됨</u>)



##### 파이썬으로 구현

```python
# list를 스택으로 활용
s = []
s.append(1)
s.append(5)
s.append(10) # 이상, 스택 push
print("my stack is:", s) 
print("popped value is:", s.pop()) # 스택 pop / .pop()은 python 내장함수.
print("my stack is:", s) 
print("peeked value is:", s[-1]) # 스택 peek
print("my stack is:", s) 
```

```
my stack is: [1, 5, 10]
popped value is: 10
my stack is: [1, 5] # pop으론 마지막 값이 제거됨(추출됨)
peeked value is: 5
my stack is: [1, 5] # peek으론 값이 제거되지 않음
```



##### 주요 활용 예시

- 웹 브라우저의 이전/다음 페이지
- 깊이우선탐색



## 2. 큐(Queue)

##### 개념

- 프로그래밍에서 목록 or 리스트에서 **접근이 양쪽에서 가능**한 구조
- 대기열 처럼, **한 쪽에선 자료가 입력**되고, 자료 **추출은 그 반대에서 발생**함(like <u>컨베이어벨트</u>)
- 기본원리: FIFO(First-In, First-Out) - **선입선출**



##### 주요 함수

1. **push**: 큐 가장 **마지막 부분**에 자료 **추가**
2. **peek**: 큐 가장 **처음 부분**의 자료 **확인**(<u>큐는 그대로 유지</u>)
3. **get**: 큐 가장 **처음 부분**의 자료 **추출**(<u>큐에서 그 자료가 제거됨</u>)



##### 파이썬으로 구현

```python
# 리스트로 큐 직접 활용
q = []
q.append(1)
q.append(5)
q.append(10)
print("queue:", q)
print("removed value:", q.pop(0)) # get / .pop(0): 인덱스가 0인 데이터를 추출
print("my queue is:", q)
print("peeked value is:", q[0])
print("my queue is:", q)
```

```
queue: [1, 5, 10]
removed value: 1
my queue is: [5, 10] # get은 queue의 맨 처음 값을 추출
peeked value is: 5
my queue is: [5, 10] # peek은 queue의 맨 처음 값을 확인(queue는 유지)
```



##### 주요 활용 예시

- 인쇄 대기열
- 너비우선탐색


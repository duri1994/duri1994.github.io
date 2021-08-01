---
title: "[프로그래머스] lv2 # 43165 타겟넘버"
categories:	
  - CodingTest  
tags:
  - Programmers  
  - Python3
  - DFS/BFS
---

# #43165 타겟넘버

## 문제
n개의 음이 아닌 정수가 있습니다. 이 수를 적절히 더하거나 빼서 타겟 넘버를 만들려고 합니다.
예를 들어 [1, 1, 1, 1, 1]로 숫자 3을 만들려면 다음 다섯 방법을 쓸 수 있습니다.

-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

<br/>

사용할 수 있는 숫자가 담긴 배열 numbers, 타겟 넘버 target이 매개변수로 주어질 때
숫자를 적절히 더하고 빼서 타겟 넘버를 만드는 방법의 수를 return 하도록 solution 함수를 작성해주세요.

##### 제한사항
주어지는 숫자의 개수는 2개 이상 20개 이하입니다.
각 숫자는 1 이상 50 이하인 자연수입니다.
타겟 넘버는 1 이상 1000 이하인 자연수입니다.

##### 입출력 예
numbers         target	return
[1, 1, 1, 1, 1]    3      	  5



## 풀이

```python
def solution(numbers, target):
    tree = [0] # 초기값
    for i in range(len(numbers)): # 반복문. numbers의 인덱스넘버가 곧 노드의 깊이이다.
        list = [] # 다음 자식 노드에 들어갈 계산결과들을 담는 리스트
        for j in range(len(tree)):
            list.append(tree[j] - numbers[i]) # 지금까지 계산값들에 다음 숫자 빼주기
            list.append(tree[j] + numbers[i]) # 더해주기

        tree = list # 부모 노드가 될 계산결과들을 tree에 담아준다.
        			# 반복문이 끝날 때에는 리프 노드의 계산값들을 담고 있을 것이다.

    answer = tree.count(target) # 리프 노드의 값들 중 target과 일치하는 것의 갯수를 반환

    return answer
```

- 이 문제는 **일종의 tree구조**로 풀어서 **BFS를 실시**하는 것으로 설명이 가능하다.

<br/>

##### tree 구조란?

- **트리 구조**는 그래프의 일종으로, **회로가 존재하지 않고, 서로 다른 두 노드(node)를 잇는 길이 하나뿐인** 그래프를 의미한다.
- 최상위 노드는 **뿌리(root) 노드**라고 한다.
- 상위단계 노드를 **부모 노드**, 부모 노드에 의해 가리켜지는 노드를 **자식 노드**라고 한다.
- 자식 노드가 존재하지 않는 노드(최종 깊이에 해당하는 노드)를 **잎(leaf) 노드**라고 한다.
- 반면 잎 노드가 아닌 노드는 **내부 노드**라고 한다.

<br/>

##### 설명

- 문제에 따르면 입력된 배열의 1번째 숫자가 있고, 여기에 2번째 숫자를 더하거나 뺄 수 있다.

- 이것은 트리구조의 루트 노드에 1번째 숫자를 넣어준 뒤, 여기에 **더하기와 빼기라는 2개의 가지**가 뻗어나와 자식 노드에 2개의 계산값이 나타나는 것으로 해석할 수 있다.

- 이후에도 계속해서 배열 내의 다음 숫자를 더하거나 빼는 방식이 반복되므로 계속해서 **다음 자식노드로 2개의 가지들이 뻗치는 구조**를 갖게 되는 것이다.

- 당연히 이 과정을 반복할 때 마다 자식 노드의 정점의 갯수는 부모 노드의 2배씩 증가할 것이다.

- 최종적으로, 마지막 숫자까지 더하거나 빼는 연산을 실시한 **모든 최종결과들이 잎 노드에 나타나게 된다.**

- 이는 마치 tree구조의 모든 잎 노드의 값들을 **BFS(너비우선탐색)를 통해 탐색하는 것**과 같다고 볼 수 있다.

- 이 **잎 노드에서 target number 값이 몇개나 나타났는지 카운트**해주면 간단히 풀린다.

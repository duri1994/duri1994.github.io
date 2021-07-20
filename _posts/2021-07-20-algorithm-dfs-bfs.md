---
title: "알고리즘 특강 Week3: DFS와 BFS"
categories:	
  - Algorithm
tags:
  - Concept
  - DFS/BFS
typora-root-url: ../
---

설명에 앞서 DFS와 BFS를 그림으로 비교해보면 다음과 같다.

<img src = "/assets/images/post_image/algorithm/dfs_bfs.gif"/>

<br/>



## DFS(깊이우선탐색)

#### 개요

- **Depth** First Search
- 트리나 그래프에서 한 경우의 수를 **최대한 깊숙히** 들어가면서 조사한다.
  - 그래서 한없이 깊게 조사하게 되는 경우 <u>스택 오버플로우의 위험성</u>이 있다.
  - 이를 방지하기 위하여 limit을 걸어주는 경우가 많다.
- 이후에 다음 경우의 수로 넘어가 이를 조사하는 과정을 반복하며 해를 찾는 과정이다.



#### 구현

- 일반적으로 **재귀함수**를 이용하거나, 단순한 **스택 배열**로 구현한다.

###### 스택으로 구현한 DFS 예시코드(미로찾기 코드 중 일부)

```python
while len(stack)>0:
	now = stack.pop()
	if now == dest:
        return True # 현재 위치가 타겟 위치라면
    x = now[1]
    y = now[0]
    if x - 1 > -1 and maps[y][x-1]==1: # 왼쪽 이동이 가능 & 아직 방문하지 않은 이동가능한 곳
    	stack.append([y, x-1]) # 탐색해야 할 지점이므로 stack에 추가
        maps[y][x-1]=2 # 방문한 곳이라고 표기
    if x + 1 < hori and maps[y][x-1]==1: # 오른쪽 이동
        stack.append([y, x-1])
        maps[y][x+1]=2
    if y - 1 > -1 and maps[y-1][x]==1: # 위로 이동
        stack.append([y-1, x])
        maps[y-1][x]=2
    if y + 1 < verti and maps[y+1][x]==1: # 아래로 이동
        stack.append([y+1, x])
        maps[y+1][x]=2
return false       
```

- **스택**은 한쪽 방향에서 데이터가 쌓이고 pop되기 때문에 한 경우의 수에서 가능한 한 깊게 조사를 한 뒤, 다음 경우의 수로 넘어간다.
  - 예시코드에선 어떤 방향으로 진행한 뒤에 막다른 곳이 나올때까지 우선 쭉 조사한 뒤, 다시 마지막 갈림길로 돌아나와 막다른 곳이 나올때까지 그 길을 조사하는 것을 반복한다.(스택에 쌓이는 순서를 그렇게 정했기 때문이다.)

<br/>

## BFS(너비우선탐색)

#### 개요

- Breadth First Search
- 트리나 그래프에서 동일한 깊이의 모든 경우의 수를 차례대로 조사한다.
- 이후에 다음 깊이로 들어가 모든 경우의 수를 조사하는 과정을 반복하며 해를 찾는 과정이다.



#### 구현

- 일반적으로 **큐**로 구현하며, <u>파이썬</u>에선 양방향으로 추가/추출이 가능한 **deque**를 활용할 수도 있다.

###### 예시코드(최단경로찾기 코드 중 일부)

```python
while len(queue) > 0:
    count = len(queue) # 같은 거리(깊이)에 있는 큐 데이터 갯수
    for time in range(count):
        now = queue.pop(0) # 큐의 맨 처음 값을 pop하여 현재 위치되어있는 정점 표시
		if now == dest:
            return answer
        for i in data: # 연결된 정점들의 조합 리스트 완전 탐색 , i = [정점1, 정점2]
			if i[0] == now and visited[i[1]-1] == False: # 방문하지 않은 연결된 경로라면
                queue.append(i[1]) # 그 경로를 큐에 추가
                visited[i[1]-1] = True # 그 경로에 방문여부 표시
            elif i[1] == now and visited[i[0]-1] == False:
                queue.append(i[0])
                visited[i[0]-1] = True
     answer += 1 # 최단경로길이 1 추가
return answer                
```






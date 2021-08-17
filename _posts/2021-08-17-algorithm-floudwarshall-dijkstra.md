---
title: "알고리즘 특강 Week8: 최단경로 - 플로이드 와샬/다익스트라"
categories:	
  - Algorithm
tags:
  - Concept
  - Python3
---

## 최단경로 유형

1. **플로이드 와샬 알고리즘**
   - <u>모든 노드를 방문하는 최단경로(최소비용경로)</u> 도출
2.  **다익스트라 알고리즘**
   - <u>특정 노드에서 다른 노드까지의 최단경로(최소비용경로)</u> 도출



## 플로이드 와샬 알고리즘 구현방법

##### 알고리즘 순서

1. 방문배열과 비용배열을 생성한다.

   - 방문배열은 방문하였을 시 True, 아직 방문하지 않았다면 False를 값으로 갖는다.

   - 비용배열은 표현가능한 최대값을 모든 점에 미리 배정한다.

2. 첫 시작점을 잡는다.

   - 해당 점의 방문배열은 True, 비용배열은 0으로 설정한다.

3. 첫 시작점과 연결된 점들의 비용배열에, 연결된 경로의 비용을 각각 담는다.

   - 연결된 점들 중 최소비용경로를 선택하고 그 점을 방문한다.

4. 다음 방문된 점의 방문배열을 True로 바꾸고 또다시 비용배열에 연결경로의 비용을 담는다.
   - 이 때, 기존에 비용이 담겨있는 점들의 경우 새로 입력되는 비용과 비교하여 최소값으로 갱신한다.
   - 비교 결과, 연결된 점들 중 최소의 비용값으로 방문할 수 있는 점을 다음 점으로 지정한다.

5.  다음 점에 대하여 4 과정을 반복한다.
6.  마지막 점까지 방문한 뒤, 비용 배열의 합을 구해주면 플로이드 와샬 알고리즘에 의한 전체 점을 잇는 최소비용경로를 도출할 수 있다.



##### 코드

```python
# costs: 연결된 노드 간 경로 및 비용을 담은 배열
# ex. [(노드1, 노드2, 비용), ...]
visited = [False for i in range(n)] # 방문배열
values = [2**31 - 1 for i in range(n)] # 비용배열

# 초기 점 설정
start = 0
visited[start] = True
values[start] = 0

while False in visited:
    # 노드 완전탐색으로 비용배열의 거리값 최소화
    for i in costs:
        if(visited[i[1]]==False and i[0]==start):
            values[i[1]] = min(values[i[1]], i[2])
        if(visited[i[0]]==False and i[1]==start): 
            values[i[1]] = min(values[i[0]], i[2])
    refer = 2**31 - 1
    
    # 방문하지 않은 노드 중 최소비용 노드 위치 탐색
    for i in range(n):
        if(visited[i]==False and values[i]!=0):
            refer = min(refer, values[i])
    answer += refer
    
    # 해당 노드 방문여부 체크, 그 노드로 이동해서 다시 반복문 실행
    for i in range(n):
        if(visited[i]==False and values[i]!=refer):
            visited[i] = True
            start = i
            break
```

<br>

## 다익스트라 알고리즘 구현방법

##### 알고리즘 순서

1. 방문배열과 비용배열을 생성한다.

   - 방문배열은 방문하였을 시 True, 아직 방문하지 않았다면 False를 값으로 갖는다.

   - 비용배열은 표현가능한 최대값을 모든 점에 미리 배정한다.

2. 첫 시작점을 잡는다.

   - 해당 점의 방문배열은 True, 비용배열은 0으로 설정한다.

3. 첫 시작점과 연결된 점들의 비용배열에, 연결된 경로의 비용을 각각 담는다.

   - 위의 비용 중 최소비용경로를 선택하고 그 점을 방문한다.

4. 다음 방문된 점의 방문배열을 True로 바꾸고 또다시 비용배열에 연결경로의 비용을 담는다.
   - 이 때, 지금 방문한 점까지 오기 위해 소요된 비용누적값을 새로 연결될 수 있는 비용에 더하여 기존값과 비교,  최소값으로 갱신해야 한다.
   - 이후, 현재 방문하지 않은 점들 중 최소비용값으로 방문할 수 있는 점을 다음 점으로 지정한다.

5.  다음 점에 대하여 4 과정을 반복한다.
6.  모든 점에 대하여 탐색이 마쳐지면(즉, 더이상 방문하지 않은 점이 없다면) 첫 시작점을 기준으로 특정 점에 도달하는데 드는 최소비용값이 비용배열에 나타난다.



```python
import sys

# costs: 연결된 노드 간 경로 및 비용을 담은 배열
# ex. [(노드1, 노드2, 비용), ...]
visited = [False for _ in range(n)] # 방문배열
values = [sys.maxsize for _ in range(n)] # 비용배열

# 0번 노드를 출발점으로 설정
visited[0] = True
values[0] = 0
length = len(visited)

while False in visited:
    checkLoc = -1
    checkValue = sys.maxsize
    
    # 미방문한 노드 중 최솟값 찾기
    for i in range(length):
    	if visited[i]==False and values[i]<checkValue:
            checkLoc = i
            checkValue = values[i]
        
        # 모든 점을 방문했다면 반복문 탈출
        if checkLoc = -1: 
            break
        
        # 경로 완전탐색으로 비용배열 수정
        visited[checkLoc] = True
        for v1, v2, c in costs:
            if v1==checkLoc and visited[v2]==False:
                values[v2] = min(values[v2], values[v1]+c)
            if v2==checkLoc and visited[v1]==False:
                values[v1] = min(values[v1], values[v2]+c)    
            
```






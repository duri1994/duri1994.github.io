---
title: "[프로그래머스] #43576 완주하지 못한 선수"
categories:	
  - CodingTest  
tags:
  - Programmers  
  - Python3
---

## 문제

[문제 보기](https://programmers.co.kr/learn/courses/30/lessons/42576)

##### 문제 설명

수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.

마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.

<br>

##### 제한사항

- 마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.
- completion의 길이는 participant의 길이보다 1 작습니다.
- 참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
- 참가자 중에는 동명이인이 있을 수 있습니다.

<br>

##### 입출력 예

| participant                                       | completion                               | return   |
| ------------------------------------------------- | ---------------------------------------- | -------- |
| ["leo", "kiki", "eden"]                           | ["eden", "kiki"]                         | "leo"    |
| ["marina", "josipa", "nikola", "vinko", "filipa"] | ["josipa", "filipa", "marina", "nikola"] | "vinko"  |
| ["mislav", "stanko", "mislav", "ana"]             | ["stanko", "ana", "mislav"]              | "mislav" |

<br>

##### 입출력 예 설명

예제 #1
"leo"는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.

예제 #2
"vinko"는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.

예제 #3
"mislav"는 참여자 명단에는 두 명이 있지만, 완주자 명단에는 한 명밖에 없기 때문에 한명은 완주하지 못했습니다.

<br>

## 풀이

```python
def solution(participant, completion):
    answer = ''

    p_dict = {}
    for i in participant:
        if (i not in p_dict.keys()):
            p_dict[i] = 1
        else:
            p_dict[i] += 1

    for i in completion:
        p_dict[i] -= 1

    for i in p_dict.keys():
        if p_dict[i] == 1:
            answer = i
            break

    return answer
```

- 참가자 명단으로부터 사람들을 딕셔너리에 저장한다.
  - 이 때, 이름이 동일한 경우 해당 이름(key)의 value값을 1씩 추가한다.
- 이후, 완주자 명단을 이용하여 위에서 구한 딕셔너리에서 해당 이름의 value값을 1씩 빼준다.
- 최종적으로, 딕셔너리에 홀로 value가 1인 key를 도출하면 정답이다.

<br>

##### 해시함수를 이용한 풀이

```python
def solution(participant, completion):
    answer = ''
    temp = 0
    dic = {}
    for part in participant:
        dic[hash(part)] = part
        temp += int(hash(part))
    for com in completion:
        temp -= hash(com) 
        
    answer = dic[temp]

    return answer
```

- 고유한 문자열은 해시함수에 의하여 고유한 해시 반환값을 내놓는다.
- 따라서, 참가자 이름들의 해시값을 key로 딕셔너리에 저장한 뒤, 모두 temp에 더해주고,
- temp로부터 완주자 이름들의 해시값을 전부 빼주면 최종적으로 완주하지 못한 사람이름의 해시값만 남는다.
- 역해시함수는 없다. 하지만 우리는 이를 위하여 딕셔너리의 key로 해시값을 잡아주었다.
  - 여기서 해당 key의 value를 도출하면 그것이 완주하지 못한 사람의 이름일 것이다.
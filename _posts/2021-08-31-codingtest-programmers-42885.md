---
title: "[프로그래머스] #42885 구명보트"
categories:	
  - Algorithm
  - CodingTest
tags:
  - Concept
  - Python3
---

## 들어가며

 조만간 국비과정에서 제공하는 코테 모의고사를 치룬다. 그동안 열심히 공부했지만 여전히 자신감이 부족한 나는 현직 개발자로 일하고 있는 친구에게 조언을 구했다. 이 친구는 항상 코테는 정말 쉬운거라고 한다. 그럴만도 하지. 초등학생 때부터 매일 코테를 끼고 살았던 친구니까. 하지만 그만큼 노하우도 가득해서 실전중심의 조언을 들을 수 있을지도 모른다는 기대가 있었다.

 그런데 친구로부터 조금 의외의 답이 돌아왔다. 친구는 코테는 '**기본적으로 문제 접근 및 조건을 세우는 것을 위주**로 공부'하는 것이 중요하다고 했다. 반면 나는 우선 프로그래밍 언어의 문법을 아는 것이 더 중요하다고 생각하던 터였다. 그러나 친구는 기본적인 문법도 중요하나, 결국은 문제가 요구하는 알고리즘이나 아이디어를 떠올리고  빠르게 풀이를 설계하는 사고력이 더 중요하다고 말했다. 그래서 최대한 많은 문제와 알고리즘을 접한 뒤 사고방식을 흡수해서 짧은 시간안에 떠올리고 문제에 적용하는 것이 핵심이라고 한다. 마치 예전에 수학을 공부하듯이 여러 유형을 접하고 접근방법에 대한 고민을 더 해보라는 것이다. ~~이상한 deque 같은 문법에 혹해서 쓸데없이 더 공부하지 말고 기본적인 list, set, dictionary나 잘쓰라고~~

 실전적인 팁으로는 **1. 시간 안배 설계**를 미리 하고 **2. 완벽히 풀지 못했더라도 쉬운 케이스만 만족하는 간단한 코드라도 작성하는 것**이 <u>최대한 점수를 획득할 수 있는 방법</u>이라고 조언해주었다. 코테는 <u>'기본 케이스 + 특수 케이스(예외처리) + 시공간 복잡도' 등을 고려</u>하여 여러 테스트 케이스의 총합으로 점수가 결정되는 만큼, 어려운 문제라고 아예 백지로 내는 것 보다는 정말 간단한 코드라도 쓰는 것이 유리하다고 한다.

 친구의 조언을 받아들여, 이제 코테 문제를 리뷰할 때도 코드에 앞서 문제를 접근하기 위해 고민한 과정들을 좀 더 자세히 기록하고 점검하는 데 집중하기로 했다. 따라서 앞으로는 자세한 정답코드리뷰보단, 어떤 알고리즘에 의해 문제에 접근하고, 예외 케이스 등은 어떻게 처리했는지 적시하며 정리를 할 것이다.

## 문제

[문제보기](https://programmers.co.kr/learn/courses/30/lessons/42885)

##### 문제 설명

무인도에 갇힌 사람들을 구명보트를 이용하여 구출하려고 합니다. 구명보트는 작아서 한 번에 최대 ***2명***씩 밖에 탈 수 없고, 무게 제한도 있습니다.

예를 들어, 사람들의 몸무게가 [70kg, 50kg, 80kg, 50kg]이고 구명보트의 무게 제한이 100kg이라면 2번째 사람과 4번째 사람은 같이 탈 수 있지만 1번째 사람과 3번째 사람의 무게의 합은 150kg이므로 구명보트의 무게 제한을 초과하여 같이 탈 수 없습니다.

구명보트를 최대한 적게 사용하여 모든 사람을 구출하려고 합니다.

사람들의 몸무게를 담은 배열 people과 구명보트의 무게 제한 limit가 매개변수로 주어질 때, 모든 사람을 구출하기 위해 필요한 구명보트 개수의 최솟값을 return 하도록 solution 함수를 작성해주세요.

<br>

##### 제한사항

- 무인도에 갇힌 사람은 1명 이상 50,000명 이하입니다.
- 각 사람의 몸무게는 40kg 이상 240kg 이하입니다.
- 구명보트의 무게 제한은 40kg 이상 240kg 이하입니다.
- 구명보트의 무게 제한은 항상 사람들의 몸무게 중 최댓값보다 크게 주어지므로 사람들을 구출할 수 없는 경우는 없습니다.

<br>

##### 입출력 예

| people           | limit | return |
| ---------------- | ----- | ------ |
| [70, 50, 80, 50] | 100   | 3      |
| [70, 80, 50]     | 100   | 3      |

<br>

## 풀이

##### 사고과정

```python
# 혼자만 타야하는 사람 먼저 걸러내기?
    '''
    m = min(people)
    pp = []
    for p in people:
        if p > (limit - m):
            answer += 1
        else:
            pp.append(p) # 나머지 인원들만 따로 빼 리스트 생성
    '''
# 이제 나머지를 어떻게 배정해야 최소가 될까? 
# 나머지 인원을 내림차순으로 정렬한 뒤,
# 앞에서부터 한명씩 태우고, 현재 제일 가벼운 사람과 매칭시켜서 탈 수 있으면 태워서 보낸다.
# 왜냐하면 문제 조건이 보트에 최대 2명밖에 타지 못하기 때문.

# 그런데 이렇게 할거면 위에서 혼자만 타는 사람을 먼저 거르는 과정을 거치지 않고 전부 이 알고리즘으로 풀면 되는거 아닌가?
# 탐욕법: 최대한 무거운 사람과 가벼운 사람을 매칭하여 2명씩 보내는 것이 유리하다.
# 따라서, 현재 가장 무거운 인원을 가장 가벼운 인원과 함께 태워보낼 수 있으면 묶어서 보낸다.
# 이것이 매 순간 최선의 선택이 된다. 나아가, 이것은 전체로 봐도 최선의 선택이라고 볼 수 있다.

# 최종 알고리즘
    # 1. 사람들을 몸무게 내림차순으로 정렬한다.
    # 2. 제일 무거운 사람을 태우고, 제일 가벼운 사람을 태웠을 때 limit을 초과하는지 점검한다.
    # 2-1. 초과한다면 제일 무거운 사람만 혼자 보내고, 남은 인원중 제일 무거운 사람에 대하여 2번을 점검한다.
    # 2-2. 초과하지 않는다면 둘을 묶어서 보내고, 남은 인원중 제일 무거운 사람 & 제일 가벼운 사람에 대하여 2번을 점검한다.
    # 3. 계속 반복하다가 최종적으로 1인만 남으면 마지막 1대에 태워 보내고, 아니라면 깔끔하게 쌍으로 묶어서 탈출한 것이므로 수행을 종료한다.
```

<br>

##### 풀이

```python
def solution(people, limit):
    answer = 0
    people = sorted(people, reverse=True) # 내림차순 정렬
    h, l = 0, (len(people) - 1) # 현재 제일 무거운 사람(h)과 제일 가벼운 사람(l)
    while h < l:
        if (people[h] + people[l]) > limit: # 둘이 탈 수 없는 경우
            answer += 1 # 1대 보내고
            h += 1 # 다음 무거운 사람을 지정
        else: # 둘이 탈 수 있는 경우
            answer += 1 # 둘이 묶어 1대로 보내고
            h += 1
            l -= 1 # 다음 무거운 사람과, 다음 가벼운 사람을 지정

    if h == l: # 위의 반복문을 빠져나왔을 때, 마지막 1명만 남는 경우
        answer += 1 # 그 1명도 보트에 태워 보낸다.

    return answer
```

- 그런데 생각해보면 while문의 조건을 `h<=l`로 잡아도 결국 동일한 answer 값이 나타나는 것을 알 수 있다.
  - 가독성은 위 코드가 낫겠지만, `h<=l`로 조건을 잡으면 굳이 하단의 if문을 안 써도 되므로 이렇게 하는 것이 더 깔끔할 것이다.

```python
def solution(people, limit):
    answer = 0
    people = sorted(people, reverse=True)
    h, l = 0, (len(people) - 1)
    while h <= l:
        if (people[h] + people[l]) > limit:
            answer += 1
            h += 1
        else:
            answer += 1
            h += 1
            l -= 1

    return answer
```

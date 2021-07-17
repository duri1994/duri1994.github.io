---
title: "알고리즘 특강 Week2: 완전탐색 연습문제 - 모의고사"
categories:	
  - Algorithm
tags:
  - 연습문제
  - Programmers
---

## 문제

https://programmers.co.kr/learn/courses/30/lessons/42840

##### 문제 설명

수포자는 수학을 포기한 사람의 준말입니다. 수포자 삼인방은 모의고사에 수학 문제를 전부 찍으려 합니다. 수포자는 1번 문제부터 마지막 문제까지 다음과 같이 찍습니다.

1번 수포자가 찍는 방식: 1, 2, 3, 4, 5, 1, 2, 3, 4, 5, ...
2번 수포자가 찍는 방식: 2, 1, 2, 3, 2, 4, 2, 5, 2, 1, 2, 3, 2, 4, 2, 5, ...
3번 수포자가 찍는 방식: 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, ...

1번 문제부터 마지막 문제까지의 정답이 순서대로 들은 배열 answers가 주어졌을 때, 가장 많은 문제를 맞힌 사람이 누구인지 배열에 담아 return 하도록 solution 함수를 작성해주세요.

##### 제한 조건

- 시험은 최대 10,000 문제로 구성되어있습니다.
- 문제의 정답은 1, 2, 3, 4, 5중 하나입니다.
- 가장 높은 점수를 받은 사람이 여럿일 경우, return하는 값을 오름차순 정렬해주세요.

##### 입출력 예

| answers     | return  |
| ----------- | ------- |
| [1,2,3,4,5] | [1]     |
| [1,3,2,4,2] | [1,2,3] |

##### 입출력 예 설명

입출력 예 #1

- 수포자 1은 모든 문제를 맞혔습니다.
- 수포자 2는 모든 문제를 틀렸습니다.
- 수포자 3은 모든 문제를 틀렸습니다.

따라서 가장 문제를 많이 맞힌 사람은 수포자 1입니다.

입출력 예 #2

- 모든 사람이 2문제씩을 맞췄습니다.



## 풀이

- 1,2,3번 수포자의 패턴이 있으므로 각 패턴에 대하여 맞춘 문제를 계산하는 함수를 정의한다.
- answers 리스트의 모든 값들을 탐색하여 각 패턴과 비교해야 하므로 완전탐색 알고리즘을 활용한다.

```python
def solution(answers):
    answer = []

    def calc1():
        ans1 = 0
        for i in range(len(answers)):
            if answers[i] == (i % 5 + 1):
                ans1 += 1

        return ans1

    def calc2():
        ans2 = 0
        for i in range(len(answers)):
            if i % 2 == 0:
                if answers[i] == 2:
                    ans2 += 1
            else:
                a = i % 8
                if (a == 1 or a == 3) and answers[i] == a:
                    ans2 += 1
                elif a == 5 and answers[i] == 4:
                    ans2 += 1
                elif a == 7 and answers[i] == 5:
                    ans2 += 1

        return ans2

    def calc3():
        ans3 = 0
        for i in range(len(answers)):
            a = i % 10
            if a < 2:
                if answers[i] == 3:
                    ans3 += 1
            elif a < 4:
                if answers[i] == 1:
                    ans3 += 1
            elif a < 6:
                if answers[i] == 2:
                    ans3 += 1
            elif a < 8:
                if answers[i] == 4:
                    ans3 += 1
            elif a < 10:
                if answers[i] == 5:
                    ans3 += 1
        return ans3

    s1, s2, s3 = calc1(), calc2(), calc3()
    s = [s1, s2, s3]
    m = max(s)
    for i in range(3):
        if s[i] == m:
            answer.append(i + 1)

    return answer
```

- 3명의 수포자 모두 같은 패턴이 반복되므로 answers 리스트의 인덱스넘버 및 특정 숫자로 나눈 나머지를 활용하면 정답 여부를 확인할 수 있다.
- 마지막의 3명의 정답갯수를 반환받은 뒤, 최대 문제를 맞춘 인원들을 정답 리스트에 추가해준다.



## 개선된 풀이

위 풀이에선 수동으로 패턴을 분석하여 정답을 구했다. 따라서 코드가 if문이 과하게 사용된 감이 있다.

다른 사람들의 코드를 참조해보니 다음과 같은 효율적인 방법이 있었다.

바로 찍기패턴 리스트를 만들어 그 리스트의 길이로 나눈 나머지를 통해서 정답을 체크하는 방법이 그것이다.



```python
def solution(answers):
    pattern1 = [1,2,3,4,5]
    pattern2 = [2,1,2,3,2,4,2,5]
    pattern3 = [3,3,1,1,2,2,4,4,5,5]
    score = [0, 0, 0]
    result = []

    for idx, i in enumerate(answers):
        if i == pattern1[idx % len(pattern1)]:
            score[0] += 1
        if i == pattern2[idx % len(pattern2)]:
            score[1] += 1
        if i == pattern3[idx % len(pattern3)]:
            score[2] += 1

    for idx, s in enumerate(score):
        if s == max(score):
            result.append(idx+1)

    return result
```

- 어차피 찍기 패턴은 반복되므로 각각 리스트로 만들어준다.
- 파라메터로 들어온 정답 리스트 인덱스 전체를 차례대로 탐색하며, 찍기 패턴의 값과 비교한다.
  - 이 때, 패턴은 반복되므로 패턴리스트의 길이로 나눈 나머지로 인덱싱을 하여 정답과 비교하면 된다.
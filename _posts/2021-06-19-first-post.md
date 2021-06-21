---
title: "SQL SELECT 문법"
categories:	
  - sql
tags:
  - mysql
  - 개념정리
---

# 들어가며

이 포스트는 sql의 SELECT문의 기본적인 문법 및 조건기호에 대하여 간단하게 요약해 놓았다.

- select 문의 기본적인 문법
- where 절의 조건기호들: >, between~in~, in(values), like, is null, and/or/not



## 기본 문법 개요

``` sql
SELECT 컬럼명1 [as] '별칭1', 컬럼명2 [as] '별칭2', ...
FROM 테이블명
WHERE 조건1 [OR/AND 조건2] ...
ORDER BY 정렬기준컬럼명 [ASC(오름차순)/DESC(내림차순)]
```

- SELECT: 조건에 맞는 데이터를 가져오는 구문

- 데이터를 가져오고 싶은 테이블 및 컬럼명을 지정
- WHERE: 원하는 조건에 해당하는 데이터만을 가져오고 싶을 때 여기에 작성
- ORDER BY: 정렬. 기준이 되는 컬럼명 및 오름차순(기본값), 내림차순 설정 가능



##### 코드 예시

```sql
SELECT last_name 'Name', job_id 'Job', salary 'Sal'
FROM employees
WHERE (job_id = 'SA_REP'
      or job_id = 'AD_PRES')
      and salary > 15000
ORDER BY salary desc;
```



## 조건기호(where절)

#### 비교조건

- '>', '<', '<>: 일치하지 않음'

- 'between val1 and val2' : val1과 val2 사이의 값

- 'in(val1, val2, val3, ...)' : 값 목록들 중 하나의 값과 일치

- like : 문자열 패턴 일치

  - %: 0개 이상의 일련의 문자
  - _: 문자 하나(언더바 갯수만큼)

- is null : null 값이 아닌 경우

- and, or, not

  

## 나가며

 앞서 배운 파이썬 기본 중 처음 배웠던 내용(클래스)에 대한 정리에 앞서 먼저 sql 언어 내용부터 정리를 하고자 하고 있다.  지금 sql 언어를 먼저 잘 정리해놓고 또 다음에 다시 파이썬을 다루게 되면 그 때 복습 차원에서 해당 내용들을 포스팅하는 것이 좋을 것이라 생각한다. 



 다음 포스팅에선 테이블 간 조인에 대하여 간단하게 정리할 예정이다.



 정식으로 각잡고 코딩을 배우는 것이 처음이기에 모든게 설레면서도 낯설고 어렵다. 차근차근 배운 것을 정돈하고 그것이 익숙해지기까지 부단히 노력해야겠다...!

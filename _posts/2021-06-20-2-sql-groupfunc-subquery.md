---
title: "SQL 그룹함수/서브쿼리"
categories:	
  - SQL
tags:
  - MySQL
  - Concept
---

# 들어가며

이 포스팅은 sql의 그룹함수, 그룹별 분류, 서브쿼리 및 데이터 조작(INSERT/UPDATE/DELETE) 방법에 대하여 요약한다.





## 그룹함수 및 그룹별 분류

#### 그룹함수



- AVG(평균), COUNT(갯수), MAX/MIN(최대/최소), SUM(합계), STDDEC(표준편차), VARIANCE(분산)
- 조건에 맞는 레코드들을 그룹화하여 특정 컬럼값들의 계산값을 출력해준다.
- 별도의 처리가 없다면 보통 null 값은 무시하고 계산한다. (평균 계산시에도 모수에서 제외)



```sql
# 직무 이름에 'REP'가 포함되는 사람들의 월급 평균, 최대, 최소, 총합 구하기
select avg(salary), max(salary), min(salary), sum(salary)
from employees
where job_id like '%REP%';

# count 함수: 테이블의 행 수 반환, 자동으로 null은 무시
select count(*) as 'COUNT'
from employees
where department_id = 50;

# 부서번호 80번인 사람 중 commission_pct가 null이 아닌 사람의 수 구하기
select count(commission_pct)
from employees
where department_id = 80;
```



#### 그룹 별 분류: HAVING절



- 그룹별 분류 후 특정 조건에 맞는 그룹만을 출력하기 위해 조건을 걸어줄 때 사용
- WHERE절은 각 레코드 별로 조건을 걸 때 사용하는 것이므로, 그룹에 대한 조건을 걸 때는 사용하면 안된다.

```sql
# 최대월급이 10000보다 큰 부서들의 부서명, 최대월급을 출력
select department_id, max(salary)
from employees
group by department_id
having max(salary) > 10000;
```



- HAVING절(그룹 조건) 및 WHERE절(레코드 조건)의 용법 차이는 다음의 코드를 참조하면 좋다.

```sql
select job_id, sum(salary) payroll
from employees
where job_id not like '%REP%' # 각 레코드에 대한 조건: 직무이름에 'REP'가 포함되지 않는 인원만
group by job_id
having sum(salary) > 13000	  # 각 그룹에 대한 조건: salary 합이 13000보다 큰 직무만
order by sum(salary);
```



## 서브쿼리

지금까지 배운 SELECT문을 기반으로 한 단일쿼리 내에 또 하나의 쿼리를 삽입할 수 있는데, 이것이 서브쿼리이다.

```sql
# 서브 쿼리 사용: 'Abel'이란 이름을 가진 사람보다 급여를 많이 받는 사람들의 이름을 출력하라.
select last_name
from employees
where salary > (select salary from employees where last_name = 'Abel');
```

- 서브쿼리는 주로 WHERE절의 조건으로 활용된다. (괄호로 묶어 사용)
- 서브 쿼리가 더 먼저 실행된다.
- 단일행 서브쿼리에는 단일행 연산자, 다중행 서브쿼리에는 다중행 연산자(IN, ANY, ALL)를 사용한다.

```sql
# 단일행 서브쿼리(하나만 비교하는 연산자)
select last_name, job_id, salary
from employees
where job_id = (select job_id from employees where employee_id = 141)
and salary > (select salary from employees where employee_id = 143);

## 다중행 서브쿼리 + 연산자(IN, ANY, ALL)
# ANY 연산자: like or, '반환값 중 하나'에 대하여만 조건을 만족하면 true
select employee_id, last_name, job_id, salary
from employees
where salary < any (select salary from employees where job_id = 'IT_PROG')
and job_id <> 'IT_PROG';

# ALL 연산자: like and, 반환값 '모두'에 대하여 조건을 만족해야만 true
select employee_id, last_name, job_id, salary
from employees
where salary < all (select salary from employees where job_id = 'IT_PROG')
and job_id <> 'IT_PROG';

# IN 연산자
# 매니저가 없는 사람의 이름만 출력
select emp.last_name
from employees emp
where emp.employee_id not in (select mgr.manager_id from employees mgr);
```



## 나가며

지금까지 전반적인 select문의 작성 방법에 대해 알아보았다.

다음 포스팅엔 데이터값을 직접 조작하는 쿼리(INSERT, UPDATE, DELETE)의 사용에 대해 요약할 것이다.


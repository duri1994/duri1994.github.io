---
title: "데이터 조작어(INSERT / UPDATE / DELETE)"
categories:	
  - sql
tags:
  - mysql
---

## 들어가며

이 포스팅은 sql의 3가지 언어 분류 중 DML(데이터 조작어)에 해당하는 INSERT, UPDATE, DELETE 구문에 대하여 요약한다.

- sql 언어의 3가지 분류(DDL, DML, DCL)
- DML: INSERT, UPDATE, DELETE



## SQL 언어의 종류

sql 언어는 크게 3가지로 분류된다.

1. DDL(데이터 정의어): create/alter/drop table/view/procedure,funcion
   - talbe, view, procedure, function을 정의하고, 수정하고, 삭제하는 언어. 



2. DML(데이터 조작어): insert/update/delete
   - 새 레코드를 추가하거나, 수정, 삭제를 하는 언어.



3. DCL(데이터 제어어): grant/revoke
   - 데이터에 대한 접근 권한과 관련된 언어. 



이 중, 데이터 조작어에 해당하는 insert/update/delete 구문에 대하여 정리한다.



## INSERT문: 새 레코드 추가

DML에 대한 설명을 위하여, 예제에서 활용할 test1 테이블을 생성한다.

```sql
# test1 table 생성
create table test1(			
id varchar(20) primary key,
pwd varchar(20) not null,
name varchar(20)		# name은 null 가능
);
```

INSERT문을 활용해 위의 테이블에 새로운 레코드들을 추가할 수 있다.

```sql
insert into test1 values('aaa', '111', 'namea');
insert into test1(name, pwd, id) values('nameb', '222', 'bbb');
insert into test1(id, pwd) values('ccc', '333'); # name은 자동으로 null 값이 들어감 
insert into test1 values('ddd', '444', null); # null 값을 직접 넣어주는 것도 가능.
```

또한 INSERT를 이용하여 다른 테이블에서 행을 복사해올 수도 있다.

```sql
# 직무이름에 'REP'가 들어간 사원 레코드를 전부 복사 
insert into emp
(select * from employees where job_id like '%REP%');
```



## UPDATE문: 레코드 수정

```sql
update 테이블명 set 컬럼명1=수정값1, 컬럼명2=수정값2, ... where 조건;
```

특정 조건을 만족하는 레코드의 컬럼값을 직접 지정하여 아래와 같이 수정할 수 있다.

```sql
# id가 ccc인 레코드의 이름을 namec로 바꿔라.
update test1 set name = 'namec' where id = 'ccc';
```



## DELETE문: 레코드 삭제

```sql
delete from 테이블명 WHERE 조건;

# id가 ccc인 레코드를 삭제하라.
delete from test1 where id = 'ccc';

# 모든 테이블의 행 삭제
set sql_safe_updates = 0 # Mysql 세이프모드 해제. 이것을 해제해야 아래의 구문이 작동함
delete from test1;

```

단, 다른 table에서 참조되는 경우, 해당 레코드는 삭제가 불가능하다. 이는 별도의 처리를 통하여 삭제할 수 있는 방법이 있으니, 차후에 알아보도록 하겠다.



## 나가며

이번 포스팅에선 데이터 조작어인 insert, update, delete의 사용에 대해 알아보았다.

다음 포스팅엔 transaction에 대하여 정리할 예정이다. 


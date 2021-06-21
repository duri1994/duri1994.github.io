---
title: "데이터 조작어(INSERT / UPDATE / DELETE), 트랜잭션"
categories:	
  - sql
tags:
  - mysql
  - 개념정리
---

## 들어가며

이 포스팅은 sql의 3가지 언어 분류 중 DML(데이터 조작어)에 해당하는 INSERT, UPDATE, DELETE 구문 및 트랜잭션 개념에 대하여 요약한다.

- sql 언어의 3가지 분류(DDL, DML, DCL)
- DML: INSERT, UPDATE, DELETE
- Transaction



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

단, 다른 table에서 참조되는 경우, 해당 레코드는 삭제가 불가능하다. 이는 제약조건을 이용하여 삭제할 수 있는 방법이 있으니, 차후에 알아보도록 하겠다.



## 트랜잭션

트랜잭션(Transaction)이란, 쓰기 작업을 하는 하나의 단위체계이다. 

데이터 조작어를 실행하는 즉시 DB에 영향을 미치게 된다면, 실수를 할 경우 데이터 복구가 쉽지 않을 것이다. 

하지만 데이터 조작어의 경우 작성된 코드가 commit 되어야 비로소 실제 DB에 적용이 된다.

이 때, commit이 되는 하나의 단위가 바로 트랜잭션이다.



#### 트랜잭션의 구분

- 트랜잭션은 항상 첫번째 DML sql문이 실행될 때 시작된다.
- 트랜잭션은 다음의 이벤트 중 하나가 발생하면 종료된다.
  1. commit 이나 rollback문이 실행되는 경우
  2. DDL(데이터 정의어): create / alter / drop => 즉, 한 문장을 실행함과 동시에 트랜잭션이 종료
  3. DCL(데이터 제어어): grant / revoke



- DML(insert, update, delete)의 경우, 여러 문장을 쓰더라도 위의 이벤트가 발생하지 않는 한 실제 db에 영향을 미치지 않으며, 트랜잭션 역시 유지된다.



#### 트랜잭션 제어문(commit, savepoint, rollback to 'savepoint name')

```sql
## 트랜잭션 제어문(COMMIT, SAVEPOINT name, ROLLBACK, ROLLBACK TO savepoint name)(p351)
set autocommit = 0 ; # Mysql workbench의 자동커밋모드 해제

insert into emp1 values(200, 'bbb', 15000, 70);
insert into emp1 values(201, 'ccc', 5000, 90);
insert into emp1 values(202, 'ddd', 4000, 100);	# insert

select * from emp1;

savepoint a;	# a 세이브포인트

update emp1 set name='new bbb', sal='12345' where emp_id=200; # update

savepoint b;	# b 세이브포인트

delete from emp1 where emp_id=202;	# delete

#rollback to b; 			# 202번 정보가 삭제되기 전으로
#rollback to a;  			# new bbb로 바뀌기 전으로
#rollback; 					# bbb,ccc,ddd 추가하기 전으로
commit;				# 쓰기완료. 현 트랜잭션 종료

rollback;			# 이제는 롤백을 해도 변화가 없음(직전 트랜잭션이 영구적으로 적용되었으므로)
```

- DML문으로 이루어진 트랜잭션에선 중간중간 세이브포인트의 지정이 가능하다.
- 따라서, 위의 코드처럼 원하는 세이브포인트로 롤백을 시킬 수 있다.
- 다만, 마지막 rollback의 경우 위에서 이미 commit이 실행되어 트랜잭션이 종료되었으므로 아무런 변화가 없다.(세이브포인트도 전부 날아감)





## 나가며

이번 포스팅에선 데이터 조작어인 insert, update, delete의 사용 및 트랜잭션 개념에 대해 알아보았다.

다음 포스팅엔 테이블을 생성/수정/삭제하고 컬럼에 적용하는 제약조건들에 대하여 요약할 것이다.


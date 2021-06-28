---
title: "SQL 테이블 생성과 제약조건"
categories:	
  - sql
tags:
  - mysql
  - 개념정리
---

## 들어가며

이 포스팅은 다음 내용을 정리하였다.

- 테이블의 생성, 수정, 삭제(create, alter, delete)
- 제약조건



## 테이블의 생성, 수정, 삭제

#### 테이블 생성(Create)

``` sql
 CREATE TABLE 이름(
 컬럼명1 타입(크기) [제약조건],
 컬럼명2 타입(크기) [제약조건],
 컬럼명3 타입(크기) default '기본값'
 ...
 );

```

create문으로 테이블의 이름과 각 컬럼의 이름, 타입, 제약조건, 기본값 등을 설정해줄 수 있다.

컬럼에 설정 가능한 타입들은 다음과 같다.

- 정수: int(integer)
- 실수: float
- 문자: char(크기) - 고정크기 / varchar(크기) - 가변크기
- 대용량 텍스트: longtext(4GB)
- 날짜: date(년월일) / datetime(년월일 시분초)



##### 코드 예시

```sql
create table test2(
num int auto_increment primary key,		# auto_increment: 자동으로 할당되는 값 # 기본키로 설정
name varchar(20) not null,				# null 허용 x
home varchar(50) default '서울',			# 기본값 서울
w_date datetime default now(),			# 날짜 기본값: 함수 now()의 반환값
msg varchar(200)
);
```

위의 primary key와 같은 제약조건은 아래에서 설명을 할 것이다.



#### 테이블 수정(Alter)

컬럼의 추가/수정/삭제, 새 컬럼의 기본값 정의, 컬럼의 타입/크기 등을 변경할 수 있다.

```sql
# (1) 컬럼 추가(add)
 ALTER TABLE 테이블명
 ADD (컬럼명 타입(크기));
  ;
  
# (2) 컬럼 변경(modify)
 ALTER TABLE 테이블명
 MODIFY 컬럼명 새타입(새크기);
 
# (3) 컬럼 삭제(drop)
 ALTER TABLE 테이블명
 DROP COLUMN 컬럼명;
 
# (4) 기타기능: 컬럼속성(이름,타입)변경, 컬럼제약조건 추가삭제
```

##### 예시코드

```sql
alter table test2
add (pwd varchar(10));	# 컬럼 추가(타입 및 크기 지정)

alter table test2
modify pwd varchar(20);	# 컬럼 수정(타입 및 크기 수정)

alter table test2
drop column pwd;		# 컬럼 삭제 cf. drop으로는 컬럼 하나씩만 삭제 가능

alter table test2
change home addr varchar(50);	# 컬럼명/타입 변경(home -> addr)
```



#### 테이블 삭제

```sql
# Delete
delete from new_test2;
rollback; # 직전에 테이블을 삭제한 명령(DML)을 취소할 수 있다.
		  # 즉, delete 명령은 롤백이 가능.

# Truncate
truncate table new_test2; # truncate(전체행 삭제) = DDL. 롤백 불가능(새로운 트랜잭션이 시작)

```



## 제약조건

제약조건이란, 테이블의 컬럼마다 어떤 값들이 들어갈 수 있는지 제한하는 조건을 말한다.

데이터베이스에서 이러한 제약조건이 필요한 이유는 바로 '데이터의 무결성'을 유지하기 위함이다.



####  제약조건의 종류

- not null: null 값은 허용하지 않음
- unique: 고유한 값만을 갖는 열 혹은 열조합 지정
- primary key: 기본키, 기본키로 지정된 컬럼은 테이블의 대표 컬럼(레코드를 구분), not null + unique
- foreign key: 참조키, 다른 참조 테이블과 관계를 구성하는 컬럼
- check: 반드시 만족해야하는 조건 지정



#### 제약조건 정의 문법

```sql
CREATE TABLE 테이블명(
	컬럼명 타입(크기) 제약조건,
    컬럼명 타입(크기) 제약조건,
    ...
    CONSTRAINT 제약조건명
    				제약조건 (제약조건을 적용할 컬럼명);

CREATE TABLE employees(
	employee_id int(6),
    first_name varchar(20),
    ...
    job_id varchar(20) NOT NULL,							# 이상 열레벨 제약조건
    CONSTRAINT emp_emp_id_pk 
						PRIMARY KEY (employee_id);			# 기본키 지정
    														# 테이블레벨 제약조건
```

제약조건의 표현방식은 2가지가 존재한다.

1. 열레벨 제약조건(약식표현)
2. 테이블레벨 제약조건(정식표현): constraint 제약조건명 제약조건 (적용할 컬럼명)



#### FOREIGN KEY 제약조건의 추가 키워드들

Foreign key(참조키)의 경우 그 특성상 별도의 문법 및 추가 키워드들이 존재한다.

- FOREIGN KEY: 테이블 레벨 제약조건(단, Oracle에선 열레벨 제약조건), 적용할 하위 테이블 컬럼 지정
- REFERENCES: 참조할 부모테이블 컬럼 지정
- ON DELETE CASCADE: 부모테이블의 일부 레코드가 삭제될 경우, 자동으로 그것을 참조하는 하위 테이블의 레코드도 삭제
- ON DELETE SET NULL: 종속된 하위 테이블의 참조키 값을 null로 변환(위와 다르게 레코드는 그대로 남음)



#### 제약조건 추가/삭제

alter문이나 drop문을 활용하여 이미 존재하는 테이블에도 추가적인 제약조건의 적용이 가능하다.

```sql
# 기본키 추가
ALTER TABLE 테이블명
ADD CONSTRAINT [사용할 제약조건 이름] PRIMARY KEY(컬럼명);

# 외래키 추가
ALTER TABLE 테이블명
ADD CONSTRAINT [사용할 제약조건 이름] FOREIGN KEY(컬럼명)
REFERENCES 부모테이블(컬럼명) [ON DELETE CASCADE / ON DELETE SET NULL]; 

# NOT NULL 추가
ALTER TABLE 테이블명
MODIFY 컬럼명[타입] CONSTRAINT [사용할제약조건이름] NOT NULL;

# 제약조건 삭제
ALTER TABLE 테이블명
DROP CONSTRAINT [제약조건이름];

ALTER 테이블명
DROP FOREIGN KEY [제약조건이름];
```



## 나가며

 Mysql에서 테이블을 생성하고 수정, 삭제하는 문법과 컬럼에 적용할 제약조건에 대하여 알아보았다.

다음엔 stored procedure, stored function, trigger 에 대하여 요약하도록 하겠다.



다음 포스팅을 기준으로 기본적인 sql문에 대한 정리가 끝나고 이에 대한 이해를 바탕으로 본격적으로 파이썬과 데이터베이스의 연동 작업에 들어간다.  VO, DAO, Service의 MVC(Model-View-Controller)  패턴의 총체적인 파이썬 코드를 구축하기 위하여 지금까지 Mysql을 통한 DB 구축 및 조작, 제어에 대해 배운 것이므로 그 개념 및 문법을 숙지하는 것이 중요하다!


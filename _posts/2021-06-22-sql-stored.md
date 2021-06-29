---
title: "SQL Stored procedure, Stored function, Cursor"
categories:	
  - SQL
tags:
  - MySQL
  - Concept
---

## 들어가며

이번 포스팅은 

- Stored Procedure
- Stored Function
- Cursor

에 대하여 정리한다. 



## Stored Procedure, 저장 프로시저

- MySQL에서 제공하는 프로그래밍 기능
- **반복적으로 활용되는 SQL문을 하나의 프로시저로 만들어** 편리하게 사용할 수 있는 기능

- 실무적으로도 유용함



#### Stored Procedure 생성 및 호출

```sql
# Stored Procedure 생성
DELIMITER $$
CREATE DEFINER=`유저명` PROCEDURE '저장프로시저명'(IN or OUT parameters)
BEGIN
	SQL문 프로그래밍...
END $$
DELIMITER ;

# 호출
CALL 저장프로시저명(parameters)
```

- 입력(in) 및 출력(out) 파라미터는 원하는 만큼 설정할 수 있다.
- MySQL Workbench에선 좌측의 Schemas 탭의 Database에 있는 Stored Procedures 항목을 우클릭하면 편리하게 새로운 저장프로시저를 생성할 수 있다.

```sql
#MySQL에서 제공되는 기본 저장프로시저 생성 틀
CREATE PROCEDURE `new_procedure` ()
BEGIN

END
```

- 위의 예시와 같이 기본적인 구문을 제공한다.
- 따라서 저장프로시저 이름, 입출력 파라미터들, 그리고 BEGIN 과 END사이에 원하는 SQL 프로그래밍을 구현하면 된다.



#### 예시코드

```sql
# 사번을 입력받아 해당 사원의 이름을 출력하는 저장프로시저
DELIMITER $$
CREATE DEFINER=`root`@`localhost` PROCEDURE `p1`(enum int, out ename varchar(20))
BEGIN
	select last_name into ename from employees
    where employee_id = enum;
END $$
DELIMITER ;

# 호출
CALL p1(100) # 사번이 100인 사원의 이름을 출력
```



## Stored Function, 저장함수

- Stored Function(저장함수)란 MySQL의 내장함수가 아닌 사용자 커스텀 함수이다.
- 저장프로시저와 매우 유사하나 그 형태와 용도의 차이가 있다. 대표적으로,
  - 파라미터로 **오로지 입력 파라미터만** 받을 수 있다. 반환값은 따로 선언해야 한다.
  - 저장프로시저는 CALL로 호출되지만, **저장함수는 SELECT문 안에서 호출된다.**



#### Stored Function 생성 및 호출

- 저장함수를 새롭게 생성하기 위해선 저장함수 생성권한을 다음과 같이 허용해주어야 한다.

```sql
SET GLOBAL log_bin_trust_function_creators = 1;
```

- 생성 문법은 저장프로시저와 거의 동일하며, 마찬가지로 MySQL에서 기본적인 생성 틀을 제공한다. 

- 좌측 Schemas 탭의 Database에 있는 Functions 항목을 우클릭하면 편리하게 새로운 저장함수를 생성할 수 있다.



#### 예시코드

```sql
# 사번을 입력받아 그 사원의 last name을 출력하는 함수
DELIMITER $$
CREATE DEFINER=`root`@`localhost` FUNCTION `f2`(enum int) 
RETURNS varchar(20) CHARSET utf8	# 반환값의 타입 설정
BEGIN
	declare ename varchar(20);	# 변수 선언
    select last_name into ename from employees where employee_id = enum;
RETURN ename;	# 반환값
END $$
DELIMITER ;

# 호출
SELECT f2(100);
```



## Cursor, 커서

- **여러 레코드를 검색해야 하는 경우** 사용되는 객체
- 커서 객체(변수)에 **'표 형태'의 값을 담을 수 있다.**
- 즉, 여러 레코드로 이루어진 검색결과를 다뤄야할 때 사용
- **저장프로시저 내부에서 사용**할 수 있다.



#### 커서의 처리 순서

1. 커서의 선언(DECLARE CURSOR)
2. 반복 조건 선언(DECLARE CONTINUE HANDLER)
   - 더 이상 읽어올 레코드가 없는 경우, 실행할 내용을 설정
3. 커서 열기(OPEN)

(LOOP 시작)

4. 커서에서 데이터 한줄씩 가져오기(FETCH)

5. 데이터 처리

(END LOOP)

​	6. 커서 닫기(CLOSE)



#### 커서 활용 예시

```sql
DELIMITER $$
CREATE DEFINER=`root`@`localhost` PROCEDURE `cursor_test`()
BEGIN
	# 아래 커서에서 데이터를 한줄씩 읽어올 때 그 데이터값을 담을 변수들 선언
	declare enum int;
    declare ename varchar(20);
    declare sal int;					
    
   
	
    declare done tinyint default 0;	# loop를 돌릴 때 조건으로 쓸 done 변수 선언
	declare c1 cursor for select employee_id, last_name, salary from employees; #1.커서선언
	declare continue handler for not found set done=1; #2.반복조건 선언
	# 더 이상 읽을 것 없으면(declare continue handler for not found) done을 1로 바꿔줌
      
    open c1; #3.커서열기
    
    l1:loop # 루프
		fetch from c1 into enum, ename, sal;#4. 커서에서 데이터 한줄씩 가져오기(fetch)
		# 위에서 선언한 3개의 변수에 방금 읽은 레코드의 값들을 담음
		if done then leave l1;	
		# 커서가 모든 데이터를 읽어 더 이상 읽을 것이 없으면, done=1=True 되면서 루프탈출
        end if;
        select enum, ename, sal;	#5.데이터처리
	end loop; # 루프종료
    
    close c1; #6.커서닫기              
END $$
DELIMITER ;

# 커서가 포함된 저장프로시저 호출 = 일반적인 저장프로시저 호출
call cursor_test();
```



## 나가며

지금까지 sql에서 제공하는 프로그래밍 기능인 저장프로시저, 저장함수, 그리고 커서에 대해 알아보았다.

다음은 Trigger(트리거)에 대해서 정리할 예정이다.

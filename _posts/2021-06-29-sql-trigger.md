---
title: "SQL Trigger"
categories:	
  - SQL
tags:
  - MySQL
  - Concept
---

## 들어가며

이번 포스팅은 Trigger 에 대하여 정리한다. 



## Trigger(트리거)

- INSERT / UPDATE / DELETE 문이 실행되기 **직전이나 직후에 실행**될 코드를 등록한 것



#### 트리거 생성 방법

```sql
CREATE TRIGGER 트리거명
AFTER(전)/BEFORE(후) insert/update/delete(DML문)
ON 테이블명
[FOR EACH ROW]	# 실행되는 레코드 수 만큼 트리거가 작동하도록 하는 제어문(대부분 씀)
```

- DML문이 실행되기 **전 or 후를 선택할 수 있다.**
- **적용할 DML문 종류**를 선택할 수 있다.
- DML문이 **적용되는 레코드 수에 맞게** 트리거를 작동시킬 수 있다.



##### 트리거의 적용 예시: 백업테이블의 활용

```sql
# 트리거 생성: emp1 table에 새 레코드가 insert될 때마다 emp1_backup테이블에 추가된 내용을 입력
DELIMITER $$
CREATE TRIGGER emp1_trig1
AFTER INSERT
ON emp1

FOR EACH ROW
BEGIN
	insert into emp1_backup(id, cmd, new_sal) values(new.emp_id, 'insert', new.sal);
END$$
DELIMITER ;
```

- 이 때 **new.** 는 <u>추가되는 새로운 값</u>들을 의미한다. 
- 반대로 **old.** 는 <u>기존의 값</u>들을 의미한다.



##### 추가 예시: 업데이트 트리거 / 삭제 트리거

```sql
# 업데이트 트리거: update될 때 마다 기존의 값들을 백업테이블에 저장
DELIMITER $$
create trigger emp1_trig2
after update 				
on emp1
for each row
begin
	insert into emp1_backup(id, cmd, old_sal, new_sal) 
    values(old.emp_id, 'update', old.sal, new.sal);
end$$
DELIMITER ;

# 삭제 트리거: delete된 값들을 백업테이블에 저장
DELIMITER $$
create trigger emp1_trig3
before delete 				
on emp1
for each row
begin
	insert into emp1_backup(id, cmd, old_sal) 
    values(old.emp_id, 'delete', old.sal);
end$$
DELIMITER ;
```



- 트리거는 DML에 의한 데이터 변동 발생 시 **자동으로** 기존 값들을 백업하거나, 변경사항을 저장할 때 유용함



## 나가며

지금까지 SQL의 기초적인 사항들에 대해선 모두 정리하였다.

이제 SQL을 통한 데이터 정의, 조작 등이 가능해졌으므로 이를 **python과의 연동을 통하여 본격적으로 DB와 코드를 연계한 프로그래밍**이 가능할 것이다. 

연동방법에 대해선 해당 포스팅에서 정리를 할 것이다.


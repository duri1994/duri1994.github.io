---
title: "파이썬: 쓰레드"
categories:	
  - Python
tags:
  - Python3
  - Concept
  - Thread
  - Threading
---

# 쓰레드(Thread)

##### 배경지식

- 프로그램: 관련있는 명령어들의 집합
- pc = program counter => code 한줄씩(fetch) 읽어와 해석, 실행 => 이렇게 여러줄의 코드를 쭉 실행(합쳐서 한개의 프로그램)
- 작업(task) - 프로그램 한개를 실행하는 과정(fetch - decode(해석) - execute 반복)

  - 하나의 작업은 프로세스(Process)와 Thread로 구현된다.
- 스케줄러: 어느 순간에 어떤 작업(task)을 실행할지 결정하는 역할
  - 프로그램은 오로지 cpu에서만 실행될 수 있으므로 잘 관리되어야 함
- 유니 태스킹: 한 순간에 한 task만 실행(예전 도스창)
- 멀티 태스킹: 한 순간에 여러 task만 실행, 하나의 프로그램을 task단위로 쪼개서 구현해야 함



##### 쓰레드란 무엇인가

- 결론부터 말하면, **멀티태스킹을 가능**하게 해주는 **프로그램(프로세스) 실행의 단위**이다.
  - 하나의 프로세스는 **여러개의 쓰레드로 구성**될 수 있다.
  - 프로세스가 **os로부터 할당받은 자원(메모리 등)을 이용**한다. 
  - 쓰레드도 프로세스처럼 ready, run, wait 등의 상태를 가진다.
- 쓰레드마다 독립적으로 스택(stack)과 레지스터(Register)가 할당된다.
  - 따라서 **각 쓰레드는 독립적인 실행 흐름이 가능**하며
  - 쓰레드간 전환이 일어날 때 각 쓰레드별로 어디까지 작업을 수행했는지 **독립적으로 기억**한다.



##### 쓰레드는 왜 쓰이는가? 쓰레드의 장점

- 프로세스보다 **생성/종료에 소요되는 시간이 짧다.**
- 쓰레드 간 **전환이 빠르다**. (여러 작업을 동시에 효율적으로 수행할 수 있다.)



##### 쓰레드가 필요한 예시 - 멀티태스킹이 필수인 예시

- <u>채팅 프로그램</u>(텍스트)
  - 내가 채팅을 쓰고 있을 때, 동시에 다른 사람의 채팅도 읽어오는 작업이 수행되어야 한다.
  - 여러 사람이 동시에 채팅을 칠 수 있어야 한다.



## 파이썬으로 쓰레드 다뤄보기

##### 쓰레드 패키지

- 파이썬은 쓰레드와 관련된 API를 활용할 수 있게 해주는 패키지를 제공한다.

```python
import threading 
```



##### 쓰레드 생성

th = **threading.Thread(target=param1, args=param2)**  
- **param1**: 이 쓰레드가 **실행할 코드를 정의한 함수**  
- **param2**: param1에서 지정한 함수가 **파라미터가 필요하다면** param2에서 **그 인자를 <u>tuple형태</u>**로 전달 



##### 쓰레드 시작

th**.start()**: 쓰레드를 **ready 상태**로 만듬
- 이후 **cpu를 획득하면 run 상태**가 되어 실행됨
- 실행 **종료 후 다시 ready 상태**로 돌아감



##### 쓰레드 대기상태 전환

**time.sleep(t)**: 실행중인 쓰레드를 **t시간만큼 대기(wait)상태**로 만듬.

- **t시간이 끝나면 다시 ready상태**로 돌아가 차례를 기다림
- 자기 **순서가 다시 돌아오면 cpu 획득해서 다시 run 상태**가 됨



##### 쓰레드 구현


```python
import threading 
import time

def f(): # 쓰레드에서 실행할 함수
    for i in range(26):
        print('th:', i)
        time.sleep(1)
        
def main():
    th = threading.Thread(target=f) # 쓰레드 생성 # 이 쓰레드는 f함수의 코드를 실행한다.
    th.start()
    s = 'abcdefghijklmnopqrstuvwxyz'
    for i in s:
        print('main:', i)
        time.sleep(0.3)
```


```python
main() # (주의) 실행결과가 실행할 때 마다 다르다.
```

    th:main: a
     0
    main: b
    main: c
    main: d
    th: 1
    main: e
    main: f
    main: g
    th: 2
    main: h
    main: i
    main: j
    th: 3
    main: k
    main: l
    main: m
    main:th: 4
     n
    main: o
    main: p
    main: q
    th: 5
    main: r
    main: s
    main: t
    th: 6
    main: u
    main: v
    main: w
    th: 7
    main: x
    main: y
    main: z
    th: 8
    th: 9
    th: 10
    th: 11
    th: 12
    th: 13
    th: 14
    th: 15
    th: 16
    th: 17
    th: 18
    th: 19
    th: 20
    th: 21
    th: 22
    th: 23
    th: 24
    th: 25


 - f작업과 main작업이 교차되기 때문에 결과가 매번 달라진다.  
 - 이는 f함수를 **쓰레드를 사용**해서 실행했기 때문이다.
 - 만약 쓰레드를 쓰지 않았다면 위에서부터 순서대로 실행되었을 것이다.



##### 멀티쓰레드 구현


```python
# 쓰레드 여러개를 동시에 실행

def f1(num): # parameter가 있는 함수
    for i in range(26):
        print(f'th{str(num)}:', i)
        time.sleep(0.5)
        
def f2():
    s = 'abcdefghijklmnopqrstuvwxyz'
    for i in s:
        print('th4:', i)
        time.sleep(0.5)
        
def main():
    th1 = threading.Thread(target=f1, args=(1,)) # 함수에 넣을 parameter를 tuple로 입력
    th1.start()
    th2 = threading.Thread(target=f1, args=(2,))
    th2.start()
    th3 = threading.Thread(target=f1, args=(3,))
    th3.start()
    th4 = threading.Thread(target=f2)
    th4.start()
    
    for i in range(100, 126):
        print('main:', i)
        time.sleep(0.5)
```


```python
main()
```

    th1: 0
    th2: 0
    th3: 0
    th4: a
    main: 100
    th1:th3: th2: 1
     11
    
    th4:main: 101
     b
    th1:th3:th2: 2
     2
     2
    th4:main:  c
    102
    th1:th3:th2: 3
    ...
    
    th1: 22
    main:th4:th2: 23
      x
    th3:123
     23
    th1: 23
    th3:th4:th2:main: 24
     124
     24
     y
    th1: 24
    th4:main:th2:th3: 125
     25
     z
     25
    th1: 25

- 4개의 쓰레드에 올라탄 작업들이 계속해서 전환되면서 실행되었음을 확인할 수 있다.


​    

## 실습: 간단한 채팅 프로그램

- 쓰레드를 이용하여 간단한 채팅 프로그램을 만들어보자.
  - 아직 서버-클라이언트의 소켓 통신은 배우지 않았으므로, 서버 메세지는 정해진 문자열을 하나씩 출력해주는 것으로 대체한다.
  - **작업1(클라이언트)**: 키보드 입력 받아 입력 받은 메시지 출력(10번) 
  - **작업2(서버역할)**: 리스트 s에서 문자열 1초마다 1개씩 읽어서 출력 

- 위의 2가지 작업은 동시에 이뤄져야 하므로 쓰레드를 활용해야할 것이다.

 

##### 실습코드1. 쓰레드의 활용

```python
# 이 코드는 jupyter notebook에선 실행안됨. 주피터는 입력을 여러번 받지 못한다. 
# pycharm 활용
import threading, time


def f1():
    for i in range(10):
        msg = input('입력할 메세지:')
        print(msg)
    print('th1 종료')

def f2():
    a = 'abcdefghij'
    s = []
    for i in a:
        s.append(i+i+i)
    for i in s:
        print('서버 메세지:',i)
        time.sleep(1)
    print('th2 종료')


def main():
    th1 = threading.Thread(target=f1)
    th1.start()
    th2 = threading.Thread(target=f2)
    th2.start()
    print('main 종료')

main()
```

    입력할 메세지:main 종료 # 주의: main함수는 th1, th2를 실행하고 바로 종료. 다만 쓰레드는 계속 실행된다.
    서버 메세지: aaa
    서버 메세지: bbb
    d서버 메세지: ccc
    서버 메세지: ddd
    ㅇㅇㅇ
    ㅇㅇㅇ
    입력할 메세지:서버 메세지: eee
    ㄱㄱ
    ㄱㄱ
    입력할 메세지:서버 메세지: fff
    ㄴㄴㄴ
    ㄴㄴㄴ
    입력할 메세지:서버 메세지: ggg
    ㄷㄷㄷ
    ㄷㄷㄷ
    입력할 메세지:서버 메세지: hhh
    ㅎㅎ서버 메세지: iii
    ㅎ
    ㅎ
    입력할 메세지:서버 메세지: jjj
    29
    29
    입력할 메세지:th2 종료
    카카
    카카
    입력할 메세지:ㅎㅎ
    ㅎㅎ
    입력할 메세지:ㅈㅈ
    ㅈㅈ
    입력할 메세지:ㅌㅌ
    ㅌㅌ
    th1 종료
    
    Process finished with exit code 0

- 위 코드는 3개의 쓰레드(main, th1, th2)가 동시에 실행되는 코드이다.  

- 만약 이 코드를 쓰레드 없이 함수를 호출하여 실행한다면, 입력을 10번 받은 이후에야 서버 메세지가 출력될 것이다.  (동시실행이 불가능하기 때문)



##### 실습코드2. 쓰레드 간 공유자원이 있을 때 동기화 처리가 필요한 이유

```python
# 실습코드2: 위험성이 존재하는 코드(동기화 처리를 하지 않은 코드)
import threading, time

# 전역변수 flag
flag = False # 공유자원: 여러개의 쓰레드가 공유해서 사용하는 데이터나 자원
             # 쓰레드의 공유자원은 동기화처리가 필요하다.

def my_input():
    global flag
    while True:
        msg = input('msg:') # 종료 메세지: /end
        if msg == '/end':
            print('채팅 종료')
            flag = True
            break
        print('my input:', msg)
    print('th1 종료')
    
def my_output():
    # global flag
    s = ['aaa', 'bbb', 'ccc', 'ddd', 'eee', 'fff', 'ggg', 'hhh', 'iii', 'jjj']
    cnt = 0
    while True:
        if flag: # 함수 내에 따로 flag가 지역변수로 선언되지 않으면 자동으로 전역변수로 적용
            break
        else:
            print('server msg:', s[cnt%10])
            cnt += 1
            time.sleep(1)
    print('th2 종료')
    
def main():
    th1 = threading.Thread(target=my_input)
    th1.start()
    th2 = threading.Thread(target=my_output)
    th2.start()
    print('main 종료')

main()
```

- 위 코드는 실행이 잘 될수도 있으나, 위험성이 존재한다.  
- 이유는 두 쓰레드에서 실행되는 함수가 **전역변수 'flag'를 공유**하기 때문이다.  
  - 공유자원은 별도로 '동기화' 처리가 필요하다.



##### 실습코드3. 동기화

- 동기화를 하면 배타적 접근 제공. 공유자원에 동시에 접근하는 쓰레드들을 관리한다.(줄세우기)
- 한 쓰레드가 공유자원을 사용 중이면 나머지 쓰레드드르이 접근을 막아준다.
- 파이썬에선 전역변수를 함수처럼 작성하여 구현한다.


```python
# 공유자원 동기화 처리
import threading, time

# 전역변수 flag
flag = False # 공유자원
             

def my_input():
    global flag
    while True:
        msg = input('msg:') # 종료 메세지: /end
        if msg == '/end':
            print('채팅 종료')
            flag = True
            break
        print('my input:', msg)
    print('th1 종료')
    
def my_output(flag): # parameter로 전역변수 flag를 받음
    # global flag
    s = ['aaa', 'bbb', 'ccc', 'ddd', 'eee', 'fff', 'ggg', 'hhh', 'iii', 'jjj']
    cnt = 0
    while True:
        if flag(): # flag를 함수처럼 사용. 
            break
        else:
            print('server msg:', s[cnt%10])
            cnt += 1
            time.sleep(1)
    print('th2 종료')
    
def main():
    th1 = threading.Thread(target=my_input)
    th1.start()
    th2 = threading.Thread(target=my_output, args=(lambda: flag,)) 
    th2.start()
    print('main 종료')

main()
```

- flag 값을 lambda 익명함수에 실어서 my_output 함수에 대입
- my_output 함수 내에선 flag 값을 '함수'처럼 인식하고 사용한다.
- 따라서 my_input에서 조작에 의해 flag값이 변하면 이를 인식하고 my_output에도 적용

- my_output함수는 전역변수 flag를 parameter로 받아 함수처럼 사용한다.
- 쓰레드 생성 시 args = (lambda: flag,) / lambda: 익명함수를 간략하게 정의하는 방법
- 이렇게 코드를 통해 '동기화'를 구현할 수 있다.



###### cf. 익명함수 lambda

- 익명함수를 간략히 정의해주는 문법. **즉석에서 한번만 실행**할 함수
- **lambda 'paramters': 'return'**


```python
# 람다: 익명함수 간략히 정의. 즉석에서 한번만 실행할 함수
# lambda 'paramters': 'return'
(lambda x, y: x+y)(2,3)
```



## 나가며

- 여러 작업을 동시해 수행할 수 있는 쓰레드를 정확히 이해하기 위해선 컴퓨터에 대한 종합적 이해가 필요하다고 느꼈다.
- 위의 배경지식은 '전산학개론' 등을 통해 배울 수 있지만 비전공자인 나에게는 생소한 부분이었다.
  - 교재로 쭉 공부하는 것은 비추. 전공자도 상당히 지루하다고 한다.
  - 잘 정리된 위키글 등을 통해 개념파악을 하고 교재는 참고서로 활용하여 공부를 하도록 해보자.

- 솔직히 코드를 구현하는 것보다는 지루하고 재미없는 과정이다! 그러나 코드에 대한 깊은 이해를 위해선 반드시 필요한 공부라고 본다.

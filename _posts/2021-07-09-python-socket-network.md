---
title: "파이썬: TCP/IP 소켓 통신"
categories:	
  - Python
tags:
  - Python3
  - Concept
  - Socket
  - TCP/IP
  - Network
typora-root-url: ../
---

## 네트워크(소켓 통신)

#### 배경지식

##### 주소

- **맥**주소: 하드웨어 주소. 랜카드에 고유넘버 있음. 로컬 네트워크 통신에서 사용
- **논리**주소: 인터넷에서 사용하는 주소
- **IPv4**: **32비트** 주소
  - **A class: netID(1B):hostID(3B)** => 네트워크 2^8개 / 한 네트워크 내 컴퓨터 2^32개
    - 동네 수는 적지만, 한 네트워크 내 컴퓨터 수가 큼 => **큰 네트워크** 구성에 적합(대규모 네트워크 형성)
  - **B class: netID(2B):hostID(2B)** => 네트워크 2^16개 / 한 네트워크 내 컴퓨터 2^16 
    - **중형 네트워크** 구성에 적합
  - **C class: netID(3B):hostID(1B)** => 네트워크 2^24개 / 한 네트워크 내 컴퓨터 2^8개
    - **실생활**에서 많이 접하는 주소



##### 서브넷마스크
1) **netID를 확인**하기 위한 마스크
	ex) A class: 255.0.0.0
	B class: 255.255.0.0
	C class: 255.255.255.0 => 개인 컴퓨터의 [제어판] -> [네트워크 및 인터넷] -> [네트워크 및 공유센터] -> [이더넷] -> [자세히] 에서 볼 수 있다.



2) **네트워크를 더 쪼개서 사용**하고자 할 때 사용 (why? 트래픽 조절을 위해)

- C class에서 netID는 3바이트로 표현됨. 1바이트는 host 주소를 표현하는데, 이 비트를 더 쪼개서 더 많은 네트워크를 표현할 수 있음(subnetID 활용)
- ex) hostID 8비트 중 2비트를 subnet으로 지정할 경우
  - 4개의 하위 네트워크를 구축할 수 있음. 이 subnet에는 컴퓨터 2^6개로 구성됨
    => 서브넷마스크: 255.255.255.192 (192 = 11000000(2)



##### 네트워크 계층

1) **응용 계층**: 데이터를 받아서 어플리케이션에서 사용. 포트번호(네트워크 앱을 구분하는 고유한 번호)  
--------소켓---------  
2) **전송계층(TCP/UDP)**: 데이터를 목적지에 전달하기 위해 길을 수립(라우팅). 패킷을 전달, 패킷 수신, 오류 체크  
3) **ip 계층(IP/ARP/RARP)**: 전송할 데이터를 패킷 단위로 단편화. 패킷에 목적지, 보내는 주소를 논리주소로 설정. 주소 변환(논리주소 <-> 맥주소)  

4) **물리계층**: 랜선으로 직접 물려있는 통신. 맥주소로 로컬 통신



##### 소켓 통신종류  

1. **TCP**(전송 제어 프로토콜)
   - <u>한번 수립한 루트를 연결이 끊어질 때까지 계속 사용</u>함 -> **신뢰성이 높은편, 연결지향적**
   - 상대방이 패킷을 받았는지 <u>확인, 순서, 오류 등을 체크</u> -> **속도는 느림**
2. **UDP**(사용자 데이터그램 프로토콜)
   - **신뢰성이 떨어짐, 비연결지향적**
   - 패킷 <u>전달할 때 마다 새 길을 수립</u>하고 <u>순서, 오류 체크 안함</u> -> **속도는 빠름**
   - **전사적으로 메세지 전달**하는 작업이나 **브로드캐스팅**에 적합함



#### 소켓 통신

 소켓(Socket)이란 프로세스들로 하여금 네트워크를 통해 서로 통신을 할 수 있도록 하는 '창구'의 역할을 하는 것을 의미한다.  각 프로세스들은 다른 프로세스와 데이터를 주고 받기 위해선 반드시 소켓을 열고, 다른 프로세스의 소켓과 연결하여야 한다. 이러한 소켓을 이용한 네트워크 통신을 소켓 통신이라고 한다.

 소켓 통신은 다음과 같은 요소들로 구성된다.

- 프로토콜: 통신 규약. TCP와 UDP가 있다. 

- IP: 컴퓨터의 고유 주소
- PORT: IP와 매칭되는 컴퓨터 내에서 네트워크와 통신을 하고 있는 프로세스를 구분할 수 있는 유일한 번호. 컴퓨터 내에서 다른 프로세스가 같은 포트번호를 가지면 안된다.

 



## 파이썬으로 서버-클라이언트 구현

파이썬을 이용하여 서버와 클라이언트가 서로 특정 포트로 연결되어 실시간으로 양방향 통신을 하는 간단한 에코프로그램을 구현해보자. 

에코(메아리)프로그램이란, 클라이언트가 서버로 보낸 메세지를 서버가 받아 그대로 클라이언트에게 되돌려 주는 프로그램을 말한다.



### 1. 서버-클라이언트가 1:1로 접속되는 에코프로그램

- 서버 측의 주소(ip)와 포트번호를 지정
  - 이 때, 다른 프로그램이 주로 사용하는 포트번호를 피해서 설정
- 서버 소켓을 열어 클라이언트가 접속할 수 있도록 함(통신종류는 TCP)


```python
# 서버 프로그램 (TCP)
import socket, time

host = 'localhost' # 서버 컴퓨터의 ip(여기선 내 컴퓨터를 서버 컴퓨터로 사용) 
                   # 본인의 ip주소 넣어도 됨(확인방법: cmd -> ipconfig)
port = 3333  # 서버 포트번호(다른 프로그램이 사용하지 않는 포트번호로 지정해야 함)

# 서버소켓 오픈(대문을 열어둠)
# socket.AF_INET: 주소종류 지정(IP) / socket.SOCK_STREAM: 통신종류 지정(UDP, TCP)
# SOCK_STREAM은 TCP를 쓰겠다는 의미
server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# 여러번 ip.port를 바인드하면 에러가 나므로, 이를 방지하기 위한 설정이 필요
# socket.SOL_SOCKET: 소켓 옵션
# SO_REUSEADDR 옵션은 현재 사용 중인 ip/포트번호를 재사용할 수 있다.
# 커널이 소켓을 사용하는 중에도 계속해서 사용할 수 있다
server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)

# server socket에 ip와 port를 붙여줌(바인드)
server_socket.bind((host, port))

# 클라이언트 접속 준비 완료
server_socket.listen()

print('echo server start') # echo program: 입력한 값을 메아리치는 기능(그대로 다시 보냄)

# accept(): 클라이언트 접속 기다리며 대기
# 클라이언트가 접속하면 서버-클라이언트 1:1 통신이 가능한 작은 소켓(client_soc)을 만들어서 반환
# 접속한 클라이언트의 주소(addr) 역시 반환
client_soc, addr = server_socket.accept()

print('connected client addr:', addr)

# recv(메시지크기): 소켓에서 크기만큼 읽는 함수
# 소켓에 읽을 데이터가 없으면 생길 때까지 대기함
data = client_soc.recv(100)
msg = data.decode() # 읽은 데이터 디코딩
print('recv msg:', msg)
client_soc.sendall(msg.encode(encoding='utf-8')) # 에코메세지 클라이언트로 보냄

time.sleep(5)
server_socket.close() # 사용했던 서버 소켓을 닫아줌
```

    echo server start
    connected client addr: ('127.0.0.1', 64465)
    recv msg: hello

```python
# 클라이언트 프로그램
# 서버 프로그램은 주피터, 이것은 파이참으로 실행 - 동시에 두개 프로그램 실행을 위해서.
import socket

server_ip = 'localhost' # 위에서 설정한 서버 ip
server_port = 3333 # 위에서 설정한 서버 포트번호

socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
socket.connect((server_ip, server_port))

msg = input('msg:') # 서버로 보낼 msg 입력
socket.sendall(msg.encode(encoding='utf-8'))

# 서버가 에코로 되돌려 보낸 메시지를 클라이언트가 받음
data = socket.recv(100)
msg = data.decode() # 읽은 데이터 디코딩
print('echo msg:', msg)

socket.close()
```

위의 소켓통신의 흐름을 간단하게 도표로 그려보면 다음과 같다.

<img src = "/assets/images/post_image/concept_socket/socket.png"/>



### 2. 위 코드를 한명의 클라이언트가 메시지 여러번 주고 받을 수 있게 수정

- 클라이언트가 '/end' 입력하면 서버, 클라이언트 모두 종료
  - 무한루프 활용


```python
# 서버
import socket, time

host = 'localhost' 
port = 3333 

# 서버소켓 오픈
server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
server_socket.bind((host, port))

# 클라이언트 접속 준비 완료
server_socket.listen()

print('echo server start')

#  클라이언트 접속 기다리며 대기 
client_soc, addr = server_socket.accept()

print('connected client addr:', addr)

# 클라이언트가 보낸 패킷 계속 받아 에코메세지 돌려줌
while True:
    data = client_soc.recv(100)
    msg = data.decode() 
    print('recv msg:', msg)
    client_soc.sendall(msg.encode(encoding='utf-8')) 
    if msg == '/end':
        break

time.sleep(5)
print('서버 종료')
server_socket.close() # 사용했던 서버 소켓을 닫아줌
```

    echo server start
    connected client addr: ('127.0.0.1', 59300)
    recv msg: wow
    recv msg: msg1
    recv msg: msg2
    recv msg: msg3
    recv msg: msg4
    recv msg: /end
    서버 종료

```python
# 클라이언트
import socket

server_ip = 'localhost' # 위에서 설정한 서버 ip
server_port = 3333 # 위에서 설정한 서버 포트번호

socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
socket.connect((server_ip, server_port))

# /end 입력될 때 까지 계속해서 서버에 패킷을 보냄
while True:
    msg = input('msg:') 
    socket.sendall(msg.encode(encoding='utf-8'))
    data = socket.recv(100)
    msg = data.decode() 
    print('echo msg:', msg)
    
    if msg == '/end':
        break

socket.close()
```



### 3. 여러 클라이언트가 에코서버에 접속할 수 있도록 서버프로그램을 수정

- 쓰레드 활용  
  - 접속하는 클라이언트마다 소켓을 오픈하고 쓰레드에 담아 각각 동시에 실행할 수 있도록 함.


```python
# 여러 클라이언트가 접속 가능한 서버 프로그램
import socket, time, threading

def f1(soc): # 클라이언트 소켓(soc)을 parameter로 사용
    while True:
        data = soc.recv(100)
        msg = data.decode() 
        print('recv msg:', msg)
        soc.sendall(msg.encode(encoding='utf-8'))
        if msg == '/end':
            break

def main():
    host = 'localhost'
    port = 3333

    # 서버소켓 오픈
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    server_socket.bind((host, port))

    # 클라이언트 접속 준비 완료
    server_socket.listen()
    print('echo server start')

    # 접속대기 반복(여러 클라이언트)
    while True:
        client_soc, addr = server_socket.accept() # 접속대기
        # 클라이언트 연결 시, 1:1 통신소켓을 오픈 +쓰레드에 전달/실행
        th = threading.Thread(target=f1, args=(client_soc,))  
        th.start()
        print('connected client addr:', addr)


    time.sleep(5)
    print('서버 종료')
    server_socket.close() 
```


```python
main()
```

    echo server start
    connected client addr: ('127.0.0.1', 49800)
    recv msg: ASD
    recv msg: /end


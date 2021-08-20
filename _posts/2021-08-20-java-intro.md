---
title: "Java: 개요 및 개발환경 구축"
categories:	
  - Java
tags:
  - Java 8
  - Concept
---



# 1. Java

#### Java의 특징

- 범용 개발언어: standalone app부터 web app, mobile에 이르기까지 다양한 곳에서 활용
- 객체지향 언어(OOP)
- 풍부한 API: 오픈소스이기 때문
- **플랫폼 독립적(platform independency)**
  - o/s(운영체제): 윈도우, 리눅스, 맥, 솔라리스 등
  - DBMS: mysql, oracle, db2 등
  - WAS(Web Application Server)
- 동적 바인딩
- 멀티쓰레드(JBM이 프로그램 단위에서 제공)
- 보안 우수
- ***complied 방식***(<u>수행속도 fast</u>) +***interpreted 방식***(<u>o/s 독립</u>, 요즘은 이 장점이 더욱 부각)
  - 수행속도는 하드웨어로 해결할 수 있는데, o/s 독립성은 그렇지 못하다.
  - 기존 C, C++ 컴파일 방식에 비해선 느릴 수 있으나 운영체제 독립적(우리나라에서 많이 씀)

<br>

#### Java version history

- 1.0, 1.1: Java
- 1.2~1.4: Java2
- 1.5(5.0), 1.6(6)~1.8(8): Java, 오픈소스
  - 실무에선 1.8 무료버전을 제일 많이 사용. 이거로 수업 진행할 예정
- 1.8.202 이후: 유료화
- 16버전: 현재

- OpenJDK: 무료

<br>

#### Java Edition

- JavaSE(Standard ed.): 앱 개발
- JavaEE(Enterprise ed.): 웹 개발(Servelt7JSP, EJB, JavaMail, JMS 등)
- JavaME(Micro ed.): 모바일 및 임베디드 개발

<br>

##### cf. 용어설명

###### 객체(object)

- i.e. 자동차, 책, 냉장고, 홍길동

<br>

###### API(Application Programming Interface)

- 미리 만들어서 제공되는 라이브러리
- 미리 만들어져 있는 **class들의 묶음**
  - `*.jar`
  - JavaSE API 모음: `t.jar`

<br>

###### OS(플랫폼) 독립적(platform independency)

- **자바 실행환경**(**JRE**: Java Runtime Env.)
  - `java.exe`(실행명령어)
  - **JRE = JVM(자바가상머신) + API**

- **자바 개발환경**(**JDK**: Java Development Kit)
  - `javac.exe`(컴파일러)
  - `java.exe`(실행명령어)
  - `javadoc.exe`(api 문서 자동생성기)
  - `jar.exe`(클래스 묶음, *.jar, *.war, *.ear)

  - **JDK = JRE + TOOLS**(`javac.exe` (컴파일러) 등)

<br>

###### 개발언어의 작동방식 종류

1. ***complied 방식***
   - c, c++, vb 등
     - (1) 소스코드작성: `Hello.cpp`
     - (2) 컴파일: `Hello.exe`(o/s lib 포함)
     - (3) 실행: `hello.exe`, `HELLO.exe`
   - 장점: <u>수행속도  fast</u>
   - 단점: o/s 종속적



2. ***interpreted 방식***
   - python, lisp, html 등
   - 전제조건: 번역기(interpreter) 설치, 브라우저
     - (1) 소스코드작성: hello.html, hello.py
     - (2) 
     - (3) 번역 실행: 한줄 읽고 번역 실행
   - 장점: <u>o/s 독립적</u>(단, 전제조건: 운영체제에 맞는 번역기가 필요)
   - 단점: 수행속도 비교적 느림(다만, 하드웨어 발달로 현재는 크게 문제되지 않음)

<br>

***

### Java 프로그래밍 개발 순서

- (1) 소스코드작성: HelloWorld.java

- (2) 컴파일:

  - 명령프롬프트 > javac <options> HelloWorld.java

    => HelloWorld.class (byte code: 바이트코드, o/s lib 포함하지 않음. 중간 단계의 기계어)

- (3) 자바 번역실행: JVM(Java Virtual Machine, 자바 가상머신)

  - 명령프롬프트> java <options> HelloWorld
    - 실행시 주의사항
      - 대소문자를 구분한다. 
      - 확장자를 포함시키면 안됨
      - 반드시 실행메서드가 존재해야됨
      - '자바 실행 메서드': `public static void main(String[] args){}`

<br>

***

### Java 프로그래밍 주의사항

##### 주의사항

- **대소문자를 구분**하므로 정확하게 표기해야 함
- 명령문 끝에는 반드시 ;(**세미콜론**)를 작성
- {}는 반드시 짝을 이뤄야 함
- **줄 맞추기**(for 가독성)
- ***이름 명명규칙*** 준수

<br>

##### 식별자(identifier) 명명 주의사항

- 이름(클래스, 변수, 메서드, 패키지, 상수 등의 이름)
- <u>영문자나 밑줄, $</u> 등으로 시작할 수 있음
- <u>숫자</u>도 사용가능, 그러나 <u>시작문자로는 사용 못한다.</u>
- 길이 제한 x
- <u>의미있는 이름</u>으로 지정해야 함
- 약어는 가능한 한 전체적으로 지정(for 가독성, 분명한 의미전달)
  - <u>용어집(설계: Data Dictionary)</u>

- *예약어(keyword)*는 식별자로 사용불가
- 공백도 불가능

<br>

##### 이름 명명규칙(Naming Convention)

- 클래스이름: Upper camel case(대문자시작+대문자시작)
  - i.e. HelloWorld, System, StringBuffer
- 변수이름: Lower camel case(소문자+이후대문자시작)
  - i.e. length, companyName
- 메서드이름(): Lower camel case
  - i.e. length(), toString()
- 패키지이름: (소문자.소문자)
  - 물리적: 폴더(디렉토리) 개념
  - 같은 종류의 클래스, 권한 제한을 목적으로 분리를 위한 개념
    	- i.e. java.lang, java.util, java.sql
- 상수이름: (모두대문자_모두대문자)
  - i.e. PI, E, MAX_VALUE
  - 커스텀 상수: `public static final 타입 상수명 = 상수값`

<br>

###### 예약어(keyword)

- Java에서 사용목적이 미리 정의되어 있는 식별자: 모두 소문자로 작성
  - i.e. public, class, static, void, if, for, do, while
- 몇가지 예외사항
  - 예약어이지만 지원x: const
  - 예약어는 아니지만 식별자로 사용 자제: sizeof
  - 예약어처럼 사용되는 상수: true/false, null

<br>

# 2. Java 개발환경 구축

#### 설치 프로그램 목록

1. JDK(자바 개발 도구)

- 자바 개발환경
- [오라클홈페이지](https://www.oracle.con/index.html) > 상단 products > software > java
- [JDK 다운로드 링크](https://www.oracle.com/java/technologies/oracle-java-archive-downloads.html)
- API Documentation(API 도움말)
  - Online
  - Offline(권장, JDK 다운로드시에 동일버전으로 받을 것)

- 8u202(무료버전) + 8u301(documentation) 같이 다운로드

- 환경변수 설정
  - JAVA_HOME = C:\Program Files\Java\jdk1.8.0_202
  - PATH = %JAVA_HOME%/BIN;기존path

<br>

2. Eclipse(IDE)

- 통합 개발환경 tool
  - 장점: 오픈소스
  - 공공기관, 엔터프라이즈(대기업) 등에서 이클립스 기반으로 개발
- [eclipse 홈페이지](eclipse.org)
- [다운로드목록](https://www.eclipse.org/downloads/packages/)
  - Eclipse IDE for Enterprise Java and Web Developers 최신버전으로 다운로드
- 압축해제(설치폴더): `C:\00.practice\eclipse`

- 실행 후 환경설정(우상단에서 Java EE로 되어있는걸 open perspective 이용하여 Java로 변경)

<br>

###### cf. 로컬폴더 구조

- 기본폴더
  - `C:\00.practice`

- 프로그램 다운로드 폴더위치
  - `down_apps`
  - `down_apps\jdk>`-
  - `down_apps\eclipse>`

<br>

#### 설치여부 및 버전확인

##### 목적(실무)

- 실무에서 코드가 잘 돌아가지 않을 때 설치여부 및 버전확인을 하는 것이 중요하다.

- 다중 프로젝트 투입(운영) 시 다양한 jdk 버전을 활용하여야 하는 경우가 자주 발생

- JDK(javac)

- JRE(java)

<br>

##### 방법

명령프롬프트

> javac -version

​	직접 설치한 javac의 버전 확인

> java -version

​	개발자는 아니지만, 다른 java관련 앱을 설치받아 사용할 때 버전값이 나온다.

> path

<br>

#### 기타 준비사항

##### 한글 인코딩

-- euc-kr, ksc5601: 한글, 영문

-- utf-8(권장): 한글, 영문 + 다국어, html5, ajax(비동기통신)

<br>

##### 이클립스 환경설정

- 단위: workspace단위, project단위, file단위

- workspace 환경설정

  - Window > preferences
  - spelling: Encoding UTF-8
  - encoding:
  - Workspace Text file encoding UTF-8
  - Web > html, css, jsp 모두: UTF-8

- Java 설정

  - Build path output folder name: bin -> classes
  - complier: version 1.8로 변경
  - installed JREs: C:\Program Files\Java\jdk1.8.0_202  (name: jdk1.8.0_202)(이렇게 버전이름으로 name을 설정해야 버전별로 나눠서 사용 손쉽게 가능)


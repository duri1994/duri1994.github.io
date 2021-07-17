---
title: "웹프로그래밍: 개요"
categories:	
  - WebProgramming
tags:
  - html/css/javascript
  - Flask
---

#### 들어가며

어제자부터 웹 프로그래밍에 대해서 찍먹으로 배우기 시작했다. 취업전선에 처음 나섰을 때 웹개발, 프론트엔드, 백엔드 등등 여러가지 알쏭달쏭한 말들을 보며 저건 뭘까 생각했는데 드디어 직접 배워볼 기회가 생겼다. 아직 배움이 매우 짧지만 어느 정도 이해한 선에서 웹 프로그래밍에 대한 간단한 개요를 작성해보고자 한다.



# 웹 프로그래밍

프로그래밍을 통해 웹페이지 등을 구현하는 웹 프로그래밍은 크게 프론트엔드와 백엔드 프로그래밍으로 나뉜다.

**프론트엔드(Front-end) 프로그래밍**은 프로세스의 시작단계로써 브라우저 단에서 클라이언트로부터 다양한 형태의 입력을 받아 그것들을 백엔드가 사용할 수 있도록 처리하는 코드를 구축하는 것을 말한다. **클라이언트 쪽 프로그래밍**이라고 이해하면 될 듯하다.

**백엔드(Back-end) 프로그래밍**은 서버 측에서 프론트엔드의 입력을 받아 이를 처리하는 코드를 작성하는 것을 말한다. 이를테면 DB에서 데이터를 저장하거나 데이터를 꺼내오는 작업 같은 것 말이다. 프로세스의 마지막 단계로써 웹페이지 상에선 가시적으로 확인할 수 없는 코드들을 의미한다. **서버 쪽 프로그래밍**이라고 이해하면 될 듯하다.



### 웹에서 사용되는 프로그래밍 언어

**Front-end(view)**: **html, css, JavaScript**, ajax, jquery, vue(프레임어) 등

- **html**: 웹 페이지의 기본 뼈대를 만든다. 이미지/텍스트/표/링크 등을 구현할 수 있다.

  - Chrome 브라우저의 개발자 도구로 웹페이지 html 소스를 확인할 수 있다.

- **css**: html에 디자인 속성을 적용하는 언어. html요소의 크기, 위치, 색상 등을 설정하는 html의 확장언어

- **JavaScript**: 웹페이지의 동적 표현, 이벤트 처리 기능 구현. html 소스 안의 script 태그 안에다 작성함

- **ajax**: asynchronous javascript and xml(비동기화 자바스크립트와 xml) - 웹페이지 변화 최소화

  - 동기화 예시: 함수 호출 시 함수를 먼저 실행하고, 그 이후에 호출한 위치의 코드가 실행
  - 비동기화 예시: 함수를 호출하면 함수 & 호출한 위치의 코드가 동시에 실행(like 쓰레드)
  - xml = DOM(Document Object Model) 역할(문서를 객체화, 문서 실시간 탐색, 수정 가능)

- **jquery**: JavaScript에서 자주 사용하는 기능을 미리 만들어놓은 라이브러리 파일

  

**Back-end(model-vo,dao,service)** - python, java 등...



### 웹 프레임워크(Framework)

**웹 프레임워크**란 웹페이지/웹애플리케이션/웹서비스 개발 보조용으로 만들어지는 애플리케이션 프레임워크의 일종이다. 맨바닥에서 웹페이지를 개발하는 것은 매우 난감한 문제이며, 개발 과정에서 겪는 어려움을 줄이는 것을 주 목적으로 한다. 주로 DB연동, 템플릿, 세션관리, 코드 재사용 등의 기능을 포함한다.

웹 프레임워크는 **풀스택(FullStack) 프레임워크**와 **마이크로(Micro) 프레임워크**로 나뉜다.

**풀스택 프레임워크**는 웹서비스를 만들 때 필요한 요소를 모두 포함하고 패키지처럼 한번에 설치하여 사용한다. 설치 직후에 웹서비스를 할 수 있어 편리하지만 속도가 느리다.

**마이크로 프레임워크**는 풀스택 프레임워크가 아닌 것으로, 기본제공 기능은 적지만 속도가 빠르며 커스터마이징에 용이하다는 장점이 있다. 대표적인 마이크로 프레임워크의 예시론 **Python 기반의 Flask**가 있다. 앞으론 Flask를 활용하여 웹 프로그래밍을 배워볼 예정이다. 이 프레임워크 위에 프론트엔드, 백엔드 코드들을 작성하여 사용한다.
---
title: "프로젝트일기: Project1 Day 3"
categories:	
  - Diary
tags:
  - 개발일기
  - 프로젝트
  - 인공지능SW개발자양성과정
---

## 3일차(210624) 예상치 못한 문제. 창의적인 로직 고민

 어느새 프로젝트 3일차 아침이 밝았다. 어제 먼저 자신의 몫을 끝낸 인원들은 코드를 합치고 테스트를 해보거나, 다른 파트에서 애를 먹는 부분을 같이 도와주기로 했다. 이번 프로젝트는 그동안 배운 vo/dao/service 패턴의 코딩을 복습하는 차원에서 하는 것이기 때문에 처음 역할을 분배할 때 모든 인원이 패턴 한셋트씩을 담당하도록 했다. 단순하게 테이블 하나씩을 맡아 그와 관련된 기능을 정의하도록 했는데, 몇몇 인원들이 코딩을 하는 것에 상당히 애를 먹고 있었다. 그래서 서로 돕기로 하고 작성하던 코드를 받아 함께 수정하며 간단한 설명을 진행했다. 어떤 분은 최근에 몸이 안좋으셔서 수업을 못듣는 바람에 나는 기존의 실습 코드를 답습한 샘플 코드로 최대한 기본적인 내용체계부터 차근차근 설명해드렸다. 그런데 다른 기능들도 회원관리기능과 비슷한 구조로 짜면 간단하게 완성할 수 있을 줄 알았는데 생각보다 작업을 진행할수록 몇몇 기능들이 생각보다 구현하는 것이 어렵다는 것을 깨닫게 되었다. 예상치 못한 복병과 맞닥뜨린 것이다.

 처음에 우리 조가 하는 프로젝트는 강사님피셜로 다른 조보다 기능이 다양하고 복잡해보인다고 했다. 처음엔 그래도 별거 아니겠거니 하고 생각했는데 까고 보니 조금 난감했다. 데이터베이스의 테이블 간 관계가 조금 복잡하고 원하는 기능을 정확하게 구현하기 위하여 실습 코드를 그대로 답습하는 것이 아닌 추가적인 응용이나 새로운 로직이 필요하다는 것을 느꼈다. 개발경험이 전무한 나에겐 조금 도전적인 과제가 될지도 모르겠다는 생각이었다. 

 다행이 대부분의 기능은 3~4시간의 고민 끝에 구현해낼 수 있었다. 해결한 것 중에 특히 배달주문을 받는 기능을 해결한 것이 기억에 남아 여기에 짧게 기록한다. 만들어놓은 테이블 체계 상 세부주문을 받기 전에 임시로 새로운 주문 레코드가 db에 저장될 필요가 있었다. 그래야 고유한 order_id가 생성되기 때문이었다. 그런데 중간에 주문을 포기하거나, 주문이 되지 않는 지역에서  주문을 시도한 경우 정상적인 주문이 들어가면 안되기 때문에, 먼저 들어갔던 임시주문을 판별하여 삭제하거나 수정해야 했다. 그래서 이 임시주문을 판별하기 위한 방법에 대한 고민을 했는데, 나름 전공이었던 화학-생물 쪽에서 매커니즘 연구 등에  많이 쓰이는 '표지(마킹)'방식을 접목시키면 좋을 것 같다는 생각이 들었다. 그래서 임시로 특정 컬럼에 '@@'문자열과 고유한 주문자 id를 결합한 특수한 문자열을 마킹 개념으로 대입함으로써, 그 레코드가 특정인의 임시 주문 레코드로 구분이 되도록 설정할 수 있었다. 분명 현장에서는 많이 쓰일지도 모르는 익숙한 트릭이어서 별거 아닐지도 모르지만, 개인적으로는 나름 뿌듯하게 생각할만한 새로운 로직의 발견이었다고 생각한다.

 어쨌든 이제 대부분의 기능 구현은 끝났다. 이제 남은 기능의 구현에 집중하고, 여러 사람들의 코드를 통합하는 과정이 필요하다. 내일부턴 3일간 휴일이기에 다음주 월요일에 같이 코드를 리뷰하고 최종 산물을 만들 수 있도록 잔업들을 마무리해야할 것이다.
---
title: "프로젝트일기: Project1 Day 4(end)"
date: '2021-06-28 23:44:00 +0900'
categories:	
  - Diary
tags:
  - 개발일기
  - 프로젝트
  - 인공지능SW개발자양성과정
typora-root-url: ../
---

## 4일차(210628) 프로젝트 마무리

  어느새 프로젝트를 마무리하는 날이 되었다. 각자 담당한 기능을 구현한 코드는 주말에 모두 수합하여 통합해놓은 상태였다. 그래서 오늘은 발표를 위한 최종 정리와 코드 리뷰를 목표로 삼았다.



##### 클래스 다이어그램

 발표 자료에 반드시 포함되어야 할 목록 중 '클래스 다이어그램'이란 생소한 것이 있었다. 인터넷으로 찾아보니 클래스 다이어그램이란 전체 프로그램에서 클래스들 간의 관계를 도식으로 표현한 그림이라고 한다. 이번 우리 프로젝트의 클래스 다이어그램을 그려보면 다음과 같았다. 

![class_diagram](/assets/images/coupangproject_class_diagram.png)

 클래스 다이어그램을 직접 그려보며 여러 가지 이점들과 우리 프로젝트의 문제점들이 생각났다.

- 한눈에 클래스 간의 관계를 볼 수 있어서 머릿속에서 구조가 자연스럽게 정리되었다.
- 코딩이 비효율적으로 구현된 부분이 보였다. 이를테면 ordercheckModel의 OrderDetailVo는 orderModel의 그것을 호출해와서 사용할 수도 있었을 것이다. 
- 다른 클래스의 메서드들이 상당수 중복되는 역할을 수행하고 있는 경우도 있었다. 만약 순차적으로 하나씩 코드를 완성했었다면, 다른 클래스의 메서드를 호출해와서 효율적인 코딩이 가능했을 것이다.
- 위의 문제점들을 생각해보면, 순차적으로 코딩하거나 아예 코딩 전에 설계 단계에서 먼저 클래스 다이어그램을 그려보아 나중에 메서드를 최소한으로 정의하고 필요할 때 호출해서 사용하는 식의 효율적인 운용이 가능할 것이라 생각했다.



## 마무리, 프로젝트를 마친 소감

##### 첫 프로그래밍 협업. 개인적으로 성공적이라 생각되는 프로젝트

 이 프로젝트는 살면서 처음으로 한 프로그래밍 협업이었다. 팀원 모두가 이런 작업이 처음이었고, 조율을 통해 역할을 적절하게 분배하고 자신이 맡은 부분을 각자 코드로 구현하여 통합하는 과정에서 많은 것을 깨닫고 느낄 수 있었다. 글재주가 없어 내가 프로젝트를 하며 느낀 것을 두서없이 쭉 적어보고자 한다.

- 프로젝트 설계의 중요성을 뼈저리게 느꼈다. 텍스트로만 각각의 기능을 계획하고 모두가 동시에 코딩 작업에 들어갔는데, 예상과 다르게 코드로 구현하는 것이 상당히 어려운 경우가 참 많았다. 또 다른 인원이 맡은 코드에서 구현될 메서드 등을 활용할 여지가 있었음에도, 동시에 작업을 진행하느라 같은 기능을 수행하는 메서드들이 여러번 정의되는 문제가 나타났다.
- 사전의 프로젝트 설계는 협업자들 사이의 '공통된 약속'을 만들어준다는 점에서도 매우 중요하다고 생각했다. 개인들이 작성하는 코드들간의 일관성 및 통일성이 어느 정도 보장되고, 최종적인 통합도 편하게 이루어질 수 있기 때문이다.
- 중앙 컨트롤타워의 중요성을 새삼 느꼈다. 병렬적으로 작업이 이루어짐에도 그 안에는 순서를 지켜면서 해야만 가능한 작업이 있는 만큼, 현재 각 인원이 어떤 작업을 수행하고 있는지, 어떤 작업을 완료했는지, 어떤 어려움에 봉착했는지 현황을 확인할 수 있는 매개체가 필요했다. 그렇게 만든 것이 구글 공유 스프레드시트 현황판이었다. 이를 통해 모든 인원들의 현황파악을 비교적 빠르고 정확하게 할 수 있었고, 특정 시점에서 어떤 작업에 집중해야할지 나아갈 방향을 수월하게 정할 수 있었다.
- 하지만 그럼에도 불구하고, 이 프로젝트의 의의는 모든 팀원들이 vo/dao/service로 이어지는 패턴의 코딩을 연습하고 개념을 숙지하는데 있었으므로 이와 관련해선 꽤 성공적인 프로젝트라고 보았다. 모두가 간단한 기능부터 때때로 아주 복잡한 기능을 구현하면서도, 최소한 vo/dao/service 패턴의 코딩에 대한 감을 잡을 수 있었다는 점이 매우 만족스러웠다.



##### 기억에 남을 첫 팀원들

- 첫 프로젝트를 함께한 팀원분들을 정말 잘 만난 것이 행운이었고, 또한 그들에게 매우 감사함을 느끼고 있다. 돌이켜보면 어쩌다보니 초짜인 내가 주도적으로 프로젝트를 추진하게 된 것 같은데 다들 적극적으로 참여해주시고 필요할 때는 결정적인 조언을 해주시는 등 정말 도움을 많이 받았다. 묵묵하게 자신의 역할을 잘 수행하면서도 내가 요청한 것들을 즉각적으로 이행해주셔서 프로젝트가 매우 성공적으로 끝날 수 있었다.
- 모두가 처음인만큼, 아직은 이 프로젝트에 관련된 내용을 완벽히 이해하지 못하는 것은 당연하다. 나도 모든걸 명확하게 깨달았다고는 절대로 말하지 못할 것이다. 프로젝트 중간에 이야기를 들어보니 몇몇 팀원분들은 코드 이해가 잘 되지 않아 제 시간에 맡은 역할을 해낼 수 있을지 고민이 많으셨다고 한다. 그럼에도 솔직하게 자신이 부족한 부분을 이야기 해주시고 열정적으로 배워보겠다는 학구열을 보여주셨다. 그래서 많이 부족한 나였지만 내가 최대한 아는 한에서 열성을 다해 설명을 해드렸고, 끝내 각자의 힘으로 자신의 역할을 다 할 수 있었다. 이렇게 열성적인 팀원분들과 함께 해서 나도 에너지를 많이 얻었던 것 같다.
- 늘 편안한 분위기에서 즐겁게 작업할 수 있었다. 실제로 오프라인에서 모여본 적은 없기에 완전히 원 팀으로 녹아든 느낌은 아니었으나 서로 이따금씩 농도 던져가며 편한 분위기를 조성해주신 팀원들 덕에 웃으면서 작업할 수 있었다. 다른 프로젝트에서 다같이 함께할 수 없다는 사실이 정말 아쉽다. 기회가 된다면 나중에 다같이 모여서 밥이라도 먹으면서 친목(?)도 다질 수 있었으면 하는 작은 소망이 있다. 물론 모두가 편안하다고 생각한다면 말이다. 나는 언제든지 찬성이니 불러주시라.



##### 나는 어떤 개발자가 되고 싶은가

 프로젝트를 진행하며 '나는 미래에 어떤 개발자가 되고 싶은지'에 대해 꾸준히 고민을 해왔다. 그 고민들을 간단히 정리해보고자 한다.

1. 기획자의 의도를 정확하게 파악하는 개발자
   - 같은 계획을 갖고 기능을 구현했음에도 한 기능에 대해서 조원들끼리 서로 이해한 내용이 다른 경우가 왕왕 있었다. 나 역시도 기획자의 의도를 착각하여 잘못된 코드를 만들기도 했었다. 결국, 기획자가 어떤 의도를 갖고 어떤 기능이 구현되기를 원하는지에 대하여 면면히 살펴보고 정확히 파악하여 작업에 들어가야 되고, 작업 중에도 끊임 없이 이에 부합하는지 점검하는 습관을 들이면 좋겠다.
2.  항상 겸손하고 열린 마인드를 가진 개발자
   - 열심히 머리를 쥐어짜가며 나름대로 열심히 쌓아올린 코드를 과감하게 버리고 체질을 개선해야 하는 때가 반드시 온다는 것을 얼핏 느낄 수 있었다. 아직 나의 개발경력은 매우 짧기 때문에 내 코드에 대한 자신감이란 것은 없다고 봐도 무방하다. 하지만 가까운 미래에 어설프게 머리가 커진 내가 근거 없는 자신감에 휩싸여 더 개선될 여지가 있음에도 기존의 스타일을 고수하다가 발전이 멈춰버리는 일은 절대로 없어야 한다고 생각했다. 당장 이번 프로젝트에서만 해도 나보다 효율적으로 코딩을 한 다른 팀원들의 작업물을 볼 수 있었고, 나는 이를 바탕으로 조금 더 발전할 여지에 대하여 생각하고 실제로 어느 정도 성장을 이뤄낼 수 있었다고 본다. 항상 스스로에게 겸손하고, 발전해나가는 개발자가 되고 싶다.
3.  소통하는 개발자
   - 물론 개발자끼리는 훌륭한 코드만으로도 충분히 소통을 할 수 있을지도 모른다. 하지만 현실에선 대부분의 경우 기획자, 디자이너 등 다양한 포지션의 동업자들과 소통을 하며 작업을 하게될 것이다. 그 때 만약, 다른 사람들이 나에게 조금 무리한 요구를 한다고 하면, 나는 곧바로 안된다고 할 것이 아니라 현실적으로 불가능한 부분과 가능한 부분을 합리적으로 판단하고 이를 정리해서 그들과의 소통을 시도해볼 것이다. 물론 군생활을 제외하면 제대로 된 조직생활을 해본적이 없기에 실제로는 예상치 못한 더욱 강력한 고난과 문제에 부딪히겠지만, 그 때마다 최대한 상황을 이성적으로 판단하고 소통을 시도하는 개발자가 될 수 있었으면 한다.



#### 끝

이번 프로젝트를 함께 즐겁게 수행했던 하늘님, 창민님, 민하님, 태현님, 시은님에게 정말 감사하다.

항상 '열정'을 외치던 모습처럼, 끝까지 열정적으로 배우고 모두 미래에 모두 멋진 개발자가 되어 봤으면 한다.
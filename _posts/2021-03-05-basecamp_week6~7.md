---
title: "[2021/2/22 ~ 2021/3/5] 베이스 캠프 6~7주차 회고"
date: 2021-03-05
categories: 
  - basecamp
toc: true
author_profile: true
toc_label: "목차" # toc 이름 설정
toc_icon: "bars" # 아이콘 설정
toc_sticky: false # 마우스 스크롤과 함께 내려갈 것인지 설정
author_profile: true
---
## 베이스캠프 6주차

### 코드리뷰 & 리팩토링

개발을 마치고 시작된 Sprint 6!

6주차에서 해야할 일은 지금까지 작성한 코드를 되돌아보고 리팩토링하는 것이었다.

이를 위해 베이스캠프 운영진님들과 코드 리뷰시간을 가졌는데, 우리 TF가 작성한 코드와 다른 TF가 작성한 코드를 비교해보며 참 많은 것을 보고 배웠던 것 같다.

그래도 코드 리뷰 중에서 가장 기억에 남았던 것은 바로 내가 작성했던 **두레이 웹 훅 기능에 대한 리뷰**였는데 그 내용은 다음과 같다.

두레이 웹 훅 알림 기능을 구현할 당시에 "우선 기능부터 되게 만들자!" 라는 생각에 알림 기능을 service 계층에서 구현하지 않고 Controller 계층에서 직접 구현하여 요청을 처리하도록 만들었다. 

Controller가 요청을 받아서 처리하는 것까지는 뭐... 이해하려고 하면 이해할 수는 있겠지만 문제는 그게 아니라 요청을 처리하는 방법 그 자체였다.

내가 구현한 두레이 웹 훅 알림 메시지 기능은 사용자가 회원가입을 하거나 예매를 했을 때 사용자의 아이디를 query string으로 갖는 특정 url에 post 요청을 보내는 방법으로 작동했다. 

어떤 문제가 있을지 짐작이 가셨을까?

"오.. 특정 사용자의 아이디만 알아도 그 사용자한테 메시지를 보낼 수 있다고?"

ㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋ 맙소사

예를 들어서 moviedapanda/webhook?memberId=a 로 post 요청을 보내면 a라는 사용자에게 두레이 메시지가 전송되는 형태였던 것이다.

정말... 이런 바보같은 실수를 하다니.. 만들어놓고 "헤헤 된다." 하며 웃었던 과거가 정말 부끄러웠다.

문제가 무엇이었는지 파악을 하고나서 코드를 수정하는 것은 어렵지 않았다.

컨트롤러 계층에 드러나있던 비즈니스 로직을 서비스 계층으로 내리고 해당 로직이 사용자에게 드러나지 않도록 수정함으로써 문제는 간단하게 해결되었다.

6주차에 느꼈던 것은 말 그대로 아는 만큼 보인다는 것, 이를 위해서는 더 많은 경험과 스스로를 갈고 닦는 시간이 필요할 것 같다.



## 베이스캠프 7주차

### 웹 서버 스케일 아웃

7주차는 6주차에 리팩토링한 서비스를 확장하는 것이 주된 과제였다.

이를 위해 웹 서버를 2대로 늘렸을 때 생기는 문제를 가정하고, 해당 문제들을 해결하는 방법을 팀원들과 같이 생각해봤는데 그 내용은 다음과 같다.

**발생 가능한 문제**

1. 두 서버간 세션은 어떻게 관리?
2. 이미지 저장과 불러오기는 어떻게?
3. 요청이 많다면 이 요청들을 각 서버에 어떻게 분산시킬까?

**해결 방법**

1. 공유 세션!
2. NHN Cloud의 Object Storage!
3. LB!

그리고 이 해결 방법들을 이용하여 웹 서버를 스케일 아웃하는 것이 Sprint 7의 과제였다.

이 중에서 내가 맡은 것은 공유 세션을 구현하는 것이었는데, 구현 방법은 레디스를 이용하는 방법과 데이터베이스를 이용하는 방법이 존재했다.

그리고 나는 DB를 이용하여 세션을 구현하기로 했다.

### 공유 세션 구축하기 - DB 세션

DB를 이용하여 공유 세션을 구현하기 위해 가장 먼저 떠오른 생각은 세션을 저장하는 테이블을 어떻게 정의할 것인가였다.

세션은 하나의 세션 아이디에 여러개의 key, value를 가지는 다수의 세션 속성들이 있기 때문에 세션 아이디를 저장하는 테이블과 세션 속성을 저장하는 테이블이 필요하다고 생각했다.

따라서 처음에는 세션 아이디를 저장하는 테이블과 세션 속성을 저장하는 테이블을 새로 정의하여 세션을 구현하기 시작했다.

그러나 우리 서비스에서 사용하는 세션의 속성들은 memberId, doorayUrl 밖에 없었고 더이상 속성들이 확장될 것처럼 느껴지지도 않았다.

따라서 팀원들과의 회의 끝에 개발 편의성을 위하여 두개의 테이블을 사용하는 것이 아니라 세션 아이디, 회원 아이디, 두레이 주소 등을 column으로 갖는 하나의 세션 테이블만을 관리하는 구조로 변경하게 되었고 최종적으로 이 구조를 따르는 DB 세션을 구현하게 되었다.

이 과정에서 세션이 뭔지, 그리고 세션 유지 및 삭제를 인터셉터로 어떻게 다룰 수 있는지를 배우며 웹 개발 지식을 쌓을 수 있었다.



## 마무리

7주차에 부서 소개 시간을 가졌는데 아직까지 희망 부서 메일을 보내지 못했다.

정확히 말하자면 부서는 선택했지만 지망을 어떤 순서대로 적어서 제출해야할지 너무 고민이 되어서 계속 고민을 하고있다. 하하.

조금만... 조금만 더 고민을 해보고 내일은 ... 꼭 제출...해야겠다...으허허!


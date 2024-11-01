---
title: 인증과 인가
layout: section
subject: 인증과 인가
---

---
layout: default
headerEnable: true
headerTitle: 인증과 인가 이해하기
---

# 인증과 인가 무슨 차이일까?

---
layout: default
headerEnable: true
headerTitle: 인증과 인가 이해하기
---

# 인증과 인가 무슨 차이일까?

* 인증(Authentication)은 누구인지 확인하는 것
* 인가(Authorization)는 무엇을 할 수 있는지 결정하는 것

---
layout: default
headerEnable: true
headerTitle: 인증과 인가 이해하기
---

# 인증(Authentication)

* 사용자가 누구인지 확인하는 것
* 어떻게 사용자가 누구인지 확인할까?
  * 본인임을 증명할 수 있는 방법
    * 아이디 / 비밀번호
    * 생체 인식
    * OAuth
    * SMS, Email(Magic Link)
    * ...
  * 토큰

---
layout: center
headerEnable: true
headerTitle:인증과 인가 이해하기
---

# 아이디 / 비밀번호와 토큰은 무슨 차이일까?

신뢰할 수 있는 제 3자란?

---
layout: default
headerEnable: true
headerTitle: 인증과 인가 이해하기
---

# 보안과 편의 사이

* 보안이 강화될수록 편의성이 떨어짐
* 매번 아이디 / 비밀번호를 입력하는 것은 불편함
  * 자주 전송할 수록 노출될 확률이 높음 -> 보안 취약
* 토큰은 어떨까?
  * 한 번 발급 받으면 계속 사용 가능
  * 탈취되면 계정을 빼앗기는 것과 같음
  * 유효 기간을 통해 보안을 유지

---
layout: default
headerEnable: true
headerTitle: 인증과 인가 이해하기
---

# 인가(Authorization)

* 사용자가 무엇을 할 수 있는지 결정하는 것
* 누구인지 확인했다고 해서 모든 것을 할 수 있는 것은 아님
* 인가를 통해 사용자가 할 수 있는 것을 제한
  * ex) 관리자, 일반 사용자
  * ex) 가게 주인, 라이더, 고객은 서로 다른 권한을 가짐

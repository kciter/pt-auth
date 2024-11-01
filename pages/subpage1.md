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
* 인증과 인가는 서로 다른 개념
  * <accent>인증은 "당신이 누구인가?"</accent>를 묻고, <accent>인가는 "당신은 무엇을 할 수 있는가?"</accent>를 묻는다

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
    * Passwordless
      * 사용자가 소유한 것 기반
        * ex) 휴대폰, 이메일, 카드, 하드웨어 토큰 등
      * 사용자의 어떠한 것 기반
        * ex) 지문, 얼굴, 음성 등
      * 위치, 네트워크 주소, 행동패턴, ...
    * 토큰
      * JWT, OAuth2, OpenID Connect
    * ...

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

---
layout: default
headerEnable: true
headerTitle: 인증과 인가 이해하기
---

# 권한을 부여하는 방법

* 역할 기반 접근 제어(Role-Based Access Control, RBAC)
  * 사용자의 역할에 따라 권한을 부여
  * ex) 관리자, 일반 사용자
* 속성 기반 접근 제어(Attribute-Based Access Control, ABAC)
  * 사용자의 속성에 따라 권한을 부여
  * ex) 사용자 역할, 직급, 리소스의 속성, 환경, ...
  * 어떠한 리소스에 접근하려면?
    * 사용자:해당부서 직원, 리소스:해당부서의 문서, 동작:읽기
    * 사용자가 해당 부서 직원이고, 해당 부서의 문서를 읽으려고 할 때에만 읽기 가능
      * 쓰기는 불가능
* 역할 기반이 더 간단하지만, 속성 기반은 더 유연함
  * 역할 기반 - Coarse-grained
  * 속성 기반 - Fine-grained
    
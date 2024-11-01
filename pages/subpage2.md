---
title: 사용자와 계정
layout: section
subject: 사용자와 계정
---

---
layout: default
headerEnable: true
headerTitle: 인증과 인가 이해하기
---

# 사용자와 계정은 무엇이 다른가?

* 보통 사용자와 계정을 동일시 하는 경우가 많음
* 하지만 사용자와 계정은 분명히 다름
* 사용자는 시스템을 이용하는 실제 사람에 대한 정보
  * name, email, phone, address, ...
* 계정은 사용자를 식별하기 위한 정보
  * id, password, token, ...

---
layout: center
headerEnable: true
headerTitle: 인증과 인가 이해하기
---

# 제품에 따라 하나의 사용자에 여러 계정이 필요할 수 있다


---
layout: default
headerEnable: true
headerTitle: 인증과 인가 이해하기
---

# 사례

* 한 사람이 여러 인증 수단을 가질 수 있음
  * 아이디 / 비밀번호, 생체 인식, OAuth, ...
* 한 사람이 여러 이메일 계정을 가지고 있을 수 있음
  * GitHub의 경우 이메일 계정을 여러 개 등록할 수 있음
* 한 사람이 여러 소셜 계정과 연동할 수 있음
  * Facebook, Google, Twitter, GitHub, ...

---
layout: default
headerEnable: true
headerTitle: 인증과 인가 이해하기
---

# 다시 사용자와 계정으로 돌아와서

* <accent>계정</accent>을 통한 <accent>인증</accent>
* <accent>사용자</accent>에 대한 <accent>인가</accent>
* 구분하여 구현하고 모델링해야 함

---
layout: default
headerEnable: true
headerTitle: 인증과 인가 이해하기
---

# 간단한 역할 기반 접근 제어(RBAC) 모델링

```
Users ---< Accounts
  |
  |---< UserRoles >--- Roles ---< RolePermissions >--- Permissions
```

* Users:Accounts = 1:N
* Users:Roles = N:M
* Roles:Permissions = N:M

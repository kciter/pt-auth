---
title: 토큰
layout: section
subject: 토큰
---

---
layout: center
headerEnable: true
headerTitle: 인증과 인가 이해하기
---

# 토큰은 크게 두 가지로 나뉜다

One-Time Token과 Persistent Token

---
layout: default
headerEnable: true
headerTitle: 인증과 인가 이해하기
---

# One-Time Token

* 한 번만 사용할 수 있는 토큰
* 주로 비밀번호 찾기, 이메일 인증 등에 사용
* 사용 후에는 폐기되어야 함
* 사용자가 토큰을 잃어버리면 다시 발급해야 함
* 보통 만료 기간이 있음
* Nonce라고 부르기도 함

---
layout: default
headerEnable: true
headerTitle: 인증과 인가 이해하기
---

# Persistent Token

* 계속 사용할 수 있는 토큰
* 한 번 인증 후 편의를 위해 발급
* 사용자가 로그아웃하거나 토큰을 폐기할 때까지 유효
* 만료 기간을 두는 것이 안전

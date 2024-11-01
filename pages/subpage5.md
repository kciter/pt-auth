---
title: OAuth 인증
layout: section
subject: OAuth 인증
---

---
layout: default
headerEnable: true
headerTitle: 인증과 인가 이해하기
---

# OAuth란?

* OAuth라고 말하면 보통 OAuth 2.0을 의미
*	OAuth(Open Authorization)는 타사 애플리케이션이 사용자 비밀번호를 알지 않고도 사용자의 승인하에 보호된 자원에 접근할 수 있도록 해주는 인증 및 인가 프레임워크
  * 말이 조금 어려운데 한 마디로 말하면, 사용자의 동의를 받아 다른 애플리케이션에서 사용자의 정보를 사용할 수 있도록 하는 것
  * OIDC 기반으로 사용자의 신원을 확인할 수 있음
  * 보통 인증/인가 둘다 OAuth로 부르는 경우가 많음
  * 그렇지만 OAuth 2.0은 인가 프로토콜이고, OIDC는 인증 프로토콜

---
layout: default
headerEnable: true
headerTitle: 인증과 인가 이해하기
---

# OpenID Connect (OIDC)란?

* OAuth 2.0 프로토콜을 기반으로 구축된 **인증(Authentication)** 프로토콜
* OAuth 2.0의 **인가(Authorization)** 기능에 **인증** 기능을 추가하여, 사용자의 신원 확인을 가능하게 함

---
layout: default
headerEnable: true
headerTitle: 인증과 인가 이해하기
---

# 인증 / 인가 흐름

```mermaid {scale: 0.38}
sequenceDiagram
    participant User as 사용자
    participant Client as 클라이언트
    participant AuthServer as 인증 서버<br>(Authorization Server / OpenID Provider)
    participant ResourceServer as 자원 서버

    Note over Client,AuthServer: **OAuth 2.0 및 OIDC 공통 흐름 시작**

    User->>Client: 보호된 자원 또는 서비스 요청
    Client->>AuthServer: 인증 요청<br>(Authorization Request)
    AuthServer->>User: 로그인 및 권한 동의 요청
    User->>AuthServer: 자격 증명 입력 및 동의
    AuthServer->>Client: 인가 코드 전달<br>(Authorization Code)

    Client->>AuthServer: 인가 코드로 토큰 요청<br>(Token Request)
    AuthServer->>Client: 액세스 토큰 발급

    alt OpenID Connect (OIDC)
        AuthServer->>Client: ID 토큰 발급
        Client->>Client: ID 토큰 검증 및 사용자 인증 완료
    end

    Note over Client,ResourceServer: OAuth 2.0 및 OIDC 공통 흐름 지속

    Client->>ResourceServer: 액세스 토큰으로 자원 요청
    ResourceServer->>Client: 보호된 자원 응답
    Client->>User: 자원 또는 서비스 제공

    alt 추가 사용자 정보 필요 시 (OIDC)
        Client->>AuthServer: UserInfo 요청<br>(액세스 토큰 사용)
        AuthServer->>Client: 사용자 정보 응답
    end
```

---
layout: default
headerEnable: true
headerTitle: 인증과 인가 이해하기
---

# 사례 / 외부 OAuth 제공자와 연동하여 회원가입하기

```mermaid {scale: 0.5}
sequenceDiagram
    participant User as 사용자
    participant Client as 클라이언트 (웹/앱)
    participant OAuthProvider as 외부 OAuth 제공자<br>(Google/Facebook 등)
    participant Server as 서버 (백엔드)

    User->>Client: 회원가입 페이지 접속
    Client->>User: "Google/Facebook으로 회원가입" 버튼 표시
    User->>Client: "Google/Facebook으로 회원가입" 클릭
    Client->>OAuthProvider: 인증 요청 (Authorization Request)
    OAuthProvider->>User: 로그인 및 권한 동의 요청
    User->>OAuthProvider: 자격 증명 입력 및 동의
    OAuthProvider->>Client: 인가 코드 전달 (Redirect)
    Client->>Server: 인가 코드 전달
    Server->>OAuthProvider: 인가 코드로 액세스 토큰 요청
    OAuthProvider->>Server: 액세스 토큰 및 사용자 정보(ID 토큰) 전달
    Server->>Server: 사용자 정보 확인 및 회원가입 처리
    Server->>Client: 자체 액세스 토큰 발급
    Client->>User: 로그인 완료 및 서비스 이용 시작
```

---
layout: default
headerEnable: true
headerTitle: 인증과 인가 이해하기
---

# 사례 / 데이터 저장을 위한 Prepare 단계가 있는 경우

```mermaid {scale: 0.4}
sequenceDiagram
    participant User as 사용자
    participant Client as 클라이언트 (웹/앱)
    participant Server as 서버 (백엔드)
    participant OAuthProvider as 외부 OAuth 제공자<br>(Google/Facebook 등)

    User->>Client: 회원가입 페이지 접속
    Client->>User: 회원가입 양식 표시 (쿠폰 코드, 약관 동의 등)
    User->>Client: 쿠폰 코드 입력 및 약관 동의
    Client->>Server: 입력 정보 전송 (쿠폰 코드, 약관 동의)
    Server->>Server: 쿠폰 코드 검증 및 정보 저장
    Server->>Client: OAuth 인증 진행 안내
    User->>Client: "Google/Facebook으로 회원가입" 클릭
    Client->>OAuthProvider: 인증 요청 (Authorization Request)
    OAuthProvider->>User: 로그인 및 권한 동의 요청
    User->>OAuthProvider: 자격 증명 입력 및 동의
    OAuthProvider->>Client: 인가 코드 전달 (Redirect)
    Client->>Server: 인가 코드 전달
    Server->>OAuthProvider: 인가 코드로 액세스 토큰 요청
    OAuthProvider->>Server: 액세스 토큰 및 사용자 정보(ID 토큰) 전달
    Server->>Server: 사용자 정보 확인 및 회원가입 처리
    Server->>Server: 이전에 저장한 정보(쿠폰, 약관 동의)와 사용자 계정 연계
    Server->>Client: 자체 액세스 토큰 발급
    Client->>User: 로그인 완료 및 서비스 이용 시작
```

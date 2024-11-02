---
title: 토큰 통신 / 보관 방법
layout: section
subject: 토큰 통신 / 보관 방법
---

---
layout: default
headerEnable: true
headerTitle: 인증과 인가 이해하기
---

# 토큰을 교환하는 방법

* HTTP 헤더에 토큰을 포함하여 전송하는 방법
* 쿠키에 토큰을 저장하여 전송하는 방법

---
layout: default
headerEnable: true
headerTitle: 인증과 인가 이해하기
---

# 무엇이 옳은가?

* 정답은 없음 결국 트레이드 오프
* 쿠키로 전송하는 방법
  * CSRF 공격에 취약
  * 클라이언트에서 접근할 수 없음 (읽는 것도 불가능)
  * 모든 요청에 토큰이 전송됨
  * 단일 도메인에서만 사용 가능
    * 해결하려면 reverse proxy를 사용해야 함
* HTTP 헤더로 전송하는 방법
  * 토큰을 어딘가에 저장해야 함
    * Not httpOnly cookie, localStorage, sessionStorage, IndexedDB, ...
  * XSS 공격에 취약 (스크립트 실행이 가능하면 탈취 가능)
  * SPA라면 쉬운 대안이 없음

---
layout: default
headerEnable: true
headerTitle: 인증과 인가 이해하기
---

# 헤더 방식 - Bearer 토큰 인증

* Bearer: 전달자, 소지자라는 뜻
* 토큰의 내용은 JWT, Opaque Token 등 아무거나 사용해도 무방
* 보통 Authorization 헤더에 토큰을 포함하여 전송

<spacer gap="20" />

```mermaid  {scale: 0.4}
sequenceDiagram
    participant Client as 클라이언트
    participant Server as 서버
    participant AuthServer as 인증 서버 (필요한 경우)

    Client->>Server: 요청 전송<br>GET /protected/resource<br>Authorization: Bearer {토큰}
    Server->>Server: 토큰 추출 및 검증
    alt 토큰이 유효한 경우
        Server->>Server: 요청된 자원에 접근 권한 확인
        alt 접근 권한 있음
            Server->>Client: 요청된 자원 응답 (HTTP 200)
        else 접근 권한 없음
            Server->>Client: 접근 거부 응답 (HTTP 403)
        end
    else 토큰이 유효하지 않거나 만료된 경우
        Server->>Client: 인증 실패 응답 (HTTP 401 Unauthorized)
    end
```

---
layout: default
headerEnable: true
headerTitle: 인증과 인가 이해하기
---

# 쿠키 방식을 통한 로그인

```mermaid  {scale: 0.6}
sequenceDiagram
    participant User as 사용자 (브라우저)
    participant FrontendPage as 프론트엔드 페이지
    participant FrontendServer as 프론트엔드 서버
    participant Server as 백엔드 서버

    User->>FrontendPage: 로그인 페이지 요청
    FrontendPage->>FrontendServer: 로그인 페이지 요청
    FrontendServer->>FrontendPage: 로그인 페이지 응답
    FrontendPage->>User: 로그인 페이지 제공

    User->>FrontendPage: 자격 증명 입력 (아이디/비밀번호)
    FrontendPage->>FrontendServer: 로그인 요청 (아이디/비밀번호)
    FrontendServer->>Server: 로그인 요청 전달 (아이디/비밀번호)
    Server->>Server: 자격 증명 검증 및 세션 생성
    Server-->>FrontendServer: 로그인 성공 응답<br>세션 정보 포함
    FrontendServer-->>FrontendPage: 로그인 성공 응답<br>Set-Cookie: 세션 쿠키 설정
    FrontendPage->>User: 로그인 성공 알림 및 서비스 페이지 제공
```

---
layout: default
headerEnable: true
headerTitle: 인증과 인가 이해하기
---

# 쿠키 방식을 통한 인증/인가

```mermaid  {scale: 0.35}
sequenceDiagram
    participant User as 사용자 (브라우저)
    participant FrontendPage as 프론트엔드 페이지
    participant FrontendServer as 프론트엔드 서버
    participant Server as 백엔드 서버

    Note over User,Server: 1. 프론트엔드에서 직접 백엔드 호출

    User->>FrontendPage: 서비스 요청 (세션 쿠키 자동 포함)
    FrontendPage->>Server: API 요청<br>(세션 쿠키 자동 포함)
    Server->>Server: 세션 쿠키로 인증 및 권한 확인
    Server-->>FrontendPage: 요청된 데이터 응답
    FrontendPage->>User: 데이터 표시

    Note over User,Server: 2. 프론트엔드 서버가 백엔드로 중개하여 호출

    User->>FrontendPage: 서비스 요청
    FrontendPage->>FrontendServer: 서비스 요청<br>(세션 쿠키 자동 포함)
    FrontendServer->>Server: API 요청<br>(세션 쿠키 포함)
    Server->>Server: 세션 쿠키로 인증 및 권한 확인
    Server-->>FrontendServer: 요청된 데이터 응답
    FrontendServer-->>FrontendPage: 데이터 응답
    FrontendPage->>User: 데이터 표시

    Note over User,Server: 3. 프론트엔드 서버가 바로 호출하는 경우

    User->>FrontendServer: 페이지 요청
    FrontendServer->>Server: API 요청 (세션 쿠키 포함)
    Server->>Server: 세션 쿠키로 인증 및 권한 확인
    Server-->>FrontendServer: 요청된 데이터 응답
    FrontendServer-->>User: 페이지 제공 (데이터 포함)
```

---
layout: default
headerEnable: true
headerTitle: 인증과 인가 이해하기
---

# 헤더 방식을 통한 로그인

```mermaid  {scale: 0.6}
sequenceDiagram
    participant User as 사용자 (브라우저)
    participant FrontendPage as 프론트엔드 페이지
    participant FrontendServer as 프론트엔드 서버
    participant Server as 백엔드 서버

    User->>FrontendPage: 로그인 페이지 요청
    FrontendPage->>FrontendServer: 로그인 페이지 요청
    FrontendServer->>FrontendPage: 로그인 페이지 응답
    FrontendPage->>User: 로그인 페이지 제공

    User->>FrontendPage: 자격 증명 입력 (아이디/비밀번호)
    FrontendPage->>Server: 로그인 요청 (아이디/비밀번호)
    Server->>Server: 자격 증명 검증 및 토큰 생성
    Server-->>FrontendPage: 로그인 성공 응답<br>액세스 토큰 발급
    FrontendPage->>FrontendPage: 액세스 토큰 저장 (세션 스토리지, 로컬 스토리지 또는 메모리)
```

---
layout: default
headerEnable: true
headerTitle: 인증과 인가 이해하기
---

# 헤더 방식을 통한 인증/인가

```mermaid  {scale: 0.32}
sequenceDiagram
    participant User as 사용자 (브라우저)
    participant FrontendPage as 프론트엔드 페이지
    participant FrontendServer as 프론트엔드 서버
    participant Server as 백엔드 서버

    Note over User,Server: 1. 프론트엔드 페이지에서 백엔드로 직접 호출

    User->>FrontendPage: 페이지 요청
    FrontendPage->>User: 페이지 제공
    User->>FrontendPage: API 요청 준비
    FrontendPage->>Server: API 요청<br>Authorization: Bearer 액세스 토큰
    Server->>Server: 토큰 검증 및 권한 확인
    Server-->>FrontendPage: 요청된 데이터 응답
    FrontendPage->>User: 데이터 표시

    Note over User,Server: 2. 프론트엔드 서버가 백엔드로 중개하여 호출

    User->>FrontendPage: 서비스 요청
    FrontendPage->>FrontendServer: API 요청 준비<br>Authorization: Bearer 액세스 토큰
    FrontendServer->>Server: API 요청<br>Authorization: Bearer 액세스 토큰
    Server->>Server: 토큰 검증 및 권한 확인
    Server-->>FrontendServer: 요청된 데이터 응답
    FrontendServer-->>FrontendPage: 데이터 응답
    FrontendPage->>User: 데이터 표시

    Note over User,Server: 3. 프론트엔드 서버가 백엔드로 직접 호출

    User->>FrontendServer: 페이지 요청
    FrontendServer->>Server: API 요청 준비<br>Authorization: Bearer 액세스 토큰
    Server->>Server: 토큰 검증 및 권한 확인
    Server-->>FrontendServer: 요청된 데이터 응답
    FrontendServer-->>User: 페이지 제공 (데이터 포함)
```

---
layout: default
headerEnable: true
headerTitle: 인증과 인가 이해하기
---

# 잠깐! 프론트엔드 서버에서 토큰을 어떻게 알죠?

* 프론트엔드 서버가 존재하는 Next.js, Nuxt.js, Gatsby 등의 SSR 프레임워크
* 이 경우 페이지 요청시 서버에서 데이터를 결합하여 내려주는 방식이 자주 사용됨
* 토큰을 어떻게 알 수 있을까?
  * HTTP(Stateless) 환경에서 프론트엔드 서버와 페이지가 같은 토큰을 공유하려면 Cookie가 답
    * 로그인 요청 시 프론트엔드 서버를 경유하여 백엔드 서버로 전달
    * httpOnly 쿠키의 경우 받은 응답을 통해 쿠키를 httpOnly로 저장
    * 헤더 방식의 경우에도 localStorage 대신 쿠키에 저장하여 서버로 전달
      * httpOnly는 쓰지 않음


---
layout: default
headerEnable: true
headerTitle: 인증과 인가 이해하기
---

# 백엔드 서버와 도메인이 다르다면?

* 도메인이 완전히 다르다면 까다로워짐
  * SameSite 설정을 None으로 하면 쿠키 전송 가능
    * HTTPS(Secure) 사용 필수
  * 서버에서 CORS 설정을 통해 쿠키가 전송될 수 있게 허용해야 함
  * 혹은 리버스 프록시 서버를 사용하여 쿠키를 전달
* 서브 도메인 정도 차이라면 쿠키 설정으로 해결 가능
  * 도메인 설정을 `.example.com`과 같이 설정
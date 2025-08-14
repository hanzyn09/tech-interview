# Web Q&A

## 브라우저에서 URL치면 발생하는 일

1. **URL 파싱**: 브라우저가 URL을 해석하여 프로토콜, 도메인, 경로 분리
2. **DNS 조회**: 도메인을 IP 주소로 변환
   - 브라우저 캐시 → OS 캐시 → 라우터 캐시 → ISP DNS 서버 → 루트 DNS 서버 순서로 조회
3. **TCP 연결**: 3-way handshake로 서버와 연결 설정
4. **HTTP 요청**: GET 요청을 서버로 전송
5. **서버 처리**: 웹 서버가 요청을 처리하고 응답 생성
6. **HTTP 응답**: HTML, CSS, JS 등을 클라이언트로 전송
7. **렌더링**: 브라우저가 HTML 파싱하여 DOM 트리 생성, CSS 적용, JS 실행
8. **화면 표시**: 최종 렌더링된 페이지를 사용자에게 표시

## 쿠키, 세션

### 쿠키 동작방식
1. **서버에서 쿠키 생성**: `Set-Cookie` 헤더로 클라이언트에 전송
2. **클라이언트 저장**: 브라우저가 쿠키를 로컬에 저장
3. **자동 전송**: 같은 도메인 요청 시 `Cookie` 헤더로 자동 전송
4. **서버에서 읽기**: 서버가 쿠키 값을 읽어 사용자 식별

### 세션 동작방식
1. **세션 생성**: 서버에서 고유한 세션 ID 생성
2. **세션 ID 전송**: 쿠키나 URL에 세션 ID 포함하여 클라이언트에 전송
3. **세션 저장**: 서버 메모리나 DB에 세션 데이터 저장
4. **세션 식별**: 클라이언트 요청 시 세션 ID로 사용자 식별
5. **세션 만료**: 타임아웃이나 로그아웃 시 세션 삭제

### 쿠키와 세션의 차이

| 구분 | 쿠키 | 세션 |
|------|------|------|
| 저장 위치 | 클라이언트 (브라우저) | 서버 |
| 보안 | 낮음 (조작 가능) | 높음 |
| 속도 | 빠름 | 상대적으로 느림 |
| 만료 시점 | 설정한 만료 시간 | 브라우저 종료 또는 타임아웃 |
| 저장 용량 | 4KB 제한 | 서버 용량에 따라 제한 |
| 사용 예시 | 자동 로그인, 장바구니 | 로그인 상태 유지 |

## 캐시
자주 사용되는 데이터를 빠르게 접근할 수 있는 저장소에 임시 저장하는 기술입니다.

**웹 캐시 종류**:
- **브라우저 캐시**: 클라이언트 측에서 리소스 저장
- **프록시 캐시**: 중간 서버에서 캐싱
- **CDN**: 지리적으로 분산된 캐시 서버
- **서버 캐시**: 애플리케이션 레벨 캐싱

**캐시 제어 헤더**:
- `Cache-Control`: 캐시 정책 지정
- `ETag`: 리소스 버전 식별
- `Last-Modified`: 마지막 수정 시간

## URI, URL, URN에 대해서

- **URI (Uniform Resource Identifier)**: 자원을 식별하는 통합된 방법
- **URL (Uniform Resource Locator)**: 자원의 위치를 나타냄
- **URN (Uniform Resource Name)**: 자원의 이름을 나타냄

**관계**: URI ⊃ URL, URN

**예시**:
- URL: `https://www.example.com/page.html`
- URN: `urn:isbn:1234567890`

## REST, REST API, RESTful 이란?

### REST (Representational State Transfer)
HTTP 프로토콜을 기반으로 한 아키텍처 스타일입니다.

**REST 원칙**:
1. **무상태성**: 서버가 클라이언트 상태를 저장하지 않음
2. **캐시 가능**: 응답이 캐시 가능해야 함
3. **계층화**: 클라이언트-서버 구조
4. **인터페이스 일관성**: 통일된 인터페이스
5. **주문형 코드**: 필요시 코드 다운로드 (선택적)

### REST API
REST 아키텍처 스타일을 따르는 API입니다.

**설계 원칙**:
- 자원은 URI로 식별
- 행위는 HTTP 메서드로 표현
- 표현은 JSON, XML 등으로 전송

**예시**:
```
GET /users/123      # 사용자 조회
POST /users         # 사용자 생성
PUT /users/123      # 사용자 전체 수정
PATCH /users/123    # 사용자 부분 수정
DELETE /users/123   # 사용자 삭제
```

### RESTful
REST 원칙을 잘 지킨 시스템을 의미합니다.

### REST API 장점, 단점

**장점**:
- 직관적이고 이해하기 쉬운 API
- HTTP 표준을 활용한 범용성
- 무상태성으로 인한 확장성
- 캐싱 가능으로 성능 향상

**단점**:
- 표준이 없어 설계 시 고려사항 많음
- HTTP 메서드 제한
- 복잡한 쿼리 표현 어려움

## HTTP 1, 2, 3의 차이

| 구분 | HTTP/1.1 | HTTP/2 | HTTP/3 |
|------|----------|--------|--------|
| 출시년도 | 1997 | 2015 | 2020 |
| 기반 프로토콜 | TCP | TCP | QUIC (UDP) |
| 다중화 | 파이프라이닝 | 스트림 멀티플렉싱 | 개선된 멀티플렉싱 |
| 헤더 압축 | 없음 | HPACK | QPACK |
| 서버 푸시 | 없음 | 지원 | 제거됨 |
| 연결 차단 | HOL 블로킹 | 해결 | 완전 해결 |

## HTTP 응답코드

| 코드 범위 | 의미 | 주요 코드 |
|-----------|------|-----------|
| 1xx | 정보 | 100 Continue |
| 2xx | 성공 | 200 OK, 201 Created, 204 No Content |
| 3xx | 리다이렉션 | 301 Moved Permanently, 302 Found, 304 Not Modified |
| 4xx | 클라이언트 오류 | 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found |
| 5xx | 서버 오류 | 500 Internal Server Error, 502 Bad Gateway, 503 Service Unavailable |

### 301, 302 차이
- **301 Moved Permanently**: 영구적 리다이렉션, 검색엔진이 URL 변경 인식
- **302 Found**: 임시적 리다이렉션, 원래 URL 유지

### 401, 403 차이
- **401 Unauthorized**: 인증되지 않음 (로그인 필요)
- **403 Forbidden**: 인증되었으나 권한 없음 (접근 금지)

## HTTPS
HTTP over SSL/TLS의 약자로, HTTP 통신을 암호화한 프로토콜입니다.

**특징**:
- 데이터 암호화
- 무결성 보장
- 서버 인증
- 기본 포트: 443

### SSL 동작방식
1. **클라이언트 Hello**: 지원 암호화 방식 전송
2. **서버 Hello**: 선택된 암호화 방식과 인증서 전송
3. **인증서 검증**: 클라이언트가 서버 인증서 검증
4. **대칭키 생성**: 클라이언트가 대칭키 생성하여 공개키로 암호화 전송
5. **핸드셰이크 완료**: 양쪽에서 같은 대칭키 보유
6. **암호화 통신**: 대칭키로 데이터 암호화하여 통신

### HSTS란
HTTP Strict Transport Security의 약자로, 웹사이트가 HTTPS만 사용하도록 강제하는 보안 정책입니다.

**동작 방식**:
1. 서버가 `Strict-Transport-Security` 헤더 전송
2. 브라우저가 해당 도메인을 HTTPS 전용으로 기억
3. 이후 HTTP 요청을 자동으로 HTTPS로 변경

### SSL Stripping이란?
중간자 공격의 일종으로, HTTPS 연결을 HTTP로 다운그레이드시켜 통신 내용을 탈취하는 공격입니다.

**방어 방법**: HSTS 설정, 인증서 핀닝

## GET과 POST의 차이

| 구분 | GET | POST |
|------|-----|------|
| 목적 | 데이터 조회 | 데이터 전송/처리 |
| 데이터 위치 | URL 쿼리스트링 | HTTP Body |
| 데이터 길이 | 제한 있음 (2048자) | 제한 없음 |
| 캐싱 | 가능 | 불가능 |
| 브라우저 히스토리 | 남음 | 남지 않음 |
| 북마크 | 가능 | 불가능 |
| 보안 | 낮음 (URL에 노출) | 상대적으로 높음 |
| 멱등성 | 있음 | 없음 |

## 검색엔진이 뭐에요
웹상의 정보를 수집, 저장, 검색할 수 있게 해주는 시스템입니다.

**동작 과정**:
1. **크롤링**: 웹 크롤러가 웹 페이지 수집
2. **인덱싱**: 수집된 페이지를 검색 가능한 형태로 저장
3. **검색**: 사용자 쿼리에 맞는 결과 반환
4. **랭킹**: 관련성과 중요도에 따라 결과 순서 결정

### 웹 크롤러는?
웹사이트를 자동으로 탐색하며 데이터를 수집하는 프로그램입니다.

**동작 방식**:
- 시드 URL에서 시작
- 링크를 따라 다른 페이지 방문
- HTML 파싱하여 내용 추출
- robots.txt 준수

## HTTP 멱등성
같은 요청을 여러 번 수행해도 결과가 동일한 성질입니다.

**멱등한 메서드**:
- `GET`: 조회, 서버 상태 변경 없음
- `PUT`: 전체 리소스 교체, 여러 번 실행해도 같은 결과
- `DELETE`: 삭제, 없는 리소스 삭제는 변화 없음

**멱등하지 않은 메서드**:
- `POST`: 새 리소스 생성, 매번 다른 리소스 생성

## 토큰 기반 인증

서버가 클라이언트에게 토큰을 발급하고, 클라이언트가 요청 시마다 토큰을 포함하여 인증하는 방식입니다.

**장점**:
- 무상태성
- 확장성
- 다양한 클라이언트 지원

### JWT
JSON Web Token의 약자로, JSON 형태의 정보를 안전하게 전송하기 위한 토큰입니다.

**구조**: `Header.Payload.Signature`

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.
eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.
SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

**특징**:
- 자체 포함적 (self-contained)
- 서명으로 무결성 보장
- 만료 시간 설정 가능

### OAuth
사용자가 다른 서비스의 계정으로 로그인할 수 있게 해주는 개방형 표준입니다.

**OAuth 2.0 플로우**:
1. 클라이언트가 인증 서버로 리다이렉션
2. 사용자가 로그인 및 권한 승인
3. 인증 서버가 인가 코드 발급
4. 클라이언트가 인가 코드로 액세스 토큰 요청
5. 액세스 토큰으로 리소스 서버 접근

## CORS란?
Cross-Origin Resource Sharing의 약자로, 다른 도메인 간의 리소스 공유를 제어하는 메커니즘입니다.

**Same-Origin Policy**: 같은 출처(프로토콜, 도메인, 포트)에서만 리소스 접근 허용

**CORS 요청 종류**:
- **Simple Request**: 단순한 GET, POST 요청
- **Preflight Request**: OPTIONS 요청으로 사전 확인

### CORS 해결방법

**서버 측 해결**:
```javascript
// Express.js 예시
app.use((req, res, next) => {
  res.header('Access-Control-Allow-Origin', '*');
  res.header('Access-Control-Allow-Methods', 'GET, POST, PUT, DELETE');
  res.header('Access-Control-Allow-Headers', 'Content-Type, Authorization');
  next();
});
```

**클라이언트 측 해결**:
- 프록시 서버 사용
- JSONP 사용 (GET만 가능)
- 서버 측 프록시 구현

## JSON, XML의 차이

| 구분 | JSON | XML |
|------|------|-----|
| 가독성 | 높음 | 낮음 |
| 파싱 속도 | 빠름 | 느림 |
| 데이터 크기 | 작음 | 큼 |
| 스키마 검증 | 제한적 | 강력함 |
| 네임스페이스 | 없음 | 지원 |
| 주석 | 불가능 | 가능 |
| 배열 표현 | 간단 | 복잡 |

**JSON 예시**:
```json
{
  "name": "John",
  "age": 30,
  "city": "Seoul"
}
```

**XML 예시**:
```xml
<person>
  <name>John</name>
  <age>30</age>
  <city>Seoul</city>
</person>
```

## MIME이란?
Multipurpose Internet Mail Extensions의 약자로, 인터넷에서 전송되는 데이터의 형식을 나타내는 표준입니다.

**구조**: `type/subtype`

**주요 MIME 타입**:
- `text/html`: HTML 문서
- `text/css`: CSS 스타일시트
- `application/json`: JSON 데이터
- `application/javascript`: JavaScript 파일
- `image/jpeg`: JPEG 이미지
- `image/png`: PNG 이미지
- `video/mp4`: MP4 비디오

## AWS란?
Amazon Web Services의 약자로, 아마존에서 제공하는 클라우드 컴퓨팅 플랫폼입니다.

**주요 서비스**:
- **EC2**: 가상 서버 (컴퓨팅)
- **S3**: 객체 스토리지
- **RDS**: 관계형 데이터베이스
- **Lambda**: 서버리스 컴퓨팅
- **CloudFront**: CDN 서비스
- **Route 53**: DNS 서비스
- **VPC**: 가상 네트워크

**클라우드 서비스 모델**:
- **IaaS**: Infrastructure as a Service (EC2)
- **PaaS**: Platform as a Service (Elastic Beanstalk)
- **SaaS**: Software as a Service (WorkMail)

**장점**:
- 초기 투자 비용 절약
- 확장성과 유연성
- 글로벌 인프라
- 다양한 서비스 제공

## HTML/CSS/JavaScript
웹 개발의 3대 핵심 기술입니다.

### HTML (HyperText Markup Language)
웹 페이지의 구조와 내용을 정의하는 마크업 언어입니다.

**주요 요소**:
- **시맨틱 태그**: `<header>`, `<nav>`, `<main>`, `<section>`, `<article>`, `<footer>`
- **폼 요소**: `<form>`, `<input>`, `<select>`, `<textarea>`
- **미디어 요소**: `<img>`, `<video>`, `<audio>`

**HTML5 특징**:
- 시맨틱 웹 지원
- 멀티미디어 지원
- 웹 스토리지 (localStorage, sessionStorage)
- 지오로케이션 API
- 웹 워커

### CSS (Cascading Style Sheets)
웹 페이지의 스타일과 레이아웃을 정의하는 언어입니다.

**주요 개념**:
- **선택자**: 스타일을 적용할 요소 지정
- **박스 모델**: margin, border, padding, content
- **레이아웃**: flexbox, grid, float, position
- **반응형 디자인**: 미디어 쿼리 활용

**CSS3 특징**:
- 애니메이션과 전환 효과
- 그라디언트
- 그림자 효과
- 둥근 모서리
- 변형(transform)

### JavaScript
웹 페이지에 동적 기능을 추가하는 프로그래밍 언어입니다.

**주요 특징**:
- **동적 타입**: 런타임에 타입 결정
- **프로토타입 기반**: 클래스 없이 상속 구현
- **함수형 프로그래밍**: 일급 객체로서의 함수
- **이벤트 기반**: 사용자 상호작용 처리

**ES6+ 주요 기능**:
- let/const, 화살표 함수, 템플릿 리터럴
- 구조 분해 할당, 스프레드 연산자
- 클래스, 모듈, Promise, async/await

**DOM 조작**:
```javascript
// 요소 선택
const element = document.getElementById('myId');
const elements = document.querySelectorAll('.myClass');

// 이벤트 처리
element.addEventListener('click', function(event) {
    // 클릭 처리
});

// 동적 내용 변경
element.innerHTML = '<p>새로운 내용</p>';
element.style.color = 'red';
```

## 성능 최적화
웹 사이트의 로딩 속도와 사용자 경험을 개선하는 기법들입니다.

### 프론트엔드 최적화
**리소스 최적화**:
- **압축**: Gzip, Brotli 압축
- **최소화**: CSS/JS 파일 minify
- **이미지 최적화**: WebP 포맷, 적절한 크기
- **번들링**: 여러 파일을 하나로 결합

**로딩 최적화**:
- **지연 로딩**: 필요한 시점에 리소스 로드
- **프리로딩**: 중요한 리소스 미리 로드
- **코드 스플리팅**: 필요한 코드만 로드
- **트리 셰이킹**: 사용하지 않는 코드 제거

**렌더링 최적화**:
- **Critical CSS**: 첫 화면에 필요한 CSS 우선 로드
- **Virtual DOM**: React, Vue 등에서 사용
- **메모이제이션**: 계산 결과 캐싱
- **디바운싱/스로틀링**: 이벤트 호출 제한

### 백엔드 최적화
**데이터베이스 최적화**:
- 인덱스 최적화
- 쿼리 튜닝
- 연결 풀 관리
- 읽기 전용 복제본 활용

**서버 최적화**:
- **캐싱**: Redis, Memcached
- **로드 밸런싱**: 트래픽 분산
- **CDN**: 정적 리소스 분산
- **압축**: 응답 데이터 압축

**성능 측정 도구**:
- **Lighthouse**: 구글의 웹 성능 측정 도구
- **WebPageTest**: 상세한 성능 분석
- **Chrome DevTools**: 브라우저 개발자 도구
- **GTmetrix**: 종합적인 성능 평가

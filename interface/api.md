# API 명세서

## 실패 시 응답 공통 포맷

```json
{
  "success": false,
  "error": {
    "code": "AUTH.INVALID_CREDENTIALS",
    "message": "이메일 또는 비밀번호가 올바르지 않습니다.",
    "details": {
      "fieldErrors": [
        { "field": "email", "reason": "INVALID_FORMAT" }
      ]
    }
  }
}
````

## 1. Auth 관련 API (Spring Boot 서버)

### 1. 로컬 회원가입

* **API TYPE**: `POST`
* **END POINT**: `/auth/join`

#### Request

```json
{
  "email": "user@example.com",
  "password": "password123!",
  "nickname": "홍길동"
}
```

#### Response

```json
{
  "success": true,
  "data": {
    "email": "user@example.com",
    "name": "홍길동"
  }
}
```

### 2. 로컬 로그인

* **API TYPE**: `POST`
* **END POINT**: `/auth/login`

#### Request

```json
{
  "email": "user@example.com",
  "password": "plainPassword123!"
}
```

#### Response

```json
{
  "success": true,
  "data": {
    "accessToken": "eyJhbGciOiJIUzI1...",
    "refreshToken": "eyJhbGciOiJIUzI1..."
  }
}
```

### 3. 로그아웃

* **API TYPE**: `POST`
* **END POINT**: `/auth/logout`
* **REQUEST**: Header, Cookie

#### Response

```json
{
  "success": true
}
```

### 4. Access Token 재발급

* **API TYPE**: `POST`
* **END POINT**: ❓ *(미정)*
* **REQUEST**: ❓ *(미정)*
* **RESPONSE**: ❓ *(미정)*

### 5. 회원정보 조회

* **API TYPE**: `GET`
* **END POINT**: ❓ *(미정)*
* **REQUEST**: ❓ *(미정)*
* **RESPONSE**: ❓ *(미정)*

## 2. 요리 추천 API (Clova X 서버)

### 1. Health 관련 API

#### 1. 헬스 체크

* **API TYPE**: `GET`
* **END POINT**: `/api/v1/health`
* **REQUEST**: 없음

#### Response

```json
{}
```

#### 2. Clova X 서버 버전 조회

* **API TYPE**: `GET`
* **END POINT**: `/api/v1/`
* **REQUEST**: 없음

#### Response

```json
{}
```

### 2. Chat 관련 API

#### 간단한 단일 메시지 채팅 (스트리밍)

* **API TYPE**: `POST`
* **END POINT**: `/api/v1/chat/simple`

#### Request

```json
{
  "message": "안녕하세요!",
  "max_tokens": 256,
  "temperature": 0.5,
  "system_prompt": null
}
```

#### Response

```json
{}
```

#### 에러 응답 (422)

```json
{
  "detail": [
    {
      "loc": ["body", "message"],
      "msg": "field required",
      "type": "value_error.missing"
    }
  ]
}
```

```

위와 같이 마크다운 문법으로 작성했습니다. 바로 복사해서 사용하실 수 있습니다!
```

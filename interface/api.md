# API 명세서


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

## 3. 식단표 관련 API (Spring Boot 서버)

### 1. 식단표 생성

* **API TYPE**: `POST`
* **END POINT**: `/meal-plan/create`

#### Request

```json
{
  "title": "9월 3주차 감량 플랜",
  "period": "weekly",
  "goals": "weight_loss",
  "age": 28,
  "gender": "female",
  "basic_metabolism": 1380,
  
  "items": [
    {
      "day_no": 1,
      "meal_type": "lunch",
      "item": "닭가슴살 샐러드",
    }, ...
  ]
}
```

#### Response

```json
{
  "success": true,
  "data": {
    "meal_plan_id": "123"
  }
}
```

### 2. 식단표 목록 조회

* **API TYPE**: `GET`
* **END POINT**: `/meal-plan`

#### Response

```json
{
  "success": true,
  "data": [
	  {
	  "title": "9월 3주차 감량 플랜",
	  "period": "weekly",
	  "goals": "weight_loss",
	  "age": 28,
	  "gender": "female",
	  "basic_metabolism": 1380,
	  "create_dt": "2025-09-14"
	  "update+dt": "2025-09-15"
	  }, ...
  ]
}
```

### 3. 식단표 상세 조회

* **API TYPE**: `GET`
* **END POINT**: `/meal-plan/{meal_plan_id}`

#### Response

```json
{
  "success": true,
  "data": [
	  {
      "day_no": 1,
      "meal_type": "lunch",
      "item": "닭가슴살 샐러드",
    }, ...
  ]
}
```

### 4. 식단표 삭제

* **API TYPE**: `DELETE`
* **END POINT**: `/meal-plan/delete/{meal_plan_id}`


#### Response

```json
{
  "success": true
}
```

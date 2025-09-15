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
    } //optional
  }
}
```


## 1. Auth 관련 API (Spring boot 서버)

1) 로컬 회원가입
- API TYPE: POST
- END POINT: /auth/join
- REQUEST:
  ```json
  {
  "email": "user@example.com",
  "password": "password123!",
  "nickname": "홍길동"
  }
  ```
- RESPONSE:
  ```json
  {
  "success": true,
  "data": {
    "email": "user@example.com",
    "name": "홍길동",
    }
  }
  ```

2) 로컬 로그인
- API TYPE: POST
- END POINT: /auth/login
- REQUEST:
  ```json
  {
  "email": "user@example.com",
  "password": "plainPassword123!"
  }
  ```
- RESPONSE:
  ```json
  {
  "success": true,
  "data": {
    "accessToken": "eyJhbGciOiJIUzI1...",
    "refreshToken": "eyJhbGciOiJIUzI1..."
    }
  }
  ```

3.  로그아웃
- API TYPE: POST
- END POINT: /auth/logout
- REQUEST: header, cookie
- RESPONSE:
  ```json
  {
  "success": true,
  }
  ```

4. access token 재발금
- API TYPE: POST
- END POINT
- REQUEST:
- RESPONSE:
  
5. 회원정보 조회
- API TYPE: GET
- END POINT:
- REQUEST:
- RESPONSE:

   

# 2. 요리 추천 API (Hyper X 서버)




  

# 테이블 명세서

## users (계정 관련 테이블)

| 컬럼 | 타입 | 제약조건 | 기본값 | 설명 |
| --- | --- | --- | --- | --- |
| id | BIGINT IDENTITY | PK |  | 사용자 ID |
| email | VARCHAR(320) | UNIQUE, NOT NULL, IDX |  | 로그인 이메일 |
| name | VARCHAR(100) | NOT NULL |  | 이름 |
| password_hash | TEXT | NOT NULL |  | 해시된 비밀번호 |
| del_yn | VARCHAR(1) | NOT NULL | 'N' | 계정 삭제 여부 (Y/N) |
| created_dt | TIMESTAMPTZ | NOT NULL | now() | 생성일 |
| updated_dt | TIMESTAMPTZ | NOT NULL | now() | 수정일 |

## refresh token (refresh token 관련 테이블)

| 컬럼 | 타입 | 제약조건 | 기본값 | 설명 |
| --- | --- | --- | --- | --- |
| id | BIGINT IDENTITY | PK |  | 토큰 ID |
| user_id | BIGINT | FK→users.id |  | 소유자 ID |
| token_hash | TEXT | UNIQUE(user_id, token_hash), NOT NULL |  | 해시된 Refresh 값 |
| issued_at | TIMESTAMPTZ | NOT NULL | now() | 발급 시각 |
| expires_at | TIMESTAMPTZ | NOT NULL |  | 만료 시각 |
| revoked | VARCHAR(1) | NOT NULL | 'N' | 폐기 여부 |
| replaced_by | SERIAL | FK→refresh_tokens.id | NULL | 회전된 새 토큰 ID |

## meal_plan (식단표 정보 관리 테이블)

| 컬럼 | 타입 | 제약조건 | 기본값 | 설명 |
| --- | --- | --- | --- | --- |
| id | BIGINT IDENTITY | PK |  | 식단표 ID |
| user_id | BIGINT | FK (users.id) |  | 식단표 소유자 user ID |
| type | VARCHAR(10) | NOT NULL (weekly / monthly) | 식단표 타입 (주간/월간) |
| goals | VARCHAR(50) | NOT NULL (weight_loss / muscle_growth / regular) |  | 식단 목적 (체중감량/근육성장/일반식) |
| age | INT |  |  | 나이 |
| gender | ENUM ('male', 'female', ‘ohter’) |  |  | 성별 |
| basic_metabolism | INT |  |  | 기초대사량 |

## meal_plan_detail (식단표 내 식단 관리 테이블)

| 컬럼 | 타입 | 제약조건 | 기본값 | 설명 |
| --- | --- | --- | --- | --- |
| id | BIGINT IDENTITY | PK | | 식단 ID |
|  |  |  |  |  |

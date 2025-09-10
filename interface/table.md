# 테이블 명세서

##계정 테이블

| 컬럼 | 타입 | 제약조건 | 기본값 | 설명 |
| --- | --- | --- | --- | --- |
| id | BIGINT IDENTITY | PK |  | 사용자 ID |
| email | VARCHAR(320) | UNIQUE, NOT NULL, IDX |  | 로그인 이메일 |
| name | VARCHAR(100) | NOT NULL |  | 이름 |
| password_hash | TEXT | NOT NULL |  | 해시된 비밀번호 |
| del_yn | VARCHAR(1) | NOT NULL | 'N' | 계정 삭제 여부 (Y/N) |
| created_dt | TIMESTAMPTZ | NOT NULL | now() | 생성일 |
| updated_dt | TIMESTAMPTZ | NOT NULL | now() | 수정일 |

##refresh token 테이블

| 컬럼 | 타입 | 제약조건 | 기본값 | 설명 |
| --- | --- | --- | --- | --- |
| id | BIGINT IDENTITY | PK |  | 토큰 ID |
| member_id | BIGINT | FK→members.id |  | 소유자 ID |
| token_hash | TEXT | UNIQUE(member_id, token_hash), NOT NULL |  | 해시된 Refresh 값 |
| issued_at | TIMESTAMPTZ | NOT NULL | now() | 발급 시각 |
| expires_at | TIMESTAMPTZ | NOT NULL |  | 만료 시각 |
| revoked | VARCHAR(1) | NOT NULL | 'N' | 폐기 여부 |
| replaced_by | SERIAL | FK→refresh_tokens.id | NULL | 회전된 새 토큰 ID |

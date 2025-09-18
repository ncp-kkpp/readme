# 테이블 명세서

## users (계정 관련 테이블)

| 컬럼         | 타입           | 제약조건                 | 기본값 | 설명              |
|--------------|----------------|--------------------------|--------|-------------------|
| id           | BIGINT IDENTITY | PK                       |        | 사용자 ID          |
| login_id     | VARCHAR(320)   | UNIQUE, NOT NULL, IDX    |        | 로그인 이메일      |
| name         | VARCHAR(100)   | NOT NULL                 |        | 이름              |
| password_hash| TEXT           | NOT NULL                 |        | 해시된 비밀번호    |
| del_yn       | VARCHAR(1)     | NOT NULL                 | 'N'    | 계정 삭제 여부 (Y/N) |
| created_dt   | TIMESTAMPTZ    | NOT NULL                 | now()  | 생성일            |
| updated_dt   | TIMESTAMPTZ    | NOT NULL                 | now()  | 수정일            |

```(sql)
CREATE TABLE users (
    id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    login_id VARCHAR(320) NOT NULL UNIQUE,
    name VARCHAR(100) NOT NULL,
    password_hash TEXT NOT NULL,
    del_yn VARCHAR(1) NOT NULL DEFAULT 'N' CHECK (del_yn IN ('Y','N')),
    created_dt TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_dt TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE INDEX idx_users_email ON users(login_id);
```


## meal_plan (식단표 관리 테이블)

| 컬럼            | 타입           | 제약조건                                  | 기본값 | 설명                               |
|-----------------|----------------|-------------------------------------------|--------|------------------------------------|
| id              | BIGINT IDENTITY | PK                                        |        | 식단표 ID                          |
| title           | VARCHAR(100)   |                                           |        | 식단표 이름                        |
| user_id         | VARCHAR(320)   | FK (users.login_id)                       |        | 식단표 소유자 user ID              |
| period          | VARCHAR(10)    | NOT NULL (weekly / monthly)               |        | 식단표 타입 (주간/월간)            |
| goal            | VARCHAR(50)    | NOT NULL (weight_loss / muscle_growth / regular) |        | 식단 목적 (체중감량/근육성장/일반식) |
| age             | INT            |                                           |        | 나이                               |
| gender          | VARCHAR(10)    | NOT NULL (male / female / other)          |        | 성별                               |
| basic_metabolism| INT            |                                           |        | 기초대사량                         |
| create_dt       | TIMESTAMPTZ    | NOT NULL                                 | now()  | 생성일                             |
| update_df       | TIMESTAMPTZ    | NOT NULL                                 | now()  | 수정일                             |
| del_yn          | VARCHAR(1)     | NOT NULL                                 | 'N'    | 삭제 여부                          |

```sql
CREATE TABLE meal_plan (
    id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,                      
    title VARCHAR(100),
    user_id VARCHAR(320) NOT NULL,                                  
    period VARCHAR(10) NOT NULL CHECK (period IN ('weekly','monthly')),
    goal VARCHAR(50) NOT NULL CHECK (goals IN ('weight_loss','muscle_growth','normal')),
    age INT,
    gender VARCHAR(10) CHECK (gender IN ('male','female','other')),
    basic_metabolism INT,
    created_dt TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_dt TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

## meal_plan_item (식단표 내 식단 관리)

**UNIQUE (meal_plan_id, day_no, meal_type)**

| 컬럼        | 타입                              | 제약조건                        | 기본값 | 설명                           |
|-------------|-----------------------------------|---------------------------------|--------|--------------------------------|
| id          | BIGINT IDENTITY                   | PK, NOT NULL                    |        | 식단 ID                        |
| meal_plan_id| BIGINT                            | FK (meal_plan.id), NOT NULL     |        | 식단표 ID                      |
| day_no      | INT                               | NOT NULL, weekly → 1~7<br>monthly → 1~31 |        | 식단 번호 (date로 변경 고민중...) |
| meal_type   | ENUM(breakfast, lunch, dinner)    | NOT NULL                        |        | 식사 종류 (아침/점심/저녁)     |
| item        | VARCHAR(1000)                     |                                 |        | 식단 내용                      |

```sql
CREATE TABLE meal_plan_item (
    id            BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    meal_plan_id  BIGINT NOT NULL,
    day_no        INT NOT NULL CHECK (day_no BETWEEN 1 AND 31),
    meal_type     VARCHAR(20) NOT NULL
                 CHECK (meal_type IN ('breakfast', 'lunch', 'dinner')),
    item          VARCHAR(1000),
    CONSTRAINT uq_meal_plan_item UNIQUE (meal_plan_id, day_no, meal_type),
    CONSTRAINT fk_meal_plan_item_plan
        FOREIGN KEY (meal_plan_id)
        REFERENCES meal_plan(id)
        ON DELETE CASCADE
);

CREATE INDEX idx_meal_plan_item_plan ON meal_plan_item(meal_plan_id);
```

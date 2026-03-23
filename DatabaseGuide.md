# 💾 Database Naming Guide

본 문서는 데이터베이스(Database), 테이블(Table), 컬럼(Column) 및 각종 제약조건(Constraint)의 일관된 명명 규칙을 정의합니다. 
**모든 팀원은 DB 스키마 설계 및 Raw SQL 작성 시 본 가이드를 엄격히 준수해야 합니다.**

---

## 📢 1. 필독: 가이드 갱신 확인 및 즉시 적용

모든 데이터베이스 설계 및 DDL(Create, Alter 등) 작업을 시작하기 전, 최신 가이드를 확인하는 것은 필수입니다.

* **최신 가이드 확인 저장소:** [GitHub - CodingGuide](https://github.com/winiel/CodingGuide.git/)
* **준수 사항:** 1. 스키마 작업 전 `git pull`을 통해 최신 가이드를 확인합니다.
  2. **가이드에 변경 사항이 있을 경우, 즉시 마이그레이션(Migration) 스크립트를 작성하여 기존 스키마 구조에 반영해야 합니다.**

---

## 🏗️ 2. 데이터베이스 (Schema) 명명 규칙

데이터베이스 자체의 이름은 인프라 가이드와 동일하게 **프로젝트 네임 가이드**를 따릅니다.

* **규칙:** `[ProjectName]_[Environment]` (프로젝트명은 **카멜 법칙/PascalCase** 적용)
* **예시:** `ProjectRG_Prod`, `MakeVisionAi_Dev`

---

## 🏷️ 3. 테이블 및 컬럼 명명 규칙 (Table & Column)

데이터베이스 내부의 객체들은 호환성과 가독성을 위해 모두 **스네이크 케이스(snake_case)**를 사용합니다.



### 3.1 테이블 (Table)
* **규칙:** 모두 소문자를 사용하며, 단어 구분은 언더바(`_`)를 사용합니다. 데이터의 집합이므로 **복수형(Plural)** 명사를 권장합니다.
* **예시:** `users`, `orders`, `product_categories`
* **다대다(M:N) 매핑 테이블:** 두 테이블의 이름을 알파벳 순서대로 언더바로 연결합니다. 
  * 예시: `products`와 `orders`의 매핑 -> `order_products`

### 3.2 일반 컬럼 (Column)
* **규칙:** 모두 소문자 및 스네이크 케이스(`snake_case`)를 사용합니다. 가급적 축약어를 피하고 명확한 단어를 사용합니다.
* **예시:** `first_name`, `email_address`, `total_price`

### 3.3 식별자 및 키 (Primary & Foreign Keys)
* **기본키 (PK):** 단순히 `id`로 명명합니다. (예: `users` 테이블의 기본키는 `id`)
* **외래키 (FK):** 참조하는 대상 테이블의 **단수형_id** 형태로 명명합니다.
  * 예시: `orders` 테이블에서 `users` 테이블을 참조할 때 -> `user_id`

### 3.4 상태 및 논리형 (Boolean / Status)
* **Boolean (논리형):** `is_`, `has_`, `can_` 등의 접두사를 사용하여 참/거짓 여부를 명확히 합니다.
  * 예시: `is_active`, `has_discount`, `is_deleted`
* **Status (상태값):** 상태를 나타내는 컬럼은 `_status` 접미사를 붙입니다.
  * 예시: `order_status`, `payment_status`

### 3.5 날짜 및 시간 (Date & Time)
* **규칙:** 날짜/시간 데이터는 과거분사 형태나 `_at`, `_date` 접미사를 사용합니다.
* **예시:**
  * 날짜와 시간(DATETIME/TIMESTAMP): `created_at`, `updated_at`, `deleted_at`
  * 날짜만(DATE): `birth_date`, `expiration_date`

### 3.6 날짜 및 시간 (Date & Time) 자동화 규칙

모든 테이블에는 데이터의 생명주기(Lifecycle)를 추적하기 위해 아래 두 컬럼을 **반드시** 포함해야 하며, 어플리케이션 로직이 아닌 DB 엔진 차원에서 자동 갱신되도록 속성을 부여합니다.

* `created_at`: 레코드가 처음 생성된 시간
    * **속성:** `DATETIME DEFAULT CURRENT_TIMESTAMP`
* `updated_at`: 레코드가 마지막으로 수정된 시간
    * **속성:** `DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP`
---

## 🔑 4. 인덱스 및 제약조건 명명 규칙 (Index & Constraint)

DB 에러 발생 시 로그를 통해 어떤 제약조건에 위배되었는지 직관적으로 파악하기 위해 접두사(Prefix) 규칙을 사용합니다.

| 종류 | 접두사 규칙 | 예시 |
| :--- | :--- | :--- |
| **Primary Key** | `pk_[table_name]` | `pk_users` |
| **Foreign Key** | `fk_[table_name]_[column_name]` | `fk_orders_user_id` |
| **Unique Key** | `uq_[table_name]_[column_name]` | `uq_users_email` |
| **Index (일반)** | `idx_[table_name]_[column_name]`| `idx_users_created_at` |

---

## ⚠️ 5. 주의 사항
1. **예약어 사용 금지:** `user`, `order`, `group`, `select` 등 데이터베이스의 예약어(Reserved Words)를 단독 테이블명이나 컬럼명으로 사용하지 마세요. (예: `user` 대신 `users` 사용)
2. **일관성 유지:** 모든 테이블에는 레코드의 생성 시점(`created_at`)과 수정 시점(`updated_at`)을 추적하는 컬럼을 기본으로 포함하는 것을 강력히 권장합니다.

## 🛠️ 6. 테이블 생성 (DDL) 실전 예시

지금까지 정의한 가이드(복수형 테이블명, 스네이크 케이스, 제약조건 명명 규칙, 시간 자동화)를 모두 적용한 `users` 테이블 생성 쿼리 예시입니다. 
생 SQL(Raw SQL)로 마이그레이션 스크립트를 작성할 때 아래 형태를 표준으로 삼습니다.

```sql
-- 테이블명: 복수형 (users)
CREATE TABLE users (
    -- 기본키: id
    id INT AUTO_INCREMENT,
    
    -- 일반 컬럼: snake_case
    email_address VARCHAR(255) NOT NULL,
    hashed_password VARCHAR(255) NOT NULL,
    is_active BOOLEAN DEFAULT TRUE,
    
    -- 자동 갱신 시간 컬럼
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    -- 제약조건 명명 규칙 적용 (pk_, uq_ 등)
    CONSTRAINT pk_users PRIMARY KEY (id),
    CONSTRAINT uq_users_email UNIQUE (email_address)
);
# 📘 Project Architecture & Coding Guide
본 문서는 프로젝트의 일관성을 유지하고, Raw SQL 기반의 데이터 접근 계층을 효율적으로 관리하기 위한 표준 가이드입니다. 모든 팀원은 개발을 시작하기 전, 아래 내용을 반드시 숙지하고 준수해야 합니다.

# 📢 1. 필독: 코딩 가이드 갱신 여부 확인 및 즉시 적용
모든 개발 작업(Feature, Hotfix, Refactoring 등)을 시작하기 전, 최신 코딩 가이드를 확인하는 것은 필수 의무 사항입니다.

## 최신 가이드 확인: GitHub - CodingGuide
## 갱신 및 적용 절차:
- 개발 시작 전 git pull을 통해 최신 가이드를 확인합니다.
- 변경 사항이 있을 경우, 현재 작업 중인 프로젝트의 관련 코드나 구조에 즉시 적용해야 합니다.
- 가이드와 일치하지 않는 코드는 코드 리뷰 단계에서 반려 사유가 될 수 있습니다.

# 🏷️ 2. 네이밍 가이드 (Naming Conventions)
## 2.1 프로젝트 네임 (Project Name)
프로젝트 명칭은 카멜 법칙(Camel Case) 또는 **파스칼 케이스(Pascal Case)**를 사용하여 의미가 명확히 드러나도록 설정합니다.

- 규칙: 단어의 첫 글자를 대문자로 시작하여 결합합니다.
- 예시: ProjectRG, MakeVisionAi

## 2.2 코드 및 파일 네이밍
- Directory & Files: kebab-case (예: user-repository/, user.repo.js)
- Variables & Functions: camelCase (예: getUserById, isVerified)
- SQL Tables & Columns: snake_case (예: user_profile, created_at)

# 📂 3. 파일 구조 (Directory Structure)
모든 소스 코드는 src/ 폴더 내에서 역할별로 엄격히 분리하여 관리합니다.

~~~
Plaintext
my-project/
├── src/                   # 소스 코드 메인
│   ├── config/            # DB 연결 및 환경 설정 (database.js 등)
│   ├── core/              # 도메인 엔티티 및 핵심 비즈니스 모델
│   ├── repositories/      # 데이터 접근 계층 (DAO)
│   │   ├── sql/           # [중요] .sql 파일 저장소 (Raw SQL)
│   │   │   ├── user.sql
│   │   │   └── post.sql
│   │   ├── user.repo.js   # SQL 로드 및 실행 로직
│   │   └── post.repo.js
│   ├── services/          # 비즈니스 로직 및 트랜잭션 관리
│   ├── utils/             # 공통 헬퍼 (sql-loader 등)
│   ├── app.js             # Entry Point
│   └── main.js            # 서버 실행/초기화
├── tests/                 # 테스트 코드 (Unit, Integration)
├── migrations/            # DB 스키마 변경 이력 (DDL)
├── .env                   # 환경 변수 (비공개)
└── README.md              # 프로젝트 개요
~~~

# 💻 4. SQL 및 리포지토리 관리 규칙
## 4.1 SQL 파일 작성 (src/repositories/sql/)
한 파일 내에 여러 쿼리를 작성할 경우, 반드시 주석 태그를 활용합니다.
~~~
SQL
-- name: findUserById
SELECT * FROM users WHERE id = :id;

-- name: updateLastLogin
UPDATE users SET last_login = NOW() WHERE id = :id;
~~~
- 보안: SQL 인젝션 방지를 위해 반드시 Parameter Binding(:id 또는 ?)을 사용하며, 문자열 연결(+) 방식을 금지합니다.

## 4.2 리포지토리 역할
- sql-loader 유틸리티를 사용하여 SQL 파일을 동적으로 로드합니다.
- 리포지토리 내에는 순수한 DB 입출력 외에 어떠한 비즈니스 로직(조건문 등)도 포함하지 않습니다.

# ⚠️ 5. 주의 사항
* 동기화: 가이드의 변경 사항이 발견되면 미루지 말고 즉시 프로젝트 코드에 반영하여 기술 부채를 방지합니다.
* 환경 변수: .env 파일은 절대 Git에 Push하지 않습니다.
* 에러 처리: DB 에러는 리포지토리에서 가공하지 않고 서비스 계층으로 전파(throw)하여 통합 처리합니다.
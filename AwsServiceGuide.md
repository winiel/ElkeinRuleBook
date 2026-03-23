# ☁️ AWS Resource Naming Guide

본 문서는 클라우드 리소스의 식별성을 높이고, 효율적인 인프라 관리 및 비용 추적을 위해 정의되었습니다. 
**모든 팀원은 AWS 리소스 생성 및 테라폼(Terraform) 코드 작성 시 본 가이드를 엄격히 준수해야 합니다.**

---

## 📢 1. 필독: 가이드 갱신 확인 및 즉시 적용

모든 인프라 구축 및 변경 작업을 시작하기 전, 최신 가이드를 확인하는 것은 필수입니다.

* **최신 가이드 확인 저장소:** [GitHub - CodingGuide](https://github.com/winiel/CodingGuide.git/)
* **준수 사항:** 1. 인프라 작업 전 `git pull`을 통해 최신 가이드를 확인합니다.
  2. **가이드에 변경 사항이 있을 경우, 기존 리소스의 이름이나 태그에도 즉시 반영(또는 마이그레이션 계획 수립)해야 합니다.**

---

## 🏷️ 2. 리소스 명명 규칙 (Naming Convention)

### 2.1 프로젝트 네임 (Project Name)
프로젝트 명칭은 **카멜 법칙(Camel Case / Pascal Case)**을 사용하여 의미가 명확히 드러나도록 설정합니다.
* **규칙:** 단어의 첫 글자를 대문자로 시작하여 결합합니다.
* **예시:** `ProjectRG`, `MakeVisionAi`

### 2.2 표준 리소스 네이밍 구조
기본적으로 다음의 4단계 구조를 결합하여 명명합니다 (PascalCase 적용).

> **`[Project][Environment][Service][Role]`**



* **Project:** 프로젝트 명칭 (예: `ProjectRG`)
* **Environment:** 실행 환경 (`Prod`, `Dev`, `Staging`, `Test`)
* **Service:** AWS 서비스 단축명 (`Vpc`, `Ec2`, `Rds`, `S3`, `Sg`, `Iam`)
* **Role:** 해당 리소스의 구체적 역할 (`Web`, `Db`, `Api`, `Logs`)

---

## 📂 3. 서비스별 명명 예시

| 리소스 유형 | 명명 규칙 (CamelCase) | 실제 적용 예시 |
| :--- | :--- | :--- |
| **VPC** | `Project + Env + Vpc` | `ProjectRGProdVpc` |
| **Subnet** | `Project + Env + Zone + Role + Subnet` | `ProjectRGProdAWebSubnet` |
| **EC2 Instance** | `Project + Env + Role + Ec2` | `MakeVisionAiDevApiEc2` |
| **RDS** | `Project + Env + Role + Rds` | `ProjectRGProdDbRds` |
| **Security Group** | `Project + Env + Role + Sg` | `MakeVisionAiDevWebSg` |
| **IAM Role** | `Project + Role + IamRole` | `ProjectRGAdminIamRole` |

> ⚠️ **S3 버킷 예외 규칙**
> S3 버킷 이름은 전역적으로 고유해야 하며 DNS 명명 규칙을 따라야 하므로, 예외적으로 **소문자와 하이픈(`-`)**만을 사용합니다.
> * 형식: `project-env-role-s3` 
> * 예시: `projectrg-prod-assets-s3`

---

## 🏷️ 4. 필수 태깅(Tagging) 전략

이름(Name) 외에도 모든 리소스에는 리소스 그룹화 및 비용 청구(Billing) 추적을 위해 다음 태그(Tag)를 필수로 입력해야 합니다.

| Key | Value (Example) | 설명 |
| :--- | :--- | :--- |
| **Name** | `ProjectRGProdWebEc2` | 위 규칙에 따른 전체 이름 |
| **Project** | `ProjectRG` | 프로젝트 식별 코드 |
| **Environment** | `Prod` | 서비스 환경 (Prod, Dev 등) |
| **Owner** | `DevTeam / 홍길동` | 리소스 생성자 또는 담당 팀 |

---

## ⚠️ 5. 주의 사항
1. **일관성:** 한 번 결정된 프로젝트 명칭(`ProjectRG`)은 인프라 전반에서 동일하게 사용합니다.
2. **약어 사용:** 인프라 이름이 너무 길어지지 않도록 표준 약어(`Prod`, `Dev`, `Web`, `App`, `Db`)를 사용합니다.
3. **특수문자 지양:** 공백이나 언더바(`_`), 하이픈(`-`) 사용을 지양하고 대문자로 단어를 구분합니다. (단, S3 버킷 등 AWS 서비스 자체 제약이 있는 경우는 예외)
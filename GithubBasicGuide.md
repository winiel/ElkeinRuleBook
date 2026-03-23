# Github Basic User Guide 🚀

본 문서는 프로젝트의 코드와 문서를 Github을 통해 안전하게 관리하고 협업하기 위한 필수 명령어 가이드입니다. 

## 1. 초기 설정 및 저장소 연결 (Init & Remote)
프로젝트 폴더를 Git 저장소로 만들고, Github의 원격 저장소(Remote Repository)와 연결하는 단계입니다. 이 작업은 프로젝트 시작 시 **최초 1회**만 수행합니다.

~~~
# 1. 현재 폴더를 Git 저장소로 초기화합니다.
git init

# 2. Github에서 생성한 빈 저장소의 주소를 연결합니다.
# ([https://github.com/사용자이름/저장소이름.git](https://github.com/사용자이름/저장소이름.git) 부분을 실제 주소로 변경하세요)
git remote add origin [https://github.com/사용자이름/저장소이름.git](https://github.com/사용자이름/저장소이름.git)

# 3. 기본 브랜치 이름을 'main'으로 설정합니다.
git branch -M main
~~~

## 2. 변경사항 저장하기 (Add & Commit)
작성한 파일(예: DATABASE_NAMING_GUIDE.md)을 장바구니에 담고(Add), 변경 내역에 대한 메모와 함께 영수증을 발행(Commit)하는 단계입니다.

~~~
# 1. 변경된 특정 파일만 장바구니(Staging Area)에 담습니다.
git add DATABASE_NAMING_GUIDE.md

# (참고) 현재 폴더의 모든 변경사항을 한 번에 담으려면 아래 명령어를 사용합니다.
# git add .

# 2. 어떤 작업을 했는지 명확한 메시지와 함께 버전을 기록합니다.
git commit -m "docs: 데이터베이스 명명 규칙 및 엔티티 매트릭스 문서 추가"
~~~

> 💡 커밋 메시지 팁: docs: (문서), feat: (새로운 기능/코드), fix: (버그 수정) 등의 접두사를 사용하면 나중에 변경 이력을 알아보기 쉽습니다.

## 3. Github에 올리기 (Push)
내 컴퓨터(Local)에 기록된 버전을 Github 서버(Remote)로 업로드하여 안전하게 백업하고 팀원들과 공유합니다.

~~~
# 1. 커밋된 내역을 원격 저장소의 main 브랜치로 밀어 올립니다.
git push -u origin main
~~~

## 4. 변경사항 내려받기 (Pull)
다른 컴퓨터에서 작업했거나 팀원이 코드를 수정하여 Github에 올렸을 때, 그 최신 코드를 내 컴퓨터로 가져옵니다. 코드를 작성하기 전에 항상 먼저 실행하는 습관을 들이는 것이 좋습니다.
~~~
# 원격 저장소의 최신 변경사항을 내 컴퓨터로 병합합니다.
git pull origin main
~~~

## 5. 안전한 개발을 위한 브랜치(Branch) 사용
메인 코드에 영향을 주지 않고 새로운 기능(예: SQL 코드 작성)을 테스트하거나 개발할 때 사용합니다.
~~~
# 1. 'feature/sql-schema'라는 이름의 새로운 브랜치를 만들고 바로 이동합니다.
git checkout -b feature/sql-schema

# --- 이 상태에서 SQL 코드 파일을 만들고 add, commit을 진행합니다 ---

# 2. 작업이 완료되면 다시 main 브랜치로 돌아옵니다.
git checkout main

# 3. 변경된 브랜치의 내용을 main 브랜치로 합칩니다.
git merge feature/sql-schema
~~~

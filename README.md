# Custom ELT Project

이 저장소는 Docker와 PostgreSQL을 활용하여 간단한 ELT(Extract, Load, Transform) 프로세스를 시연하는 사용자 지정 ELT 프로젝트를 포함하고 있습니다.

## ETL

ELT는 "Extract, Load, Transform"의 약자로, 데이터 엔지니어링에서 사용됩니다. 이는 데이터 웨어하우스나 데이터베이스로 데이터를 추출(Extract), 적재(Load), 그리고 변환(Transform)하는 과정을 의미합니다. 데이터를 추출하여 필요한 형식으로 변환한 후에 데이터베이스나 데이터 웨어하우스로 적재하는 과정을 포함합니다.

## Repository Structure

1. docker-compose.yaml: 이 파일은 Docker Compose의 구성을 포함하며, 여러 Docker 컨테이너를 조정하는 데 사용됩니다. 다음 세 가지 서비스를 정의합니다:

   - source_postgres: 원본 PostgreSQL 데이터베이스입니다.

   - destination_postgres: 대상 PostgreSQL 데이터베이스입니다.

   - elt_script: ELT 스크립트를 실행하는 서비스입니다.

2. elt_script/elt_script.py: 이 Python 스크립트는 ELT 프로세스를 수행합니다. 원본 PostgreSQL 데이터베이스가 사용 가능해질 때까지 대기한 다음, 데이터를 SQL 파일로 덤프하고 이 데이터를 대상 PostgreSQL 데이터베이스로 로드합니다.

3. source_db_init/init.sql: 이 SQL 스크립트는 원본 데이터베이스를 샘플 데이터로 초기화합니다. 사용자, 영화, 영화 카테고리, 배우 및 영화 배우에 대한 테이블을 만들고 이러한 테이블에 샘플 데이터를 삽입합니다.

## How it works

1. Docker Compose: `docker-compose.yaml` 파일을 사용해, 세 개의 docker 컨테이너를 생성합니다.

   - 샘플 데이터가 있는 원본 PostgreSQL 데이터베이스입니다.
   - 대상 PostgreSQL 데이터베이스입니다.
   - ELT 스크립트를 실행하는 Python 환경입니다.

2. ELT Process: `elt_script.py`는 원본 PostgreSQL 데이터베이스가 사용 가능해질 때까지 대기합니다. 사용 가능하면 스크립트는 pg_dump를 사용하여 원본 데이터베이스를 SQL 파일로 덤프합니다. 그런 다음, 이 SQL 파일을 대상 PostgreSQL 데이터베이스로 로드하기 위해 psql을 사용합니다.

3. Database Initialize: `init.sql` 스크립트는 샘플 데이터로 원본 데이터베이스를 초기화합니다. 여러 테이블을 만들고 이러한 테이블에 샘플 데이터를 삽입합니다.

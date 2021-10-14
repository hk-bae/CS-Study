# SQL
## SQL 이란?
- 국제 표준 데이터베이스 언어이며, 관계형 데이터베이스(RDB) 를 지원하는 언어이다
- 관계 대수와 관계 해석을 기초로 한 혼합 데이터 언어이다.
## DDL (Data Define Language)
DB의 구조, 데이터 형식 등 DB를 구축하거나 수정할 목적으로 사용하는 언어이다.
#### CREATE
1. CREATE SCHEMA - 스키마를 정의하는 SQL
   ```sql 
   CREATE SCHEMA {schema name} AUTHORIZATION {user_id};
   ```
2. CREATE DOMAIN - 도메인을 정의하는 SQL
   ```sql
   CREATE DOMAIN {domain name} [AS] {data type}
                 [DEFAULT value]
                 [CONSTRAINT {제약조건} CHECK {범위}];
   ```
   > data type : integer, float, CHAR, VARCHAR, BIT, VARBIT, DATE, TIME
3. CREATE TABLE - 테이블을 정의하는 SQL
   ```sql
   CREATE TABLE {table name}
          # column 정의
          {column name} [DEFAULT {default value}] [NOT NULL],
          # ...
          # 제약 조건
          [UNIQUE({대체키 컬럼명},...] ,
          [FOREIGN KEY(외래키 컬럼명),...]
                      # 참조 제약조건
                      [REFERENCES {참조 테이블}(기본키 컬럼명)]
                      [ON DELETE {option}]
                      [ON UPDATE {option}]
                      [CONSTRAINT {제약조건명}],
          [CHECK (조건식)]
   ```
   4. CREATE VIEW - 뷰를 정의하는 SQL
      > View : 하나 이상의 기본 테이블로 부터 유도되는 가상 테이블.
   ```sql
   CREATE VIEW {view name} [(컬럼명,...)]
   AS SELECT 문
   ```
   5. CREATE INDEX - 인덱스를 정의하는 명령문
   ```sql
   CREATE [UNIQUE] INDEX 인덱스명
   ON 테이블명 (컬럼명 [ASC | DESC] [, 속성명 [ASC | DESC]])
   [CLUSTER];
   ```
#### ALTER
테이블에 대한 정의를 변경하는 명령문
1. ALTER TABLE
   1. 컬럼 추가 
   ```sql
   ALTER TABLE 테이블명 ADD {컬럼명} {데이터 타입} [DEFAULT {기본값}]
   ```
   2. 컬럼 제약 조건 변경
   ```sql
   ALTER TABLE 테이블명 ALTER {컬럼명} {데이터 타입} [데이터타입] [SET DEFAULT {기본값}]
   ```
   3. 컬럼 삭제
   ```sql
   ALTER TABLE 테이블명 DROP COLUMN 속성명 [CASCADE];
   ```
#### DROP
스키마, 도메인, 기본 테이블, 뷰 테이블, 인덱스 ,제약 조건 등을 제거하는 SQL
```sql
# 스키마 제거
DROP SCHEMA {스키마명} [CASCADE | RESTRICT];
# 도메인 제거
DROP DOMAIN {도메인명} [CASCADE | RESTRICT];
# 테이블 제거
DROP TABLE {도메인명} [CASCADE | RESTRICT];
# 뷰명 제거
DROP VIEW {도메인명} [CASCADE | RESTRICT];
# 인덱스 제거
DROP INDEX {도메인명} [CASCADE | RESTRICT];
# 제약조건 제거
DROP CONSTRAINT {도메인명} [CASCADE | RESTRICT];
```
## DCL (Data Control Language)
- DCL은 데이터의 보안, 무결성, 회복, 병행 제어 등을 정의하는데 사용하는 언어이다.
- DCL은 데이터베이스 관리자(DBA)가 데이터 관리를 목적으로 사용한다.
### GRANT / REVOKE
   데이터베이스 관리자가 데이터베이스 사용자에게 권한을 부여하거나 취소하기 위한 SQL
   > GRANT : 권한 부여를 위한 SQL
   ```sql
   GRANT {PREVILEGE LIST} TO {USER ID LIST} [IDENTIFIED BY {PASSWORD}];
   ```
   > REVOKE : 권한 취소를 위한 명령어
   ```sql
   REVOKE [GRAONT OPTION FOR] {PRIVIIELEGE LIST} ON {개체} FROM {USER} [CASCADE];
   ```
   - PRIVILEGE : ALL, SELECT, INSERT< DELETE, UPDATE, ALTER 등
   - WITH GRANT OPTION : 부여받은 권한을 다른 사용자에게 다시 부여할 수 있는 권한을 부여
   - GRANT OPTION FOR : 다른 사용자에게 권한을 부여할 수 있는 권한을 취소
   - CASCADE : 권한 취소 시 권한을 부여받았던 사용자가 다른 사용자에게 부여한 권한도 연쇄적으로 취소
### COMMIT
   트랜잭션이 성겅족으로 끝나면 데이터베이스가 새로운 일관성 상태를 가지기 위해 변경된 모든 내용을 데이터베이스에 반영하기 위한 명령어.
   - COMMIT 명령어를 실행하지 않아도 DML 문이 성공적으로 완료되면 자동으로 COMMIT이 된다.
   - AUTO COMMIT : DML이 실패하면 자동으로 ROLLBACK이 되도록 하는 기능
        ```sql 
        set autocommit = true;
        ```
### ROLLBACK
   아직 COMMIT 되지 않은 변경된 모든 내용들을 취소하고 데이터베이스를 이전 상태로 되돌리는 명령어이다.
### SAVEPOINT
   트랜잭션 내에 ROLLBACK 할 위치인 저장점을 지정하는 명령어이다.
## DML (Data Manipulation Language)
### INSERT INTO
테이블에 새로운 튜플을 삽입
```sql
INSERT INTO {테이블명} ([컬럼명, ...])
VALUES (데이터,...)
```
### DELETE FROM
기본 테이블에 있는 튜플 중에 특정 튜플을 삭제할 때 사용한다.
```sql
DELETE
FROM 테이블명
[WHERE 조건];
```
### UPDATE SET
테이블에 있는 튜플 중에서 특정 튜플의 내용을 변경할 때 사용한다.
```sql
update 테이블명
SET {컬럼명} = 데이터
[SET {컬럼명} = 데이터]
[WHERE 조건]
```

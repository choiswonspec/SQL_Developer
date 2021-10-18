https://www.notion.so/SQL-4c528ea916fd4f6e865feb2ec3e6f355

# DDL(DATA DEFINITION LANGUAGE)

### 데이터 유형

- 대표적인 데이터 유형 4가지
  1. CHARACTER(s) : 고정 길이 문자열.  주민번호 같은 데이터에 적합
  2. VARCHAR(s) : 가변 길이 문자열. 길이가 다양한 컬럼에 적합. 공백이 다르면 다른 문자로 판단
  3. NUMERIC : 정수, 실수 등 숫자 정보
  4. DATETIME : 날짜와 시작 정보

### CREATE TABLE

- 테이블과 컬럼 정의

  - 기본키 컬럼 지정.

    고유 값을 가져야함.

    여러 개의 컬럼으로도 만들어질 수 있다.

  - 테이블간의 관계는 기본키(PRIMARY KEY) 와 외부키(FOREIGN KEY)를 활용해 설정

  - 테이블 생성 구문

  ```sql
  CREATE TABLE 테이블이름 (
  		컬럼명1 DATATYPE [DEFAULT 형식],
  		컬럼명2 DATATYPE [DEFAULT 형식],
  		컬럼명3 DATATYPE [DEFAULT 형식],
  );
  ```

  - 테이블  및 컬럼 생성 시 주의할 규칙
    1. 다른 테이블과 중복되지 말아야함
    2. 한 테이블 내에서 컬럼명 중복 불가. 다른 테이블끼리는 가능
    3. 테이블 이름을 지정하고 각 컬럼들은 괄호 () 로 묶어 지정
    4. 각 컬럼들은 콤마 , 로 구분되고, 테이블 생성문 끝은 세미콜론 ; 로 끝난다
    5. 데이터 표준화 관점으로 컬럼들의 이름을 일관성 있게 사용해라
    6. 컬럼 뒤에 데이터 유형은 꼭 지정해라
    7. 테이블명과 컬럼명은 반드시 문자로 시작해야함
    8. 사전에 정의한 예약어는 쓸 수 없다
    9. 이름에 A-Z, a-z, 0-9, _, $, #  문자만 허용
    10. 대소문자는 구분하지 않는다. 기본적으로 대문자 사용
    11. DATETIME 유형에는 별도의 크기를 지정하지 않는다.
    12. 반드시 가질 수 있는 최대 크기를 지정해야 한다.
    13. 컬럼과 컬럼의 구분은 콤마 , 로 하되, 마지막 컬럼은 콤마 , 를 찍지 않는다.
    14. 컬럼에 대한 제약조건은 CONSTRAINT를 이용하여 추가한다.

  ```sql
  # Oracle
  CREATE TABLE PLAYER (
  PLAYER_ID CHAR(7) NOT NULL,
  PLAYER_NAME VARCHAR2(20) NOT NULL,
  TEAM_ID CHAR(3) NOT NULL,
  BACK_NO NUMBER(2),
  BIRTH_DATE DATE, 
  WEIGHT NUMBER(3),
  CONSTRAINT PLAYER_PK PRIMARY KEY (PLAYER_ID),
  CONSTRAINT PLAYER_FK FOREIGN KEY (TEAM_ID) REFERENCES TEAM(TEAM_ID)
  );
  
  # SQL Server
  CREATE TABLE PLAYER (
  PLAYER_ID CHAR(7) NOT NULL,
  PLAYER_NAME VARCHAR2(20) NOT NULL,
  TEAM_ID CHAR(3) NOT NULL,
  BACK_NO TINYINT,  # 이부분이 다름
  BIRTH_DATE DATE, 
  WEIGHT SMALLINT,# 이부분이 다름
  CONSTRAINT PLAYER_PK PRIMARY KEY (PLAYER_ID),
  CONSTRAINT PLAYER_FK FOREIGN KEY (TEAM_ID) REFERENCES TEAM(TEAM_ID)
  );
  ```

- 제약 조건

  - 제약조건의 종류

    1. PRIMARY KEY 기본키

       하나의 테이블에 하나의 기본키 제약만 정의 가능

       기본키 제약을 정의하면 DBMS는 자동으로 UNIQUE 인덱스를 생성,

       기본키에는 NULL 입력 불가

       기본키 제약 = 고유키 제약 & NOT NULL 제약

    2. UNIQUE KEY 고유키

       테이블에 저장된 행 데이터를 고유하게 식별하기 위한 고유키를 정의.

       NULL은 고유키 제약의 대상이 아니므로 NULL 값을 가진 행이 여러 개 있어도 고유키 제약 위반이 아니다.

    3. NOT NULL

       디폴트 상태는 모든 컬럼에서 NULL 허가이다.

       이 제약을 통해 입력 필수로 만들 수 있다.

    4. CHECK

       입력할 수 있는 값의 범위 등을 제한

       TRUE or FALSE 로 평가할 수 있는 논리식을 지정

    5. FOREIGN KEY 외래키

       외래키 지정시 참조 무결성 제약 옵션 선택 가능

  - NULL 의미

    - NULL(ASCII 코드 00번)은 공백(ASCII 코드 32번) 이나 숫자 0 (ASCII 48번) 과는 전혀 다른 값
    - 조건에 맞는 데이터가 없을 때의 공집합과도 다르다
    - '아직 정의되지 않은 미지의 값' 또는 '현재 데이터를 입력하지 못하는 경우'를 의미

  ```sql
  	테이블 명 : TEAM
  컬럼명 : 
  TEAM_ID : 팀 고유 ID, 문자 고정 자릿수 3자리
  TEAM_NAME : 팀 명, 문자 가변 자릿수 40자리
  STADIUM_ID : 구장 고유 ID, 문자 고정 자릿수 3자리
  제약조건 : 
  기본키 : TEAM_ID 제약 조건명은 TEAM_ID_PK
  NOT NULL : TEAM_NAME, STADIUM_ID 제약 조건명은 미적용
  
  Oracle
  
  CREATE TABLE TEAM(
  TEAM_ID CHAR(3) NOT NULL, 
  TEAM_NAME VARCHAR2(40) NOT NULL,
  STADIUM_ID CHAR(3) NOT NULL
  CONSTRAINT TEAM_PK PRIMARY KEY (TEAM_ID),
  CONSTRAINT TEAM_FK FOREIGN KEY (STADIUM_ID) REFERENCES STADIUM(STADIUM_ID)
  );
  
  SQL Server
  
  CREATE TABLE TEAM(
  TEAM_ID CHAR(3) NOT NULL, 
  TEAM_NAME VARCHAR2(40) NOT NULL,
  STADIUM_ID CHAR(3) NOT NULL
  CONSTRAINT TEAM_PK PRIMARY KEY (TEAM_ID),
  CONSTRAINT TEAM_FK FOREIGN KEY (STADIUM_ID) REFERENCES STADIUM(STADIUM_ID)
  );
  ```

  - 생성된 테이블 구조 확인

    테이블 생성 후 제대로 만들어졌는지 확인.

    Oracle 의 경우 DESCRIBE 테이블명 또는 DESC 테이블명

    SQL Server 의 경우 exec sp_help 'dbo.테이블명'

    ```sql
    Oracle
    
    DESCRIBE PLAYER 
    혹은
    DESC PLAYER
    
    SQL Server
    
    exec sp_help 'dbo.PLAYER' 
    ```

  - SELECT 문장을 통한 테이블 생성 CREATE TABLE ~ AS SELECT ~ CTAS 방법이라고 부름.

    주의할 점 : 기존 테이블의 제약조건 중 NOT NULL 만 새로운 복제 테이블에 적용이 되고 기본키, 고유키, 외래키, CHECK 등의 다른 제약 조건은 없어진다. 제약 조건을 추가하려면 ALTER TABLE 을 사용.

    SQL Server 에서는 SELECT ~ INTO ~ 를 활용하면 된다.

    ```sql
    Oracle
    
    CREATE TABLE TEAM_TEMP
    AS SELECT * FROM TEAM;
    
    ---
    
    SQL Server
    
    SELECT * INTO TEAM_TEMP FROM TEAM;
    ```

### ALTER TABLE

- 컬럼 추가/삭제, 제약조건 추가/삭제

- ADD COLUMN

  기존의 테이블에 필요한 컬럼 추가하기.

  ```sql
  ALTER TABLE 테이블명
  ADD 추가할 컬럼명 데이터 유형;
  ```

  새롭게 추가된 컬럼은 테이블의 마지막 컬럼이 되며 컬럼의 위치를 지정할 수 없다.

  예제)

  ```sql
  Oracle
  
  ALTER TABLE PLAYER
  ADD (ADDRESS VARCHAR2(80));
  
  SQL Server
  ALTER TABLE PLAYER
  ADD ADDRESS VARCHAR(80);
  ```

- DROP COLUMN

  테이블에서 컬럼 삭제. 데이터가 있거나 없거나 모두 삭제 가능.

  한 번에 하나의 컬럼만 삭제 가능하며, 삭제 후 최소 하나 이상의 컬럼이 테이블에 존재해야함.

  한 번 삭제된 컬럼은 복구 불가.

  ```sql
  ALTER TABLE 테이블명
  DROP COLUMN 삭제할 컬럼명;
  ```

  ```sql
  Oracle, SQL Server 둘 다
  
  ALTER TABLE PLAYER
  DROP COLUMN ADDRESS;
  ```

- MODIFY COLUMN

  데이터의 유형, DEFAULT 값, NOT NULL 제약 조건에 대한 변경

  ```sql
  Oracle , 
  
  ALTER TABLE 테이블명
  MODIFY (컬럼명1 데이터유형 [DEFAULT 식] [NOT NULL],
  			컬럼명2 데이터유형 ...);
  ----
  SQL Server 
  
  ALTER TABLE 테이블명
  ALTER (컬럼명1 데이터유형 [DEFAULT 식] [NOT NULL],
  			컬럼명2 데이터유형 ...);
  ```

  컬럼 변경 시 고려해야할 사항

  1. 컬럼의 크기는 늘릴 수 있지만 줄이지는 못한다. 기존 데이터가 훼손될 여지가 있기 때문
  2. 해당 컬럼이 NULL값만 가지고 있거나 테이블에 아무 행도 없어면 컬럼 폭 줄일 수 있다.
  3. NULL 값만 가지고 있으면 데이터 유형도 변경 가능
  4. 해당 컬럼의 DEFAULT 값을 바꾸면 변경 작업 이후 발생하는 행 삽입에만 영향을 미침
  5. 해당 컬럼에 NULL 값이 없을 경우에만 NOT NULL 제약조건 추가 가능

  예제)

  ```sql
  Oracle 
  
  ALTER TABLE TEAM_TEMP
  MODIFY (ORIG_YYYY VARCHAR2(8) DEFAULT '20020129' NOT NULL);
  
  ---
  
  SQL Server
  ALTER TABLE TEAM_TEMP
  ALTER COLUMN ORIG_YYYY VARCHAR2(8) NOT NULL);
  
  ALTER TABLE TEAM_TEMP
  ALTER CONSTRAINT DF_ORIG_YYYY DEFAULT '20020129' FOR ORIG_YYYY;
  ```

- RENAME COLUMN

  컬럼명 변경

  ```sql
  ALTER TABLE 테이블명
  RENAME COLUMN 변경해야 할 컬럼명 TO 새로운 컬럼명;
  ```

  RENAME COLUMN으로 컬럼명 변경하면 해당 컬럼과 관계된 제약조건에 대해서도 자동으로 변경되는 장점이 있지만, ADD/DROP COLUMN 처럼 ANSI/ISO 에 명시되어 있는 기능이 아니고 Oracle 등 일부 DBMS에서만 지원하는 기능.

  ```sql
  ALTER TABLE PLAYER
  RENAME COLUMN PLAYER_ID TO TEMP_ID;
  ```

  SQL Server 에서는 sp_rename 저장 프로시저를 이용하여 컬럼 이름 변경 가능

  ```sql
  sp_rename 변경해야할컬럼명, 새로운컬럼명, 'COLUMN';
  
  ex)
  sp_rename 'dbo.TEAM_TEMP.TEAM_ID', 'TEAM_TEMP_ID', 'COLUMN';
  
  주의: 엔티티 이름 부분을 변경하면 스크립트 및 저장 프로시저를 손상시킬 수 있다.
  ```

- DROP CONSTRAINT

  제약 조건 삭제

  ALTER TABLE 테이블명 DROP CONSTRAINT 제약조건명;

  ```sql
  Oracle, SQL Server 둘 다
  
  ALTER TABLE PLAYER
  DROP CONSTRAINT PLAYER_FK;
  ```

- ADD CONSTRAINT

  제약 조건 추가

  ALTER TABLE 테이블명

  ADD CONSTRAINT 제약조건명 제약조건 (컬럼명);

  ```sql
  Oracle
  
  ALTER TABLE PLAYER
  ADD CONSTRAINT PLAYER_FK FOREIGN KEY (TEAM_ID) REFERENCES TEAM(TEAM_ID);
  
  ---
  
  SQL Server
  
  ALTER TABLE PLAYER
  ADD CONSTRAINT PLAYER_FK 
  FOREIGN KEY (TEAM_ID) REFERENCES TEAM(TEAM_ID);
  ```

  여기서 Table TEAM을 삭제하면 삭제되지 않는다. FOREGIN KEY 제약 조건을 참조하므로 삭제되지 않음. 참조 무결성 옵션..

- RENAME TABLE

  테이블 이름 변경

  ```sql
  Oracle
  RENAME 변경전테이블명 TO 변경후테이블명;
  
  SQL Server
  sp_rename 변경전테이블명, 변경후테이블명;
  ```

  예제)

  ```sql
  Oracle
  
  RENAME TEAM TO TEAM_BACKUP;
  
  ---
  
  SQL Server
  
  sp_rename 'dbo.TEAM', 'TEAM_BACKUP';
  ```

### DROP TABLE

- 테이블을 완전 삭제

  ```sql
  DROP TABLE 테이블명 [CASCADE CONSTRAINT];
  ```

  DROP 명령어는 테이블의 모든 데이터 및 구조를 삭제

  CASCADE CONSTRAINT 옵션 : 해당 테이블과 관계가 있었던 참조되는 제약조건에 대해서도 삭제한다.

  SQL Server 에서는 CASCADE 옵션이 존재하지 않으며 테이블이 삭제하기 전 참조하는 FOREIGN KEY 제약 조건 또는 참조하는 테이블을 먼저 삭제해야 한다

  ```sql
  예제)
  
  Oracle, SQL Server 둘 다
  
  DROP TABLE PLAYER;
  ```

### TRUNCATE TABLE

- 테이블 자체의 삭제가 아니라 해당 테이블을 구성하는 모든 행들이 제거되고 저장 공간을 재사용 가능하게함.

  삭제 후 복구는 불가능하다

  ```sql
  Oracle, SQL Server 둘 다
  
  TRUNCATE TABLE 테이블명
  ```

# DML (DATA MANIPULATION LANGUAGE)

- 데이터들을 입력, 수정, 삭제, 조회
- 데이터베이스는 DDL 명령어인 경우 직접 데이터베이스의 테이블에 영향을 미치기 때문에 DDL 명령어를 입력하는 순간 명령어에 해당하는 작업이 즉시 AUTO COMMIT 된다.
- 하지만 DML 명령어의 경우, 조작하려는 테이블을 메모리 버퍼에 올려놓고 작업을 하기 때문에 COMMIT 명령어를 입력하여 TRANSACTION을 종료해야한다.
- 그러나 SQL Server의 겨우 DML도 AUTO COMMIT으로 처리 된다.
- 테이블의 전체 데이터를 삭제하는 경우 시스템 활용 측면에서 DELETE TABLE 보다는 시스템 부하가 적은 TRUNCATE TABLE을 권고한다. 단, TRUNCATE 의 경우 데이터의 로그가 없으므로 ROLLBACK이 불가능하다. ㄱ

### INSERT 데이터 입력

- 데이터 입력 방법 두 가지 유형

  ```sql
  # 첫 번째. 데이터를 추가할 컬럼을 명시한 후 데이터 추가
  INSERT INTO 테이블명 (COLUMN_LIST)
  VALUES (COLUMN_LIST에 넣을 VALUE_LIST);
  # 이 방법의 경우 컬럼의 순서는 테이블의 컬럼 순서와 일치하지 않아도 된다
  # 정의하지 않은 컬럼은 Default로 NULL이 입력된다
  # 단, Primary KEY 나 Not NULL로 지정된 컬럼은 NULL이 허용되지 않는다.
  
  # 두 번째. 데이터를 모든 컬럼에 추가.
  INSERT INTO 테이블명
  VALUES (전체 COLUMN에 넣을 VALUE_LIST);
  # 이 경우 컬럼의 순서대로 빠짐없이 데이터가 입력되어야 한다.
  ```

  - 컬럼명과 입력되어야 하는 값을 서로 1:1로 매핑해서 입력하면 된다.
  - CHAR 이나 VARCHAR2 등 문자 유형은 ' 로 입력할 값을 입력
  - 숫자일 경우 ' 를 붙이지 않는다

  예제)

  ```sql
  INSERT INTO PLAYER (PLAYER_ID, PLAYER_NAME, TEAM_ID, POSITION, BACK_NO)
  VALUES ('2002007', '박지성', 'K07', 'MF',7) # 숫자에는 ' 가 없다.
  # 여기에 없는 컬럼들은 모두 NULL 값이 입력됨.
  ```

  ```sql
  INSERT INTO PLAYER 
  VALUES ('2002010', '이청용', 'K07','BlueDragon','2002', 'MF','17', NULL, '1', 180, 69) 
  # 순서가 꼭 맞아야 한다
  ```

### UPDATE

- 데이터 수정

  ```sql
  UPDATE 테이블명
  SET 수정되어야 할 컬럼명 = 수정되기를 원하는 새로운 값;
  ```

  예제)

  ```sql
  # 선수 테이블의 백넘버를 모두 99로 수정
  UPDATE PLAYER
  SET BACK_NO = 99;
  ```

### DELETE

- 데이터 삭제

- FROM 절은 생략 가능.

- WHERE 절을 사용하지 않으면 테이블의 전체 데이터가 삭제된다.

- 기본코드

  ```sql
  DELETE [FROM] 삭제를 원하는 정보가 들어있는 테이블명;
  ```

  예제) 선수 테이블의 데이터를 전부 삭제

  ```sql
  DELETE FROM PLAYER;
  ```

### SELECT

- 데이터 조회

  ```sql
  SELECT [ALL/DISTINCT] 보고 싶은 컬럼명1, 보고 싶은 컬럼명2, ...
  FROM 해당 컬럼들이 있는 테이블명;
  
  - ALL: Default 옵션이므로 별도로 표시하지 않는다. 중복된 데이터가 있어도 모두 출력한다는 옵션
  - DISTINCT : 중복된 데이터가 있는 경우 1건으로 처리해서 출력한다.
  ```

  예제)

  ```sql
  SELECT PLAYER_ID, PLAYER_NAME, TEAM_ID, POSITION, BACK_NO
  FROM PLAYER;
  ```

  ```sql
  # DISTINCT 옵션
  SELECT DISTINCT POSITION
  FROM PLAYER;
  # 이 코드는 Python 의 Unique 와 같다.
  # 중복제거를 통해 POSITION에 어떤 값들이 있는지 확인. NULL값도 포함된다.
  ```

- WILDCARD * 사용하기

  모든 컬럼 조회

  ```sql
  SELECT *
  FROM PLAYER;
  ```

- ALIAS 부여하기

  조회된 결과에 일종의 별명(ALIAS, ALIASES)을 부여해서 컬럼 레이블 변경 가능

  컬럼 별명(ALIAS) 에 대한 사항 정리

  1. 컬럼명 바로 뒤에 온다

  2. 컬럼명과 ALIAS 사이에 AS, as 키워드를 사용할 수도 있다. 꼭 사용하지 않아도 됨.

  3. 이중 인용부호는 ALIAS가 공백, 특수문자를 포함할 경우와 대소문자 구분이 필요한 경우 사용

  4. 컬럼 별명을 사용할 때 별명 사이에 공백이 있는 경우 " " 를 사용해야 한다.

     SQL Server의 경우 " ", ' ', [ ]  3가지의 방식으로 가능

  예제) 입력한 선수들의 정보를 컬럼 별명을 이용하여 출력한다.

  ```sql
  SELECT PLAYER_NAME AS 선수명, POSITION AS 위치, HEIGHT AS 키, WEIGHT AS 몸무게
  FROM PLAYERR;
  
  # AS를 생략해도 됨.
  ```

  ```sql
  SELECT PLAYER_NAME "선수 이름", POSITION "그라운드 포지션", HEIGHT "키", WEIGHT "몸무게"
  FROM PLAYERR;
  ```

### 산술 연산자와 합성 연산자

- 산술 연산자

  1. NUMBER 와 DATE 자료형에 대해 적용 가능
  2. 우선순위를 위한 괄호 적용 가능
  3. 산술 연산자 우선 순위는 () , * , / , + , -  순서이다.
  4. 기존의 컬럼에 대해 새로운 의미를 부여한 것이므로 적절한 ALIAS를 새롭게 부여하는 것이 좋다

  예제)

  ```sql
  SELECT PLAYER_NAME 이름, HEIGHT - WEIGHT "키-몸무게"
  FROM PLAYER;
  ```

  ```sql
  # BMI 비만지수 측정
  SELECT PLAYER_NAME 이름, ROUNT(WEIGHT/((HEIGHT/100)*(HEIGHT/100)),2) "BMI 비만지수"
  FROM PLAYER;
  
  # ROUND() 함수는 반올림을 위한 내장 함수
  ```

- 합성 연산자 CONCATENATION

  문자와 문자를 연결하는 합성 연산자를 사용

  1. 문자와 문자를 연결하는 경우 2개의 수직 바 || 에 의해 이루어진다. (Oracle)
  2. 문자와 문자를 연결하는 경우 + 표시에 의해 이루어진다 (SQL Server)
  3. 두 벤더 모두 공통적으로 CONCAT (string1, string2) 함수를 사용할 수 있다.
  4. 컬럼과 문자 또는 다른 컬럼과 연결시킨다.
  5. 문자 표현식의 결과에 의해 새로운 컬럼을 생성한다.

  예제)

  ```sql
  다음과 같은 선수들의 출력 형태를 만들어 본다
  출력 형태) 선수명 선수, 키 cm, 몸무게 kg
  예) 박지성 선수, 176 cm, 70 kg
  
  Oracle
  
  SELECT PLAYER_NAME || '선수', || HEIGHT || 'cm', || WEIGHT || 'kg' 체격정보
  FROM PLAYER;
  
  SQL Server
  
  SELECT PLAYER_NAME + '선수, ' + HEIGHT + 'cm, ' + WEIGHT + 'kg'체격정보
  FROM PLAYER;
  
  실행 결과
  체격정보
  정경량선수,173cm,65kg ... 다음과 같이 뒤에 선수, cm, kg 가 붙어서 표현된다.
  ```

# TCL ( TRANSACTION CONTROL LANGUAGE )

- 트랜잭션 개요
  - 트랜잭션은 하나의 논리적 작업 단위를 구성하는 세부적인 연산들의 집합이며 데이터베이스의 논리적 연산단위이다. 데이터베이스의 응용 프로그램은 트랜잭션의 집합이다.
  - 트랜잭션이란 밀접히 관련되어 분리될 수 없는 한 개 이상의 데이터베이스 조작을 가리킨다.
  - 하나의 트랜잭션에는 하나 이상의 SQL 문장이 포함된다.
  - 트랜잭션은 분할할 수 없는 최소의 단위이다. 그렇기 때문에 전부 적용하거나 전부 취소한다.
  - 즉, TRANSACTION은 ALL OR NOTHING의 개념인 것.
- TCL 3가지 명령어
  - 올바르게 반영된 데이터를 데이터베이스에 반영시키는 것을 COMMIT 이라고 한다
  - 트랜잭션 이전의 상태로 되돌리는 것을 ROLLBACK 이라고 한다.
  - 저장되는 지점을 SAVEPOINT 라고 한다.
- 트랜잭션의 대상
  - 트랜잭션의 대상이 되는 SQL문은 UPDATE, INSERT, DELETE 등 데이터를 수정하는 DML
  - SELECT 문장은 직접적인 트랜잭션의 대상이 아니지만, SELECT FOR UPDATE 등 배타적 LOCK을 요구하는 SELECT 문장은 트랜잭션의 대상이 될 수 있다.
- 트랜잭션의 특성
  - 원장성(atomicity) : 트랜잭션에서 정의된 연산들은 모두 성공적으로 실행되던지 아니면 전혀 실행되지 않은 상태로 남아 있어야 한다. (all or nothing)
  - 일관성(consistency) : 트랜잭션이 실행되기 전의 데이터베이스 내용이 잘못 되어 있지 않다면 트랜잭션이 실행된 이후에도 데이터베이스의 내용에 잘못이 있으면 안된다.
  - 고립성(isolation) : 트랜잭션이 실행되는 도중에 다른 트랜잭션의 영향을 받아 잘못된 결과를 만들어서는 안된다.
  - 지속성(durability) : 트랜잭션이 성공적으로 수행되면 그 트랜잭션이 갱신한 데이터베이스의 내용은 영구적으로 저장된다.

### COMMIT

- 입력 또는 수정한 자료에 대해 문제가 없다고 판한 후 COMMIT 명령어를 통해 트랜잭션 완료 가능
- COMMIT 이나 ROLLBACK 이전의 데이터 상태는 다음과 같다
  - 단지 메모리 BUFFER 에만 영향을 받았기 때문에 데이터의 변경 이전 상태로 복구 가능
  - 현재 사용자는 SELECT 문장으로 결과를 확인 가능하다
  - 다른 사용자는 현재 사용자가 수행한 명령의 결과를 볼 수 없다.
  - 변경된 행은 잠금 (LOCKING)이 설정되어서 다른 사용작다 변경할 수 없다.

예제) Oracle 의 COMMIT

```sql
Oracle

INSERT INTO PLAYER (PLAYER_ID, TEAM_ID, PLAYER_NAME, POSITION, HEIGHT, WEIGHT, BACK_NO)
VALUES ('1997035', 'K02', '이운재', 'GK', 182, 82, 1);
1개 행이 만들어진다.

COMMIT;
커밋이 완료됨.
```

COMMIT 명령어는 이전의 INSERT 문장, UPDATE 문장, DELETE 문장을 사용한 후에 이런 변경 작업이 완료되었음을 데이터베이스에 알려주기 위해 사용

- COMMIT 이후의 데이터 상태는 다음과 같다.

  - 데이터에 대한 변경 사항이 데이터베이스에 반영된다.
  - 이전 데이터는 영원히 잃어버리게 된다.
  - 모든 사용자는 결과를 볼 수 있다.
  - 관련된 행에 대한 잠금(LOCKING)이 풀리고, 다른 사용자들이 행을 조작할 수 있게 된다.

- SQL Server 의 COMMIT

  - Oracle은 DML을 실행하는 경우 DBMS가 트랜잭션을 내부적으로 실행하며 DML 문장 수행 후 사용자가 임의로 COMMIT 혹은 ROLLBACK을 수행해 주어야 트랜잭션이 종료된다. (일부 툴에서는 AUTO COMMIT을 옵션으로 선택 가능)
  - SQL Server는 기본적으로 AUTO COMMIT 모드이다. DML 수행 후 사용자가 따로 COMMIT이나 ROLLBACK을 처리할 필요가 없다. DML 구문이 성공이면 자동으로 COMMIT이 되고 오류가 발생할 경우 자동으로 ROLLBACK 처리가 된다.

  ```sql
  SQL Server
  
  INSERT INTO PLAYER (PLAYER_ID, TEAM_ID, PLAYER_NAME, POSITION, HEIGHT, WEIGHT, BACK_NO)
  VALUES ('1997035', 'K02', '이운재', 'GK', 182, 82, 1);
  ```

- SQL Server 트랜잭션은 기본적으로 3가지 방식으로 이루어진다.

  1. AUTO COMMIT

     SQL Server의 기본 방식, DML, DDL 을 수행할 때마다 DMBS가 트랜잭션을 컨트롤 하는 방식.

     자동 COMMIT, 자동 ROLLBACK

  2. 암시적 트랜잭션

     Oracle과 같은 방식으로 처리된다. 트랜잭션의 끝은 사용자가 명시적으로 COMMIT, ROLLBACK 으로 처리한다. 인스턴스 단위 또는 세션 단위로 설정할 수 있다. 인스턴스 단위로 설정하려면  서버 속성 창의 연결화면에서 기본연결 옵션 중 암시적 트랜잭션에 체크를 해주면 된다. 세션 단위로 설정하기 위해서는 세션 옵션 중 SET IMPLICIT TRANSACTION ON 을 사용하면 된다.

  3. 명시적 트랜잭션

     트랜잭션의 시작과 끝은 모두 사용자가 명시적으로 지정하는 방식. BEGIN TRANSACTION (BEGIN TRAN 구문도 가능)으로 트랜잭션을 시작하고 COMMIT TRANSACTION(TRANSACTION은 생략가능) 또는 ROLLBACK TRANSACTION(TRANSACTION은 생략 가능)으로 트랜잭션을 종료한다.

     ROLLBACK 구문을 만나면 최초의 BEGIN TRANSACTION 시점까지 모두 ROLLBACK이 수행된다.

### ROLLBACK

- 입력 ,수정, 삭제 한 데이터에 대해 COMMIT 이전에는 변경 사항을 취소할 수 있는데 데이터베이스에서는 롤백(ROLLBACK) 기능을 사용한다.

- 롤백(ROLLBACK)은 데이터 변경 사항이 취소되어 데이터의 이전 상태로 복구되며, 관련된 행에 대한 잠금(LOCKING)이 풀리고 다른 사용자들이 데이터 벼경을 할 수 있게 된다.

- 예제

  ```sql
  Oracle 
  
  INSERT INTO PLAYER (PLAYER_ID, TEAM_ID, PLAYER_NAME, POSITION, HEIGHT, WEIGHT, BACK_NO)
  VALUES ('1997035', 'K02', '이운재', 'GK', 182, 82, 1);
  
  ROLLBACK;
  ```

- SQL Server 의 ROLLBACK

  ```sql
  SQL Server
  
  BEGIN TRAN
  INSERT INTO PLAYER (PLAYER_ID, TEAM_ID, PLAYER_NAME, POSITION, HEIGHT, WEIGHT, BACK_NO)
  VALUES ('1997035', 'K02', '이운재', 'GK', 182, 82, 1);
  
  ROLLBACK;
  ```

  데이터에 대한 변경 사항은 취소된다

  이전 데이터는 다시 재저장된다

  관련된 행에 대한 잠금(LOCKING)이 풀리고, 다른 사용자들이 행을 조작할 수 있게 된다.

- COMMIT, ROLLBACK을 사용의 효과

  1. 데이터 무결성 보장
  2. 영구적인 변경을 하기 전에 데이터의 변경 사항 확인 가능
  3. 논리적으로 연관된 작업을 그룹핑하여 처리 가능

### SAVEPOINT

- 저장점 지정하여 ROLLBACK 할 때 저장점까지만 ROLLBACK 가능.

- 복수의 저장점 정의 가능

- 동일이름으로 저장점 정의 시, 나중에 정의한 저장점이 유효함

  ```sql
  SAVEPOINT SVPT1;
  
  ROLLBACK TO SVPT1; # 해당 저장점으로 롤백
  
  # SQL Server 는 SAVE TRANSACTION을 사용하여 동일한 기능 수행.
  SAVE TRANSACTION SVTR1;
  ROLLBACK TRANSACTION SVTR1;
  ```

- 예제

  ```sql
  Oracle
  
  SAVEPOINT SVPT1;
  
  ---
  
  INSERT INTO PLAYER (PLAYER_ID, TEAM_ID, PLAYER_NAME, POSITION, HEIGHT, WEIGHT, BACK_NO)
  VALUES ('1997035', 'K02', '이운재', 'GK', 182, 82, 1);
  
  ---
  
  ROLLBACK TO SVPT1;
  ```

  ```sql
  SQL Server
  
  SAVE TRAN SVTR2;
  
  ---
  
  INSERT INTO PLAYER (PLAYER_ID, TEAM_ID, PLAYER_NAME, POSITION, HEIGHT, WEIGHT, BACK_NO)
  VALUES ('1997035', 'K02', '이운재', 'GK', 182, 82, 1);
  
  ---
  
  ROLLBACK TRAN SVPT12;
  ```

- 과거로 ROLLBACK 후에 그거보다 더 미래인 지점으로 ROLLBACK은 불가능하다.

- 저장점 없이 ROLLBACK 을 싱행할 경우 반영안된 모든 변경 사항을 취소하고 트랜잭션 시작 위치로 간다

# WHERE 절

### WHERE 조건절 개요

- 두 개 이상의 테이블에 대한 조인 조건을 기술
- 결과를 제한하기 위한 조건을 기술
- WHERE절의 JOIN 과 FROM 절의 JOIN이 있다.
- WHERE 절은 FROM절 뒤에 오게 된다.

```sql
SELECT [DISTINCT/ALL] 컬럼명 [ALIAS명] # [] 안에있는것들은 선택 옵션
FROM 테이블명
WHERE 조건식;
```

- WHERE 절 조건식은 아래 내용으로 구성된다.
  1. 컬럼명 ( 보통 조건식의 좌측에 위치 )
  2. 비교 연산자
  3. 문자, 숫자, 표현식 ( 보통 조건식의 우측에 위치 )
  4. 비교 컬럼명 (JOIN 사용시)

### 연산자의 종류

1. 비교 연산자 (부정 비교 연산자 포함)

   - =, > , >=
   - 예시

   ```sql
   SELECT PLAYER_NAME, POSITION, BACK_NO, HEIGHT
   FROM PLAYER
   WHERE TEAM_ID = 'K02';
   ```

   - 문자 유형간의 비교 조건이 발생하는 경우
     1. 비교 연산자의 양쪽이 모두 CHAR 유형 타입인 경우 길이가 서로 다른 CHAR 형 타입이면 작은 쪽에 SPACE를 추가하여 길이를 같게 한 후  비교 서로 다른 문자가 나올 때까지 비교한다. 달라진 첫 번째 문자의 값에 따라 크기를 결정한다 BLACK의 수만 다르다면 서로 같은 값으로 결정한다.
     2. 비교 연산자의 어느 한 쪽이 VARCHAR 유형 타입인 경우 서로 다른 문자가 나올 때까지 비교한다 길이가 다르다면 짧은 것이 끝날 때까지만 비교한 후에 길이가 긴 것이 크다고 판단한다 길이가 같고 다른 것이 없다면 같다고 판단한다 VARCHAR은 NOT NULL 까지 길이를 말한다
     3. 상수값과 비교할 경우 상수 쪽을 변수 타입과 동일하게 바꾸고 비교한다 변수 쪽이 CHAR 유형 타입이면 위의 CAHAR 유형 타입의 경우를 적용한다 변수 쪽이 VARCHAR 유형 타입이면 위의 VARCHAR 유형 타입의 경우를 적용한다.

2. SQL 연산자 (부정 SQL 연산자 포함)

   - BEETWEEN a AND b

     a와 b 값 사이에 있는가. a와 b 도 포함

   - IN (list)

     리스트에 있는 값 중에서 어느 하나라도 일치하는가

   - LIKE '비교문자열'

     비교문자열과 형태가 일치하는가. %, _ 사용 가능

     % : 0개 이상의 어떤 문자를 의미

     _ : 1개인 단일 문자를 의미

   - IS NULL

     NULL 값과의 수치연산은 NULL값을 리턴한다

     NULL 값과의 비교연산은 FALSE를 리턴한다

     어떤 값과도 비교할 수도 없으며, 특정 값보다 크다, 적다로 표현 불가

   - 예제

   ```sql
   # IN 연산자
   # JOB이 MANAGER 이면서 20번 부서에 속하거나, JOB이 CLERK 이면서 30번 부서에 속하는 조건부
   SELECT ENAME, JOB, DEPTNO
   FROM EMP
   WHERE (JOB, DEPTNO) IN (('MANAGER',20),('CLERK',30));
   
   # LIKE 연산자
   # 장씨 성을 가진 선수들 출력
   SELECT PLAYER_NAME, POSITION, BACK_NO, HEIGHT
   FROM PLAYER
   WHERE PLAYER_NAME LIKE '장%' 
   
   # BETWEEN a AND b 연산자
   # 키가 170 이상 180 이하인 선수 출력
   SELECT PLAYER_NAME, POSITION, BACK_NO, HEIGHT
   FROM PLAYER
   WHERE HEIGHT BETWEEN 170 AND 180;
   
   # IS NULL 연산자
   SELECT PLAYER_NAME, POSITION, BACK_NO, HEIGHT
   FROM PLAYER
   WHERE POSITION IS NULL: #POSITION = NULL; 이렇게 표현하면 안됨.
   ```

3. 논리 연산자

   - AND

     두 조건이 TRUE 이면 결과도 TRUE

   - OR

     두 조건중 하나만 TRUE 이면 결과도 TRUE

   - NOT

     뒤에 오는 조건에 반대되는 결과를 되돌려 줌.

   - 예제

   ```sql
   SELECT PLAYER_NAME, POSITION, TEAM_ID
   FROM PLAYER
   WHERE TEAM_ID = 'K02'
   AND HEIGHT >= 170;
   
   SELECT PLAYER_NAME, POSITION, BACK_NO, HEIGHT
   FROM PLAYER
   WHERE TEAM_ID IN ('K02', 'K07') AND POSITION = 'MF';
   ```

   연산자 우선순위를 잘 파악하고 괄호를 적절하게 사용해서 원하는 결과가 나오게 해야한다

   ```sql
   SELECT PLAYER_NAME, POSITION, BACK_NO, HEIGHT
   FROM PLAYER
   WHERE (TEAM_ID = 'K02' OR TEAM_ID = 'K07')
   AND POSITION = 'MF'
   AND HEIGHT >= 170
   AND HEIGHT <= 180;
   ```

4. 부정 비교 연산자

   - !=

     같지 않다

   - ^=

     같지 않다

   - <>

     같지 않다

   - NOT 컬럼명 =

     ~와 같지 않다

   - NOT 컬럼명 >

     ~보다 크지 않다

5. 부정 SQL 연산자

   - NOT BETWEEN a AND b

     a와 b의 값 사이에 있지 않다

   - NOT IN (list)

     list 값과 일치하지 않는다

   - IS NOT NULL

     NULL 값을 갖지 않는다.

   - 부정 비교 연산자 & 부정 SQL 연산자 예제

   ```sql
   SELECT PLAYER_NAME, POSITION, BACK_NO, HEIGHT
   FROM PLAYER
   WHERE TEAM_ID = 'K02'
   AND NOT POSITION = 'MF'
   AND NOT HEIGHT BETWEEN 175 AND 185;
   
   위의 코드와 같은 Oracle 코드
   SELECT PLAYER_NAME, POSITION, BACK_NO, HEIGHT
   FROM PLAYER
   WHERE TEAM_ID = 'K02'
   AND POSITION <> 'MF'
   AND HEIGHT NOT BETWEEN 175 AND 185;
   
   # 국적 컬럼이 NULL이 아닌 선수와 국적을 표시하라
   SELECT PLAYER_NAME, NATION
   FROM PLAYER
   WHERE NATION IS NOT NULL;
   ```

6. ROWNUM, TOP 사용

   - ROWNUM

     Oracle의 ROWNUM은 컬럼과 비슷한 성격의 Pseudo Column 으로써 SQL 처리 결과 집합의 각 행에 대해 임시로 부여되는 일련번호이다.

     테이블이나 집합에서 원하는 만큼의 행만 가져오고 싶을 때 WHERE 절에서 행의 개수를 제한하는 목적으로 사용한다.

     - 한 건의 행만 가져오고 싶을 때
       - SELECT PLAYER_NAME FROM PLAYER WHERE ROWNUM = 1;
       - SELECT PLAYER_NAME FROM PLAYER WHERE ROWNUM <= 1;
       - SELECT PLAYER_NAME FROM PLAYER WHERE ROWNUM < 2;
     - 두 건 이상의 N 행을 가져오고 싶을 때는 ROWNUM = N; 을 사용할 수 없다
       - SELECT PLAYER_NAME FROM PLAYER WHERE ROWNUM <= N;
       - SELECT PLAYER_NAME FROM PLAYER WHERE ROWNUM < N+1;
     - 추가 ROWNUM의 용도로는 테이블 내의 고유한 키나 인덱스 값을 만들 수 있다
       - UPDATE MY_TABLE SET COLUMN1 = ROWNUM;

   - TOP 절

     SQL Server는 TOP절을 사용하여 결과 집합으로 출력되는 행의 수를 제한할 수 있다.

     TOP 절의 표현식은 다음과 같다.

     ```sql
     TOP (Expression) [PERCENT] [WITH TIES]
     ```

     - Expression : 반환할 행의 수를 지정하는 숫자이다.

     - PERCENT : 쿼리 결과 집합에서 처음 Expression%의 행만 반환됨을 나타낸다.

     - WITH TIES : ORDER BY 절이 지정된 경우에만 사용할 수 있으며, TOP N(PERCENT)의 마지막 행과 같은 값이 있는 경우 추가 행이 출력되도록 지정할 수 있다.

     - 한 건의 행만 가져오고 싶을 때

       SELECT TOP(1) PLAYER_NAME FROM PLAYER;

     - 두 건 이상의 N 행을 가져오고 싶을 때

       SELECT TOP(N) PLAYER_NAME FROM PLAYER;

     SQL 문장에서 ORDER BY 절이 사용되지 않으면 Oracle 의 ROWNUM과 SQL Server의 TOP 절은 같은 기능을 하지만, ORDER BY 절이 같이 사용되면 기능의 차이가 발생한다. 이 부분은 1장 8절에 나옴

### !!! 연산 우선순위

1. 괄호 ()
2. NOT 연산자
3. 비교 연산자(=, >, >=), SQL 비교 연산자(BEETWEEN a AND b, IN (list), LIKE, IS NULL)
4. AND
5. OR

# 함수 (FUNCTION)

### 내장 함수 (BUILT-IN FUNCTION)

- 데이터베이스를 설치하면 기본적으로 제공됨
- 간편한 조작을 가능하게 해줌
- 입력 값이 몇 개인지 상관없이 출력은 하나만 되는 M:1 관계임.
- 단일행 함수(Single-Row Function) : 단일 행이 함수의 입력 값으로 사용됨
  - 처리하는 데이터의 형식에 따라 문자형, 숫자형, 날짜형, 변환형, NULL 관련 함수로 나누어짐
- 다중행 함수(Multi-Row Function) : 여러 행의 값이 함수의 입력 값으로 사용됨
  - 집계 함수(Aggregate Function)
  - 그룹 함수(Group Function)
  - 윈도우 함수(Window Function)
- 기본 코드

```sql
함수명 (컬럼이나 표현식 [, Arg1, Arg2, ...])
```

### 단일행 함수

- 종류. ( / 로 구분된것들은 Oracle/SQL Server)

  - 문자형 함수 : 문자를 입력하면 문자나 숫자 값을 반환한다.

    - LOWER(문자열) : 문자열의 알파벳 문자를 소문자로 바꾼다

      ex) LOWER('SQL Expert') → 'sql expert'

    - UPPER(문자열) : 문자열의 알파벳 문자를 대문자로 바꾼다 ex) UPPER('sql expert') → 'SQL EXPERT'

    - ASCII(문자) : 문자나 숫자를 ASCII 코드 번호로 바꾼다 ex) ASCII('A') → 65

    - CHR/CHAR(ASCII번호) : ASCII 코드 번호를 문자나 숫자로 바꾼다 ex) CHR(65) / CHAR(65) → 'A'

    - CONCAT(문자열1, 문자열2) : Oracle, MY SQL에서 유효한 함수이며 문자열1과 문자열2를 연결한다. 합성 연산자 '||'(Oracle) 이나 '+'(SQL Server) 와 동일하다 ex) CONCAT('RDBMS', 'SQL') or 'RDBMS' || 'SQL' or 'RDBMS' + 'SQL' → 'RDBMS SQL'

    - SUBSTR/SUBSTRING( 문자열, m[, n ] ) : 문자열 중 m위치에서 n개의 문자 길이에 해당하는 문자를 돌려준다. n이 생략되면 마지막 문자까지이다 ex) SUBSTR('SQL Expert', 5, 3)/ SUBSTRING('SQL Expert', 5, 3) → 'Exp',

    - LENGTH(Oracle)/LEN(SQL Server) : 문자열의 개수를 숫자값으로 돌려준다 ex) LENGTH('SQL Expert') / LEN('SQL Expert') : 10

    - LTRIM(문자열 [, 지정문자]) : 문자열의 첫 문자부터 확인해서 지정 문자가 나타나면 해당 문자를 제거. 지정 문자가 생략되면 공백 값이 Default. SQL Server에서는 LTRIM 함수에 지정문자를 사용할 수 없다. 즉 공백만 제거 가능하다 ex) LTRIM('xxxYYZZxYZ', 'x') → 'YYZZxYZ'

    - RTRIM문자열 [, 지정문자]) : 문자열의 므ㅏ지막 문자부터 확인해서 지정 문자가 나타나면 해당 문자를 제거. 지정 문자가 생략되면 공백 값이 Default. SQL Server에서는 RTRIM 함수에 지정문자를 사용할 수 없다. 즉 공백만 제거 가능하다 ex) RTRIM('XXYYzzXYzz', 'z') → 'XXYYzzXY' RTRIM('XXYYZZXYZ          ') → 'XXYYZZXYZ 공백 제거됨.

    - TRIM([leading|trailing|both] 지정문자 FROM 문자열) : 문자열에서 머리말, 꼬리말, 또는 양쪽에 있는 지정 문자를 제거한다. 생략되면 both 가 Defualt 이다. SQL Server 에서는 TRIM 함수에 지정문자를 사용 불가하여 공백만 제거 가능 ex) TRIM('x' FROM 'xxYYZZxYZxx) → 'YYZZxYZ'

    - Oracle 의 함수 사용

    ```sql
    Oracle
    
    SELECT LENGTH('SQL Ecpert')
    FROM DUAL; 
    
    LENGTH('SQL Expert')
    결과
    -------------------
                     10
    ```

    Oracle은 SELECT 절과 FROM 절 두 개의 절을 SELECT 문장의 필수 절로 지정하였으므로 사용자 테이블이 필요 없는 SQL 문장의 경우에도 필수적으로 DUAL 이라는 테이블을 FROM 절에 지정한다.

    - DUAL 테이블의 특성
      - 사용자 SYS가 소유하며 모든 사용자가 엑세스 가능한 테이블이다
      - SELECT ~ FROM ~ 의 형식을 갖추기 위한 일종의 DUMMY 테이블이다
      - DUMMY 라는 문자열 유형의 컬럼에 'X' 라는 값이 들어 있는 행을 1건 포함한다

    ```sql
    DESC DUAL;
    
    결과
    컬럼              NULL 가능         데이터 유형
    ------           ------           --------
    DUMMY                             VARCHAR2(1)
    
    ------------------------------------------------
    
    SELECT * FROM DUAL;
    
    결과
    DUMMY
    -----
        X
    ```

    ```sql
    # 문자 길이 구하기
    Oracle
    
    SELECT LEN('SQL Expert') AS ColumnLength;
    
    결과
    ColumnLength
    ----------
    10
    
    # CONCAT함수로 문구 추가하기
    SELECT CONCAT(PLAYER_NAME, ' 축구선수') 선수명
    FROM PLAYER;
    
    # CONCAT 함수는 Oracle의 '||' 합성 연산자이므로 다음과 같다
    Oracle
    SELECT PLAYER_NAME || ' 축구선수' AS 선수명
    FROM PLAYER;
    
    # SQL Server에서는 '+'
    SQL Server
    SELECT PLAYER_NAME + ' 축구선수' AS 선수명
    FROM PLAYER;
    ```

    - SQL Server 나 Sybase의 함수 사용

      Sybase 나 SQL Server의 경우 SELECT 절만으로도 SQL 문장 수행이 가능.

      때문에 DUAL 이란 DUMMY 테이블이 필요 없다.

      그러나 Sybase 나 SQL Server 의 경우에도 사용자 테이블의 컬럼을 사용할 때는 FROM 절이 필수적으로 사용되어야 한다.

    - 특별한 제약 조건이 없다면 함수는 여러 개 중첩하여 사용이 가능하다. 함수 내부에 다른 함수를 사용하며 안쪽에 있는 함수부터 실행되어 그 결과가 인자로 전달된다.

      ```sql
      함수3 (함수2 (함수1 (컬럼이나 표현식 [, Arg1]) [[, Arg2]) [, Arg3])
      ```

      ex) 경기장의 지역번호와 전화번호를 합친 번호의 길이를 구하시오

      ```sql
      Oracle
      SELECT STADIUM_ID, DDD||TEL as TEL, LENGTH(DDD||TEL) as T_LEN
      FROM STADIUM;
      
      SQL Server
      SELECT STADIUM_ID, DDD+TEL as TEL, LEN(DDD+TEL) as T_LEN
      FROM STADIUM;
      ```

  - 숫자형 함수 : 숫자를 입력하면 숫자 값을 반환한다. Oracle/SQL Server

    - ABS(숫자) : 숫자의 절대값을 돌려준다. ABS(-15) : 15
    - SIGN(숫자) : 숫자가 양수인지, 음수인지 0인지를 구별한다. SIGN(-20) : -1, SIGN(0) : 0, SIGN(+20) : 1
    - MOD(숫자1, 숫자2) : 숫자 1을 숫자2로 나누어 나머지 값을 리턴한다. MOD 함수는 %연산자로도 대체 가능함 (ex: 7%3) MOD(7, 3) / 7%3 : 1
    - CEIL/CEILING(숫자) : 숫자보다 크거나 같은 최소 정수를 리턴한다 CEIL(38.123) : 39, CEILING(38.123) : 39, CEILING(-38.123) : -38
    - FLOOR(숫자) : 숫자보다 작거나 같은 최대 정수를 리턴한다 FLOOR(38.123) : 38, FLOOR(-38.123) : -39
    - ROUND(숫자 [, m]) : 숫자를 소수점 m 자리에서 반올림하여 리턴. m이 생략되면 디폴트 값은 0 ROUND(38.5235 ,3) : 38.524, ROUND(38.5235, 1) : 38.5, ROUND(38.5235, 0) : 39 ROUNT(38.5235) : 39(인수 0이 Default)
    - TRUNC(숫자 [ ,m]) : 숫자를 소수점 m 자리에서 잘라서 버림. m이 생략되면 디폴트 값은 0 SQL Server 에서 TRUNC 함수는 제공되지 않음 TRUNC(38.5235 ,3) : 38.523, TRUNC(38.5235, 1) : 38.5, TRUNC(38.5235, 0) : 38 TRUNC(38.5235) : 38(인수 0이 Default)
    - SIN, COS, TAN(숫자) : 숫자의 삼각함수 값을 리턴
    - EXP(), POWER(), SQRT(), LOG(), LN() : 지수, 거듭 제곱, 제곱근, 자연 로그 값을 리턴

    ```sql
    # 소수점 이하 한 자리까지 반올림 및 내림하여 출력하라
    SQL Server
    
    SELECT ENAME, ROUND(SAL/12,1), TRUNC(SAL/12,1)
    FROM EMP;
    
    # 정수 기준으로 반올림 및 올림하여 출력하라
    SQL Server
    
    SELECT ENAME, ROUND(SAL/12), CEILING(SAL/12)
    FROM EMP;
    ```

  - 날짜형 함수 : DATE 타입의 값을 연산한다. Oracle/SQL Server

    - SYSDATE/GETDATE() 현재 날짜와 시각을 출력

    - EXTRACT('YEAR'|'MONTH'|'DAY' from d) / DATEPART(YEAR'|'MONTH'|'DAY' , d) 날짜 데이터에서 년/월/일 데이터를 출력. 시간/분/초 도 가능하다

    - TO_NUMBER(TO_CHAR(d, 'YYYY')) / YEAR(d)

      TO_NUMBER(TO_CHAR(d, 'MM')) / MONTH(d)

      TO_NUMBER(TO_CHAR(d, 'DD')) / DAY(d)

      TO_NUMBER(TO_CHAR(d, 'YYYY'|'MM'|'DD')) / YEAR|MONTH|DAY

      위의 4개는 날짜 데이터에서 년/월/일 데이터를 출력하는 함수. Oracle EXTRACT YEAR/MONTH/DAY 옵션이나 SQL Server DEPARY YEAR/MONTH/DAY 옵션과 같은 기능

      TO_NUMBER 함수 제외시 문자형으로 출력된다.

    - 날짜형 변수도 덧셈, 뺄셈, 같은 산술 연산이 가능.

      - 날짜 + 숫자 → 숫자 만큼의 일수를 날짜에 더하고 결과도 날짜형이다
      - 날짜 - 숫자 → 숫자 만큼의 일수를 날짜에서 빼고 결과도 날짜형
      - 날짜1 - 날짜2 → 두 날짜간의 차이 일수를 구한다. 결과는 그 날짜 수이다.
      - 날짜 + 숫자/24 → 시간을 날짜에 더한다. 결과도 날짜형이다.

    예제)

    ```sql
    # 현재 날짜 데이터 확인
    Oracle
    
    SELECT SYSDATE
    FROM DUAL;
    
    SYSDATE
    결과
    -------
    12/07/18
    
    SQL Server
    
    SELECT GETDATE() AS CURRENTTIME;
    
    CURRENTTIME
    결과
    ---------------
    2012-07-18 12:10:02.047
    ```

    ```sql
    사원 테이블의 입사일자에서 년, 월, 일 데이터를 각각 출력. 다음 네 코드는 다 같은 결과를 얻음
    
    Oracle 1
    
    SELECT ENAME, HIREDATE,
    			 EXTRACT(YEAR FROM HIREDATE) 입사년도,
    			 EXTRACT(MONTH FROM HIREDATE) 입사월,
    			 EXTRACT(DAY FROM HIREDATE) 입사일,
    FROM EMP;
    
    ----------------------------------------
    Oracle 2
    
    SELECT ENAME, HIREDATE,
    			 TO_NUMBER(TO_CHAR(HIREDATE, 'YYYY') 입사년도,
    			 TO_NUMBER(TO_CHAR(HIREDATE, 'MM') 입사월,
    			 TO_NUMBER(TO_CHAR(HIREDATE, 'DD') 입사일,  # TO_NUMBER 함수 제외시 문자형으로 출력됨 01,02,03...
    FROM EMP;
    
    ----------------------------------------
    SQL Server 1
    
    SELECT ENAME, HIREDATE, 
    			 DATEPART(YEAR, HIREDATE) 입사년도,
    			 DATEPART(MONTH, HIREDATE) 입사월,
    			 DATEPART(DAY, HIREDATE) 입사일,
    FROM EMP;
    
    ----------------------------------------
    SQL Server 2
    
    SELECT ENAME, HIREDATE, 
    			 YEAR(HIREDATE) 입사년도,
    			 MONTH(HIREDATE) 입사월,
    			 DAY(HIREDATE) 입사일,
    FROM EMP;
    ```

  여기서부터

  - 변환형 함수 : 문자, 숫자, 날짜형 값의 데이터 타입을 변환한다. 크게 명시적 데이터 유형 변환과 암시적 데이터 유형 변환으로 나뉜다

    - 명시적(Explicit) 데이터 유형 변환 : 데이터 변환형 함수로 데이터 유형을 변환하도록 명시해 주는 경우
    - 암시적(Implicit) 데이터 유형 변환 : 데이터베이스가 자동으로 데이터 유형을 변환하여 계산하는 경우

    TO_NUMBER,

    TO_CHAR,

    TO_DATE / CAST, CONVERT

  - NULL관련 함수 : NULL 을 처리하기 위한 함수 NVL / ISNULL,  NULLIF, COALESCE

- 단일행 함수의 중요 특징

  - SELECT, WHERE, ORDER BY 절에 사용 가능
  - 각 행들에 대해 개별적으로 적용하여 데이터 값들을 조작하고, 각각의 행에 대한 조작 결과를 리턴한다
  - 여러 인자(Argument)를 입력해도 단 하나의 결과만 리턴
  - 함수의 인자(Argument)로 상수, 변수, 표현식이 사용 가능하고, 하나의 인수를 가지는 경우도 있지만 여러 개의 인수를 가질 수도 있다.
  - 특별한 경우가 아니면 함수의 인자(Argument)로 함수를 사용하는 함수의 중첩이 가능하다
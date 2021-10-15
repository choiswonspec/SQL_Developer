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
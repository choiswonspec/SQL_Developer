https://www.notion.so/SQL-01592e79d34c432a997906db1445b5a4

# 제 2장 : SQL 활용

- 제 1절 : 표준 조인
- 제 2절 : 집합 연산자
- 제 3절 : 계층형 질의와 셀프 조인
- 제 4절 : 서브쿼리
- 제 5절 : 그룹 함수
- 제 6절 : 윈도우 함수
- 제 7절 : DCL
- 제 8절 : 절차형 SQL

------

- 집합 연산자 :

  - 두 개 이상의 테이블에서 조인을 사용하지 않고 연관된 데이터를 조회할 때 사용.
  - SELECT 절의 컬럼 수가 동일하고 SELECT 절의 동일 위치에 존재하는 칼럼의 데이터 타입이 상호 호환할 때 사용 가능

- 일반 집합 연산자

  - UNION : 합집합(중복 행 1개로) 정렬
  - UNION ALL : 합집합 (중복 행도 표시) 정렬 X
  - INTERSECT : 교집합(중복 행 1개로)
  - MINUS : 차집합(중복 행 1개로)
  - CROSS JOIN : 곱집합(PRODUCT)

  ALIAS 는 처음 테이블.

  정렬은 마지막 테이블 기준

- 순수 관계 연산자 : 관계형 DB를 새롭게 구현

  - SELECT -> WHERE절로 구현
  - PROJECT -> SELECT절로 구현
  - NATRUAL JOIN -> 다양한 JOIN 으로 구현
  - DIVIDE -> 사용x {a,x}{a,y}{a,z} divdie {x,z} = {a}

------

## FROM JOIN 절 형태

1. INNER JOIN
2. NATURAL JOIN
3. USING 조건절
4. ON 조건절
5. CROSS JOIN
6. OUTER JOIN

- INNER JOIN :

  JOIN 조건에서 동일한 값이 있는 행만 반환

  USING 이나 ON 절을 필수적으로 사용 (그 동안 WHERE 절에서 사용하던 JOIN 조건을 FROM 절에서 정의하겠다는 표시이므로)

  - 예제) 테이블1, 테이블2에 공통으로 컬럼1을 가지고 있는 경우.

    WHERE 절 JOIN 조건

    ```sql
    SELECT 테이블1.컬럼1, 컬럼2, 컬럼3, 컬럼4 
    FROM 테이블1 테이블2 
    WHERE 테이블1.컬럼1 = 테이블2.컬럼1;
    ```

    위 SQL과 아래 SQL은 같은 결과

    ```sql
    FROM 절 JOIN 조건
    SELECT 테이블1.컬럼1, 컬럼2, 컬럼3,  컬럼4 
    FROM 테이블1 INNER JOIN 테이블2 
    ON 테이블1.컬럼1 = 테이블2.컬럼1;
    ```

    INNER 는 JOIN의 디폴트 옵션으로 아래와 같이 생략 가능하다.

    ```sql
    SELECT 테이블1.컬럼1, 컬럼2, 컬럼3, 컬럼4
    FROM 테이블1 JOIN 테이블2
    ON 테이블1.컬럼1 = 테이블2.컬럼1;
    ```

- NATURAL JOIN :

  두 테이블 간의 동일한 이름을 갖는 모든 칼럼들에 대해 EQUI(=) JOIN 수행.

  NATURAL JOIN 이 명시되면 추가로 USING 조건절, ON 조건절, WHERE 절에서 JOIN 조건을 정의할 수 없다

  SQL Sever는 지원하지 않는 기능.

  - 예제) 테이블1, 테이블2에 공통으로 컬럼1을 가지고 있음.

    ```sql
    SELECT 컬럼1, 컬럼2, 컬럼3, 컬럼4
    FROM 테이블1 NATURAL JOIN 테이블2
    ```

    위 SQL은 별도의 JOIN 칼럼을 지정하지 않았지만 두 개의 테이블에서 컬럼1라는 공통된 컬럼을 자동으로 인식하여 JOIN처리를 한다.

    JOIN에 사용된 컬럼들은 같은 데이터 유형이여야 햐며 ALIAS나 테이블 명과 같은 접두사를 붙일 수 없다.

    ```sql
    SELECT 테이블1.컬럼1, 컬럼2, 컬럼3, 컬럼4
    FROM 테이블1 NATURAL JOIN 테이블2
    
    ERROR : NATURAL JOIN 에 사용된 열은 식별자를 가질 수 없음.
    ```

    NATURAL JOIN은 JOIN이 되는 테이블의 데이터 성격과 컬럼명 등이 동일해야 하는 제약 조건이 있다.

- INNER JOIN과 NATURAL JOIN의 차이점

  - 별도의 컬럼 순서를 지정하지 않으면 NATURAL JOIN의 기준이 되는 컬럼들이 먼저 출력된다.
  - INNER JOIN의 경우 첫번째 테이블, 두번째 테이블의 컬럼 순서대로 데이터가 출력된다.
  - NATURAL JOIN은 JOIN에 사용된 같은 이름의 칼럼을 하나로 처리하지만 INNER JOIN은 별개로 표시한다.

- USING 조건절 두 테이블의 같은 이름을 가진 칼럼들 중에서 원하는 칼럼에 대해서만 선택적으로 EQUI JOIN을 할 수 있다.

  - 예제) 세 개의 컬럼명이 모두 같은 DEPT, DEPT_TEMP 테이블을 DEPTNO 컬럼을 이용한 INNER JOIN의 USING 조건절로 수행한다.

    ```sql
    SELECT * 
    FROM DEPT JOIN DEPT_TEMP
    USING (DEPTNO);
    ```

    위의 코드처럼 와일드카드(*)를 사용하여 컬럼 순서를 지정하지 않으면 USING절에 사용된 컬럼이 다른 컬럼보다 먼저 출력됨.

  - 공통된 컬럼은 하나로 처리한다.(테이블을 따로 명시하지 않음), 에러 발생.

  - 공통된 컬럼에서 한쪽 테이블에서 몇 개의 데이터가 변경된다면, JOIN 했을 시 그 데이터들은 제외되어 결과로 나타난다.

  - NATURAL JOIN과 마찬가지로 JOIN 칼럼에 대해서 ALIAS나 테이블 이름과 같은 접두사를 붙일 수 없다. (ex : 테이블명.컬럼명 → 컬럼명)

    따라서 같은 컬럼이지만 두 테이블에서 컬럼 이름이 다르다면, USING절은 사용할 수 없다.!!!!! 중요..

    ```sql
    # 두 테이블에서 컬럼 이름이 다르기 때문에 USING절 사용 불가능한 경우
    SELECT TEAM_NAME, TEAM_ID, STADIUM_NAME
    FROM TEAM JOIN STADIUM
    ON TEAM.TEAM_ID = STADIUM.HOMETEAM_ID
    ORDER BY TEAM_ID
    ```

  - SQL Server 지원 x

- ON 조건절 ON 조건절과  WHERE 조건절을 분리하여 이해가 쉬우며,

  칼럼명이 다르더라도 JOIN 조건을 사용할 수 있는 장점이 있다.

  ALIAS나 테이블명 반드시 사용해야 함. USING절과의 큰 차이점.

  - 예제)

    ```sql
    SELECT E.EMPNO, E.ENAME, E.DEPTNO, D.DNAME
    FROM EMP E JOIN DEPT D
    ON (E.DEPTNO = D.DEPTNO);
    ```

  - ON 조건절은 WHERE절의 JOIN 조건과 같은 기능을 하면서도, 명시적으로 JOIN 조건을 구분할 수 있으므로 가장 많이 사용됨. 가독성은 떨어짐.

  가. WHERE 절과의 혼용

  - ON 조건절과 WHERE 검색 조건은 충돌 없이 사용 가능.

  ```sql
  # DEPTNO로 JOIN 하지만 그 값이 30인 데이터들만 가져옴.
  SELECT E.ENAME, E.DEPTNO, D.DEPTNO, D.DNAME
  FROM EMP E JOIN DEPT D ON (E.DEPTNO = D.DEPTNO)
  WHERE E.DEPTNO = 30;
  ```

나. ON 조건절 + 데이터 검증 조건 추가

- ON 조건절에 JOIN조건 외에 데이터 검색 조건을 추가할 수 있으나, 검색 조건 목적인 경우 WHERE 조건을 그냥 사용해라. 다만 아우터 조인에서 조인의 대상을 제한하기 위한 목적으로 사용되는 추가 조건의 경우 ON 절로 해야 한다.

```sql
# WHERE 절 사용. 권장하는 방법
SELECT E.ENAME, E.MGR, D.DEPTNO, D.DNAME
FROM EMP E JOIN DEPT D ON (E.DEPTNO = D.DEPTNO)
WHERE E.MGR = 7698;

# ON 절 사용
SELECT E.ENAME, E.MGR, D.DEPTNO, D.DNAME
FROM EMP E JOIN DEPT D
ON (E.DEPTNO = D.DEPTNO AND E.MGR = 7698 );
```

- p.292 부터 예제들 한번씩 확인.

- 다중 테이블 JOIN

  - ex) 사원과 DEPT 테이블의 소속 부서명, DEPT_TEMP 테이블의 바뀐 부서명 정보를 출력

  ```sql
  # ON 절 사용해서 다중 테이블 JOIN
  SELECT E.EMPNO, D.DEPTNO, D.DNAME, T.DNAME New_DNAME
  FROM EMP E JOIN DEPT D
  ON (E.DEPTNO = D.DEPTNO)
  JOIN DEPT_TEMP T
  ON (E.DEPTNO = T.DEPTNO)
  
  # 고전 방식인 WHERE 절을 이용한 다중 테이블 INNER JOIN
  SELECT E.EMPNO, D.DEPTNO, D.DNAME, T.DNAME New_DNAME
  FROM EMP E, DEPT D, DEPT_TEMP T
  WHERE E.DEPTNO = D.DEPTNO
  AND E.DEPTNO = T.DEPTNO
  ```

- CROSS JOIN

  일반 집합 연산자의 PRODUCT의 개념. 테이블 간 JOIN 조건이 없는 경우 생길 수 있는 모든 데이터의 조합을 말한다.

  CARTESIAN PRODUCT 또는 CROSS PRODUCT 라고 표현함.

  결과는 양쪽 집합의 M*N건의 데이터 조합이 발생한다.

  ```sql
  # 기본 사용 방법
  SELECT ENAME, DNAME
  FROM EMP CROSS JOIN DEPT
  ORDER BY ENAME;
  ```

  - NATURAL JOIN의 경우 WHERE 절에서 JOIN 조건을 추가할 수 없지만, CROSS JOIN의 경우 WHERE 절에 JOIN 조건 추가 가능. 그러나 INNER JOIN과 같은 결과이기 때문에 권장하지 않음.

  ```sql
  SELECT ENAME, DNAME
  FROM EMP CROSS JOIN DEPT
  WHERE EMP.DEPTNO = DEPT.DEPTNO;
  
  # INNER 조인과 위의 코드의 기능은 같다.
  SELECT ENAME, DNAME
  FROM EMP INNER JOIN DEPT
  WHERE EMP.DEPTNO = DEPT.DEPTNo
  ```

- OUTER JOIN (LEFT, RIGHT, FULL)

  1. JOIN 조건에서 동일한 값이 없는 행도 반환 가능하다
  2. OUTER JOIN 역시 JOIN 조건을 FROM 절에서 정의하겠다는 표시이므로 USING 이나 ON 조건절 반드시 사용해야 함.
  3. 식에서 (+) 안붙은 쪽으로 JOIN 한다

  - LEFT OUTER JOIN :

    먼저 표기된 좌측 테이블에 해당하는 데이터를 읽은 후, 나중 표기된 우측 테이블에서 JOIN 대상 데이터를 읽어 온다.

    우측 값에서 같은 값이 없는 경우 NULL 값으로 채운다

    LEFT JOIN 으로 OUTER 는 생략 가능

    ```sql
    SELECT STADIUM_NAME, STADIUM.STADIUM_ID, SEAT_COUNT, HOMETEAM_ID, TEAM_NAME
    FROM STADIUM LEFT OUTER JOIN TEAM
    ON STADIUM.HOMETEAM_ID = TEAM.TEAM_ID
    ORDER BY HOMETEAM_ID;
    ```

  - RIGHT OUTER JOIN :

    LEFT OUTER JOIN의 반대. 우측 테이블이 기준이 되어 결과 생성.

    OUTER 키워드 생략 가능

    ```sql
    SELECT STADIUM_NAME, STADIUM.STADIUM_ID, SEAT_COUNT, HOMETEAM_ID, TEAM_NAME
    FROM STADIUM RIGHT OUTER JOIN TEAM
    ON STADIUM.HOMETEAM_ID = TEAM.TEAM_ID
    ORDER BY HOMETEAM_ID;
    ```

  - FULL OUTER JOIN :

    좌우측 테이블의 모든 데이터를 읽어 JOIN 하여 결과를 생성한다.

    LEFT OUTER JOIN 과 RIGHT OUTER JOIN의 결과를 합집합한 것.

    UNION ALL 이 아닌 UNION 기능과 같으므로 중복 데이터는 삭제한다.

    OUTER 키워드는 생략 가능

    ```sql
    # 먼저 DEPT테이블에서 수정하여 DEPT_TEMP 테이블을 생성
    # 결과적으로, DEPT_TEMP 테이블의 새로운 DEPTNO 데이터는 DEPT 테이블의 DEPTNO 와 2건은 동일하고 2건은 새로운 DEPTNO 가 생성됨.
    UPDATE DEPT_TEMP
    SET DEPTNO = DEPTNO + 20;
    
    SELECT * FROM DEPT_TEMP
    
    # DEPTNO 기준으로 두 테이블을 FULL OUTER JOIN으로 출력
    SELECT *
    FROM DEPT FULL OUTER JOIN DEPT_TEMP
    ON DEPT.DEPTNO = DEPT_TEMP.DEPTNO;
    
    # 위 코드와 같은 코드. UNION 절은 다음 절에서 설명.
    SELECT L*
    FROM DEPT L LEFT OUTER JOIN DEPT_TEMP R
    ON L.DEPTNO = R.DEPTNO
    UNION
    SELECT *
    FROM DEPT L RIGHT OUTER JOIN DEPT_TEMP R
    ON L.DEPTNO = R.DEPTNO;
    ```

------

## 집합 연산자 (SET OPERATOR)

1. 두 개 이상의 테이블에서 조인을 사용하지 않고 연관된 데이터를 조회하는 방법중 하나

2. 2개 이상의 질의 결과를 하나의 결과로 만든다

3. 일반적으로 집합 연산자를 사용하는 상황

   1. 서로 다른 테이블에서 유사한 형태의 결과를 반환하는 것을 하나의 결과로 합칠 때
   2. 동일 테이블에서 서로 다른 질의를 수행하여 결과를 합칠 때
   3. 튜닝관점에서 실행계획을 분리하고자 하는 목적

4. 집합 연산자를 사용하기 위해 만족해야하는 제약조건

   1. SELECT 절의 컬럼 수가 동일
   2. SELECT 절의 동일 위치에 존재하는 컬럼의 데이터 타입이 상호 호환 가능해야함. 반드시 동일한 데이터 타입일 필요는 없음.

   두 조건만 만족한다면 어떤 종류의 SELECT 문이라도 이용 가능

5. 집합 연산자의 종류

   1. UNION : 여러 개의 SQL문의 결과에 대한 합집합. 결과에서 모든 중복된 행은 하나의 행으로 만든다.
   2. UNION ALL : UNION 결과에서 중복된 행도 그대로 결과로 표시. 일반적으로 여러 결과가 상호 베타적일 때 많이 사용. 개별 SQL문의 결과가 서로 중복되지 않는 경우 UNION과 결과가 동일하나 정렬의 순서에 차이가 존재
   3. INTERSECT : 여러 개의 SQL문의 결과에 대한 교집합. 중복된 행은 하나의 행으로 만든다.
   4. EXCEPT : 앞 SQL 문의 결과에서 뒤 SQL문의 결과에 대한 차집합. 중복된 행은 하나의 행으로 만든다. 일부 데이터베이스는 MINUS를 사용.

- 합집합 예제

  ```sql
  SELECT PLAYER_NAME, BACK_NO
  FROM PLAYER
  WHERE TEAM_ID = 'K02'
  UNION
  SELECT PLAYER_NAME, BACK_NO
  FROM PLAYER
  WHERE TEAM_ID = 'K07' ORDER BY 1;
  ```

  ORDER BY는 집합 연산 적용 후 결과에 대한 정렬이므로 마지막 줄에 한번만 기술

- 차집합 예제

  삼성 블루윙즈 선수들과 포지션이 미드필더가 아닌 선수들을 보고 싶다!

  삼성 블루윙즈 선수들 집합과 미드필더 집합의 차집합 실행

  ```sql
  # Oracle 은 MINUS 사용
  SELECT TEAM_ID, PLAYER_NAME, POSITION, BACK_NO, HEIGHT
  FROM PLAYER
  WHERE TEAM_ID ='K02'
  MINUS
  SELECT TEAM_ID, PLAYER_NAME, POSITION, BACK_NO, HEIGHT
  FROM PLAYER
  WHERE POSITION = 'MF'
  ORDER BY 1,2,3,4,5;
  
  # SQL Server는 EXCEPT 사용
  SELECT TEAM_ID, PLAYER_NAME, POSITION, BACK_NO, HEIGHT
  FROM PLAYER
  WHERE TEAM_ID ='K02'
  EXCEPT
  SELECT TEAM_ID, PLAYER_NAME, POSITION, BACK_NO, HEIGHT
  FROM PLAYER
  WHERE POSITION = 'MF'
  ORDER BY 1,2,3,4,5;
  
  # 다음과 같이 표현 가능
  SELECT TEAM_ID, PLAYER_NAME, POSITION, BACK_NO, HEIGHT
  FROM PLAYER
  WHERE TEAM_ID ='K02' AND POSITION <>'MF'
  
  # 다음과 같이도 표현 가능 NOT EXISTS 사용
  SELECT TEAM_ID, PLAYER_NAME, POSITION, BACK_NO, HEIGHT
  FROM PLAYER X
  WHERE TEAM_ID ='K02' 
  AND NOT EXISTS (SELECT 1 FROM PLAYER Y WHERE Y.PLAYER_ID = X.PLAYER_ID AND POSITION='MF')
  ORDER BY 1,2,3,4,5;
  
  # 다음과 같이도 표현 가능. NOT IN 사용
  SELECT TEAM_ID, PLAYER_NAME, POSITION, BACK_NO, HEIGHT
  FROM PLAYER
  WHERE TEAM_ID ='K02' 
  AND PLAYER_ID NOT IN (SELECT PLAYER_ID FROM PLAYER WHERE POSITON = 'MF')
  ORDER BY 1,2,3,4,5;
  ```

- 교집합 예제

  ```sql
  SELECT TEAM_ID, PLAYER_NAME, POSITION, BACK_NO, HEIGHT
  FROM PLAYER
  WHERE TEAM_ID ='K02'
  INTERSECT
  SELECT TEAM_ID, PLAYER_NAME, POSITION, BACK_NO, HEIGHT
  FROM PLAYER
  WHERE POSITION = 'GK'
  ORDER BY 1,2,3,4,5;
  
  # 논리 연산자만으로 같은 결과를 얻을 수 있다
  SELECT TEAM_ID, PLAYER_NAME, POSITION, BACK_NO, HEIGHT
  FROM PLAYER
  WHERE TEAM_ID ='K02' AND POSITON = 'GK'
  ORDER BY 1,2,3,4,5;
  
  # EXISTS 로 같은 결과를 얻을 수 있다
  SELECT TEAM_ID, PLAYER_NAME, POSITION, BACK_NO, HEIGHT
  FROM PLAYER X
  WHERE X.TEAM_ID ='K02' 
  AND EXISTS (SELECT 1 FROM PLAYER Y WHERE Y.PLAYER_ID = X.PLAYER_ID AND Y.POSITION='GK')
  ORDER BY 1,2,3,4,5;
  
  # IN 으로 같은 결과를 얻을 수 있다
  SELECT TEAM_ID, PLAYER_NAME, POSITION, BACK_NO, HEIGHT
  FROM PLAYER
  WHERE TEAM_ID ='K02' 
  AND PLAYER_ID IN (SELECT PLAYER_ID FROM PLAYER WHERE POSITON = 'GK')
  ORDER BY 1,2,3,4,5;
  ```

- p.309 부터 예제를 다시 한번 읽어봐라.

------

## 계층형 질의 Hierarchical Query :

- 테이블에 계층형 데이터가 존재하는 경우 데이터를 조회하기 위해 사용

- 계층형 데이터 조회는 DBMS 벤터와 버전에 따라 다른 방법으로 지원한다. Oracle, SQL Server  만 외우면 됨.

- 계층형 데이터 : 동일 테이블에 계층적으로 상위, 하위 데이터가 포함된 것.

  [제목 없음](https://www.notion.so/e3df90fd309c4268ae09f0ff7f8a398f)

- Oracle 계층형 질의

  - START WITH : 계층 구조 전개의 시작 위치(root뿌리 데이터) 지정

  - CONNECT BY : 다음에 전개될 자식 데이터 지정

    - PRIOR : CONNECT BY 절에 사용되며, 현재 읽은 칼럼을 지정한다.

      PRIOR 자식 = 부모 형태를 사용하면 계층구조에서 부모 데이터에서 자식 데이터 (부모→자식) 방향으로 전개하는 순방향 전개를 한다

      PRIOR 부모 = 자식 형태는 역방향 전개

    - NOCYCLE : 데이터를 전개하면서 이미 나타났던 동일한 데이터가 전개 중에 다시 나타난다면 이것을 사이클(Cycle)이 형성되었다고 한다.

      사이클이 발생한 데이터는 런타임 오류가 발생한다.

      NOCYCLE 을 추가하면 사이클이 발생한 이후의 데이터는 전개하지 않는다. 즉, 동일한 데이터는 전개되지 않음

    - ORDER SIBLINGS BY : 형제 노드(동일 LEVEL)간의 정렬 수행

    - WHERE : 모든 전개를 수행한 후에 지정된 조건을 만족하는 데이터만 추출한다. (필터링)

  - Oracle은 계층형 질의를 사용할 때 다음과 같은 가상 컬럼(Pseudo Column)을 제공함.

    - LEVEL : 루트 데이터이면 1, 그 하위 데이터면 2, 리프 데이터까지 1씩 증가
    - CONNECT_BY_ISLEAF : 전체 과정에서 해당 데이터가 리프 데이터면 1, 그렇지 않으면 0
    - CONNECT_BY_ISCYCLE : 해당 데이터가 조상이면 1, 아니면 0 (CYCLE 옵션 사용했을 시만 사용 가능)

    ```sql
    SELECT LEVEL, LPAD(' ', 4*(LEVEL-1)) || 사원 사원, 관리자, CONNECT_BY_ISLEAF ISLEAF
    FROM 사원 START WITH 관리자 IS NULL
    CONNECT BY PRIOR 사원 = 관리자; # 관리자 -> 사원 순으로 전개됨.
    
    # LPAD는 들여쓰기를 위한 함수
    
    결과
    LEVEL  사원    관리자   ISLEAF
    -----  ---    ----    -----
    1      A                  0
    2       B       A         1
    2       C       A         0
    3        D      C         1
    3        E      C         1
    
    # 관리자 -> 사원 순으로 순방향 전개 됨.
    # 사원은 루트 데이터이기 때문에 LEVEL 1
    # 그 자식 데이터인 B,C 는 LEVEL 2
    
    # 역방향 전개 시...하위데이터에서 상위데이터로 전개
    SELECT LEVEL, LPAD(' ', 4*(LEVEL-1)) || 사원 사원, 관리자, CONNECT_BY_ISLEAF ISLEAF
    FROM 사원 START WITH 사원 = 'D' # D 사원부터 시작
    CONNECT BY PRIOR 관리자 = 사원; # 사원 -> 관리자 순으로 전개됨.
    
    결과
    LEVEL  사원    관리자   ISLEAF
    -----  ---    ----    -----
    1      D         C        0
    2       C        A        0
    3        A                1
    
    # 역방향 전개이므로 하위 데이터 -> 상위 데이터
    # D가 루트 데이터가 되어 LEVEL이 1
    ```

  - Oracle은 계층형 질의를 사용할 때 사용자 편의성을 위해 다음과 같은 함수를 제공

    - SYS_CONNECT_BY_PATH : 루트 데이터부터 현재 전개할 데이터까지의 경로를 표시한다.

      사용법 : SYS_CONNECT_BY_PATH(컬럼, 경로분리자)

    - CONNECT_BY_ROOT : 현재 전개할 데이터의 루트 데이터를 표시한다. 단항 연산자이다

      사용법: CONNECT_BY_ROOT 컬럼

    ```sql
    # 예제
    SELECT CONNECT_BY_ROOT 사원 루트사원,
    SYS_CONNECT_BY_PATH(사원, '/') 경로, 사원, 관리자
    FROM 사원
    START WITH 관리자 IS NULL
    CONNECT BY PRIOR 사원 = 관리자
    
    # 결과
    루트사원  경로    사원     관리자
    -----  ---    ----    -----
    A      /A        A       
    A      /A/B      B       A
    A      /A/C      C       A  
    A      /A/C/D    D       C
    A      /A/C/E    E       C 
    ```

- SQL Server 계층형 질의

  p.325 부터 내용인데 이해가 어려워 skip 함

------

## 셀프 조인 Self Join

- 한 테이블 내 두 칼럼이 연관 관계가 있을 때 동일 테이블 사이의 조인

- FROM절에 동일 테이블이 2 번 이상 나타난다.

- 반드시 테이블 별칭을 사용해야 함. 테이블과 컬럼 이름이 모두 동일하기 때문.

- 컬럼에도 반드시 테이블 별칭을 사용해서 어느 테이블의 컬럼인지 식별해줘야함.

- 이외의 사항은 조인과 동일

  ```sql
  # 기본적인 사용 방법
  SELECT ALIAS명1.컬럼명, ALIAS명2.컬럼명, ...
  FROM 테이블1 ALIAS명1, 테이블2 ALIAS명2 
  WHERE ALIAS명1.컬럼명2 = ALIAS명2.컬럼명1
  
  # 셀프 조인을 활용한 예시
  # 두단계 외 계층을 나타내는 컬럼 생성하기
  SELECT E1.사원, E1.관리자, E2.관리자 차상위_관리자 # 3번째 컬럼의 이름은 차상위_관리자로 한다.
  FROM 사원 E1, 사원 E2 # 별칭 설정은 필수 과정임
  WHERE E1.관리자 = E2.사원 # 그 사원의 관리자로 가져온다
  ORDER BY E1.사원;
  
  # 위의 코드에서 LEFT OUTER JOIN을 사용하면 관리자가 존재하지 않는 데이터까지 모두 결과에 반영됨.
  SELECT E1.사원, E1.관리자, E2.관리자 차상위_관리자
  FROM 사원 E1 LEFT OUTER JOIN 사원 E2 
  ON (E1.관리자 = E2.사원) # 그 사원의 관리자로 가져온다
  ORDER BY E1.사원;
  ```

------

## 서브 쿼리 :

하나의 SQL문안에 포함되어 있는 또 다른 SQL 문

알려지지 않은 기준을 이용한 검색에 사용.

1. 서브쿼리를 괄호로 감싸서 사용한다.

2. 서브쿼리는 단일 행 또는 복수 행 비교 연산자와 함께 사용 가능하다.

3. 단일 행 비교 연산자는 서브쿼리의 결과가 반드시 1건 이하여야 하고 복수 행 비교 연산자는 결과 건수와 상관없다.

4. 서브쿼리에서는 ORDER BY를 사용하지 못한다. ORDER BY 절은 SELECT 절에서 오직 한 개만 올 수 있기 때문에 ORDER BY 절은 메인쿼리의 마지막 문장에 위치해야 한다

5. 서브쿼리는 메인쿼리의 컬럼을 모두 사용할 수 있지만 메인쿼리는 서브쿼리의 컬럼을 사용 불가.

6. 서브쿼리는 서브쿼리 레벨과는 상관없이 항상 메인쿼리 레벨로 결과 집합이 생성된다.

   ex) 메인쿼리 조직(1), 서브쿼리 사원(M) 인 테이블을 사용하면 결과는 조직(1)레벨이 된다.

7. SELECT, FROM, WHERE, HAVING, ORDER BY, INSERT-VALUES, UPDATE-SET 절에 사용 가능

- 서브쿼리의 종류

  1. 동작하는 방식에 따라 분류

     - Un-Correlated(비연관) 서브 쿼리
       - 서브쿼리가 메인쿼리 컬럼을 가지고 있지 않는 형태의 서브쿼리
       - 메인쿼리에 값(서브쿼리가 실행된 결과)을 제공하기 위한 목적으로 주로 사용
     - Correlated(연관) 서브 쿼리
       - 서브쿼리가 메인쿼리 컬럼을 가지고 있는 형태의 서브쿼리
       - 일반적으로 메인쿼리가 먼저 수행되어 읽혀진 데이터를 서브쿼리에서 조건이 맞는지 확인하고자 할 때 주로 사용.

     서브쿼리는 메인쿼리 안에 포함된 종속적인 관계이기 때문에 논리적인 실행순서는 항상 메인쿼리에서 읽혀진 데이터에 대해 서브쿼리에서 해당 조건이 만족하는지를 확인하는 방식으로 수행되어야 한다. 그러나 실제 서브쿼리의 실행순서는 상황에 따라 달라질 수 있다. 반환되는 데이터의 형태에 따라 서브쿼리는 세가지로 분류한다.

  2. 반환되는 데이터의 형태에 따라 분류

     - Single Row 서브쿼리 (단일 행 서브쿼리)
       - 서브쿼리의 실행 결과가 항상 1건 이하인 서브쿼리를 의미
       - 단일 행 서브쿼리는 단일 행 비교 연산자와 함께 사용
       - 단일 행 비교 연산자에는 =, <, <=, >=, >, <> 이 있다.
     - Multi Row 서브쿼리 (다중 행 서브쿼리)
       - 서브쿼리의 실행 결과가 여러 건인 서브쿼리를 의미
       - 다중 행 서브쿼리는 다중 행 비교 연산자와 함께 사용
       - 다중 행 비교 연산자에는  IN, ALL, ANY, SOME, EXISTS
     - Multi Column 서브쿼리 (다중 컬럼 서브쿼리)
       - 서브쿼리의 실행 결과로 여러 컬럼을 반환.
       - 메인쿼리의 조건절에 여러 컬럼을 동시에 비교할 수 있다.
       - 서브쿼리와 메인쿼리에서 비교하고자 하는 컬럼 개수와 컬럼의 위치가 동일해야 한다.

- 단일 행 서브 쿼리

  - 서브쿼리가 단일 행 비교 연산자  =, <, <=, >=, >, <> 와 함께 사용할 때는 서브쿼리의 결과 건수가 반드시 1건 이하여야 한다.

  - 만약 서브쿼리 결과 건수가 2건 이상이면 Run TIme 오류가 발생한다.

    ```sql
    # 서브쿼리에서 정남일 선수가 포함된 팀의 ID를 뽑아 메인쿼리에 이용
    SELECT PLAYER_NAME 선수명, POSITION 포지션, BACK_NO 백넘버
    FROM PLAYER
    WHERE TEAM_ID = (SELECT TEAM_ID
    								FROM PLAYER
    								WHERE PLAYER_NAME = '정남일') # 서브쿼리의 결과가 1개여야함.
    ORDER BY PLAYER_NAME;
    ```

    ```sql
    # 서브쿼리에서 평균 키를 구한 후 메인쿼리에 이용
    SELECT PLAYER_NAME 선수명, POSITION 포지션, BACK_NO 백넘버
    FROM PLAYER
    WHERE HEIGHT <= (SELECT AVG(HEIGHT)
    								FROM PLAYER)
    ORDER BY PLAYER_NAME;
    ```

- 다중 행 서브 쿼리

  - 서브쿼리의 결과가 2건 이상 반환될 수 있다면 반드시 다중 행 비교 연산자  IN, ALL, ANY, SOME, EXISTS 와 함께 사용되어야 한다. 그렇지 않으면 오류가 발생한다.

  - 다중 행 비교 연산자

    IN : 서브쿼리의 결과에 존재하는 임의의 값과 동일한 조건을 의미 (Multiple OR 조건)

    ALL : 서브쿼리의 결과에 존재하는 모든 값을 만족하는 조건을 의미. 비교 연산자로 >를 사용했다면 메인쿼리는 서브쿼리의 모든 결과 값을 만족해야 하므로, 서브쿼리의 결과의 최대값보다 큰 모든 건이 조건을 만족한다.

    ANY : 서브쿼리의 결과에 존재하는 어느 하나의 값이라도 만족하는 조건을 의미. 비교 연산자로 >를 사용했다면 메인쿼리는 서브쿼리의 값들 중 어떤 값이라도 만족하면 되므로, 서브쿼리의 결과의 최소값보다 큰 모든 건이 조건을 만족한다. SOME은 ANY와 동일함.

    EXISTS : 서브쿼리의 결과를 만족하는 값이 존재하는지 여부를 확인하는 조건을 의미. 조건을 [출처] 서브쿼리의 종류 | 작성자 찐 만족하는 건이 여러 건이더라도 1건만 찾으면 더이상 검색하지 않는다.

  ```sql
  # 서브 쿼리의 결과가 2개이상일 때 IN을 활용하여 메인쿼리 실행
  SELECT REGION_NAME 연고지명, TEAM_NAME 팀명, E_TEAM_NAME 영문팀명
  FROM TEAM
  WHERE TEAM_ID IN (SELECT TEAM_ID
  									FROM PLAYER
  									WHERE PLAYER_NAME = '정현수') # 서브쿼리의 결과가 2개 나옴.
  ORDER BY TEAM_NAME;
  ```

- 다중 컬럼 서브 쿼리

  - 다중 컬럼 서브쿼리는 서브쿼리의 결과로 여러 개의 컬럼이 반환되어 메인 쿼리의 조건과 동시에 비교되는 것을 의미

  ```sql
  SELECT TEAM_ID 팀코드, PLAYER_NAME 선수명, POSITION 포지션, BACK_NO 백넘버, HEIGHT 키 
  FROM PLAYER
  WHERE (TEAM_ID, HEIGHT) IN (SELECT TEAM_ID, MIN(HEIGHT)
  														FROM PLAYER
  														GROUP BY TEAM_ID). # 두개의 컬럼이 결과로 나옴
  ORDER BY TEAM_ID, PLAYER_NAME;
  
  # 서브 컬럼의 결과로 나온 두개의 컬럼이 TEAM_ID 와 HEIGHT 컬럼과 각각 비교되어 결과로 나온다.
  # 동일 팀에서 해당 조건을 만족하는 선수가 여러 명 존재하면 여러 명이 결과로 반환될 수 있다. 
  # 하지만 이 기능은 SQL Server 에서는 지원되지 않는다.
  ```

- 연관 서브커리

  - 서브쿼리 내에 메인쿼리 컬럼이 사용된 서크쿼리 이다.

  ```sql
  SELECT T.TEAM_NAME 팀명, M.PLAYER_NAME 선수명, M.POSITION 포지션, M.BACK_NO 백넘버, M.HEIGHT 키
  FROM PLAYER M, TEAM T
  WHERE M.TEAM_ID = T.TEAM_ID AND M.HEIGHT < (SELECT AVG(S.HEIGHT)
  																						FROM PLAYER S
  																						WHERE S.TEAM_ID = M.TEAM_ID
  																						AND S.HEIGHT IS NOT NULL
  																						GROUP BY S.TEAM_ID )
  ORDER BY 선수명;
  
  # 예를 들어 가비 선수는 삼성 블루윙즈팀 소속이므로 삼성블루윙즈팀 소속의 평균키를 구하고 그 평균키와 가비 선수의 키를 비교하여 적을 경우에 선수에 대한 정보를 출력한다.
  # 만약, 평균키보다 선수의 키가 크거나 같으면 조건에 맞지 않기 때문에 해당 데이터는 출력되지 않는다.
  # 이와 같은 작업을 메인쿼리에 존재하는 모든 행에 대해서 반복 수행한다.
  ```

  - EXISTS 서브쿼리 :

    항상 연관 서브쿼리로 사용된다.

    또한 EXISTS 서브쿼리의 특징은 아무리 조건을 만족하는 건이 여러 건이더라도 조건을 만족하는 1건만 찾으면 추가적인 검색을 진행하지 않는다.

  ```sql
  # EXISTS 서브쿼리를 사용하여 특정 날짜 사이에 경기가 있는 경기장을 조회
  SELECT STADIUM_ID ID, STADIUM_NAME 경기장명
  FROM STADIUM A
  WHERE EXISTS (SELECT 1
  							FROM SCHEDULE X
  							WHERE X.STADIUM_ID = A.STADIUM_ID
  							AND X.SCHE_DATE BETWEEN '20120501' AND '20120502' )
  ```

- 그밖에 위치에서 사용하는 서브쿼리

  - SELECT 절에서 사용하는 서브쿼리인 스칼라 서브쿼리 ( Scalar Subquery )

    스칼라 서브쿼리는 한 행, 한 컬럼만을 반환하는 서브쿼리를 말한다.

    스칼라 서브쿼리는 컬럼을 쓸 수 있는 대부분의 곳에서 사용할 수 있다.

    스칼라 서브쿼리 또한 단일 행 서브쿼리이기 때문에 결과가 2건이상 반환되면 오류를 반환한다.

    ```sql
    # 선수 정보와 해당 선수가 속한 팀의 평균 키를 함께 출력하는 예제
    SELECT PLAYER_NAME 선수명, HEIGHT 키, (SELECT AVG(HIEHGT)
    																		FROM PLAYER X
    																		WHERE X.TEAM_ID = P.TEAM_ID) 팀평균키
    FROM PLAYER P
    
    # 메인쿼리 부분에서 선수들의 정보를 출력하고
    # 스칼라 서브쿼리 부분에서 해당 선수의 소속별 평균키를 알아낸다
    # 스칼라 서브쿼리는 메인쿼리의 결과 건수 만큼 반복수행 된다.
    # 같은 테이블의 별칭을 다르게 해서 사용했음이 확인됨.
    ```

  - FROM 절에서 사용하는 서브쿼리인 인라인 뷰 ( Inline View )

    - 테이블 명이 오는 FROM 절에 서브쿼리를 넣으면 결과가 마치 실행 시에 동적으로 생성된 테이블인 것처럼 사용할 수 있다. 인라인 뷰는 테이블 명이 올 수 있는 곳에 사용할 수 있다.

    - 인라인 뷰는 SQL문이 실행될 때만 임시적으로 생성되는 동적인 뷰이기 때문에 데이터베이스에 해당 정보가 저장되지 않는다.

    - 그래서 일반적인 뷰를 정적 뷰(Static View)라고 하고 인라인 뷰를 동적 뷰(Dynamic View)라고 한다.

    - 서브 쿼리의 컬럼은 메인쿼리에서 사용할 수 없지만, 인라인 뷰는 동적으로 생성된 테이블이므로 인라인 뷰를 사용하는 것은 조인 방식을 사용하는 것과 같다. 그렇기 때문에 인라인 뷰의 컬럼은 SQL문을 자유롭게 참조할 수 있다.

      ```sql
      # K-리그 선수들 중에서 포지션이 MF인 선수들의 소속팀명 및 선수 정보 출력.
      SELECT T.TEAM_NAME 팀명, P.PLAYER_NAME 선수명, P.BACK_NO 백넘버
      FROM (SELECT TEAM_ID, PLAYER_NAME, BACK_NO
      			FROM PLAYER
      			WHERE POSITION = 'MF') P, TEAM T
      WHERE P.TEAM_ID = T.TEAM_ID
      ORDER BY 선수명;
      
      # 인라인 뷰에서 포지션이 미드필더인 선수들을 추출한 임시 테이블이 하나 형성 되고 
      # 그 테이블을 FROM절에 활용하여 메인 쿼리에서 사용한다.
      ```

    - 인라인 뷰에서는 ORDER BY 절을 상요할 수 있다. 인라인 뷰에 먼저 정렬을 수행하고 정렬된 결과 중에서 일부 데이터를 추출하는 것을 TOP-N쿼리라고 한다. TOP-N 쿼리를 수행하기 위해서는 정렬 작업과 정렬 결과 중에서 일부 데이터만을 추출할 수 있는 방법이 필요하다. Oracle 에서는 ROWNUM이라는 연산자를 통해서 결과로 추출하고자 하는 데이터 건수를 제약할 수 있다.

      ```sql
      Oracle
      
      SELECT PLAYER_NAME 선수명, POSITION 포지션, BACK_NO 백넘버, HEIGHT 키
      FROM (SELECT PLAYER_NAME, POSITION, BACK_NO, HEIGHT
      			FROM PLAYER
      			WHERE HEIGHT IS NOT NULL
      			ORDER BY HEIGHT DESC)
      WHERE ROWNUM <= 5;
      ```

      ```sql
      SQL Server
      
      SELECT TOP(5) PLAYER_NAME AS 선수명, POSITION AS 포지션, BACK_NO AS 백넘저, HEIGHT AS 키
      FROM PLAYER
      WHERE HEIGHT IS NOT NULL
      ORDER BY HEIGHT DESC
      ```

      해당 SQL 문의 인라인 뷰에서 선수의 키를 내림차순으로 정렬 후 메인쿼리에서 ROWNUM을 사용해서 5명의 선수의 정보만을 추출함. 이것은 모든 선수 들 중 가장 키가 큰 5명의 선수를 출력한 것.

      다만, 다른 선수 중에서 키가 192인 선수(5번쨰 선수와 동일한 키)가 더 존재하더라도 해당 SQL문에서는 데이터가 출력되지 않는다. 이런 데이터까지 추출하고자 한다면 분석함수의 RANK관련 함수를 사용해야 한다.

  - HAVING 절에서 서브쿼리 사용하기.

    - HAVING 절은 그룹함수와 함께 사용될 때 그룹핑된 결과에 대해 부가적인 조건을 주기 위해 사용한다.

      ```sql
      # 평균키가 삼성 블루윙즈팀의 평균키보다 작은 팀의 이름과 해당 팀의 평균키를 구하기.
      SELECT P.TEAM_ID 팀코드, T.TEAM_NAME 팀명, AVG(P.HEIGHT) 평균키
      FROM PLAYER P, TEAM T
      WHERE P.TEAM_ID = T.TEAM_ID
      GROUP BY P.TEAM_ID, T.TEAM_NAME
      HAVING AVG(P.HEIGHT) < (SELECT AVG(HEIGHT)
      												FROM PLAYER
      												WHERE TEAM_ID = 'K02')
      ```

  - UPDATE 문의 SET 절에서 사용하기

    - 예시

      ```sql
      # TEAM 테이블에 STADIUM_NAME 컬럼을 사전에 추가 했다고 가정.(ALTER TABLE ADD COLUMN)
      # TEAM 테이블에 추가된 STADIUM_NAME의 값을 STADIUM 테이블에 이용하여 변경하고자 할때 다음과 같이 코드를 짬
      UPDATE TEAM A
      SET A.STADIUM_NAME = (SELECT X.STADIUM_NAME
      											FROM STADIUM X
      											WHERE X.STADIUM_ID = A.STADIUM_ID);
      ```

      서브쿼리를 사용한 변경 작업을 할 때 서브쿼리의 결과가 NULL을 반환할 경우 해당 컬럼의 결과가 NULL이 될 수 있기 때문에 주의.

  - INSERT 문의 VALUES 절에서 사용하기

    - 예시

      ```sql
      # PLAYER 테이블에 '홍길동'을 삽입하고자 할 때, PLAYER_ID 의 값을 현재 사용중인 PLAYER_ID
      # 에 1을 더한 값으로 넣고자 할 때 코드.
      INSERT INTO PLAYER(PLAYER_ID, PLAYER_NAME, TEAM_ID)
      VALUES((SELECT TO_CHAR(MAX(TO_NUMBER(PLAYER_ID))+1)
      FROM PLAYER), '홍길동', 'K06');
      ```

- 뷰 VIEW

  - 테이블은 실제로 데이터를 가지고 있는 반면, 뷰는 실제 데이터르르 가지고 있지 않고 단지 뷰 정의 (VIew Definition)만을 가지고 있다.

  - 뷰가 사용되면 뷰 정의를 참조해서 DBMS 내부적으로 질의를 재작성(Rewrite)하여 질의를 수행한다.

  - 뷰는 실제 데이터를 가지고 있지 않지만 테이블이 수행하는 역할을 수행하기 때문에 가상 테이블(Virtual Table)이라고도 한다

  - 뷰 사용의 장점.

    1. 독립성 : 테이블 구조가 변경되어도 뷰를 사용하는 응용 프로그램은 변경하지 않아도 된다.
    2. 편리성 : 복잡한 질의를 뷰로 생성함으로써 관련 질의를 단순하게 작성 가능. 또 해당 형태의 SQL문을 자주 사용할 때 뷰를 이용하면 편리하게 사용 가능
    3. 보안성 : 직원의 급여정보와 같이 숨기고 싶은 정보가 존재한다면, 뷰를 생성할 때 해당 컬럼을 뺴고 생성함으로써 정보를 감출 수 있다.

  - 뷰는 다음과 같이 CREATE VIEW 문을 통해서 생성할 수 있다.

    ```sql
    CREATE VIEW V_PLAYER_TEAM
    AS SELECT P.PLAYER_NAME, P.POSITION, P.BACK_NO, P.TEAM_ID, T.TEAM_NAME
    	 FROM PLAYER P, TEAM T
    	 WHERE P.TEAM_ID = T.TEAM_ID;
    
    # 이 뷰는 선수 정보와 해당 선수가 속한 팀명을 함께 추출하는 것.으로 그 명칭은 V_PLAYER_TEAM
    ```

  - 뷰는 테이블뿐만 아니라 이미 존재하는 뷰를 참조해서도 생성할 수 있다.

    ```sql
    CREATE VIEW V_PLAYER_TEAM_FILTER
    AS SELECT PLAYER_NAME, POSITION, BACK_NO, TEAM_NAME
    	 FROM V_PLAYER_TEAM. # 위에서 생성한 뷰를 이용하여 다시 뷰를 생성함.
    	 WHERE POSITION IN ('GK','MF'); # 포지션이 골기퍼 , 미드필더로만 추출
    ```

  - 뷰를 사용하려면 해당 뷰의 이름을 이용하면 된다

    ```sql
    # V_PLAYER_TEAM 안에서 성이 황씨인 선수들 추출
    SELECT PLAYER_NAME, POSITION, BACK_NO, TEAM_ID, TEAM_NAME
    FROM V_PLAYER_TEAM
    WHERE PLAYER_NAME LIKE '황%'
    
    # 위의 코드를 실행하면 DBMS가 내부적으로 SQL문을 다음과 같이 재작성함. 인라인 뷰와 유사한 형태
    SELECT PLAYER_NAME, POSITION, BACK_NO, TEAM_ID, TEAM_NAME
    FROM (SELECT P.PLAYER_NAME, P.POSITION, P.BACK_NO, P.TEAM_ID, T.TEAM_NAME
    			FROM PLAYER P, TEAM T
    			WHERE P.TEAM_ID = T.TEAM_ID)
    WHERE PLAYER_NAME LIKE '황%'
    ```

  - 뷰는 데이터를 저장하지 않고도 데이터를 조회할 수 있다.

  - 뷰를 제거하기 위해서는 DROP VIEW 문을 사용

    ```sql
    DROP VIEW V_PLAYER_TEAM;
    DROP VIEW V_PLAYER_TEAM_FILTER;
    ```

------

## 그룹 함수 (GROUP 함수)

- ANSI/ISO SQL 표준에서 정의하는 데이터 분석을 위한 세가지 함수

  1. AGGREGATE FUNCTION
  2. GROUP FUNCTION
  3. WINDOW FUNCTION

- AGGREGATE FUNCTION

  - 각종 집계 함수들 포함. COUNT, SUM, AVG, MAX, MIN 등.

- GROUP FUNCTION

  - 소계, 중계, 합계, 총 합계 등 의 표현

  - ROLLUP 함수 :

    ROLLUP에 지정된 Grouping Columns의 List는 Subtotal을 생성하기 위해 사용.

    Grouping Columns의 수를 N이라고 했을 때 N+1 Level의 Subtotal 이 생성.

    중요한 것은, ROLLUP의 인수는 계층 구조이므로 인수 순서가 바뀌면 수행 결과도 바뀌므로 순서에 주의

    - 일반적인 GROUP BY 절 사용

    ```sql
    예제) 부서명과 업무명을 기준으로 사원수와 급여 합을 집계.
    
    SELECT DNAME, JOB, COUNT(*) "Total Empl", SUM(SAL) "Total Sal"
    FROM EMP, DEPT
    WHERE DEPT.DEPTNO = EMP.DEPTNO
    GROUP BY DNAME, JOB;
    
    결과는 DNAME과 JOB의 조합 별 Total Empl, Total Sal 값이 주어짐.
    ```

    - GROUP BY 절 + ORDER BY 절 사용.

      자동으로 정렬되지 않기 때문에 정렬을 원하면 ORDER BY 절 사용해야함.

    ```sql
    SELECT DNAME, JOB, COUNT(*) "Total Empl", SUM(SAL) "Total Sal"
    FROM EMP, DEPT
    WHERE DEPT.DEPTNO = EMP.DEPTNO
    GROUP BY DNAME, JOB
    ORDER BY DNAME, JOB; 
    ```

    - ROLLUP 함수 사용

      ROLLUP 을 사용하면 각 DNAME 별 모든 JOB의 SUBTOTAL 이 따로 생성됨. DNAME이 3종류 니까 3건 추가.

      그리고 GRAND TOTAL 이 마지막 1건으로 생성됨.

    ```sql
    SELECT DNAME, JOB, COUNT(*) "Total Empl", SUM(SAL) "Total Sal"
    FROM EMP, DEPT
    WHERE DEPT.DEPTNO = EMP.DEPTNO
    GROUP BY ROLLUP (DNAME, JOB);
    ```

    - ROLLUP 함수 + ORDER BY 절 사용

    ```sql
    SELECT DNAME, JOB, COUNT(*) "Total Empl", SUM(SAL) "Total Sal"
    FROM EMP, DEPT
    WHERE DEPT.DEPTNO = EMP.DEPTNO
    GROUP BY ROLLUP (DNAME, JOB)
    ORDER BY DNAME, JOB; 
    ```

    - GROUPING 함수 사용

      ROLLUP, CUBE, GROUPING SETS 등 새로운 그룹 함수를 지원하기 위해 GROUPING 함수가 추가됨

      ROLLUP 이나 CUBE 에 의한 소계가 계산된 결과에는 GROUPING(EXPR) = 1 이 표시됨.

      그 외의 결과에는 GROUPING(EXPR) = 0 이 표시됨.

      GROUPING 함수와 CASE/DECODE를 이용해 소계를 나타내는 필드에 원하는 문자열 지정 가능

      보고서 작성에 유용하게 사용 가능하다.

    ```sql
    예제) ROLLUP 함수를 추가한 집계 보고서에서 집계 레코드를 구분할 수 있는 GROUPING 함수 추가.
    SELECT DNAME, GROUPING(DNAME), JOB, GROUPING(JOB), COUNT(*) "Total Empl", SUM(SAL) "Total Sal"
    FROM EMP, DEPT
    WHERE DEPT.DEPTNO = EMP.DEPTNO
    GROUP BY ROLLUP (DNAME, JOB);
    
    결과에서 부서별, 업무별과 전체 집계를 표시한 레코드에서는 GROUPING이 붙은 컬럼에 1을 리턴한 것을 확인 가능
    전체 합계를 나타내는 마지막 레코드에는 GROUPING(DNAME), GROUPING(JOB) 모두 1을 리턴함.
    ```

    - GROUPING 함수 + CASE 사용

      결과에서 집계 부분이 비어있던 곳에 특정 텍스트를 입힐 수 있다.

    ```sql
    SELECT 
    CASE GROUPING(DNAME) WHEN 1 THEN 'ALL Departments' ELSE DNAME END AS DNAME,
    CASE GROUPING(JOB) WHEN 1 THEN 'ALL Jobs' ELSE JOB END AS JOB,
    COUNT(*) "Total Empl", 
    SUM(SAL) "Total Sal"
    FROM EMP, DEPT
    WHERE DEPT.DEPTNO = EMP.DEPTNO
    GROUP BY ROLLUP (DNAME, JOB);
    
    Oracle 의 경우 DECODE 함수를 사용해 좀 더 짧게 표현 가능
    
    SELECT
    DECODE(GROUPING(DNAME), 1, 'ALL Departments', DNAME) AS DNAME,
    DECODE(GROUPING(JOB), 1, 'ALL Jobs', JOB) AS JOB,
    COUNT(*) "Total Empl", 
    SUM(SAL) "Total Sal"
    FROM EMP, DEPT
    WHERE DEPT.DEPTNO = EMP.DEPTNO
    GROUP BY ROLLUP (DNAME, JOB);
    ```

    - ROLLUP 함수 일부 사용

      GROUP BY ROLLUP (DNAME, JOB) 조건에서 GROUP BY DNAME, ROLLUP(JOB) 조건으로 변경한 경우.

    ```sql
    SELECT 
    CASE GROUPING(DNAME) WHEN 1 THEN 'ALL Departments' ELSE DNAME END AS DNAME,
    CASE GROUPING(JOB) WHEN 1 THEN 'ALL Jobs' ELSE JOB END AS JOB,
    COUNT(*) "Total Empl", 
    SUM(SAL) "Total Sal"
    FROM EMP, DEPT
    WHERE DEPT.DEPTNO = EMP.DEPTNO
    GROUP BY DNAME, ROLLUP(JOB); # JOB 에만 적용
    
    결과는 두 컬럼 전체에 대한 집계가 생략된것이 차이점임.
    ```

    - ROLLUP 함수 결합 컬럼 사용

    ```sql
    JOB과 MGR은 하나의 집합으로 간주하고, 부서별, JOB&MGR 에 대한 ROLLUP 결과 출력
    SELECT DNAME, JOB, MGR, SUM(SAL) "Total Sal"
    FROM EMP, DEPT
    WHERE DEPT.DEPTNO = EMP.DEPTNO
    GROUP BY ROLLUP (DNAME, (JOB, MGR)); 
    
    JOB, MGR 을 소계시 하나의 지합으로 간주하여 구분하지 않음!!!
    ```

  - CUBE 함수

    - ROLLUP 에서는 단지 가능한 Subtotal 만 생성했지만, CUBE는 결합 가능한 모든 값에 대하여 다차원 집계를 생성한다.
    - CUBE를 사용할 경우 내부적으로는 Grouping Columns의 순서를 바꾸어서 또 한 번의 Query를 추가 수행해야 한다. 뿐만 아니라 Grand Total은 양쪽의 Query 에서 모두 생성되므로 한 번의 Query에서는 제거되어야 하므로 ROLLUP에 비해 시스템의 연산 부담이 많다.
    - 따라서, Grouping Columns이 가질 수 있는 모든 경우에 대해 Subtotal을 생성해야 하는 경우에 CUBE를 사용하는것이 바람직하다.
    - CUBE 함수의 경우 표시된 인수들에 대한 계층별 집계를 구할 수 있으며, 이 때 표시된 인수들 간에는 계층 구조인 ROLLUP과는 달리 평등한 관계이므로 인수의 순서가 바뀌는 경우 행간에 정렬 순서는 바뀔 수 있어도 데이터 결과는 같다.
    - CUBE 결과에 대한 정렬이 필요한 경우는 ORDER BY 절에 명시적으로 정렬 컬럼을 표시해야함
    - CUBE 함수 이용

    ```sql
    GROUP BY ROLLUP (DNAME, JOB) 조건에서 GROUP BY CUBE (DNAME, JOB) 조건으로 변경해서 수행한다.
    
    SELECT 
    CASE GROUPING(DNAME) WHEN 1 THEN 'ALL Departments' ELSE DNAME END AS DNAME,
    CASE GROUPING(JOB) WHEN 1 THEN 'ALL Jobs' ELSE JOB END AS JOB,
    COUNT(*) "Total Empl", 
    SUM(SAL) "Total Sal"
    FROM EMP, DEPT
    WHERE DEPT.DEPTNO = EMP.DEPTNO
    GROUP BY CUBE (DNAME, JOB); # CUBE 함수를 사용
    ```

    CUBE는 GROUPING COLUMNS이 가질 수 있는 모든 경우의 수에 대해 Subtotal을 생성.

    GROUPING COLUMNS의 수가 N이면, 2의 N승 Level의 Subtotal을 생성하게 된다.

    - UNION ALL 사용 SQL

    UNION ALL 은 Set Operation 내용으로, 여러 SQL 문장을 연결하는 역할 수행.

    ```sql
    SELECT DNAME, JOB, COUNT(*) "Total Empl", SUM(SAL) "Total Sal"
    FROM EMP, DEPT
    WHERE DEPT.DEPTNO = EMP.DEPTNO
    GROUP BY DNAME, JOB
    UNION ALL
    SELECT DNAME, 'All Jobs', COUNT(*) "Total Empl", SUM(SAL) "Total Sal"
    FROM EMP, DEPT
    WHERE DEPT.DEPTNO = EMP.DEPTNO
    GROUP BY DNAME
    UNION ALL
    SELECT 'All Departments', JOB, COUNT(*) "Total Empl", SUM(SAL) "Total Sal"
    FROM EMP, DEPT
    WHERE DEPT.DEPTNO = EMP.DEPTNO
    GROUP BY JOB
    UNION ALL
    SELECT 'All Departments', 'All JObs', COUNT(*) "Total Empl", SUM(SAL) "Total Sal"
    FROM EMP, DEPT
    WHERE DEPT.DEPTNO = EMP.DEPTNO
    ```

    CUBE 함수를 사용하면서 가장 크게 개선되는 부분은 CUBE 사용 전 SQL에서 EMP, DEPT 테이블을 네 번이나 반복 엑세스하는 부분은 CUBE 사용 SQL 에서는 한 번으로 줄일 수 있는 부분이다.

    기존에 같은 테이블을 네 번 엑세스하는 이유가 되었던 부서와 업무 별 소계와 총계 부분을 CUBE 함수를 사용함으로써 한 번의 엑세스만으로 구현한다.

    결과적으로 수행속도 및 자원 사용율을 개선할 수 있으며, SQL 문장도 더 짧아졌으므로 가독성도 높아졌다. 실행 결과는 STEP5의 결과와 동일하다. ROLLUP 함수도 똑같은 개선 효과를 얻을 수 있다.

  - GROUPING SETS 함수

    GROUPING SETS 를 이용해 더욱 다양한 소계 집하을 만들 수 있는데, GROUP BY SQL 문장을 여러 번 반복하지 않아도 원하는 결과를 쉽게 얻을 수 있다.

    GROUPING SETS에 표시된 인수들에 대한 개별 집계를 구할 수 있으며, 이 떄 표시된 인수들 간에는 계층 구조인 ROLLUP과는 달리 평등한 관계이므로 순서가 바뀌어도 결과는 같다.

    그리고 GROUPING SETS 함수도 결과에 대한 정렬이 필요한 경우는 ORDER BY 절에 명시적으로 정렬 컬럼이 표시가 되어야 한다.

    - 일반 그룹함수를 이용한 SQL

    ```sql
    일반 그룹함수를 이용하여 부서별, JOB별 인원수와 급여 합 구하기
    
    SELECT DNAME, 'ALL Jobs' JOB, COUNT(*) "Total Empl", SUM(SAL) "Total Sal"
    FROM EMP, DEPT
    WHERE DEPT.DEPTNO = EMP.DEPTNO
    GROUP BY DNAME
    UNION ALL
    SELECT 'All Departments' DNAME, JOB, COUNT(*) "Total Empl", SUM(SAL) "Total Sal"
    FROM EMP, DEPT
    WHERE DEPT.DEPTNO = EMP.DEPTNO
    GROUP BY JOB;
    ```

    - GROUPING SETS 사용 SQL

    ```sql
    GROUPING SETS 함수를 이용하여 부서별, JOB별 인원수와 급여 합 구하기
    
    SELECT DECODE(GROUPING(DNAME), 1, 'All Departments', DNAME) AS DNAME,
    			 DECODE(GROUPING(JOB), 1, 'All Jobs', JOB) AS JOB,
    			 COUNT(*) "Total Empl",
    			 SUM(SAL) "Total Sal"
    FROM EMP, DEPT
    *WHERE DEPT.DEPTNO = EMP.DEPTNO*
    GROUP BY GROUPING SETS (DNAME, JOB);
    
    위 코드와 결과는 같으며 괄호로 묶은 집합 별로(괄호 내는 계층 구조가 아닌 하나의 데이터로 간주함)
    집계를 구할 수 있다. GROUPING SETS의 경우 일반 그룹함수를 이용한 SQL과 결과 데이터는 같으나 행들의
    정렬 순서는 다를 수 있다.
    ```

    - GROUPING SETS 사용 SQL - 순서 변경

    ```sql
    SELECT DECODE(GROUPING(DNAME), 1, 'All Departments', DNAME) AS DNAME,
    			 DECODE(GROUPING(JOB), 1, 'All Jobs', JOB) AS JOB,
    			 COUNT(*) "Total Empl",
    			 SUM(SAL) "Total Sal"
    FROM EMP, DEPT
    *WHERE DEPT.DEPTNO = EMP.DEPTNO*
    GROUP BY GROUPING SETS (JOB, DNAME);
    ```

    - 3개의 인수를 이용한 GROUPING SETS 이용

    ```sql
    SELECT DNAME, JOB, MGR, SUM(SAL) "Total Sal"
    FROM EMP, DEPT
    *WHERE DEPT.DEPTNO = EMP.DEPTNO*
    GROUP BY GROUPING SETS ((DNAME,JOB,MGR),(DNAME, JOB), (JOB, MGR));
    
    GROUPING SETS 함수 사용 시 괄호로 묶은 집합별로 (괄호 내는 계층구조가 아닌 하나의 데이터로 간주합)
    집계를 구할 수 있다.
    ```

- WINDOW FUNCTION. 그냥 여기서부터 교재 봐라

  - 기존 관계형 데이터베이스는 행과 행간의 관계를 정의하거나, 행과 행간을 비교, 연산하는 것을 하나의 SQL으로 처리 하는 것이 매우 어려운 문제

  - WINDOW FUNCTION을 활용해 행과 행간의 관계를 쉽게 정의하여 복잡한 프로그램을 하나의 SQL문장으로 쉽게 해결 가능

  - 분석 함수(ANALYTIC FUNCTION), 순위 함수(RANK FUNCTION)으로도 알려여 있는 윈도우 함수는 데이터웨어하우스에서 발전한 기능

  - WINDOW 함수는 기존에 사용하던 집계 함수도 있고, 새로이 WINDOW 함수 전용으로 만들어진 기능도 있다.

  - WINDOW 함수는 다른 함수와 달리 중첩해서 사용하지 못하지만, 서브쿼리에서는 사용 가능

  - WINDOW FUNCTION 종류 5개 그룹

    1. 그룹 내 순위(RANK) 관련 함수.

       RANK, DENSE_RANK, ROW_NUMBER

       대부분의 DBMS에서 지원

       [ 순위 관련 함수 ]

       - RANK : 동일한 값에 대해서는 동일한 순위를 부여 (1,2,2,4)
       - DENSE_RANK : 동일한 순위를 하나의 등수로 간주(1,2,2,3)
       - ROW_NUMBER : 동일한 값이라도 고유한 순위 부여 (1,2,3,4)

    2. 그룹 내 집계(AGGREGATE) 관련 함수

       SUM, MAN, MIN, AVG, COUNT

       대부분의 DBMS에서 지원하나, SQL Server의 경우 집계 함수는 뒤에서 설명할 OVER절 내의 ORDER BY 구문을 지원하지 않음.

       [ 집계 관련 수함 ]

       - SUM : 파티션별 윈도우의 합 구할 수 있다. e.g. 같은 매니저를 두고 있는 사원들의 월급 합
       - MAX, MIN : 파티션별 윈도우의 최대 최소 , 값을 구할 수 있다 같은 . e.g. 매니저를 두고 있는 사원들 중 최대 값
       - AVG : 원하는 조건에 맞는 데이터에 대한 통계 값 e.g. 같은 매니저 내에서 앞의 사번과 뒤의 사번의 평균 ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING (현재 행을 기준으로 파티션 내에서 앞의 1 , 건 현재행 뒤의 , 1건을 범위로 지정)
       - COUNT : 조건에 맞는 데이터에 대한 통계 값 ex) 50 본인의 급여보다 이하가 적거나 150 이하로 많은 급여를 받는 인원수

    3. 그룹 내 순서 관련 함수

       FIRST_VALUE, LAST_VALUE, LAG, LEAD

       Oracle에서만 지원되는 함수이지만, FIRST_VALUE, LAST_VALUE 함수는 MAX, MIN 함수와 비슷한 결과를 얻을 수 있고, LAG, LEAD 함수는 DW에서 유용하게 사용되는 기능

       [ 행 순서 관련 함수 ] - SQL Server 지원 X

       - FIRST_VALUE : 파티션별 윈도우의 처음 값
       - LAST_VALUE : 파티션별 윈도우의 마지막 값
       - LAG : 파티션별 윈도우에서 이전 몇 번째 행의 값
       - LEAD : 파티션별 윈도우에서 이후 몇 번째 행의 값

    4. 그룹 내 비율 관련 함수

       CUME_DIST, PERCENT_RANK, NTILE, RATIO_TO_REPORT

       CUME_DIST, PERCENT_RANK 함수는 ANSI/ISO SQL 표준과 Oracle DMBS에서 지원

       NTILE함수는 ANSI/ISO SQL 표준에는 없지만, Oracle, SQL Server 에서 지원

       RATIO_TO_REPORT 함수는 Oracle 에서만 지원

       [ 비율 관련 함수 ]

       - RATIO_TO_REPORT : SUM 파티션 내 전체 에 대한 행별 칼럼 값의 백분율을 소수점으로 구할 수 있다. >0, <=1
       - PERCENT_RANK : 파티션별 윈도우에서 처음 값을 0, 1 마지막 값을 로 하여 행의 순서별 백분율을 구한다. 0>=,<=1
       - CUME_DIST : 현재 행보다 작거나 같은 건수에 대한 누적백분율을 구한다. >0, <=1
       - NTILE : 파티션별 전체 건수를 인수 값으로 N등분한 결과를 구할 수 있다.

    5. 선형 분석을 포함한 통계 분석 관련 함수.. 통계에 특화된 기능..

       CORR, COVAR_POP, COVAR_SAMP, STDDEV, STDDEV_POP, STDDEV_SAMP, VARIANCE, VAR_POP, VAR_SAMP, REGR_(LINEAR REGRESSION), REGR_SLOPE, REGR_INTERCEPT, REGR_COUNT, REGR_R2, REGR_AVGX, REGR_AVGY, REGR_SXX, REGR_SYY, REGR_SXY

  - WINDOW FUNCTION SYNTAX

    WINDOW 함수에는 OVER 문구가 키워드로 필수 포함된다

    ```sql
    SELECT WINDOW_FUNCTION (ARGUMENTS) OVER
    ( [PARTITION BY 컬럼] [ORDER BY 절] [WINDOWING 절] )
    FROM 테이블 명;
    ```

    - WINDOW_FUNCTION

      기존에 사용하던 함수도 있고, 새롭게 WINDOW 함수용으로 추가된 함수도 있다.

    - ARGUMENTS (인수)

      함수에 따라 0 ~ N개의 인수가 지정될 수 있다.

    - PARTITON BY 절

      전체 집합을 기준에 의해 소그룹으로 나눌 수 있다.

    - ORDER BY 절

      어떤 항목에 대해 순위를 지정할 지 ORDER BY 절을 기술한다.

    - WINDOWING 절

      WINDOWING 절은 함수의 대상이 되는 행 기준의 범위를 강력하게 지정할 수 있다.

      ROWS는 물리적인 결과 행의 수를, RANGE는 논리적인 값에 의한 범위를 나타낸다.  둘 중의 하나를 선택해서 사용할 수 있다. 다만, WINDOWING 절은 SQL Server에서는 지원하지 않는다.

      ```sql
      BETWEEN 사용 타입 
      ROWS | RANGE BETWEEN
      UNBOUNDED PRECEDING | CURRENT ROW | VALUE_EXPR PRECEDING/FOLLOWING
      AND
      UNBOUNDED FOLLOWING | CURRENT ROW | VALUE_EXPR PRECEDING/FOLLOWING
      
      BETWEEN 미사용 타입
      ROWS | RANGE
      UNBOUNDED PRECEDING | CURRENT ROW | VALUE_EXPR PRECEDING 
      ```

---











- ROLLUP : Subtotal을 생성하기 위해 사용, Grouping Columns N 의 수를 이라고 했을 때 N+1

Level Subtotal . 의 이 생성된다 인수 순서에 주의

- GROUPING : 1, 0 집계 표시면 아니면
- CUBE : 결합 가능한 모든 값에 대하여 다차원 집계를 생성 에 , ROLLUP 비해 시스템에 부하 심함. 2^N CUBE(A, B) = GROUPING SETS(A, B, (A,B ), ())
- GROUPING SETS : 인수들에 대한 개별 집계를 구할 수 있다 다양한 . 소계 집합 생성 가능

------

- 윈도우 함수 : 행과 행간의 관계를 정의하거나 행과 행간을 비교 연산하는 , 함수

[DCL] : 유저 생성하고 권한을 제어할 수 있는 명령어

< Oracle SQL Server 과 의 사용자 아키텍처 차이 >

- Oracle : DB 유저를 통해 에 접속을 하는 형태 와 , ID PW 방식으로 인스턴스에 접속을 하고 그에 해당하는 스키마에 오브젝트 생성 등의 권한을 부여받게 됨

- SQL Server : 인스턴스에 접속하기 위해 로그인이라는 것을 생성하게 되며 인스턴스 내에 , 존재하는 다수의 DB에 연결하여 작업하기 위해 유저를 생성한 후 로그인과 유저를 매핑해 주어야 한다 인증 . Windows 방식과 혼합 모드 방식이 존재함

- 시스템 권한 : SQL 사용자가 문을 실행하기 위해 필요한 적절한 권한

- GRANT : 권한 부여

- REVOKE : 권한 취소

  GRANT CREATE USER TO SCOTT; CONN SCOTT/TIGER(ID/PW) CREATE USER PJS IDENTIFIED BY KOREA7; GRANT CREATE SESSION TO PJS;

  GRANT CREATE TABLE TO PJS; REVOKE CREATE TABLE FROM PJS;

  모든 유저는 각각 자신이 생성한 테이블 외에 다른 유저의 테이블에 접근하려면 해당 테이블에 대한 오브젝트 권한을 소유자로부터 부여받아야 한다.

- ROLE : 유저에게 알맞은 권한들을 한 번에 부여하기 위해 사용하는 것 CREATE ROLE LOGIN_TABLE; GRANT CREATE TABLE TO LOGIN_TABLE; DROP USER PJS CASCADE; CASCADE : 하위 오브젝트까지 삭제

------

- 절차형 SQL : SQL문의 연속적인 실행이나 조건에 따른 분기처리를 이용하여 특정 기능을 수행하는 저장 모듈을 생성할 수 있다, Procedure, User Defined Function, Trigger 등이 있음
- 저장 모듈 : PL/SQL DB 문장을 서버에 저장하여 사용자와 애플리케이션 사이에서 공유할 수 있도록 만든 일종의 SQL , 컴포넌트 프로그램 독립적으로 실행되거나 다른 프로그램으로부터 실행될 수 있는 완전한 실행 프로그램
- PL/SQL 특징

1. Block 구조로 되어있어 각 기능별로 모듈화 가능
2. , 변수 상수 등을 선언하여 SQL 문장 간 값을 교환
3. IF, LOOP 등의 절차형 언어를 사용하여 절차적인 프로그램이 가능하도록 한다.
4. DBMS 정의 에러나 사용자 정의 에러를 정의하여 사용할 수 있다.
5. PL/SQL Oracle 은 에 내장되어 있으므로 호환성 굳
6. 응용 프로그램의 성능을 향상시킨다.
7. Block -> 단위로 처리 통신량을 줄일 수 있다.

- DECLARE : BEGIN~END 절에서 사용될 변수와 인수에 대한 정의 및 데이터 타입 선언부
- BEGIN~END : 개발자가 처리하고자 하는 SQL문과 여러 가지 비교문 제어문을 , 이용 필요한 로직 처리
- EXCEPTION : BEGIN~END 절에서 실행되는

SQL문이 실행될 때 에러가 발생하면 그 에러를 어떻게 처리할지 정의하는 예외 처리부

- T-SQL : SQL Server 근본적으로 를 제어하는 언어 CREATE Procedure schema_NAME.Procedure_name
- Trigger : 특정한 테이블에 : INSERT, UPDATE, DELETE DML 와 같은 문이 수행되었을 때 에서 , DB 자동으로 동작하도록 작성된 프로그램 사용자 호출이 , 아닌 DB 자동 수행

< 프로시저와 트리거의 차이점 >

- 프로시저 : BEGIN~END COMMIT, 절 내에 ROLLBACK과 같은 트랜잭션 종료 명령어 사용가능. EXECUTE 명령어로 실행
- 트리거 : BEGIN~END . 절 내에 사용 불가 생성 후 자동 실행.
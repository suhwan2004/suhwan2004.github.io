---
title: "SQLD - Part2 - Chapter1 - SQL 기본 - 2"
categories:
  - SQLD
tags:
  - SQLD
---

### WHERE 절

- INSERT를 제외한 DML문을 수행할 때 원하는 데이터만 골라 수행할 수 있도록 해주는 구문이다.
- 비교 연산자

  - '=' , '<' , '<=' , '>' , '=>'

- 부정 비교 연산자
  - '!=' , '^=' , '<>' 셋 다 같지 않음이다.
  - 'not 컬럼명 =' 또한 같지 않음이다. 'not 컬럼명 >'는 크지 않음이다.
- SQL 연산자
  - BETWEEN A AND B : A와 B의 사이.
  - LIKE '비교 문자열' : 비교 문자열을 포함.
  - IN (LIST) : LIST 중 하나와 일치.
  - IS NULL : NULL 값.
- 부정 SQL 연산자
  - NOT BETWEEN A AND B : A와 B의 사이가 아닌 것.
  - NOT IN (LIST) : LIST 중 일치하는 것이 없음.
  - IS NOT NULL : NULL값이 아님 (NULL이 아닌 행 조회에 사용)
- 논리 연산자
  - AND : 모든 조건이 TRUE여야 함.
  - OR : 하나 이상의 조건이 TRUE여야 함.
  - NOT : TRUE면 FALSE, FALSE면 TRUE.

### GROUP BY, HAVING 절

- GROUP BY

  - 데이터를 그룹별로 묶을 수 있도록 해주는 절.

- 집계 함수

  - COUNT(\*) : 전체 ROW를 COUNT한 값.
  - COUNT(컬럼) : 컬럼값이 NULL인 ROW를 제외하고 COUNT 하여 반환
  - COUNT(DISTINCT 컬럼) : 컬럼값이 NULL이 아닌 ROW에서 중복을 제거한 COUNT를 반환함.
  - SUM(컬럼), AVG(컬럼), MIN(컬럼), MAX(컬럼) : 우리가 잘 아는 그것...

- HAVING

  - 데이터를 그룹핑한 후 특정 그룹을 골라낼 때 씀.
  - 보통 집계함수에 대한 조건절을 씀.

- ORDER BY

  - 보통 SELECT문에 논리적으로 맨 마지막에 수행됨.
  - 우리가 아는 sort임. 옵션으로는 ASC(오름), DESC(내림)가 있음.
  - 다중 sort는 ORDER BY 컬럼, 조건, 컬럼1, 조건1...이런 식임.
    - 조건이 ASC라면 생략 가능.

- JOIN

  - 각기 다른 테이블을 한 번에 보여줄 때 쓰는 쿼리
  - EQUI JOIN : Equal 조건으로 JOIN하는 것. 가장 흔한 방식
  - Non EQUI JOIN : Equal 조건이 아닌 다른 조건(BETWEEN, >, < ...)으로 JOIN하는 방식.
    - 테이블 간에 PK, FK 연관관계가 없어도 JOIN 가능.
    - 하나의 쿼리에 EQUI, Non EQUI 동시에 사용 가능.
  - INNER JOIN : 조건에 만족하는 교집합들만 출력됨.
  - OUTER JOIN : 조건에 만족하지 못하는 행들도 출력됨.
    - LEFT OUTER JOIN : 오른쪽과 겹치는 값, 안 겹치는 왼쪽 값 출력
    - RIGHT OUTER JOIN : 왼쪽과 겹치는 값, 안 겹치는 오른쪽 값 출력

- STANDARD JOIN(여러 DB에서 공통적으로 돌아가는 JOIN QUERY)

  - INNER JOIN

    - JOIN 조건을 ON 절을 사용하여 작성해야 함.
      - ex) SELECT A.PRODUCT_CODE, B.MEMBER_ID FROM PRODUCT A INNER JOIN PRODUCT_REVIEW B ON A.PRODUCT_CODE = B.PRODUCT_CODE;

  - OUTER JOIN

    - LEFT OUTER JOIN, RIGHT OUTER JOIN : 왼쪽(오른쪽)은 무조건 출력하는데 오른쪽(왼쪽) 테이블에 속한 Row들 중에 데이터가 없는 값이 있으면 NULL로 채워줌.

  - FULL OUTER JOIN

    - 왼, 오른쪽 테이블의 데이터가 모두 출력되는 방식 각각 하나라도 없으면 NULL이 되는 방식은 위와 동일하다.

  - Natural JOIN

    - A와 B 테이블에서 같은 이름을 가진 컬럼들이 모두 동일한 데이터를 가지고 있을 경우 JOIN이 됨.

  - CROSS JOIN (Cartesian Product)

    - A, B 테이블 사이에 JOIN 조건이 없는 경우, 조합할 수 있는 모든 경우를 출력하는 방식임.
    - SELECT BOY_NAME, GIRL_NAME FROM GIRL, BOY B;
      => SELECT BOY_NAME, GIRL_NAME FROM GIRL CROSS JOIN BOY B;

  -

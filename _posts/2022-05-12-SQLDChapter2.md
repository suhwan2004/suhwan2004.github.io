---
title: "SQLD - PART1 -Chapter2 데이터 모델과 SQL"
categories:
  - SQLD
tags:
  - SQLD
---

### 정규화(Normalization)

- 데이터 정합성(데이터의 정확성과 일관성을 유지하고 보장)을 위해 엔터티를 작은 단위로 분리하는 과정.
- 일반적으로 입력, 수정, 삭제의 성능은 향상 시켜줄 수 있음.
- 무지성 정규화는 NONO. 이 바닥에도 일정한 룰이 존재함. (정처기 때 했던 원부이결다조)

1. 제1정규형

- 모든 속성은 반드시 하나의 값만 가져야 한다.
  - 이름 : '김수환'이 속한 행에서 직업 : "학생", "프로그래머", "불타는효자" 이렇게 직업이 세 개라면 제 1정규형에 위배된다고 볼 수 있다. => 이럴 땐, 하나의 행을 3개로 분리해야 되겠죠?
  - 값만 중복되는 것 말고도, 유사한 속성이 반복되도 위배된다.
    - 예를 들어, 한 테이블에 속성이 이름, 생년월일, 사이트1, 사이트2, 사이트3이 존재한다면? 사이트라는 속성으로 하나만 존재해도 되는데 중복되니 이 것도 제1정규형이 못 된다.

2. 제2정규형

- 엔터티의 모든 일반속성은 반드시 모든 주식별자에 종속되어야 한다. (이게 뭔 말임?)
- 예를 들어, 주문번호, 음료코드, 주문수량, 음료명이라는 속성을 가진 테이블이 있다고 해보자.
- 여기서 주식별자는 주문번호, 음료코드 라고 했을 때 사실상 일반속성인 음료명은 음료코드에만 종속되지 주문번호와는 거의 별개의 속성이라 봐도 무방하다.
- 이럴 때, 제2정규형에 위배된다는 것이다. 이럴 때는 음료코드를 외부 키로 한 테이블을 하나 더 만들어야 된다
  - 그럼 대략,
    - 주문번호, 음료코드, 주문수량 테이블
    - 음료코드, 음료명 테이블이 존재하는 것이다.

3. 제3정규형

- 주식별자가 아닌 모든 속성 간에는 서로 종속될 수 없다.
- 예를 들어, 일련번호(주식별자), 이름, 생년월일, 소속사코드, 소속명이라는 속성이 존재하는 테이블이 있다고 생각해보자.
- 자세히 보면, 소속사코드, 소속사명이 서로 종속되있는 모습을 볼 수있다.
- 역시 이럴때도, 테이블을 제2에서 했던 것 처럼 분리해주면 된다...
  - 일련번호, 이름, 생년월일, 소속사코드
  - 소속사코드, 소속사명

ㅤ
ㅤ

### 주의사항

- 정규화가 성능이 좋아지는 건 맞는데 남용하면 오히려 성능 저하를 일으킬 수도 있다.
- 과도한 정규화는 NONO~

ㅤ
ㅤ

### 반정규화(De-Normalization)

- 데이터의 조회 성능을 향상시키기 위해 데이터의 중복을 허용하거나 데이터를 그루핑하는 과정이다.
- 조회성능이 향상될 수도 있지만, 반대로 입력, 수정, 삭제의 성능은 내려갈 수 있고 데이터 정합성 이슈가 발생할 수도 있다...
- 보통 반정규화는 정규화가 끝난 이후에 거치게 된다. 마찬가지로 룰도 있다!

1. 테이블 반정규화

   1. 테이블 병합 => JOIN이 많이 발생될 때 사용
      - 1:1 관계 테이블 병합
      - 1:M 관계 테이블 병합
   2. 테이블 분할
      - 테이블 수직 분할(속성 분할)
      - 테이블 수평 분할(인스턴스 분할, 파티셔닝)
   3. 테이블 추가
      - 중복 테이블 추가
      - 통계 테이블 추가
      - 이력 테이블 추가
      - 부분 테이블 추가

2. 컬럼 반정규화

- 중복 컬럼 추가 : JOIN이 많이 발생될 때 사용
- 파생 컬럼 추가 : 수행 시 부하가 염려되는 계산값을 미리 컬럼으로 추가
- 이력 테이블 컬럼 추가 : 조회 속도를 위해 조회 기준이 될 것으로 판단되는 컬럼 미리 추가.

3. 관계 반정규화(중복관계 추가)

- JOIN이 많이 발생될 때 사용. 데이터 무결성을 안 깨뜨리고 성능 향상이 가능함.
  ㅤ
  ㅤ
  ㅤ

### 트랜잭션

- 데이터를 조작하기 위한 하나의 논리적인 작업 단위이다.
  ㅤ
  ㅤ
  ㅤ

### NULL

- 존재하지 않음. 즉, 값이 없음을 의미한다.
- 0과 NULL은 다르다. 0이라는 값은 존재하나, NULL은 아무것도 입력이 안된 것이다.
- SUM을 할 때는 NULL을 빼고 계산, 가로로 범위를 잡고 값을 출력할 때는 결과값이 NULL이 되버린다...

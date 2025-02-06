---
date: 2025-01-12-sql-select
layout: post
title: SQL 'SELECT’ Problem Solving
subtitle: 'Written by Data Analysis Team'
description: >-
  Solving SQL Function SELECT Problems on Programmers.
image: 2025-01-12-sql-select/thumb.png
optimized_image: 2025-01-12-sql-select/thumb.png
category: Corporate Analysis
tags:
  - Data
  - Data Analysis
  - Problem-Solving
author: jihee
paginate: false
---

# SQL 문제 풀이 정리

## 1. 평균 일일 대여 요금 구하기

### 문제
평균 일일 대여 요금을 계산하시오.

### SQL 쿼리
```sql
SELECT ROUND(AVG(daily_fee)) AS average_fee
FROM car_rental_company_car
WHERE car_type='SUV';
```

### 출력 값

| average_fee |
|------------|
| 93727 |

### 풀이 과정
- `AVG(daily_fee)`: `daily_fee`의 평균값을 계산
- `ROUND(AVG(daily_fee))`: 계산된 평균을 반올림
- `AS average_fee`: 결과 값에 별칭 부여
- `FROM car_rental_company_car`: 데이터를 가져올 테이블 지정
- `WHERE car_type='SUV'`: `car_type`이 'SUV'인 데이터만 필터링

---

## 2. 재구매가 일어난 상품과 회원 리스트 구하기

### 문제
재구매가 일어난 상품과 회원 리스트를 조회하시오.

### SQL 쿼리
```sql
SELECT DISTINCT USER_ID, PRODUCT_ID
FROM ONLINE_SALE
GROUP BY USER_ID, PRODUCT_ID
HAVING COUNT(*) > 1
ORDER BY USER_ID ASC, PRODUCT_ID DESC;
```

### 풀이 과정
1. `SELECT DISTINCT USER_ID, PRODUCT_ID` : 중복 제거
2. `FROM ONLINE_SALE` : 데이터 조회 테이블 지정
3. `GROUP BY USER_ID, PRODUCT_ID` : `USER_ID`와 `PRODUCT_ID`별로 그룹화
4. `HAVING COUNT(*) > 1` : 2번 이상 구매한 데이터만 필터링
5. `ORDER BY USER_ID ASC, PRODUCT_ID DESC` : `USER_ID` 오름차순, `PRODUCT_ID` 내림차순 정렬

---

## 3. 과일로 만든 아이스크림 고르기

### 문제
과일로 만든 아이스크림 중 총 주문량이 3000개 이상인 제품을 조회하시오.

### SQL 쿼리
```sql
SELECT b.flavor
FROM FIRST_HALF as a
JOIN ICECREAM_INFO as b
ON a.flavor = b.flavor
WHERE a.total_order > 3000 AND b.ingredient_type = 'fruit_based'
ORDER BY a.total_order DESC;
```

### 풀이 과정
- `JOIN`을 사용하여 `FIRST_HALF` 테이블과 `ICECREAM_INFO` 테이블을 `flavor` 기준으로 결합
- `WHERE` 조건을 사용하여 총 주문량이 3000개 이상(`a.total_order > 3000`)이고, 과일 기반(`b.ingredient_type = 'fruit_based'`)인 아이스크림만 필터링
- `ORDER BY a.total_order DESC` : 총 주문량을 기준으로 내림차순 정렬

---

## 4. 서울에 위치한 식당 목록 출력하기

### 문제
서울에 위치한 식당 정보를 조회하시오.

### SQL 쿼리
```sql
SELECT b.REST_ID, a.REST_NAME, a.FOOD_TYPE, a.FAVORITES, a.ADDRESS
FROM rest_info as a
JOIN rest_review as b
ON a.rest_id = b.rest_id
HAVING address LIKE '서울%'
ORDER BY score DESC, favorites DESC;
```

### 풀이 과정
- `JOIN`을 사용하여 `rest_info`와 `rest_review` 테이블을 `rest_id` 기준으로 결합
- `HAVING address LIKE '서울%'`를 사용하여 주소가 '서울'로 시작하는 데이터만 필터링
- `ORDER BY score DESC, favorites DESC` : 리뷰 점수와 즐겨찾기 개수를 기준으로 내림차순 정렬

---

## SQL 쿼리 실행 순서 정리
1. `SELECT`
2. `FROM`
3. `JOIN`
4. `WHERE`
5. `GROUP BY`
6. `HAVING`
7. `ORDER BY`
8. `LIMIT`

---

## 5. 흉부외과 또는 일반외과 의사 목록 출력하기

### 문제
흉부외과 또는 일반외과 의사 목록을 조회하시오.

### SQL 쿼리
```sql
SELECT DR_NAME, DR_ID, MCDP_CD, DATE_FORMAT(HIRE_YMD, '%Y-%m-%d')
FROM DOCTOR
WHERE MCDP_CD IN ('CS', 'GS')
ORDER BY HIRE_YMD DESC, DR_NAME ASC;
```

### 풀이 과정
- `DATE_FORMAT(HIRE_YMD, '%Y-%m-%d')`: 날짜 형식을 `YYYY-MM-DD`로 변환
- `WHERE MCDP_CD IN ('CS', 'GS')`: 흉부외과(CS) 또는 일반외과(GS) 의사만 필터링
- `ORDER BY HIRE_YMD DESC, DR_NAME ASC`: 고용 날짜 기준 내림차순, 의사 이름 기준 오름차순 정렬

---

## 6. 조건에 부합하는 중고거래 댓글 조회하기

### 문제
2022년 10월에 작성된 중고거래 게시글에 대한 댓글을 조회하시오.

### SQL 쿼리
```sql
SELECT a.TITLE, a.BOARD_ID, b.REPLY_ID, b.WRITER_ID, b.CONTENTS, DATE_FORMAT(b.CREATED_DATE, '%Y-%m-%d') AS CREATED_DATE
FROM USED_GOODS_BOARD AS a
JOIN USED_GOODS_REPLY AS b
ON a.BOARD_ID = b.BOARD_ID
WHERE DATE_FORMAT(a.CREATED_DATE, '%Y-%m') = '2022-10'
ORDER BY b.CREATED_DATE ASC, TITLE ASC;
```

### 풀이 과정
- `JOIN`을 사용하여 `USED_GOODS_BOARD`와 `USED_GOODS_REPLY` 테이블을 `BOARD_ID` 기준으로 결합
- `WHERE DATE_FORMAT(a.CREATED_DATE, '%Y-%m') = '2022-10'`: 2022년 10월에 작성된 게시글만 필터링
- `ORDER BY b.CREATED_DATE ASC, TITLE ASC`: 댓글 작성일 기준 오름차순, 게시글 제목 기준 오름차순 정렬

---

## 7. 3월에 태어난 여성 회원 목록 출력하기

### 문제
3월에 태어난 여성 회원 목록을 조회하시오.

### SQL 쿼리
```sql
SELECT MEMBER_ID, MEMBER_NAME, GENDER, DATE_FORMAT(DATE_OF_BIRTH, '%y-%m-%d') AS DATE_OF_BIRTH
FROM MEMBER_PROFILE
WHERE MONTH(DATE_OF_BIRTH) = 3 AND GENDER = 'W' AND TLNO IS NOT NULL;
```

### 풀이 과정
- `MONTH(DATE_OF_BIRTH) = 3`: 생일이 3월인 회원 필터링
- `GENDER = 'W'`: 성별이 여성인 회원만 필터링
- `TLNO IS NOT NULL`: 전화번호가 존재하는 회원만 필터링
- `DATE_FORMAT(DATE_OF_BIRTH, '%y-%m-%d')`: 날짜를 `YY-MM-DD` 형식으로 변환

---

## 8. 대장균들의 자식 수 구하기

### 문제
대장균들의 ID별 자식 수를 조회하시오.

### SQL 쿼리
```sql
SELECT a.ID, COUNT(b.PARENT_ID) AS CHILD_COUNT
FROM ECOLI_DATA AS a
LEFT JOIN ECOLI_DATA AS b
ON a.ID = b.PARENT_ID
GROUP BY a.ID
ORDER BY a.ID ASC;
```

### 풀이 과정
- `LEFT JOIN`을 사용하여 같은 테이블 `ECOLI_DATA`를 `ID`와 `PARENT_ID` 기준으로 연결
- `COUNT(b.PARENT_ID)`: 특정 `ID`를 부모로 가진 자식 개수를 계산
- `GROUP BY a.ID`: 각 `ID`별로 그룹화
- `ORDER BY a.ID ASC`: `ID` 기준 오름차순 정렬

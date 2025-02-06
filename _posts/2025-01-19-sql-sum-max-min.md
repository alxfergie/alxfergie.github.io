---
date: 2025-01-19-sql-sum-max-min
layout: post
title: SQL ‘SUM,MAX,MIN’ Problem Solving
subtitle: 'Written by Data Analysis Team'
description: >-
  Solving SQL Function SUM, MAX, MIN Problems on Programmers
image: 2025-01-19-sql-sum-max-min/thumb.png
optimized_image: 2025-01-19-sql-sum-max-min/thumb.png
category: Corporate Analysis
tags:
  - Data
  - Data Analysis
  - Problem-Solving
author: jihee
paginate: false
---

# SQL 문제 풀이 정리

## 1. 가장 비싼 상품 구하기

### 문제
가장 비싼 상품의 가격을 조회하시오.

### SQL 쿼리
```sql
SELECT MAX(PRICE) AS MAX_PRICE
FROM PRODUCT;
```

### 풀이 과정
- `MAX(PRICE)`: `PRICE` 컬럼에서 최대값을 찾음

---

## 2. 최댓값 구하기

### 문제
가장 최근에 들어온 동물의 `DATETIME` 값을 조회하시오.

### SQL 쿼리
```sql
SELECT MAX(DATETIME)
FROM ANIMAL_INS;
```

### 풀이 과정
- `MAX(DATETIME)`: 가장 최근 날짜 데이터를 가져옴
- 반대로 가장 오래된 `DATETIME`을 찾으려면 `MIN(DATETIME)`을 사용

---

## 3. 잡은 물고기 중 가장 큰 물고기의 길이 구하기

### 문제
잡은 물고기 중 가장 긴 물고기의 길이를 조회하시오.

### SQL 쿼리
```sql
SELECT CONCAT(MAX(LENGTH), 'cm') AS MAX_LENGTH
FROM FISH_INFO;
```

### 풀이 과정
- `MAX(LENGTH)`: 가장 긴 물고기의 길이를 찾음
- `CONCAT(MAX(LENGTH), 'cm')`: 결과값에 'cm' 단위를 추가

---

## 4. 연도별 대장균 크기의 편차 구하기

### 문제
연도별 대장균 크기의 최대 편차를 계산하시오.

### SQL 쿼리
```sql
SELECT
    EXTRACT(YEAR FROM DIFFERENTIATION_DATE) AS YEAR,
    (MAX(SIZE_OF_COLONY) OVER (PARTITION BY EXTRACT(YEAR FROM DIFFERENTIATION_DATE)) - SIZE_OF_COLONY) AS YEAR_DEV,
    ID
FROM ECOLI_DATA
ORDER BY YEAR ASC, YEAR_DEV ASC;
```

### 풀이 과정
- `EXTRACT(YEAR FROM DIFFERENTIATION_DATE)`: 날짜에서 연도 추출
- `PARTITION BY EXTRACT(YEAR FROM DIFFERENTIATION_DATE)`: 같은 연도별 그룹화하여 최대값 계산
- `MAX(SIZE_OF_COLONY) - SIZE_OF_COLONY`: 최대값과의 편차 계산

---

## 5. 중복 제거하기

### 문제
중복된 이름을 제외하고 전체 개수를 조회하시오.

### SQL 쿼리
```sql
SELECT COUNT(DISTINCT(NAME))
FROM ANIMAL_INS;
```

### 풀이 과정
- `COUNT(DISTINCT(NAME))`: 중복을 제거한 후 개수를 계산
- 단순 개수를 원할 경우 `COUNT(NAME)` 사용 가능

---

## 6. 조건에 맞는 아이템들의 가격의 총합 구하기

### 문제
레전드 등급의 아이템 총 가격을 계산하시오.

### SQL 쿼리
```sql
SELECT SUM(PRICE) AS TOTAL_PRICE
FROM ITEM_INFO
WHERE RARITY = 'LEGEND';
```

### 풀이 과정
- `SUM(PRICE)`: 가격 합산
- `WHERE RARITY = 'LEGEND'`: `RARITY` 값이 'LEGEND'인 데이터만 필터링

---

## 7. 물고기 종류별 대어 찾기

### 문제
각 물고기 종류별로 가장 큰 물고기를 찾으시오.

### SQL 쿼리
```sql
SELECT
    a.ID, b.FISH_NAME, a.LENGTH
FROM FISH_INFO a
JOIN FISH_NAME_INFO b ON a.FISH_TYPE = b.FISH_TYPE
WHERE a.LENGTH = (
    SELECT MAX(LENGTH)
    FROM FISH_INFO sub_a
    WHERE sub_a.FISH_TYPE = a.FISH_TYPE
    AND sub_a.LENGTH > 10  -- 10cm 이하 물고기 제외
)
ORDER BY a.ID ASC;
```

### 풀이 과정
1. `JOIN`을 사용하여 `FISH_INFO`와 `FISH_NAME_INFO`를 `FISH_TYPE` 기준으로 연결
2. 서브쿼리를 사용하여 각 `FISH_TYPE`에서 가장 긴 물고기를 찾음
3. `AND sub_a.LENGTH > 10`: 10cm 이하 물고기 제외
4. `ORDER BY a.ID ASC`: ID 기준 오름차순 정렬
5. `WHERE sub_a.FISH_TYPE = a.FISH_TYPE`을 사용하여 메인쿼리와 서브쿼리를 연결함  
   - 서브쿼리를 연결하지 않으면 메인쿼리 (`a.FISH_TYPE`)는 모든 물고기 종류를 다 하나씩 보고 있고,  
     서브쿼리는 여러 종류 물고기 중 제일 긴 물고기를 찾아야 함  
   - 이 조건이 없으면 다른 종류의 물고기까지 섞여 특정 물고기 종류의 가장 긴 물고기를 찾을 수 없음  
   - 따라서 `WHERE sub_a.FISH_TYPE = a.FISH_TYPE` 조건을 반드시 추가해야 함

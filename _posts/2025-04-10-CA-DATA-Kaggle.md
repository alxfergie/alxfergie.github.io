---
date: 2025-04-10-CA-DATA-Kaggle
layout: post
title: Corporate Analysis, Data Team Week 4
subtitle: 'Written by CA-Data Team'
description: >-
  각자 가설을 설정하고 Excel 및 Power Query를 활용한 분석 연습 수행
image: 2025-04-10-CA-DATA-Kaggle/thumb.png
optimized_image: 2025-04-10-CA-DATA-Kaggle/thumb.png
category: Data Analysis
tags:
  - Data
  - Data Analysis
  - Problem-Solving

author: jihee
paginate: false
---

# 📌 주제 선정 및 데이터 수집 계획 수립
CA 데이터 팀은 ESG 중 환경(Environment)에 초점을 맞춰 프로젝트를 진행하고 있으며, 그 중에서도 탄소배출량 (Scope 1) 데이터를 주요 분석 대상으로 삼았습니다. 팀원 각자는 기업 10곳에 데이터 요청 메일을 보냈지만, 민감한 정보라는 이유로 대부분 거절되었고, 이에 따라 Kaggle을 통한 대체 데이터 분석 실습을 시작하게 되었습니다.

---

## 📊 Kaggle 데이터 실습: 넷플릭스 데이터 분석

✅ 공통 목표: 각자 가설을 설정하고 Excel 및 Power Query를 활용한 분석 연습 수행

### 이지희
- 가설: 일본에서 제작된 영화는 대다수가 호러 또는 스릴러 장르일 것이다.
- Excel 피벗 테이블을 활용하여 일본 콘텐츠(총 245편) 중 해당 장르를 포함하는 콘텐츠 수 분석

![]({{site.url}}/assets/img/2025-04-10-CA-DATA-Kaggle/jihee.png)

- 결과: 호러/스릴러 콘텐츠는 전체의 21편으로, 예상보다 낮은 비중을 차지
- 결론: 가설은 틀림. 실제로는 Anime Feature가 높은 비중
- 시각화 결과로도 호러 장르 비중이 낮음을 확인

![]({{site.url}}/assets/img/2025-04-10-CA-DATA-Kaggle/chart.png)

### 최온혁
- 가설: 한국 넷플릭스 콘텐츠는 단편 영화보다 시즌 기반 TV Show가 더 많다
- Power Query를 활용해 South Korea 필터링 → Group By로 유형별 콘텐츠 수 비교
- 결과: TV Show 170편, Movie 61편으로 약 2.8배 차이
- 결론: 가설은 참
- SQL 코드도 추가로 작성하여 향후 확장성 고려
![]({{site.url}}/assets/img/2025-04-10-CA-DATA-Kaggle/onhyuck.png)

```sql
SELECT type, COUNT(*) FROM netflix_titles WHERE country LIKE '%South Korea%' GROUP BY type;
```

### 정채원
- 가설: 미국의 TV Show 장르는 crime이 가장 많을 것이다
- listed_in 열 분할 → TV Show 및 United States 필터링 후 장르별 빈도 분석

![]({{site.url}}/assets/img/2025-04-10-CA-DATA-Kaggle/chaewon.png)
- 결과: 가장 많은 장르는 Kids’ TV → **가설은 틀림**

### 김은재
- 가설: 코로나 시기(2020년)에 TV Show 수가 더 많았을 것이다
- date_added에서 연도 추출 → 피벗테이블로 연도별 Movie/TV Show 비교
![]({{site.url}}/assets/img/2025-04-10-CA-DATA-Kaggle/eunjae.png)
- 결과: 2020년 Movie가 TV Show의 2배 이상 → 가설은 틀림
- 다만, 2018~2021 트렌드를 보면 코로나 시기엔 TV Show 비중이 상대적으로 증가함
![]({{site.url}}/assets/img/2025-04-10-CA-DATA-Kaggle/eunjaeTrend.png)

### 신우진
가설: 특정 국가는 특정 장르에 집중되어 있다
Power Query로 장르 정리 → 피벗 테이블과 함수 활용해 국가별 장르 분포 분석
분석 파일 첨부
![]({{site.url}}/assets/img/2025-04-10-CA-DATA-Kaggle/woojin.png)


---

## ✅ 기술 역량 강화 활동
- Power Query 학습 2일차까지 완료
- Kaggle 데이터셋을 활용한 다양한 실습을 통해 분석 감각 향상

---

## 📌 향후 계획
- Power Query 학습 3일차까지 완료 예정
- Kaggle에서 상위 난이도 분석 실습 진행
- 향후 프로젝트가 ESG가 아닌 일반 데이터 분석 프로젝트로 전환될 가능성 고려
- 이를 대비해 각자 흥미 있는 데이터 분석 주제를 사전 조사하기로 함

---
layout: post
related_posts:
  - /aiLog/aiPratice
title:  "서울시 자전거 공유 데이터셋(데이터 전처리 및 탐색적 데이터 분석)"
date:   2025-12-16
categories:
  - aiPratice
description: >
  서울시의 공공자전거 대여 서비스인 ‘따릉이’의 대여 수요를 예측하는 문제에 사용되는 데이터셋. 
  특정 시간대와 날씨, 요일, 공휴일 여부, 기온, 습도 등 다양한 데이터를 활용하여 자전거 대여 수요를 예측한다.
---
* toc
{:toc .large-only}


## 데이터셋 칼럼

- Date : 연월일
- Rented Bike count - 매 시간마다 대여한 자전거 수
- Hour - 하루 중 시간
- Temperature - 온도
- Humidity - 습도 %
- Windspeed - 풍속 m/s
- Visibility - 가시거리 m
- Dew point temperature - 이슬점 온도
- Solar radiation - 태양 복사 MJ/m2
- Rainfall - 강우량 mm
- Snowfall - 적설량 cm
- Seasons - 겨울, 봄, 여름, 가을
- Holiday - 휴일/휴일 없음
- Functional Day - 운영되지 않았던 날, 정상적으로 운영된 날

<br />

---

## 데이터 전처리 및 탐색적 데이터 분석

### 탐색적 데이터 분석(EDA, Exploratory Data Analysis)이란?

> EDA는 **통계적 요약(Summary Static)**과 **시각화 도구(Graphical Visualization)**를 사용하여 데이터를 깊이 있게 조사하는 접근 방식이다.            
> 이는 분석가가 데이터에 대한 가정을 세우거나, 모델링을 시작하기 전에 데이터를 이해하는데 초점을 맞춘다.

#### 주요 목적

- **데이터의 이해**: 데이터가 어떻게 구성되어 있는지, 각 변수가 어떤 분포를 가지고 있는지 파악한다.
- **이상치/결측치 발견**: 데이터의 잘못된 값(이상치, Outliers)이나 빠진 값(결측치, Missing Values)이 있는지 확인하여 데이터 전처리(Pre-processing)의 필요성을 판단한다.
- **변수간의 관계 파악**: 변수들 사이의 숨겨진 패턴, 추세, 또는 상관관계(Correlation)가 있는지 발견하여 분석의 방향을 설정한다.
- **가설 설정**: 데이터에 대한 초기 가설을 생성하거나, 기존 가설을 검증할 수 있는 통계적 근거를 찾는다.

<br />

---

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

bike_df = pd.read_csv('[경로]/SeoulBikeData.csv', encoding='CP949')
```

### ※ CP949

- Microsoft Windows의 한국어 문자 인코딩이다.
- ECU-KR을 확장한 형태로, 더 많은 한국어 문자(한자, 확장 문자 등)을 지원합니다.
- 주로 Windows 환경에서 저장된 한글 파일에서 사용된다.

```python
bike_df.info()
```

```txt
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 8760 entries, 0 to 8759
Data columns (total 14 columns):
 #   Column                    Non-Null Count  Dtype  
---  ------                    --------------  -----  
 0   Date                      8760 non-null   object 
 1   Rented Bike Count         8760 non-null   int64  
 2   Hour                      8760 non-null   int64  
 3   Temperature(캜)            8760 non-null   float64
 4   Humidity(%)               8760 non-null   int64  
 5   Wind speed (m/s)          8760 non-null   float64
 6   Visibility (10m)          8760 non-null   int64  
 7   Dew point temperature(캜)  8760 non-null   float64
 8   Solar Radiation (MJ/m2)   8760 non-null   float64
 9   Rainfall(mm)              8760 non-null   float64
 10  Snowfall (cm)             8760 non-null   float64
 11  Seasons                   8760 non-null   object 
 12  Holiday                   8760 non-null   object 
 13  Functioning Day           8760 non-null   object 
dtypes: float64(6), int64(4), object(4)
memory usage: 958.3+ KB
```

```python
bike_df.columns
```

```
Index(['Date', 'Rented Bike Count', 'Hour', 'Temperature(캜)', 'Humidity(%)',
       'Wind speed (m/s)', 'Visibility (10m)', 'Dew point temperature(캜)',
       'Solar Radiation (MJ/m2)', 'Rainfall(mm)', 'Snowfall (cm)', 'Seasons',
       'Holiday', 'Functioning Day'],
      dtype='object')
```

> 컬럼명의 한글이 깨져있기에 영어로 일괄적으로 수정한다.

```python
bike_df.columns = ['Date', 'Rented Bike Count', 'Hour', 'Temperature', 'Humidity',
       'Wind speed', 'Visibility', 'Dew point temperature',
       'Solar Radiation', 'Rainfall', 'Snowfall', 'Seasons',
       'Holiday', 'Functioning Day']
```

```python
sns.scatterplot(x='Temperature', y='Rented Bike Count', data=bike_df, alpha=0.3)
```

얼마만큼 데이터가 조화롭게 있냐 조합평균 
1, 2, 8, 9 
4, 6, 4, 6 => 평균적임

만약 심지의 길이가 길면 진동이 세다는 것을 뜻하고, 즉, 데이터가 들쑥날쑥하다는 것을 의미한다.


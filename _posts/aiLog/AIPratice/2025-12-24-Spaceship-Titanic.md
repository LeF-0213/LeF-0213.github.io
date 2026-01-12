---
layout: post
related_posts:
  - /aiLog/aiPratice
title:  "Spaceship Titanic 프로젝트"
date:   2025-12-24
categories:
  - aiPratice
description: >
  GBM의 한계를 XGBoost와 LightGBM으로 알아보기
---
* toc
{:toc .large-only}

## 프로젝트 목표

1. GBM (Baseline): 기본 모델을 구축하여 성능의 기준점을 잡고 모델의 한계점 확인.
2. XGBoost, LightGBM 모델을 사용하여 GBM의 한계점 확인 및 개선된 다른 모델들의 장점 확인.

## 문제 정의

- 배경: 우주선 타이타닉의 미스터리

> 2912년, 인류의 새로운 거주지를 향해 항해하던 성간 여객선 '스페이스 타이타닉(Spaceship Titanic)'이 먼지 구름 속 시공간 이상 현상과 충돌했다. 함선은 무사했으나, 승객의 약 절반이 다른 차원으로 전송(Transported)되는 초유의 사태가 발생함.

- 과제

> 손상된 우주선 컴퓨터 시스템에서 간신히 복구된 승객 기록 데이터를 활용하여, 어떤 승객이 다른 차원으로 이동(실종)되었는지를 예측하는 이진 분류(Binary Classification) 모델을 개발해야 한다.

- 과제 목적

> 구조대원들이 실종 가능성이 높은 구역과 대상을 정확히 식별하게 함으로써, 제한된 구조 자원을 효율적으로 배치하고 실종 승객 구출 확률을 극대화하기 위함.


```python
# 표준 데이터 처리 라이브러리
import pandas as pd # 데이터 분석의 필수 도구. 엑셀 같은 표 형태(DataFrame)로 데이터를 관리하며, 필터링·그룹화·통계 계산 등을 수행
import numpy as np # 대규모 행렬 연산을 빠르게 처리하며, 판다스 데이터의 내부 연산을 담당
import os # 운영체제 시스템과 소통. 파일 경로 설정이나 폴더 내 파일 목록을 확인할 때 사용
import zipfile # Kaggle 데이터셋이 .zip 형태일 때 파이썬 내에서 바로 압축을 풀기 위해 사용
import warnings # 파이썬은 버전 업데이트가 잦아 "나중에 이 기능은 사라집니다" 같은 경고가 많이 뜸. 이를 filterwarnings('ignore')로 가려주어 결과창을 깔끔하게 유지
warnings.filterwarnings('ignore') # 향후 버전 업데이트와 관련된 경고 메시지를 모두 무시

# 시각화 라이브러리
import matplotlib.pyplot as plt # 가장 기본적인 그래프 도구, 그래프의 뼈대를 그리고 세밀한 설정을 할 때 사용
import seaborn as sns # Matplotlib을 기반으로 더 예쁘고 복잡한 통계 그래프(상관계수 히트맵 등)를 한 줄의 코드로 그리게 해줌.
plt.style.use('seaborn-v0_8-darkgrid') # 배경에 격자를 넣고 세련된 색감을 적용
plt.rcParams['figure.figsize'] = (12, 6) # 그래프의 기본 크기(figsize)나 글꼴 크기(font.size)를 전역적으로 고정해준다.
plt.rcParams['font.size'] = 10

# Sklearn 기본 도구
# train_test_split: 전체 데이터를 학습용(공부)과 테스트용(시험)으로 나눕니다. 모델이 답을 외우는 '과적합'을 막는 첫 번째 단계
# cross_val_score: 데이터를 여러 번 쪼개서 평균 점수를 냅니다. 우연히 점수가 잘 나온 건지, 정말 실력이 좋은 건지 검증
from sklearn.model_selection import train_test_split, cross_val_score, KFold
from sklearn.impute import SimpleImputer # SimpleImputer: 데이터에 뚫린 구멍(결측치)을 평균값이나 중앙값으로 메워줌.
from sklearn.preprocessing import LabelEncoder # 범주형 변수를 수치형 변수로 변환하는 데 사용, LabelEncoder: 'Red', 'Blue' 같은 글자 데이터를 0, 1 같은 숫자로 바꿔 트리가 인식하게 함.
from sklearn.metrics import accuracy_score, roc_auc_score, classification_report, confusion_matrix, f1_score # 전체 데이터 중 정답을 맞힌 비율을 계산

# 부스팅 알고리즘
# GradientBoostingClassifier (GBM): 부스팅의 원조. 이전 모델의 실수를 다음 모델이 계속 고쳐나가는 정석적인 방식. 성능은 좋지만 속도가 느리다.
from sklearn.ensemble import GradientBoostingClassifier
# XGBoost: GBM의 속도 문제를 병렬 처리로 해결. 과적합 방지 기능이 강력
import xgboost as xgb
# 나무를 옆으로 키우는 게 아니라 깊게(Leaf-wise) 키워 압도적인 속도와 적은 메모리 사용량이 장점. 대용량데이터에 적합
import lightgbm as lgb

# 유틸리티
import time # 모델 학습 시작 시간과 종료 시간을 기록하여, 주로 특정 코드 블록이나 모델 학습이 얼마나 오래 걸리는지 측정하는 데 사용(데이터셋 비교를 위한 것)
from collections import defaultdict # 키(Key)가 없는 딕셔너리에 접근해도 에러를 내지 않고 기본값을 만들어줌. 전처리 과정에서 복잡한 카운팅이나 데이터 정리를 할 때 유용하다.
```

### Kaggle 데이터 다운로드 및 로드


```python
import zipfile

zip_path = '/content/drive/MyDrive/세미 프로젝트/data/spaceship-titanic.zip'

with zipfile.ZipFile(zip_path, 'r') as zip_ref:
  zip_ref.extractall('spaceship_data')
```


```python
sp_titanic = pd.read_csv('spaceship_data/train.csv')

print(f"학습 데이터 크기: {sp_titanic.shape}")
```

    학습 데이터 크기: (8693, 14)


## 탐색적 데이터 분석 (EDA, Exploratory Data Analysis)


```python
# 데이터 기본 정보 확인
sp_titanic.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 8693 entries, 0 to 8692
    Data columns (total 14 columns):
     #   Column        Non-Null Count  Dtype  
    ---  ------        --------------  -----  
     0   PassengerId   8693 non-null   object 
     1   HomePlanet    8492 non-null   object 
     2   CryoSleep     8476 non-null   object 
     3   Cabin         8494 non-null   object 
     4   Destination   8511 non-null   object 
     5   Age           8514 non-null   float64
     6   VIP           8490 non-null   object 
     7   RoomService   8512 non-null   float64
     8   FoodCourt     8510 non-null   float64
     9   ShoppingMall  8485 non-null   float64
     10  Spa           8510 non-null   float64
     11  VRDeck        8505 non-null   float64
     12  Name          8493 non-null   object 
     13  Transported   8693 non-null   bool   
    dtypes: bool(1), float64(6), object(7)
    memory usage: 891.5+ KB



```python
sp_titanic.describe()
```


  <div id="df-a1320fa1-2d73-431f-af39-86e77423e452" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Age</th>
      <th>RoomService</th>
      <th>FoodCourt</th>
      <th>ShoppingMall</th>
      <th>Spa</th>
      <th>VRDeck</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>8514.000000</td>
      <td>8512.000000</td>
      <td>8510.000000</td>
      <td>8485.000000</td>
      <td>8510.000000</td>
      <td>8505.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>28.827930</td>
      <td>224.687617</td>
      <td>458.077203</td>
      <td>173.729169</td>
      <td>311.138778</td>
      <td>304.854791</td>
    </tr>
    <tr>
      <th>std</th>
      <td>14.489021</td>
      <td>666.717663</td>
      <td>1611.489240</td>
      <td>604.696458</td>
      <td>1136.705535</td>
      <td>1145.717189</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>19.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>27.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>38.000000</td>
      <td>47.000000</td>
      <td>76.000000</td>
      <td>27.000000</td>
      <td>59.000000</td>
      <td>46.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>79.000000</td>
      <td>14327.000000</td>
      <td>29813.000000</td>
      <td>23492.000000</td>
      <td>22408.000000</td>
      <td>24133.000000</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-a1320fa1-2d73-431f-af39-86e77423e452')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-a1320fa1-2d73-431f-af39-86e77423e452 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-a1320fa1-2d73-431f-af39-86e77423e452');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


    <div id="df-de361ce0-7620-44d7-abbd-dbde3cf1917d">
      <button class="colab-df-quickchart" onclick="quickchart('df-de361ce0-7620-44d7-abbd-dbde3cf1917d')"
                title="Suggest charts"
                style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
      </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

      <script>
        async function quickchart(key) {
          const quickchartButtonEl =
            document.querySelector('#' + key + ' button');
          quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
          quickchartButtonEl.classList.add('colab-df-spinner');
          try {
            const charts = await google.colab.kernel.invokeFunction(
                'suggestCharts', [key], {});
          } catch (error) {
            console.error('Error during call to suggestCharts:', error);
          }
          quickchartButtonEl.classList.remove('colab-df-spinner');
          quickchartButtonEl.classList.add('colab-df-quickchart-complete');
        }
        (() => {
          let quickchartButtonEl =
            document.querySelector('#df-de361ce0-7620-44d7-abbd-dbde3cf1917d button');
          quickchartButtonEl.style.display =
            google.colab.kernel.accessAllowed ? 'block' : 'none';
        })();
      </script>
    </div>

    </div>
  </div>




### 데이터 구조 분석
- 총 샘플: 8,693 개
- 특성: 13개 (타겟 불포함)
- 타겟 변수: Transported (bool)
- 데이터 타입: 수치형 6개, 범주형 7개, 불린 1개
  - float64: 평균값으로 채우거나 이상치(Outlier)를 제거하는 처리가 필요
  - object: . 모델에 넣으려면 숫자로 바꾸는 인코딩(Encoding) 과정 필요

- 수치형 데이터:
> Age, RoomService, FoodCourt, ShoppingMall, Spa, VRDeck
- 범주형 데이터:
> PasseengerId, HomePlanet, CryoSleep, Cabin, Destination, VIP, Name


### 데이터셋 컬럼 설명

| 구분 | 컬럼명 | 의미 (설명) | 데이터 특성 및 예시 |
|------|--------|-------------|---------------------|
| 개인 식별 | PassengerId | 승객 고유 번호 | gggg_pp (gggg: 그룹 번호, pp: 그룹 내 순번) |
| | Name | 승객 이름 | Firstname Lastname (가족 유추 가능) |
| 탑승 정보 | HomePlanet | 출발 행성 | Earth, Europa, Mars |
|  | CryoSleep | 동면 여부 | True: 캡슐 내 수면 중 (활동 및 지출 0) |
|  | Cabin | 객실 위치 | deck/num/side (P: 좌현, S: 우현) |
|  | Destination | 목적지 행성 | TRAPPIST-1e, 55 Cancri e, PSO J318.5-22 |
|  | VIP | VIP 서비스 여부 | 특별 우대 서비스 신청 여부 (True / False) |
| 인적 사항 | Age | 승객 나이 | 0세(영유아)부터 79세까지 존재 |
| 지출 내역 | RoomService | 룸서비스 비용 | 객실 내 주문 서비스 지불 금액 |
|  | FoodCourt | 푸드코트 비용 | 식당가 이용 지불 금액 |
|  | ShoppingMall | 쇼핑몰 비용 | 상점 내 물품 구매 금액 |
|  | Spa | 스파 비용 | 편의 시설(스파, 마사지) 지불 금액 |
|  | VRDeck | 가상현실 데크 비용 | 가상현실 엔터테인먼트 시설 지불 금액 |
| 목표 변수 | Transported | [타겟 변수] 소환 여부 | 0: 소환 안 됨, 1: 다른 차원으로 소환됨 |



```python
# 결측치 분석
# GBM은 결측치를 처리하지 못하므로 결측치 패턴 파악이 필수
def missing_report(df):
  report = pd.DataFrame({
    'Column': df.columns,
    'Missing_Count': df.isnull().sum(),
    'Missing_Percent': ((df.isnull().sum()) / len(sp_titanic) * 100).round(2)
  })

  report = report[report['Missing_Count'] > 0].sort_values('Missing_Count')
  total_missing = df.isnull().sum().sum()
  print("\t\t[결측치 현황]")
  print(f"Total missing count: {total_missing}")
  return report

missing_report(sp_titanic)
```

    		[결측치 현황]
    Total missing count: 2324

  <div id="df-386d422d-6577-45e3-b270-b3155a30898b" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Column</th>
      <th>Missing_Count</th>
      <th>Missing_Percent</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Age</th>
      <td>Age</td>
      <td>179</td>
      <td>2.06</td>
    </tr>
    <tr>
      <th>RoomService</th>
      <td>RoomService</td>
      <td>181</td>
      <td>2.08</td>
    </tr>
    <tr>
      <th>Destination</th>
      <td>Destination</td>
      <td>182</td>
      <td>2.09</td>
    </tr>
    <tr>
      <th>FoodCourt</th>
      <td>FoodCourt</td>
      <td>183</td>
      <td>2.11</td>
    </tr>
    <tr>
      <th>Spa</th>
      <td>Spa</td>
      <td>183</td>
      <td>2.11</td>
    </tr>
    <tr>
      <th>VRDeck</th>
      <td>VRDeck</td>
      <td>188</td>
      <td>2.16</td>
    </tr>
    <tr>
      <th>Cabin</th>
      <td>Cabin</td>
      <td>199</td>
      <td>2.29</td>
    </tr>
    <tr>
      <th>Name</th>
      <td>Name</td>
      <td>200</td>
      <td>2.30</td>
    </tr>
    <tr>
      <th>HomePlanet</th>
      <td>HomePlanet</td>
      <td>201</td>
      <td>2.31</td>
    </tr>
    <tr>
      <th>VIP</th>
      <td>VIP</td>
      <td>203</td>
      <td>2.34</td>
    </tr>
    <tr>
      <th>ShoppingMall</th>
      <td>ShoppingMall</td>
      <td>208</td>
      <td>2.39</td>
    </tr>
    <tr>
      <th>CryoSleep</th>
      <td>CryoSleep</td>
      <td>217</td>
      <td>2.50</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-386d422d-6577-45e3-b270-b3155a30898b')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-386d422d-6577-45e3-b270-b3155a30898b button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-386d422d-6577-45e3-b270-b3155a30898b');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


    <div id="df-9e453add-5c22-42dc-889c-35b6adb15096">
      <button class="colab-df-quickchart" onclick="quickchart('df-9e453add-5c22-42dc-889c-35b6adb15096')"
                title="Suggest charts"
                style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
      </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

      <script>
        async function quickchart(key) {
          const quickchartButtonEl =
            document.querySelector('#' + key + ' button');
          quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
          quickchartButtonEl.classList.add('colab-df-spinner');
          try {
            const charts = await google.colab.kernel.invokeFunction(
                'suggestCharts', [key], {});
          } catch (error) {
            console.error('Error during call to suggestCharts:', error);
          }
          quickchartButtonEl.classList.remove('colab-df-spinner');
          quickchartButtonEl.classList.add('colab-df-quickchart-complete');
        }
        (() => {
          let quickchartButtonEl =
            document.querySelector('#df-9e453add-5c22-42dc-889c-35b6adb15096 button');
          quickchartButtonEl.style.display =
            google.colab.kernel.accessAllowed ? 'block' : 'none';
        })();
      </script>
    </div>

    </div>
  </div>





```python
# 결측치 시각화
plt.figure(figsize=(10, 6))
sns.barplot(x='Column', y='Missing_Percent', data=missing_report(sp_titanic))
plt.title('Missing Data Percentage by Column')
plt.show()
```

    		[결측치 현황]
    Total missing count: 2324



    
![png](../../../_ipynbs/2025-12-24-Spaceship-Titanic_files/2025-12-24-Spaceship-Titanic_13_1.png)
    



```python
# 타겟 변수(우리가 예측하고자 하는 정답, 정답지) 분포
# 여기서는, Transported

# '클래스 비율'은 모델 학습의 난이도를 결정한다.
# 균형 잡힌 데이터 (Balanced Data):
# 만약 True와 False 비율이 50 : 50 정도로 비슷하다면, 모델이 편향되지 않고 공정하게 학습할 수 있다.
# 불균형 데이터 (Imbalanced Data):
# 한쪽이 90% 이상이라면, 모델이 단순히 "무조건 많은 쪽으로 찍는" 나쁜 습관을 가질 수 있어 별도의 처리가 필요하다.

print("[타겟 변수 (Transported) 분포]\n")
# 데이터프레임에서 True(소환됨)가 몇 명인지, False(소환 안 됨)가 몇 명인지 각각 계산
# 데이터의 전체 규모를 파악하기 위함
print(f"규모: {sp_titanic['Transported'].value_counts()}\n")
# normalize=True 옵션은 개수가 아닌 (각 클래스의 개수) / (전체 데이터 개수)를 계산해서 **상대적 비율(0~1 사이의 값)**로 결과로 바꿔줌.
print(f"비율: {sp_titanic['Transported'].value_counts(normalize=True)}")
```

    [타겟 변수 (Transported) 분포]
    
    규모: Transported
    True     4378
    False    4315
    Name: count, dtype: int64
    
    비율: Transported
    True     0.503624
    False    0.496376
    Name: proportion, dtype: float64


클래스 비율
- True: 0.503624
- False: 0.496376

> 정답지가 50 : 50 정도로 비슷함으로 균형잡힌 데이터로 볼 수 있다.


```python
# 수치형 변수

# describe() 결과에서 가장 중요한 것:
# 평균(mean)과 중간값(50%)의 차이, 최대값(max)
# 평균과 중간값이 비슷하다면 정규분포에 가까움.
# 평균이 이상치에 영향을 받는지 여부 파악 (최댓값, 최솟값)

print("수치형 변수 기술 통계]\n")
numeric_cols = ['Age', 'RoomService', 'FoodCourt', 'ShoppingMall', 'Spa', 'VRDeck']
print(sp_titanic[numeric_cols].describe())

```

    수치형 변수 기술 통계]
    
                   Age   RoomService     FoodCourt  ShoppingMall           Spa  \
    count  8514.000000   8512.000000   8510.000000   8485.000000   8510.000000   
    mean     28.827930    224.687617    458.077203    173.729169    311.138778   
    std      14.489021    666.717663   1611.489240    604.696458   1136.705535   
    min       0.000000      0.000000      0.000000      0.000000      0.000000   
    25%      19.000000      0.000000      0.000000      0.000000      0.000000   
    50%      27.000000      0.000000      0.000000      0.000000      0.000000   
    75%      38.000000     47.000000     76.000000     27.000000     59.000000   
    max      79.000000  14327.000000  29813.000000  23492.000000  22408.000000   
    
                 VRDeck  
    count   8505.000000  
    mean     304.854791  
    std     1145.717189  
    min        0.000000  
    25%        0.000000  
    50%        0.000000  
    75%       46.000000  
    max    24133.000000  


### 수치형 데이터 분석

- 평균(mean)과 중간값(50%)의 비교
  - 평균 28.827930 와 중간값 27이 비슷하다.
  - 지출 변수는 평균은 높지만, 50% 는 0 이다.

- 지출 변수의 우편향(Skewness, 데이터 분포가 한쪽으로 얼마나 치우쳐 있는지를 나타내는 통계 지표)
  - RoomService, FoodCourt, ShoppingMall, Spa, VRDeck 컬럼에서 0% ~ 50% 지점이 모두 0이다.
  - 75% 또한 평균보다 낮다.
  - 동면 승객이거나 검소한 승객일 가능성
  - max값은 매우 크기 때문에 부자 승객이 존재한다는 것을 확인
  - 로그 변환(Log Transformation, 너무 큰 숫자는 확 줄여주고, 0에 가까운 작은 숫자들 사이의 간격은 넓혀주는 기법) 필요성₩

- 이상치(Outlier)와 최소/최대값 확인
  - Age는 0이 존재하므로 영유아 존재 확인,
  성인과의 이송될 확률 비교 필요할 수도 있다.
  - Max값이 mean값과 차이가 크므로, 일반적인 승객과 부자 승객은 완전히 다른 패턴을 띌 수 있다.
  - 숫자의 단위가 너무 크면 모델이 그 변수에만 과도하게 집착하게 되어 나머지 중요한 정보(나이, 고향 등)를 무시하는 결과가 발생함으로, 이들을 별도로 처리하거나 모델이 잘 학습하도록 스케일링(Scaling)이 필요하다.

- 결측치 재확인 및 처리 방향
  - 지출 항목은 중간값이 0 이므로, 평균보다는 0으로 채우는 것이 훨씬 합리적일 수 있다.


## 데이터 전처리 1단계: 파생 변수 및 특성 추출

1. Cabin 정보를 분해하여 공간적 특성 추출
  - 우주선의 위치가 생존에 영향을 줄 수 있기 때문.
2. PassengerId에서 가족/그룹 정보 추출
  - 가족 단위 생존률이 다를 수 있음.
3. 지출 패턴 분석으로 승객 행동 특성 파악
  - 동면 승객은 지출이 0일 확률이 높음, 지출에 따른 승객 동선 파악 용이
4. 도메인 지식(우리가 알고있는 지식이나 상식) 기반 파생 변수 생성
  - Age 그룹화: 연령대별로 생존률이 다를 수 있음
  - VIP와 지출의 상호작용: VIP 승객의 지출 패턴이 일반 승객과 다름
  - CryoSleep 와 다른 변수의 일관성 체크:
  동면중인데 지출이 있다면 데이터 오류 가능성이 있음.

### 결측치가 아닌 파생 변수 및 특성 추출을 먼저하는 이유:

1. 유츄할 수 있는 기회를 놓치기 때문 (데이터의 연쇄성)

  > 데이터들은 서로 연결되어 있어, 하나를 미리 채워버리면 그 연결고리가 끊김.

  - 시나리오:
    > 승객 '김사과'의 Cabin 정보가 비어있습니다(NaN). 하지만 PassengerId를 보니 0123_01이고, 같은 그룹인 0123_02 '김망고'의 Cabin은 B/5/P입니다.

  - 만약 먼저 채운다면?
      > '김사과'의 Cabin을 빈칸이라고 해서 미리 Unknown으로 채워버리면, 나중에 컴퓨터는 김사과 -> Unknown 구역

  - 특성 추출을 먼저 하면?:
    > 일단 Cabin을 쪼개놓고 나중에 가족 관계를 보니, 같은 그룹(0123) -> 그럼 사과도 망고랑 같은 B/5/P 라고 정확한 값을 추리해서 채울 기회가 생긴다.

2. "결측치(빈칸) 자체가 정보"이기 때문
  > 빈칸이라는 사실이 사고 당시의 긴박한 상황을 말해준다.

  - 시나리오:
    > 우주선의 특정 구역(예: 식당가)에 차원 균열이 급습해서 그곳에 있던 사람들의 명단이나 객실 기록 장치가 통째로 파손되었다고 가정.

  - 만약 먼저 채운다면?:
    > Cabin이 비어있는 사람들을 모두 '최빈값(가장 많은 층)'인 F층으로 채워버리면, 모델은 이 사람들이 사고가 난 특수 구역에 있었다는 사실을 영영 모르게 됨.

  - 특성 추출을 먼저 하면?:
    > Cabin을 쪼갰을 때 NaN으로 남겨두면, 모델은 학습 과정에서 이송된 사람들은 유독 객실 정보가 비어있는 경우가 많음 -> 정보가 비어있다는 건 사고 중심지에 있었다는 뜻"이라고 결측치 자체를 하나의 강력한 단서로 활용할 수 있다.
  
3. 정보 손실 방지

  > 함부로 값을 채우면 데이터의 성격이 변질되어 버린다.

  - 시나리오:
    > Age가 비어있는 승객이 있고, 이 승객은 VIP 서비스도 이용했고 Spa에서 5,000달러를 썼다고 가정한다.

  - 만약 먼저 채운다면?:
    > Age 빈칸을 전체 평균인 28세로 채우고 나서 AgeGroup 파생 변수를 만든다. 그럼 이 승객은 그냥 평범한 '20대 성인' 그룹에 들어간다.

  - 특성 추출을 먼저 하면?:
    > 일단 Age를 비워둔 채로 AgeGroup을 만든다. 나중에 결측치를 채울 때, '이 사람은 VIP고 돈을 엄청 썼군 -> 그럼 최소한 어린이나 청소년은 아님'이라고 판단하여 훨씬 데이터의 맥락에 맞는 '부유한 중장년층'으로 분류할 수 있다.


```python
def advanced_feature(df):

  df = df.copy()

  # 1. Cabin 분해
  # Cabin 형태: 'deck/num/side' (예: B/0/P)
  # deck: 우주선의 층
  # num: 방 번호
  # side: Port(P, 좌측), Starboard(S, 우측)
  df['Cabin_deck'] = df['Cabin'].str.split('/').str[0]
  df['Cabin_num'] = df['Cabin'].str.split('/').str[1]
  df['Cabin_side'] = df['Cabin'].str.split('/').str[2]

  df['Cabin_deck'] = df['Cabin_deck'].fillna('Unknown')
  # 데이터 성질을 숫자로 바꾸고, 에러를 내는 대신 Nan으로 강제 변환 (문자가 아닌 숫자로 처리하기 위함)
  df['Cabin_num'] = pd.to_numeric(df['Cabin_num'], errors='coerce')
  df['Cabin_side'] = df['Cabin_side'].fillna('Unknown')

  # 2. PassengerId 분해
  # PassengerId 형태: "gggg_pp" (예: 0001_01)
  # gggg: 그룹 ID (같은 그룹 = 가족/일행)
  # pp: 그룹 내 개인 번호
  df['Group'] = df['PassengerId'].apply(lambda x: x.split('_')[0])
  df['GroupSize'] = df.groupby('Group')['Group'].transform('count')
  df['IsAlone'] = (df['GroupSize'] == 1).astype(int)

  # GroupSize 기반 Cabin 추론
  # 얼마나 채웠는지 보기 위한 것
  filled_deck = 0
  filled_num = 0
  filled_side = 0

  for group in df[df['IsAlone'] == 0]['Group'].unique():
    mask = df['Group'] == group
    group_data = df[mask]

    # Cabin_deck 추론 (최빈값)
    valid_decks = group_data[group_data['Cabin_deck'] != 'Unknown']['Cabin_deck']
    if len(valid_decks) > 0:
      # 같은 그룹 중에서 같은 가장 많은 Cabin_deck 수
      # mode(): 가장 자주 등장하는 값(최빈값)을 찾아주는 함수
      mode_deck = valid_decks.mode()
      if len(mode_deck) > 0:
        # 같은 그룹이면서 Cabin_deck이 결측치인 사람
        fill_mask = mask & (df['Cabin_deck'] == 'Unknown')
        # 결측치인 사람을 그룹 최빈값으로 덮어씀
        df.loc[fill_mask, 'Cabin_deck'] = mode_deck[0]
        # 몇 명 채웠는지 확인 하기 위한 것
        filled_deck += fill_mask.sum()

    # Cabin_num 추론 (평균값)
    # 보통 일행은 방을 나란히 배정받는 경우가 많음
    valid_nums = group_data[group_data['Cabin_num'].notna()]['Cabin_num']
    if len(valid_nums) > 0:
      mean_num = valid_nums.mean()
      fill_mask = mask & df['Cabin_num'].isna()
      # fill_mask인 행의 Cabin_num 값을 평균값으로 바꾸기
      df.loc[fill_mask, 'Cabin_num'] = mean_num
      filled_num += fill_mask.sum()

    # Cabin_side 추론 (최빈값)
    valid_sides = group_data[group_data['Cabin_side'] != 'Unknown']['Cabin_side']
    if len(valid_sides) > 0:
      mode_side = valid_sides.mode()
      if len(mode_side) > 0:
        fill_mask = mask & (df['Cabin_side'] == 'Unknown')
        df.loc[fill_mask, 'Cabin_side'] = mode_side[0]
        filled_side += fill_mask.sum()

  print(f"Cabin_deck 채우기: {filled_deck}명")
  print(f"Cabin_num 채우기: {filled_num}명")
  print(f"Cabin_side 채우기: {filled_side}명")

  # 3. 지출 패턴 분석
  # CryoSleep = True이면 모든 지출을 0으로 설정
  exp_cols = ['RoomService', 'FoodCourt', 'ShoppingMall', 'Spa', 'VRDeck']

  # CryoSleep = True이면, 지출이 0일 가능성
  df.loc[df['CryoSleep'] == True, exp_cols] = 0
  # 지출이 0이면, CryoSleep = True일 가능성
  total_spent = df[exp_cols].sum(axis=1)
  df.loc[(total_spent == 0) & df['CryoSleep'].isna(), 'CryoSleep'] = True

  # 총 지출
  df['TotalSpent'] = df[exp_cols].sum(axis=1)  # 논리적으로 일관된 값 유지
  # 지출 여부
  df['HasSpent'] = (df['TotalSpent'] > 0).astype(int)
  # 지출 다양성
  df['SpentDiversity'] = (df[exp_cols] > 0).sum(axis=1)
  # 평균 지출 (0 제외)
  df['AvgSpent'] = df[exp_cols].replace(0, np.nan).mean(axis=1).fillna(0)
  # 최대 지출 항목
  df['MaxSpentCategory'] = df[exp_cols].idxmax(axis=1)
  # 지출 집중도
  df['SpentConcentration'] = df[exp_cols].max(axis=1) / (df['TotalSpent'] + 1) # 나누기 0 방지
  # VIP와 지출 상호작용
  df['VIP_Spent'] = df['VIP'].fillna(False).astype(int) * df['TotalSpent'] # 공식 VIP
  # 1인당 지출 (그룹 고려)
  df['SpentPerPerson'] = df['TotalSpent'] / df['GroupSize']

  # 5. CryoSleep와 다른 변수의 일관성 체크
  # CryoSleep = True인데 지출이 있다면 데이터 오류 가능성
  df['CryoSleep_Inconsistent'] = ((df['CryoSleep'] == True) & (df['TotalSpent'] > 0)).astype(int)
  # VIP인데 동면중인 사람 (희귀할 가능성이 높음)
  df['VIP_In_CryoSleep'] = ((df['VIP'] == True) & (df['CryoSleep'] == True)).astype(int)

  return df
```

### 데이터 전처리: MNAR 대응

MNAR이란?
- 비무작위 결측으로, 값이 특정 범위 일 때만 결측되어, 결측 여부 자체가 예측값과 관련이 있는 상태를 말한다.

1. 방 번호가 결측인 경우
  - 승객이 기록 시스템이 파괴된 가장 위험한 구역에 있을 확률이 높음
2. VIP 여부가 누락된 승객인 경우
  - VIP 여부가 누락된 승객은 정식 승객 명단에 없던 밀항자이거나, 특별 관리가 필요 없는 일반 승객일 가능성
3. CryoSleep가 결측인 경우
  - 동면 여부가 기록되지 않았다면, 사고 당시 캡슐 시스템에 일시적인 오류가 있었던 구역일 가능성


```python
# 결측 여부 저장
sp_titanic['CryoSleep_was_missing'] = sp_titanic['CryoSleep'].isna().astype(int)
sp_titanic['Cabin_was_missing'] = sp_titanic['Cabin'].isna().astype(int)
sp_titanic['Age_was_missing'] = sp_titanic['Age'].isna().astype(int)
sp_titanic['VIP_was_missing'] = sp_titanic['VIP'].isna().astype(int)
```


```python
missing_cols = ['CryoSleep_was_missing', 'Cabin_was_missing', 'Age_was_missing', 'VIP_was_missing']
print(sp_titanic[missing_cols].sum())
print("총합: ", sp_titanic[missing_cols].sum().sum())
```

    CryoSleep_was_missing    217
    Cabin_was_missing        199
    Age_was_missing          179
    VIP_was_missing          203
    dtype: int64
    총합:  798



```python
# 데이터 전처리: 특성 공학 + MNAR 대응 적용
sp_titanic = advanced_feature(sp_titanic)
```

    Cabin_deck 채우기: 100명
    Cabin_num 채우기: 100명
    Cabin_side 채우기: 100명



```python
# 남은 결측치 확인
missing_report(sp_titanic)
```

    		[결측치 현황]
    Total missing count: 1964






  <div id="df-74a0b12a-0196-4669-bc86-afd43259f197" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Column</th>
      <th>Missing_Count</th>
      <th>Missing_Percent</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Cabin_num</th>
      <td>Cabin_num</td>
      <td>99</td>
      <td>1.14</td>
    </tr>
    <tr>
      <th>ShoppingMall</th>
      <td>ShoppingMall</td>
      <td>112</td>
      <td>1.29</td>
    </tr>
    <tr>
      <th>RoomService</th>
      <td>RoomService</td>
      <td>113</td>
      <td>1.30</td>
    </tr>
    <tr>
      <th>FoodCourt</th>
      <td>FoodCourt</td>
      <td>113</td>
      <td>1.30</td>
    </tr>
    <tr>
      <th>Spa</th>
      <td>Spa</td>
      <td>118</td>
      <td>1.36</td>
    </tr>
    <tr>
      <th>CryoSleep</th>
      <td>CryoSleep</td>
      <td>119</td>
      <td>1.37</td>
    </tr>
    <tr>
      <th>VRDeck</th>
      <td>VRDeck</td>
      <td>126</td>
      <td>1.45</td>
    </tr>
    <tr>
      <th>Age</th>
      <td>Age</td>
      <td>179</td>
      <td>2.06</td>
    </tr>
    <tr>
      <th>Destination</th>
      <td>Destination</td>
      <td>182</td>
      <td>2.09</td>
    </tr>
    <tr>
      <th>Cabin</th>
      <td>Cabin</td>
      <td>199</td>
      <td>2.29</td>
    </tr>
    <tr>
      <th>Name</th>
      <td>Name</td>
      <td>200</td>
      <td>2.30</td>
    </tr>
    <tr>
      <th>HomePlanet</th>
      <td>HomePlanet</td>
      <td>201</td>
      <td>2.31</td>
    </tr>
    <tr>
      <th>VIP</th>
      <td>VIP</td>
      <td>203</td>
      <td>2.34</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-74a0b12a-0196-4669-bc86-afd43259f197')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-74a0b12a-0196-4669-bc86-afd43259f197 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-74a0b12a-0196-4669-bc86-afd43259f197');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


    <div id="df-9c24a289-0bfa-49c9-a9c2-bd8788296dcb">
      <button class="colab-df-quickchart" onclick="quickchart('df-9c24a289-0bfa-49c9-a9c2-bd8788296dcb')"
                title="Suggest charts"
                style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
      </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

      <script>
        async function quickchart(key) {
          const quickchartButtonEl =
            document.querySelector('#' + key + ' button');
          quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
          quickchartButtonEl.classList.add('colab-df-spinner');
          try {
            const charts = await google.colab.kernel.invokeFunction(
                'suggestCharts', [key], {});
          } catch (error) {
            console.error('Error during call to suggestCharts:', error);
          }
          quickchartButtonEl.classList.remove('colab-df-spinner');
          quickchartButtonEl.classList.add('colab-df-quickchart-complete');
        }
        (() => {
          let quickchartButtonEl =
            document.querySelector('#df-9c24a289-0bfa-49c9-a9c2-bd8788296dcb button');
          quickchartButtonEl.style.display =
            google.colab.kernel.accessAllowed ? 'block' : 'none';
        })();
      </script>
    </div>

    </div>
  </div>





```python
# 타겟 변수 분리 및 데이터 전처리
X = sp_titanic.drop(['PassengerId', 'Age', 'Cabin', 'Group', 'Name', 'Transported'], axis=1, errors='ignore')
y = sp_titanic['Transported'].astype(int)
X.shape
y.shape # 파이썬에서 요소가 하나뿐인 튜플 뒤에 쉼표를 붙여 표현
```




    (8693,)



### 결측치 처리


```python
# 범주형 변수
X = X.fillna({col: 'Unknown' for col in X.select_dtypes(include='object').columns})
# Boolean 변수
X = X.fillna({col: False for col in X.select_dtypes(include='bool').columns})
# 수치형 변수
# 지출변수는 50%까지는 대부분 0인 경우가 많음
exp_cols = ['RoomService', 'FoodCourt', 'ShoppingMall', 'Spa', 'VRDeck']
X[exp_cols] = X[exp_cols].fillna(0)
# 같은 가족의 Cabin 정보로 추론하기 위해 Cabin을 분해했었음
# 그룹기반 추론을 했지만 여전히 결측, 혼자 여행이거나 그룹 전체가 정보가 없음
X['Cabin_num'] = X['Cabin_num'].fillna(-1)
```


```python
# 남은 결측치 확인
missing_report(X)
```

    		[결측치 현황]
    Total missing count: 0






  <div id="df-03869602-7cfe-4508-9e63-92bc87309905" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Column</th>
      <th>Missing_Count</th>
      <th>Missing_Percent</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-03869602-7cfe-4508-9e63-92bc87309905')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-03869602-7cfe-4508-9e63-92bc87309905 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-03869602-7cfe-4508-9e63-92bc87309905');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


    </div>
  </div>





```python
X.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 8693 entries, 0 to 8692
    Data columns (total 28 columns):
     #   Column                  Non-Null Count  Dtype  
    ---  ------                  --------------  -----  
     0   HomePlanet              8693 non-null   object 
     1   CryoSleep               8693 non-null   object 
     2   Destination             8693 non-null   object 
     3   VIP                     8693 non-null   object 
     4   RoomService             8693 non-null   float64
     5   FoodCourt               8693 non-null   float64
     6   ShoppingMall            8693 non-null   float64
     7   Spa                     8693 non-null   float64
     8   VRDeck                  8693 non-null   float64
     9   CryoSleep_was_missing   8693 non-null   int64  
     10  Cabin_was_missing       8693 non-null   int64  
     11  Age_was_missing         8693 non-null   int64  
     12  VIP_was_missing         8693 non-null   int64  
     13  Cabin_deck              8693 non-null   object 
     14  Cabin_num               8693 non-null   float64
     15  Cabin_side              8693 non-null   object 
     16  GroupSize               8693 non-null   int64  
     17  IsAlone                 8693 non-null   int64  
     18  TotalSpent              8693 non-null   float64
     19  HasSpent                8693 non-null   int64  
     20  SpentDiversity          8693 non-null   int64  
     21  AvgSpent                8693 non-null   float64
     22  MaxSpentCategory        8693 non-null   object 
     23  SpentConcentration      8693 non-null   float64
     24  VIP_Spent               8693 non-null   float64
     25  SpentPerPerson          8693 non-null   float64
     26  CryoSleep_Inconsistent  8693 non-null   int64  
     27  VIP_In_CryoSleep        8693 non-null   int64  
    dtypes: float64(11), int64(10), object(7)
    memory usage: 1.9+ MB



```python
# object 타입의 고유값 개수 확인
for c in X.select_dtypes(include="object").columns:
    print(f"{c} : {X[c].nunique()}")
```

    HomePlanet : 4
    CryoSleep : 3
    Destination : 4
    VIP : 3
    Cabin_deck : 9
    Cabin_side : 3
    MaxSpentCategory : 5



```python
X['VIP'].value_counts()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>count</th>
    </tr>
    <tr>
      <th>VIP</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>False</th>
      <td>8291</td>
    </tr>
    <tr>
      <th>Unknown</th>
      <td>203</td>
    </tr>
    <tr>
      <th>True</th>
      <td>199</td>
    </tr>
  </tbody>
</table>
</div><br><label><b>dtype:</b> int64</label>




```python
# 지출액이 상위 10% 안에 드는 사람만 VIP=True로, 나머지는 False로 채우기
spent_threshold = X['TotalSpent'].quantile(0.9)
X.loc[(X['VIP'] == "Unknown") & (X['TotalSpent'] >= spent_threshold), 'VIP'] = True
X.loc[X['VIP'] == "Unknown", 'VIP'] = False
```


```python
X['VIP'].value_counts()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>count</th>
    </tr>
    <tr>
      <th>VIP</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>False</th>
      <td>8479</td>
    </tr>
    <tr>
      <th>True</th>
      <td>214</td>
    </tr>
  </tbody>
</table>
</div><br><label><b>dtype:</b> int64</label>




```python
X['VIP'] = X['VIP'].astype(int)
```


```python
sns.barplot(x=X['CryoSleep'], y=y)
```




    <Axes: xlabel='CryoSleep', ylabel='Transported'>




    
![png](../../../_ipynbs/2025-12-24-Spaceship-Titanic_files/2025-12-24-Spaceship-Titanic_36_1.png)
    



```python
sns.barplot(x=X['VIP'], y=y)
```




    <Axes: xlabel='VIP', ylabel='Transported'>




    
![png](../../../_ipynbs/2025-12-24-Spaceship-Titanic_files/2025-12-24-Spaceship-Titanic_37_1.png)
    



```python
sns.barplot(x=X['IsAlone'], y=y)
```




    <Axes: xlabel='IsAlone', ylabel='Transported'>




    
![png](../../../_ipynbs/2025-12-24-Spaceship-Titanic_files/2025-12-24-Spaceship-Titanic_38_1.png)
    


## 범주형 변수 인코딩
**One-Hot이 좋을까? Label이 좋을까?**

---
- Label Encoding
    - 각 범주를 0, 1, 2, ... 정수로 변환
  
    - 예시: HomePlanet
      ```
      Earth  → 0
      Europa → 1
      Mars   → 2
      ```
    
  > 메모리 효율적 (컬럼 수 증가 없음), 트리 모델에 최적 (순서 무관하게 잘 작동), 빠른 학습 속도 등의 장점이 있으나, 정수로 변환하는 과정에서 순서 관계를 값의 크기로 잘못 학습할 수 있다는 단점이 존재한다.
  
- One-Hot Encoding
  - 각 범주를 별도의 이진(0/1) 컬럼으로 변환
  
  - 예시: HomePlanet (3개 범주)
    ```
    Earth  → [1, 0, 0]
    Europa → [0, 1, 0]
    Mars   → [0, 0, 1]
    ```
    
  > 선형 모델에 최적 (범주 간 순서 관계 없음), 신경망(Deep Learning)에 적합, 수학적으로 정확한 표현을 할 수 있지만, 특성 수가 많아지고, 메모리 사용량 증가, 트리 모델에서 비효율적 (학습 느려짐)이라는 단점이 존재한다.


```python
# 범주형 컬럼
object_cols = X.select_dtypes(include='object').columns
```


```python
# 라벨 인코딩
X_label = X.copy()

label_encoders = {}
for col in object_cols:
  le = LabelEncoder()
  # 혹시 섞여 있을지 모를 결측치(NaN)나 숫자형 데이터를 모두 문자열로 통일
  # fit_transform:
  # fit: 데이터에 어떤 글자들이 있는지 학습
  # transform: 학습한 내용을 바탕으로 글자를 숫자로 변환
  X_label[col] = le.fit_transform(X_label[col].astype(str))
  label_encoders[col] = le

print(f"최종 특성 개수: {X_label.shape[1]}개")
# .memory_usage(deep=True): 각 컬럼별 메모리 사용량을 바이트(Byte) 단위로 가져옴.
# .sum() / 1024**2 바이트 단위를 메가바이트(MB) 단위로 변환
print(f"메모리 사용: {X_label.memory_usage(deep=True).sum() / 1024**2: .2f} MB")
```

    최종 특성 개수: 28개
    메모리 사용:  1.86 MB



```python
# One-Hot 인코딩
X_onehot = X.copy()
# 다중공선성 방지
X_onehot = pd.get_dummies(X_onehot, columns=object_cols, drop_first=True)

print(f'최종 특성 개수: {X_onehot.shape[1]}개')
print(f'메모리 사용: {X_onehot.memory_usage(deep=True).sum() / 1024**2:.2f} MB')
print(f'증가한 칼럼: {X_onehot.shape[1] - X_label.shape[1]}개')
```

    최종 특성 개수: 44개
    메모리 사용: 1.64 MB
    증가한 칼럼: 16개


### 교차검증 (Cross Validation)을 사용한 이유

- 가장 큰 이유는 특정적인 패턴을 암기하는 것을 막아, 각 부분을 다양한 특성 컬럼들을 번갈아 검증으로 사용해 과적합을 방지하기 위해서이다.

<img src='https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FcSllTG%2FbtsLIeMnXeU%2FAAAAAAAAAAAAAAAAAAAAAGTwEF3Oz69DEd4KRo2DahZtQJFD107Ml33aBZhqapiB%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1767193199%26allow_ip%3D%26allow_referer%3D%26signature%3DTxN3yyj7Ktkt0jkxDiLArQWmgmU%253D' />

----

### predict_proba(), ROC AUC Score 사용 이유

- predict_proba(): 0.0 ~ 1.0 사이의 명확한 확률값을 통해 예측의 신뢰도를 파악하기 위해서 사용.

- ROC AUC Score: 이진 분류 모델의 성능을 평가하는 지표. 모델의 분류 성능이 얼마나 좋은지 비교하기 확실히 비교하기 위해 사용. 값이 1에 가까울수록 완벽한 분류 성능을 의미하고, 0.5에 가까울수록 찍는 것에 가깝다.


```python
def cross_validation_model(model, model_name, X, y, n_splits=5):
  print(model_name)
  kf = KFold(n_splits=n_splits, shuffle=True, random_state=2026)

  acc_list, f1_list, roc_list = [], [], []
  all_y_test, all_y_proba = [], []

  total_train_time = 0

  # 교차 검증
  for fold, (train_idx, test_idx) in enumerate(kf.split(X), 1):
    # iloc: 판다스의 인덱서로, "몇 번째 행"인지 숫자 위치를 기준으로 데이터를 가져온다.
    X_train, X_test = X.iloc[train_idx], X.iloc[test_idx]
    y_train, y_test = y.iloc[train_idx], y.iloc[test_idx]

    # 모델 학습
    # fit(): 머신러닝에서 모델을 훈련시키는 데 사용되는 가장 기본적인 명령어
    start_time = time.time()
    model.fit(X_train, y_train)
    end_time = time.time()

    total_train_time += (end_time - start_time)

    # 확률값 예측 (Transported=True 확률, 즉, 실종될 확률)
    y_proba = model.predict_proba(X_test)[:, 1]

    # 기본 임계값(0.5)로 예측
    y_pred_default = (y_proba >= 0.5).astype(int)

    acc_list.append(accuracy_score(y_test, y_pred_default))
    f1_list.append(f1_score(y_test, y_pred_default))
    roc_list.append(roc_auc_score(y_test, y_pred_default))

    all_y_test.extend(y_test)
    all_y_proba.extend(y_proba)
    print(f"Fold {fold}: ACC={acc_list[-1]:.4f}, F1={f1_list[-1]:.4f}, Time{end_time - start_time:.2f}s")

  avg_train_time = total_train_time / n_splits
  print(f"평균 학습 시간: {avg_train_time:.2f}s")

  # 임계값 최적화 로직
  thresholds = np.arange(0.3, 0.7, 0.01)
  all_y_test_arr = np.array(all_y_test)
  all_y_proba_arr = np.array(all_y_proba)

  scores = []
  best_t = 0.5
  best_avg = 0
  for t in thresholds:
    y_pred = (all_y_proba_arr >= t).astype(int)
    acc = accuracy_score(all_y_test_arr, y_pred)
    f1 = f1_score(all_y_test_arr, y_pred)
    roc = roc_auc_score(all_y_test_arr, y_pred)
    avg = (acc + f1 + roc) / 3
    scores.append(avg)

    if avg > best_avg:
      best_avg = avg
      best_t = t

  final_y_pred = (all_y_proba_arr >= best_t).astype(int)

  print(classification_report(all_y_test_arr, final_y_pred))

  return {
      'model_name': model_name,
      'acc_mean': np.mean(acc_list),
      'acc_std': np.std(acc_list),
      'f1_mean': np.mean(f1_list),
      'f1_std': np.std(f1_list),
      'roc_mean': np.mean(roc_list),
      'roc_std': np.std(roc_list),
      'y_test': all_y_test_arr,
      'y_pred': final_y_pred,
      'y_proba': all_y_proba_arr,
      'best_threshold': best_t,
      'train_time_avg': avg_train_time
  }
```

> 만약 매 Fold마다 임계값을 새로 찾으면, 5개의 서로 다른 임계값이 나온다. 그러면 최종적으로 어떤 임계값을 써야 할지 결정하기 어렵고, 모델이 너무 복잡해지기 때문에, 예측 확률을 다 모은 뒤 단 하나의 최적 임계값을 찾는 게 낫겠다 판단.


```python
models = {
    'GBM': GradientBoostingClassifier(
        n_estimators=1000,
        learning_rate=0.03,
        max_depth=2, # 특성이 복잡하지 않기에, 과적합 방지
        subsample=0.8, # 랜덤 샘플링, 과적합 방지
        random_state=2026
    ),
    'XGBoost': xgb.XGBClassifier(
        n_estimators=1000,
        learning_rate=0.03,
        max_depth=2,
        subsample=0.8,
        random_state=2026,
        eval_metric='logloss' # 조기 종료, 정답을 잘 맞추는지 아닌지 판단해서 잘 못 맞추면 조기 종료
    ),
    'LightGBM': lgb.LGBMClassifier(
        n_estimators=1000,
        learning_rate=0.03,
        max_depth=2,
        subsample=0.8,
        random_state=2026,
        verbose=-1 # LightGBM은 경고 메시지가 많아서 출력결과만 보고싶기 때문에 사용
    )
}
```


```python
# 모든 모델 학습 및 평가
results = {}
for name, model in models.items():
    results[name] = cross_validation_model(model, name, X_onehot, y)
```

    GBM
    Fold 1: ACC=0.8091, F1=0.8184, Time15.65s
    Fold 2: ACC=0.8033, F1=0.8068, Time21.58s
    Fold 3: ACC=0.7987, F1=0.8108, Time17.67s
    Fold 4: ACC=0.7992, F1=0.8064, Time15.82s
    Fold 5: ACC=0.8032, F1=0.8032, Time15.24s
    평균 학습 시간: 17.19s
                  precision    recall  f1-score   support
    
               0       0.85      0.73      0.78      4315
               1       0.76      0.88      0.82      4378
    
        accuracy                           0.80      8693
       macro avg       0.81      0.80      0.80      8693
    weighted avg       0.81      0.80      0.80      8693
    
    XGBoost
    Fold 1: ACC=0.8045, F1=0.8138, Time1.04s
    Fold 2: ACC=0.8062, F1=0.8095, Time1.02s
    Fold 3: ACC=0.8022, F1=0.8145, Time0.99s
    Fold 4: ACC=0.7963, F1=0.8040, Time1.01s
    Fold 5: ACC=0.8113, F1=0.8115, Time0.99s
    평균 학습 시간: 1.01s
                  precision    recall  f1-score   support
    
               0       0.82      0.77      0.80      4315
               1       0.79      0.83      0.81      4378
    
        accuracy                           0.80      8693
       macro avg       0.81      0.80      0.80      8693
    weighted avg       0.80      0.80      0.80      8693
    
    LightGBM
    Fold 1: ACC=0.8005, F1=0.8107, Time0.65s
    Fold 2: ACC=0.7959, F1=0.8000, Time0.66s
    Fold 3: ACC=0.7953, F1=0.8094, Time0.66s
    Fold 4: ACC=0.7969, F1=0.8049, Time0.66s
    Fold 5: ACC=0.8055, F1=0.8071, Time0.98s
    평균 학습 시간: 0.72s
                  precision    recall  f1-score   support
    
               0       0.86      0.71      0.78      4315
               1       0.76      0.89      0.82      4378
    
        accuracy                           0.80      8693
       macro avg       0.81      0.80      0.80      8693
    weighted avg       0.81      0.80      0.80      8693
    



```python
for name, model in models.items():
    cross_validation_model(model, name, X_label, y)
```

    GBM
    Fold 1: ACC=0.8045, F1=0.8140, Time18.29s
    Fold 2: ACC=0.7970, F1=0.8018, Time16.83s
    Fold 3: ACC=0.8005, F1=0.8125, Time14.41s
    Fold 4: ACC=0.7923, F1=0.8013, Time14.52s
    Fold 5: ACC=0.8015, F1=0.8007, Time14.38s
    평균 학습 시간: 15.69s
                  precision    recall  f1-score   support
    
               0       0.84      0.73      0.78      4315
               1       0.77      0.87      0.81      4378
    
        accuracy                           0.80      8693
       macro avg       0.80      0.80      0.80      8693
    weighted avg       0.80      0.80      0.80      8693
    
    XGBoost
    Fold 1: ACC=0.8074, F1=0.8168, Time0.81s
    Fold 2: ACC=0.7953, F1=0.7991, Time0.85s
    Fold 3: ACC=0.7976, F1=0.8103, Time0.87s
    Fold 4: ACC=0.7946, F1=0.8040, Time0.86s
    Fold 5: ACC=0.8044, F1=0.8048, Time3.36s
    평균 학습 시간: 1.35s
                  precision    recall  f1-score   support
    
               0       0.83      0.75      0.79      4315
               1       0.77      0.85      0.81      4378
    
        accuracy                           0.80      8693
       macro avg       0.80      0.80      0.80      8693
    weighted avg       0.80      0.80      0.80      8693
    
    LightGBM
    Fold 1: ACC=0.7982, F1=0.8095, Time0.75s
    Fold 2: ACC=0.7982, F1=0.8025, Time0.70s
    Fold 3: ACC=0.8005, F1=0.8143, Time0.68s
    Fold 4: ACC=0.7975, F1=0.8072, Time0.67s
    Fold 5: ACC=0.7980, F1=0.8005, Time0.67s
    평균 학습 시간: 0.70s
                  precision    recall  f1-score   support
    
               0       0.83      0.75      0.79      4315
               1       0.77      0.85      0.81      4378
    
        accuracy                           0.80      8693
       macro avg       0.80      0.80      0.80      8693
    weighted avg       0.80      0.80      0.80      8693
    


## 결과 분석 및 시각화


```python
plt.figure(figsize=(18, 10))
```




    <Figure size 1800x1000 with 0 Axes>




    <Figure size 1800x1000 with 0 Axes>



```python
# 성능 비교 막대 그래프
ax1 = plt.subplot(2, 3, 1)
metrics = ['Accuracy', 'F1-Score', 'ROC-AUC']
x = np.arange(len(metrics))
width = 0.2
for i, (name, result) in enumerate(results.items()):
    values = [result['acc_mean'], result['f1_mean'], result['roc_mean']]
    errors = [result['acc_std'], result['f1_std'], result['roc_std']]
    ax1.bar(x + i*width, values, width, label=name, yerr=errors, capsize=5)
ax1.set_title('Model Performance Comparison')
ax1.set_xticks(x + width)
ax1.set_xticklabels(metrics)
ax1.set_ylim(0.75, 0.85)
ax1.legend()
ax1.grid(True, alpha=0.3)
```


    
![png](../../../_ipynbs/2025-12-24-Spaceship-Titanic_files/2025-12-24-Spaceship-Titanic_51_0.png)
    



```python
# 학습 시간 비교 그래프
ax2 = plt.subplot(2, 3, 3)
model_names = list(results.keys())
train_times = [r['train_time_avg'] for r in results.values()]
colors = sns.color_palette('pastel', len(model_names))

bars = ax2.bar(model_names, train_times, color=colors)
ax2.set_title('Average Training Time (sec)')
ax2.set_ylabel('Seconds')
ax2.grid(True, axis='y', alpha=0.3)

for bar in bars:
    height = bar.get_height()
    ax2.text(bar.get_x() + bar.get_width()/2., height, f'{height:.3f}s', ha='center', va='bottom')
```


    
![png](../../../_ipynbs/2025-12-24-Spaceship-Titanic_files/2025-12-24-Spaceship-Titanic_52_0.png)
    



```python
from sklearn.metrics import roc_curve

# ROC Curve 비교
ax5 = plt.subplot(2, 3, 5)

for name, result in results.items():
    fpr, tpr, _ = roc_curve(result['y_test'], result['y_proba'])
    ax5.plot(fpr, tpr, label=f"{name} (AUC={result['roc_mean']:.3f})")

ax5.plot([0, 1], [0, 1], 'k--', label='Random')
ax5.set_xlabel('False Positive Rate')
ax5.set_ylabel('True Positive Rate')
ax5.set_title('ROC Curve 비교')

ax5.legend(loc='lower right', fontsize='small')

ax5.grid(True, alpha=0.3)
```


    
![png](../../../_ipynbs/2025-12-24-Spaceship-Titanic_files/2025-12-24-Spaceship-Titanic_53_0.png)
    


### Confusion Matrix (오차 행렬)
"전송된 사람을 안 됐다고 헷갈렸나? 아니면 안 된 사람을 됐다고 헷갈렸나?"

이 표는 모델의 **'착각 패턴'**을 보여준다.

- TP (True Positive): 모델이 "전송됨"이라고 했는데, 실제로도 전송됨. (정답!)

- TN (True Negative): 모델이 "전송 안 됨"이라고 했는데, 실제로도 전송 안 됨. (정답!)

- FP (False Positive): 모델이 "전송됨"이라고 예측했지만, 실제로는 전송 안 됨. (과잉 예측)

- FN (False Negative): 모델이 "전송 안 됨"이라고 예측했지만, 실제로는 전송됨. (놓침)

> Spaceship Titanic에서의 해석: 만약 FN이 너무 높다면, 우리 모델은 "전송된 승객을 찾아내는 능력이 부족하다"고 판단할 수 있다.


```python
from sklearn.metrics import confusion_matrix

# 혼동 행렬 출력
for idx, (name, result) in enumerate(results.items(), 4):
    ax = plt.subplot(2, 3, idx)
    cm = confusion_matrix(result['y_test'], result['y_pred'])
    sns.heatmap(cm, annot=True, fmt='d', cmap='Blues', ax=ax)
    ax.set_title(f'{name} - Confusion Matrix')
    ax.set_xlabel('Predicted')
    ax.set_ylabel('Actual')
```


    
![png](../../../_ipynbs/2025-12-24-Spaceship-Titanic_files/2025-12-24-Spaceship-Titanic_55_0.png)
    

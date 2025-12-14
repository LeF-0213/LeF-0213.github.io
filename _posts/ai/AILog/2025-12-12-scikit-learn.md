---
layout: post
related_posts:
  - /ai/aiLog
title:  "사이킷런(scikit-learn)"
date:   2025-12-12
categories:
  - aiLog
description: >
  
---
* toc
{:toc .large-only}

# 사이킷런 (scikit-learn)

사이킷런(scikit-learn)은 파이썬(Python)으로 작성된 오픈소스 머신러닝 라이브러리로, 데이터 분석과 예측 모델 구축을 위해 널리 사용된다. 간단하고 일관된 인터페이스를 제공하며, 지도 학습(Supervised Learning)과 비지도 학습(Unsupervised Learning) 알고리즘을 모두 지원한다. 

주로 분류(Classification), 회귀(Regression)

```bash
pip install sklearn
```


## Iris 데이터셋

아이리스(Iris) 데이터셋은 머신러닝과 통계학에서 가장 널리 사용되는 대표적인 샘플 데이터셋이다. 이 데이터셋은 불꽃(Iris)의 세 가지 품종(Setosa, Versicolor, Virginica)에 대한 정보를 포함하고 있다.

### 데이터셋

데이터셋(Dataset)은 머신러닝과 데이터 과학에서 학습, 검증, 테스트하기 위해 사용되는 데이터의 집합이다. 데이터셋은 일반적으로 **입력 데이터(Features)와 정답 레이블(Labels)**로 구성되며, 학습용 데이터셋(Training Dataset), 검증용 데이터셋(Validation Dataset), 테스트용 데이터셋

예를 들어 데이터가 총 다섯개로 되어있고 이 데이터셋은 입력데이터로 쓸수 있는 꽃이의 너비와 길이 어떤 꽃인지 네개가 들어있고, 총 150개가 들어있다. 이것을 컴퓨터 하드웨어에 학습을 시키려고 한다.
머신러닝 딥러닝에 크게 세가지로 나누어 사용한다. 기계가 공부를 해야하는 학습용 데이터셋, 평가를 잘 하고 있는 지 중간중간 확인하기 위한 검증용 데이터셋, 105개 중에 일부는 학습을 하고(100개), 일부는 모의고사를 본다(단, 겹치지 않게!, 30개), 그리고 잘 하고 있는 지 최종적으로 확인하는 테스트용 데이터셋(20개) 하이퍼파라미터를 조정하는 것은 제대로 검증이 이루어지지 않을 때 모델의 성능을 조절하는 것이다.

Classes 3 => 꽃 종류 3개
Samples per class 50 => 꽃 총 150개
Samples total 150
Dimensionality 4 => 차원
Features real, positive => 맞다 틀리다

```m
Returns:
dataBunch
Dictionary-like object, with the following attributes.

data{ndarray, dataframe} of shape (150, 4)
The data matrix. If as_frame=True, data will be a pandas DataFrame.

target: {ndarray, Series} of shape (150,)
The classification target. If as_frame=True, target will be a pandas Series.

feature_names: list
The names of the dataset columns.

target_names: ndarray of shape (3, )
The names of target classes.

frame: DataFrame of shape (150, 5)
Only present when as_frame=True. DataFrame with data and target.

Added in version 0.23.

DESCR: str
The full description of the dataset.

filename: str
The path to the location of the data.

Added in version 0.20.

(data, target)tuple if return_X_y is True
A tuple of two ndarray. The first containing a 2D array of shape (n_samples, n_features) with each row representing one sample and each column representing the features. The second ndarray of shape (n_samples,) containing the target samples.

Added in version 0.18.
```

X_train: 공부할 데이터만 들어감
X_test: 테스트 볼 데이터만 들어감
y_train: 공부할 때 답안지
y_test: 검증할 때 답안지

 X_train
    - 공부할 데이터
- X_test
    - 테스트 볼 데이터
- y_train
    - 공부할 데이터의 답
- y_test 
    - 테스트할 데이터의 답

변수는 다르게 해도 되지만 순서는 같아야함.
관례로 matrix 즉, x축으로 y축으로 여러개가 있을 때는 대문자,
한 개짜리 리스트는 소문자

문제에 대한 데이터(독립 변수)
test_size=0.2 => 나중에 테스트할 때 20%를 쓰겠다(검증할 때 빠짐)
random_state: 기본으로 true인데 값을 넣어주는 이유는, 주어진 숫자에 대해 섞이는 순서를 고정시켜주기 위한 것


## SVC (Support Vector Classifier)

### 서포트 백터 머신 (Support Vector Machine, SVM)

서포트 벡터 머신(SVM)은 두 개 이상의 클래스()

초평면(평면으로 나누어주는 것, 즉 공간을 나누는 것)
여러번 반복 실행 epoch
조금씩 수정 -> 순전파 역전파 기울기 보정

### accuracy_score

accuracy_score는 사이킷런(sklearn)

## 원-핫 인코딩 (One-Hot Encoding)

원-핫 인코딩은 주어진 범주형 변수의 각 고유값(카테고리)을 독립적인 이진(Binary) 컬럼으로 만들고, 해당 데이터가 어떤 카테고리에 속하는지를 0과 1로 표현하는 방식이다.



## **데이터셋 컬럼**
- **BHK**: 주택에 포함된 침실, 거실, 주방의 총 개수를 의미합니다.
- **Rent**: 주택(아파트/플랫)의 월 임대료를 나타냅니다.
- **Size**: 주택(아파트/플랫)의 면적을 평방피트(Square Feet)로 나타냅니다.
- **Floor**: 주택이 위치한 층수와 건물의 총 층수를 나타냅니다. (예: 2층 중 1층, 5층 중 3층 등)
- **Area Type**: 주택의 면적이 어떤 방식으로 계산되었는지를 나타냅니다. (예: 전체 면적, 실사용 면적, 건축 면적 등)
- **Area Locality**: 주택(아파트/플랫)이 위치한 구체적인 지역이나 동네 정보를 나타냅니다.
- **City**: 주택(아파트/플랫)이 위치한 도시를 나타냅니다.
- **Furnishing Status**: 주택이 가구가 완비되었는지(Furnished), 부분적으로 갖추어졌는지(Semi-Furnished), 아니면 비어 있는지(Unfurnished)를 나타냅니다.
- Tenant Preferred: 집주인 또는 중개인이 선호하는 임차인 유형을 나타냅니다. (예: 가족, 싱글, 직장인 등)
- **Bathroom**: 주택에 있는 욕실의 개수를 나타냅니다.
- **Point of Contact**: 주택(아파트/플랫)에 대한 추가 정보를 얻기 위해 연락해야 할 담당자나 중개인의 정보를 나타냅니다.

### boxplot

Boxplot은 데이터의 중앙값, 사분위수, 이상치 등을 시각적으로 표현하는 통계 그래프이다. 주로 데이터 분포와 이상치를 빠르게 파악하기 위해 사용된다.

- **중앙값 (Median, Q2)**: 데이터를 크기 순으로 정렬했을 때 중간에 위치한 값
- **Q1 (제 1사분위수, 25%)**: 하위 25%에 해당하는 값
- **Q3 (제 3사분위수, 75%)**: 상위 25%에 해당하는 값
- **IQR (Interguartile Range, 사분위 범위)**: Q3 - Q1, IQR은 데이터의 중간 50% 범위를 의미한다.
- **Minimum**: Q1 - 1.5 x IQR

## 로그 변환으로 RMSE 개선

주택 임대료 데이터셋은 한 쪽으로 치우친 분포를 가지고 있다. 값이 큰 임대료(Outliers)가 평균과 모델 예측 결과에 큰 영향을 미치기 때문에 로그 변환을 통해 값의 범위를 축소하고, 분포를 정규 분포(Normal Distribution)에 가깝게 만든다. 

## 앙상블 모델 적용

### ※ 랜덤 포레스트

**과적합(overfitting)** -> 학습은 잘 되었지만, test에서는 엉망인 경우
과접합된 모델들은 전처리를 잘 해주고, 여러가지 파라미터 값을 조절해야한다(decisiontree에서 자주 일어난다.)

### ※ XGBoost

과적합이 될 가능성이 높음.
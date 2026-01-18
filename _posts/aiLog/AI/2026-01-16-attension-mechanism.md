---
layout: post
related_posts:
  - /aiLog/ai
title:  "어텐션 메커니즘(Attention Mechanism)"
date:   2026-01-16
categories:
  - ai
description: >
  
---
* toc
{:toc .large-only}

## 어텐션 매커니즘

기존의 RNN(Recurrent Neural Network)나 LSTM(Long Short-Term Memory) 모델은 입력데이터를 순차적으로 처리하기 때문에 긴 문장에서 중요한 정보를 잃어버리거나, 멀리 떨어진 단어 간의 관계를 잘 파악하지 못하는 문제가 있었다. 어텐션은 입력 문장의 모든 단어를 한 번에 보고, 어떤 단어가 중요한지 가중치를 계산하여 집중하는 방법이다.

예를 들어, "나는 오늘 학교에서 수학 시험을 봤다." 라는 문장에서 "시험" 이라는 단어가 가장 중요한 의미를 가진다고 가정한다. 어텐션은 이 문장을 처리할 때 "시험"에 더 높은 가중치를 주고, 덜 중요한 단어에는 낮은 가중치를 주는 방식으로 학습한다.

## 단어 임베딩과 문맥

단어의 의미는 그 단어가 어떤 문맥에서 쓰이느냐에 따라 달라진다. 예를 들어 "귤 과 사과" 에서는 "사과" 가 과일 의미로 쓰이고, "어제일 을 사과" 에서는 사죄 의미로 쓰인다. 단어 임베딩 학습에서는 이런 문맥적 **공동출현 정보** 를 가중치로 반영하여, 벡터 공간에서 단어들을 서로 끌어당기거나 밀어내며 위치를 조정한다. 그 결과 '사과' 벡터는 과일과 사죄라는 두 의미의 문맥 사이 어딘가에 자리 잡게 되고, 이것이 바로 단어 임베딩이 단어 간 의미 관계를 학습하는 방식이다.

아래 그림은 단어 임베딩이 문맥을 통해 단어 사이의 관계를 어떻게 학습하는지를 보여준다. 왼쪽 표는 문장에서 단어들이 함께 등장할 때 생기는 공동출현 가중치(W1, W2)를 나타내며, 예를 들어 "귤 과 사과" 라는 문맥에서는 사과가 과일 쪽으로, "어제일 을 사과" 라는 문맥에서는 사과가 행위·사죄 쪽으로 끌리게 된다. 오른쪽 그림은 이러한 가중치가 반영되어 학습 과정에서 단어 벡터가 조금씩 이동하는 과정을 시각화한 것으로, 결국 사과는 과일 의미와 사죄 의미 사이의 중간 어딘가에 위치하게 된다. 즉, 단어의 의미는 문맥 속에서 결정되며, 임베딩 학습은 자주 함께 등장하는 단어끼리 가까워지도록 단어 벡터 공간을 정렬하는 과정이다.

<img src='https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FkQNxV%2FbtsP918Xhx4%2FAAAAAAAAAAAAAAAAAAAAAG5ntIzYDsxeb39it8Jl8P6z2vHMXqOhAlAZsHhSy9nr%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1769871599%26allow_ip%3D%26allow_referer%3D%26signature%3D6pVD4JPYQU39AsqSHLCAzR74ey4%253D' />

## 토큰화와 패딩

자연어 처리에서 문장을 다루기 위해서는 먼저 문장을 단어 단위로 나누는 토큰화(tokenization) 과정이 필요하다. 예를 들어 "커피 한잔 어때?" 라는 문장은 [커피, 한잔, 어때] 로 나눠지고, 각 단어는 **임베딩** 을 통해 **고정된 길이의 숫자 벡터 (예: 512차원)** 로 표현된다. 하지만 문장의 길이는 제각각이기 때문에, 모델에 넣기 위해서는 모든 문장을 같은 길이로 맞춰야 한다. 이때 짧은 문장은 패딩(padding) 을 이용해 빈칸을 채워 준다.

즉, "커피 -> T1, 한잔 -> T2, 어때 -> T3" 와 같은 실제 단어 벡터 뒤에 **패딩 토큰(PD)** 을 추가하여 입력을 일정하게 만들면, 모델은 문장의 의미를 잃지 않으면서도 효율적으로 학습할 수 있다. 예를 들어, "오늘 날씨 어때?" 라는 문장은 [오늘, 날씨, 어때, PD], "커피 한잔 어때?" 는 [커피, 한잔, 어때, PD] 처럼 동일한 길이로 변환된다.

> 이렇게 토큰화와 패딩은 텍스트 데이터를 딥러닝 모델이 다룰 수 있는 숫자 행렬로 바꾸는 가장 기본적이면서도 중요한 과정이다.

<img src='https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FcGPntS%2FbtsP7zlBgWc%2FAAAAAAAAAAAAAAAAAAAAAOPzNFpjxpq2HTcWl_zwaHMWSF2d0NQm5Mwan-tgNrmz%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1769871599%26allow_ip%3D%26allow_referer%3D%26signature%3DB5rf505TyYQR9mlnPIBPDJQInVo%253D' />

## 셀프 어텐션 (Self-Attention)

셀프 어텐션에서는 입력 임베딩을 이용해 단어들 사이의 관계를 수치적으로 계산한다. **먼저 각 단어의 임베딩 벡터를 복사하여 두 개의 행렬을 만들고, 하나는 그대로 두고 다른 하나는 전치(transpose)하여 행과 열을 바꾼다.** 이렇게 준비된 **두 행렬을 곱하면 각 단어가 다른 단어와 얼마나 관련이 있는지를 나타내는 유사도 행렬이 만들어진다.**

예를 들어 "귤, 과, 사과" 라는 문장에서 '사과' 벡터와 '귤' 벡터의 **내적을 통해 두 단어의 관련성을 계산**하고, 이를 'W1' 같은 값으로 저장한다. 이 행렬은 결국 모델이 어떤 단어를 더 주목해야 할지를 결정하는 근거가 된다.

즉, 셀프 어텐션은 행렬 곱 연산을 통해 모든 단어 쌍의 관계를 **한 번에 계산**하고, 이를 토대로 문맥을 더 깊이 이해하게 만드는 핵심 매커니즘이다.

<img src='https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FdhRsny%2FbtsP9874MZl%2FAAAAAAAAAAAAAAAAAAAAAC2FED42l0VsdJdXT8t62Q2YApoCRdsIC3kH4-WOELX5%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1769871599%26allow_ip%3D%26allow_referer%3D%26signature%3DQ4AcTBVfqkMskmxvJ%252FkjRIf9mdg%253D' />

<img src='https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FbohPN8%2FbtsP9Iok5Wg%2FAAAAAAAAAAAAAAAAAAAAAC6hvP_XW-bRNpDKqv6RVFixPtU7QOt1FxDrnRjTlGdh%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1769871599%26allow_ip%3D%26allow_referer%3D%26signature%3DDff9Qzh79PJfC5dskHL6e%252F26c84%253D' />

이전 단계에서 얻은 유사도 행렬(4x4)이 준비되고, 여기에 각 단어의 64차원 임베딩을 담은 Value 행렬(4x64)을 곱해 준다.

그 결과는 (4x64) 형태의 새로운 표현으로, 각 단어가 다른 단어들과의 관계를 고려해 업데이트된 벡터가 된다. 예를 들어 "커피 한잔 어때" 라는 문장에서, '커피' 라는 단어 벡터는 단순히 자기 자신만을 표현하는 것이 아니라, "한잔" 이나 "어때" 같은 단어와의 관련성 가중치를 곱해 더해진 최종 벡터로 변환된다.

이렇게 해서 모델은 문맥 속에서 각 단어의 의미를 풍부하게 반영할 수 있으며, 이것이 트랜스포머가 단어 간 관계를 이해하는 중요한 방식이다.

## 어텐션 스코어 (Attention Score)

Self-Attention에서는 쿼리(Query)와 키(Key)의 내적을 통해 단어 간 유사도를 계산하지만, 값이 너무 커질 경우 학습이 불안정해지므로 이를 헤드 차원의 제곱근으로 나누어 스케일링한다. 이후 스케일링된 값은 소프트맥스(softmax) 함수를 거쳐 0과 1 사이의 확률 값으로 변환되고, 각 행의 합이 1이 되도록 정규화된다. 이렇게 하면 모델은 어떤 단어에 더 집중할지 비율을 정할 수 있다. 또한 문장의 길이를 맞추기 위해 추가된 패딩 토큰은 소프트맥스 전에 -$\infty$ 를 더해 결과적으로 가중치가 0이 되도록 처리한다.

<img src='https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FDKgSL%2FbtsP8ENeqTx%2FAAAAAAAAAAAAAAAAAAAAAGlFeWxhlIzUryL5dBLmjIRl-RKf1TNflm_xAmAYnmS8%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1769871599%26allow_ip%3D%26allow_referer%3D%26signature%3DOOZBjIH7F4rme7l5fHRN8AfI9jQ%253D' />

## 멀티-헤더 어텐션

트랜스포머 모델에서 **단어 하나는 보통 512차원의 임베딩 벡터로 표현** 되는데, 이 벡터 전체를 한 번에 다루는 대신 **8개의 64차원 벡터로 분할** 하여 각각을 **다른 '시선(head)'** 으로 처리한다. 이렇게하면 모델은 같은 문맥 속 단어라도 의미의 다양한 측면-예를 들어 '커피' 라는 단어가 음료로 쓰였는지, 일상 대화 속 제안으로 쓰였는지-를 동시에 포착할 수 있다.

즉, 하나의 임베딩을 **여러 개의 작은 벡터(헤드)로 나눠 병렬적으로 어텐션을 수행함** 으로써, 문맥을 더 풍부하게 이해하고 단어 간 복잡한 관계를 학습할 수 있게 되는 것이다.

<img src='https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2Fbo697M%2FbtsP99lCOEq%2FAAAAAAAAAAAAAAAAAAAAAOtu1NoSGtukMgdh6QKsOcZYnn5vuPwIF_oLLSsYGi3s%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1769871599%26allow_ip%3D%26allow_referer%3D%26signature%3D8DtrOwDLsUeYt4Ertn%252BNnLgAuVM%253D' />

> 예를 들어, 문장 "커피 한잔 어때?" 에서 '커피' 라는 단어 벡터(512 차원)는 8개의 헤드로 분리된다. 어떤 헤드는 '커피-음료' 관계에 집중하고, 다른 헤드는 '커피-제안 표현(어때)' 관계를 본다거나, 또 다른 헤드는 '커피-숫자 표현(한잔)' 관계를 본다고 할 수 있다. 이렇게 다양한 관점을 합쳐 최종적으로 더 풍부한 표현이 만들어지는 것이 멀티-헤드 어텐션의 장점이다.

## 헤드 결합으로 완성되는 표현

멀티-헤드 어텐션에서는 하나의 512차원 임베딩 벡터를 8개의 64차원 벡터로 나눠 각각 독립적으로 어텐션을 계산한다. 이렇게 얻은 여러 개의 결과 행렬(예: 4x64)을 다시 이어 붙여(concatenate) 원래 차원인 512차원의 행렬로 복원하는데, 이 과정을 거쳐 최종 출력 'X'가 만들어진다.

즉, 각 헤드는 같은 단어라도 서로 다른 문맥적 특징을 학습하며, 마지막에 이 다양한 시선들을 합쳐 더 풍부한 의미 표현을 형성하는 것이다. 예를 들어 "커피 한잔 어때?" 라는 문장에서 어떤 헤드는 "커피-음료" 관계를, 다른 헤드는 "한잔-수량 표현"을, 또 다른 헤드는 "어때-제안" 의미를 포착한 뒤, 이 결과들을 모아 하나의 통합된 표현으로 완성하는 방식이다.

<img src='https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FcoNu9O%2FbtsP8NQXDqI%2FAAAAAAAAAAAAAAAAAAAAAIFJehPIM1qeD58npKxtqryxLatfx7w23NS5gL7dNSVl%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1769871599%26allow_ip%3D%26allow_referer%3D%26signature%3D8YH5tD0sN%252BiRB5Wos1fnuwKcGBA%253D' />

## 쿼리(Query), 키(Key), 값(Value)

Self-Attention은 입력 임베딩에서 쿼리(Query), 키(Key), 벨류(Value)라는 세 가지 행렬을 복사해 만들어낸다. Query는 질문을 던지는 역할, Key는 단서를 제공하는 역할, Value는 실제 답변 역할을 한다고 이해할 수 있다.

예를 들어 한 문장의 각 단어 벡터(4x64)를 기준으로 쿼리와 키의 내적을 계산하면 단어 간 연관성을 나타내는 4x4 유사도 행렬이 생성되고, 이 값으로 Value 행렬을 가중합하여 최종 출력(4x64)이 만들어진다. 즉, "커피 한잔 어때?" 라는 문장에서 '커피'라는 단어는 Query로서 다른 단어들을 바라보고, Key를 통해 어떤 단어가 중요한지를 판단하며, 그 결과 Value가 조정되어 문맥에 맞는 새로운 '커피' 표현이 완성되는 것이다.

이처럼 Query・Key・Value는 Self-Attention이 단어 간 관계를 정교하게 계산하고 문맥 이해를 가능하게 하는 핵심 요소이다.

<img src='https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2Fm7Cmb%2FbtsQaPGX5KU%2FAAAAAAAAAAAAAAAAAAAAAHS98d3KbKz0m5OUn5k2kQBPMJigRJHTUDHMghvC0cG_%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1769871599%26allow_ip%3D%26allow_referer%3D%26signature%3DPmgxsmIYu%252BspOdOcoEsARuSHiZw%253D' />

## 문장의 길이와 배치 처리

트랜스포머 모델에서 문장을 어텐션에 입력하려면 일정한 형식으로 변환해야 한다. 먼저 문장의 시작과 끝을 알리기 위해 (start of sentence), (end of sentence) 토큰을 붙이고, 문장의 길이를 맞추기 위해 **짧은 문장은 패딩(PD)토큰** 으로 채운다. 이렇게 하면 "커피 한잔 어때" 같은 짧은 문장과 "오늘 날씨 좋네", "옷이 어울려요" 처럼 길이가 다른 문장도 동일한 길이의 행렬로 변환할 수 있다. 

또 한 번에 여러 개의 문장을 처리하기 위해 **배치(batch) 단위** 로 묶어 입력하면, 모델은 **병렬 연산** 을 통해 더 빠르고 효율적으로 학습할 수 있다. 즉, 문장 길이를 맞추고 배치 크기를 설정하는 과정은 어텐션이 문맥을 올바르게 이해하고 GPU 연산 효율을 극대화하기 위한 핵심 전처리 단계이다

<img src='https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FlEJYe%2FbtsQbCNVWSK%2FAAAAAAAAAAAAAAAAAAAAANeUTe_5wZjNTtYtMaSiXrGDx9A1Tz0qMZ5elChUwsIR%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1769871599%26allow_ip%3D%26allow_referer%3D%26signature%3DQCPp%252FuLsoquq%252F5VSRC0m5KVFFF4%253D' />


```python
import numpy as np
```


```python
# Numpy 배열을 출력할 때 모양을 설정
# precision: 소수점 이하 자릿수
# suppress: 일반 소수로 출력 (1.2345e-05 -> 1.2345)
np.set_printoptions(precision=4, suppress=True)
```


```python
# <sos>: Start Of Sentence
# <eos>: End Of Sentence
# PAD: Padding, 문장 길이를 맞추기 위한 공백 
# 단어와 해당 임베딩 벡터를 딕셔너리로 정의한다.
embedding_dict = {
    '<sos>': np.random.rand(512),
    '<eos>': np.random.rand(512),
    '커피': np.random.rand(512),
    '한잔': np.random.rand(512),
    '어때': np.random.rand(512),
    '오늘': np.random.rand(512),
    '날씨': np.random.rand(512),
    '좋네': np.random.rand(512),
    '옷이': np.random.rand(512),
    '어울려요': np.random.rand(512),
    'PAD': np.zeros(512) # 패딩 벡터는 0으로 채운다.
}

# 입력 문장, batch 단위로 구성
sentences = [
    ['<sos>', '커피', '한잔', '어때', '<eos>'],
    ['<sos>', '오늘', '날씨', '좋네', '<eos>'],
    ['<sos>', '옷이', '어울려요', '<eos>', 'PAD']
]
```


```python
# 토큰을 임베딩 벡터로 변환
embeddings = np.array([[embedding_dict[token] for token in sentence] for sentence in sentences])
embeddings, embeddings.shape
```




    (array([[[0.4266, 0.9475, 0.8611, ..., 0.3354, 0.982 , 0.3394],
             [0.6636, 0.5676, 0.0001, ..., 0.7915, 0.9142, 0.8164],
             [0.9106, 0.2473, 0.7182, ..., 0.5799, 0.3855, 0.617 ],
             [0.096 , 0.0761, 0.622 , ..., 0.4261, 0.8039, 0.6794],
             [0.1878, 0.7932, 0.2356, ..., 0.2663, 0.026 , 0.8768]],
     
            [[0.4266, 0.9475, 0.8611, ..., 0.3354, 0.982 , 0.3394],
             [0.8984, 0.1601, 0.0653, ..., 0.6084, 0.0694, 0.6523],
             [0.8525, 0.0893, 0.8109, ..., 0.4433, 0.8805, 0.0058],
             [0.1323, 0.2188, 0.4026, ..., 0.9435, 0.4376, 0.394 ],
             [0.1878, 0.7932, 0.2356, ..., 0.2663, 0.026 , 0.8768]],
     
            [[0.4266, 0.9475, 0.8611, ..., 0.3354, 0.982 , 0.3394],
             [0.5405, 0.3199, 0.7256, ..., 0.5877, 0.0785, 0.664 ],
             [0.0062, 0.0341, 0.5522, ..., 0.0718, 0.4385, 0.3704],
             [0.1878, 0.7932, 0.2356, ..., 0.2663, 0.026 , 0.8768],
             [0.    , 0.    , 0.    , ..., 0.    , 0.    , 0.    ]]],
           shape=(3, 5, 512)),
     (3, 5, 512))




```python
# 어텐션을 수행하는 Head의 개수 
# 여러명의 전문가(Head)가 각기 다른 관점에서 문장을 동시에 분석하는 설정
num_heads = 8 
# 512 // 8 = 64, 각 헤드는 64차원의 정보를 담당
head_dim = 512 // num_heads
# (3, 5, 512) 형태였던 전체 임베딩을 마지막축 (axis=2, 512차원 축)을 기준으로 8등분한다.
# 그 결과, (3, 5, 64) 크기를 가진 행렬 8개가 담긴 리스트가 생성된다.
heads = np.split(embeddings, num_heads, axis=2)
queries = heads.copy()
# 내적(dot-product)을 하기 위해 행과 열을 바꿔야 한다(전치, transpose). 
# (3, 5, 64) -> (3, 64, 5)
keys = [head.transpose(0, 2, 1) for head in heads]
values = heads.copy()

print('query 행렬의 형태: ', queries[0].shape)
print('key 행렬의 형태: ', keys[0].shape)
print('value 행렬의 형태: ', values[0].shape)
```

    query 행렬의 형태:  (3, 5, 64)
    key 행렬의 형태:  (3, 64, 5)
    value 행렬의 형태:  (3, 5, 64)



```python
tokens_of_interest = ['커피', '한잔', '어때']
indices_of_interest = [sentences[0].index(token) for token in tokens_of_interest]
print(indices_of_interest)

print("어텐션 이전의 임베딩 테이블 중 ['커피', '한잔', '어때'] 토큰의 평균 값: ")
# embedding[0, indices_of_interest, :]
# 0번째 문장에서 [0, 1, 2] 위치에 해당하는 512차원 벡터 3개를 가져오기
# np.mena(..., axis=1): 각 단어별로 512개의 숫자 평균을 구한다.
initial_avg = np.mean(embeddings[0, indices_of_interest, :], axis=1)
print(initial_avg) # 어텐션 전후를 비교하기 위함
```

    [1, 2, 3]
    어텐션 이전의 임베딩 테이블 중 ['커피', '한잔', '어때'] 토큰의 평균 값: 
    [0.4919 0.4958 0.4847]



```python
attention_scores = np.matmul(queries[0], keys[0]) # shape: (3, 5, 5)
scaling_factor = np.sqrt(head_dim) # 값이 너무 커지는 것을 방지
scaled_attention_scores = attention_scores / scaling_factor

mask = np.array([[token == 'PAD' for token in sentence] for sentence in sentences])
print(mask)
# 행은 하나뿐이고 열이 5개임을 명시
# 각 단어가 질문할 때, 5개의 패딩 정보가 필요
mask = mask[:, np.newaxis, :]
print(mask)
# np.where(조건, 참일 때 값, 거짓일 때 값)
# 조건(mask): True인 위치 (즉, 단어가 'PAD' 인 곳)
# -np.inf: $e^{-\infty} = 0$이 되어, 확률값이 0이 된다. 
# 따라서 모델이 해당 단어를 참고하지 않는다.
# 거짓일 때, 원래 계산된 scaled_attention_scores를 유지
scaled_attention_scores = np.where(mask, -np.inf, scaled_attention_scores)
scaled_attention_scores
```

    [[False False False False False]
     [False False False False False]
     [False False False False  True]]
    [[[False False False False False]]
    
     [[False False False False False]]
    
     [[False False False False  True]]]





    array([[[2.6226, 1.8266, 2.144 , 1.9771, 1.9899],
            [1.8266, 2.6896, 1.8102, 2.0573, 2.0995],
            [2.144 , 1.8102, 2.7713, 2.0386, 2.0675],
            [1.9771, 2.0573, 2.0386, 2.7015, 2.1225],
            [1.9899, 2.0995, 2.0675, 2.1225, 2.8563]],
    
           [[2.6226, 1.7379, 1.7447, 2.1472, 1.9899],
            [1.7379, 2.3865, 1.7324, 1.9243, 1.8049],
            [1.7447, 1.7324, 2.513 , 1.9916, 1.9669],
            [2.1472, 1.9243, 1.9916, 3.0684, 2.4429],
            [1.9899, 1.8049, 1.9669, 2.4429, 2.8563]],
    
           [[2.6226, 1.7711, 1.8154, 1.9899,   -inf],
            [1.7711, 2.665 , 1.9747, 2.0727,   -inf],
            [1.8154, 1.9747, 2.4278, 1.8103,   -inf],
            [1.9899, 2.0727, 1.8103, 2.8563,   -inf],
            [0.    , 0.    , 0.    , 0.    ,   -inf]]])




```python
def softmax(x):
    # axis=-1: 마지막 축(S)에 대해 softmax를 적용
    # x- np.max(x): Max Trick
    # 지수 함수(e^x)는 인자가 조금만 커져도 값이 무한대(inf)로 발산해버린다.
    # 모든 값에서 최댓값을 빼주면 가장 큰 값이 0이 된다. (e^0 = 1)
    # 나머지 값들은 모두 0보다 작거나 같아지므로, 
    # 결과적으로 지수 값이 0과 1 사이에 머물게 되어 계산이 매우 안전해진다.
    # np.exp(): 안전해진 값들에 지수 함수를 취한다.
    # 큰 값은 더 크게, 작은 값은 더 작게 변하여 변별력이 생긴다.
    # keepdims=True: 축을 계산해서 합치더라도, 그 축의 자리는 빈 칸(크기 1)으로 남겨두라는 옵션
    # (3, 5)가 아닌 (3, 5, 5) 형태로 남아있을 수 있음
    exp_x = np.exp(x - np.max(x, axis=-1, keepdims=True))
    # 어텐션 점수를 확률(0~1 사이의 값)'로 변환
    return exp_x / np.sum(exp_x, axis=-1, keepdims=True)                   
```


```python
restored_heads = []

for i in range(num_heads):
    query = queries[i]
    key = keys[i]
    value = values[i]

    # 내적 계산 후 스케일링
    attention_scores = np.matmul(query, key) / scaling_factor

    # 패딩 처리
    mask = np.array([[token == 'PAD' for token in sentence] for sentence in sentences])
    mask = mask[:, np.newaxis, :] # 차원을 맞추기 위해 확장
    attention_scores = np.where(mask, -np.inf, attention_scores)

    # 소프트맥스 적용
    attention_weights = softmax(attention_scores)

    # 밸류와의 곱셈
    restored_head = np.matmul(attention_weights, value)
    restored_heads.append(restored_head)

final_output = np.concatenate(restored_heads, axis=2)

print("어텐션 이후의 결과 중 '커피', '한잔', '어때' 토큰의 평균 값: ",
     np.mean(final_output))
print(final_output.shape)
```

    어텐션 이후의 결과 중 '커피', '한잔', '어때' 토큰의 평균 값:  0.4935735493389238
    (3, 5, 512)


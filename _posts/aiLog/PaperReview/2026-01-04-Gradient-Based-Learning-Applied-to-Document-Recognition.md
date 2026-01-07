---
layout: post
related_posts:
  - /aiLog/ai
title:  "딥러닝의 시작, LeNet-5 논문 리뷰"
date:   2026-01-04
categories:
  - ai
description: >
  Gradient-Based Learning Applied to Document Recognition (Yann LeCun et al., 1998)
---
* toc
{:toc .large-only}

## 🏷️ 논문 개요

## 논문 제목의 의미

**Gradient-Based Learning Applied to Document Recognition**

- **Gradient-Based Learning**: 경사(기울기) 기반 학습, 즉 오차 역전파(Backpropagation)을 통한 경사하강법 기반 학습 방법.
- **Document Recognition**: 문서 인식, 특히 손글씨 숫자와 문자 인식
- 핵심: "기울기를 이용한 학습 방법을 문서 인식 문제에 적용했다."

### 논문 한 줄 요약

기존의 수작업 특징 추출 방법을 벗어나, CNN(Convolutional Neural Network, LeNet-5) 아키텍처를 통해 픽셀 이미지에서 직접 특성을 스스로 학습하여 손글씨 인식 문제를 해결하고,
Graph Transformer Networks(GTN)를 통해 전체 문서 인식 시스템을 종단간 (end-to-end)로 학습시킬 수 있음을 제시한 연구.

<br />

---

## Abstract

역전파 알고리즘으로 학습된 다층 신경망은 성공적인 경사 기반 학습(Gradient-Based Learning) 기법의 가장 좋은 예를 구성한다. 적절한 네트워크 구조가 주어지면, 경사 기반 학습 알고리즘은 복잡한 결정 표면(complex decision surface)를 합성할 수 있으며, 이를 통해 손글씨 문자와 같은 고차원 패턴을 최소한의 전처리로 분류할 수 있다. 본 논문은 손글씨 문자 인식에 적용된 여러 방법들을 검토하고, 이를 표준 손글씨 숫자 인식 과제에서 비교한다. 2차원 형태의 변동성에 특화되어 설계된 합성곱 신경망(Convolutional Neural Networks, CNN)은 다른 모든 기법보다 우수한 성능을 보이는 것으로 나타났다.

실제 문서 인식 시스템(real-time document recognition systems)은 필드 추출(field extraction), 분할(segmentation), 인식(recognition), 그리고 언어 모델링(language modeling)을 포함한 여러 모듈로 구성되어 있다. 그래프 트랜스포머 네트워크(Graph Transformer Networks, GTN)라고 불리는 새로운 학습 패러다임은 이러한 다중 모듈 시스템이 전체적으로(global) 경사 기반 방식으로 학습되도록 하여, 전체적인 성능 측정치를 최소화할 수 있게 한다.

두 개의 온라인 손글씨 인식 시스템이 기술된다. 실험에서는 전역 학습(global training)의 장점과 Graph Transformer Networks의 유연성이 입증되었다.

은행 수표 판독용 그래프 트랜스포머 네트워크(GTN)도 함께 다뤄졌다. 이는 합성곱 신경망(CNN) 문자 인식기에 전역 학습 기법을 결합한 것이다. 이 시스템은 사업용 및 개인용 수표에서 높은 정확도를 달성했으며, 매년 수백만 장의 수표를 자동으로 판독하고 있다.

Keywords - Neural Networks, OCR, Documnet Recognition, Machine Learning, Gradient-Based Learning, Convolutional Neural Networks, Graph Transformer Networks, Finite State Transducers

<br />

---

## 🕰️ 시대 배경: 왜 이 연구가 필요했을까?

### 1950년대: 고양이 실험에서 시작된 영감

<img src='https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FbW47Vi%2Fbtsz5iGtA1p%2FAAAAAAAAAAAAAAAAAAAAADk8jC3qNo272KhG0UuAg9ZcCLoPttEchntmqP1sWORA%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1769871599%26allow_ip%3D%26allow_referer%3D%26signature%3D0zThWIxZr3mNLeBek%252FjCfUeFTgw%253D' width='100%' />

1950년대, Hubel과 Wiesel의 고양이 시각피질 연구는 딥러닝의 탄생에 결정적인 영감을 주었다.

- 고양이에게 시각 자극을 줬을 때 **전체 뉴런이 아닌 특정 부분만 활성화**
- 특정 자극(edge, corenr 등)에만 반응하는 뉴런 존재
- **국소 수용 영역(Local Receptive Field)** 개념 발견
- 즉, 고양이의 시각 피질 안의 뉴런들은 일정한 시각적 자극에만 반응하는 **'국부 수용 영역 (Local receptive field)'** 갖고, 
- 이 국부 수용 영역들이 서로 겹쳐 전체적인 시야를 형성한다는 것.

### 1990년대: 우체국의 고민

<img alt="Image" src="https://github.com/user-attachments/assets/de7c1bc3-bb10-4050-bd4b-c02c0b5e16d6" width='100%' />

당시 논문의 저자 Yann LeCun 팀은 손글씨로 적힌 우편 번호의 기계적 분류에 관심이 있었다.

- **우편번호 손글씨 자동 분류** 필요성
- 우체국의 인건비와 분류 시간 절감
- 대용량 데이터 베이스(large databases) 이용 가능성 증가
- data를 이용하여 "기계가 스스로 학습"하는 모델의 필요성

<br />

---

## 🚫 기존 방법의 한계

### 전통적 패턴 인식의 문제점

<img alt="Image" src="https://github.com/user-attachments/assets/d6778df5-f74d-4d30-a91e-5ba5b7a73c2e" width='100%' />

### Feature Extraction의 치명적 한계

과거에는 컴퓨터가 이미지를 직접 이해하지 못해, 사람이 중간에서 '통역사' 역할을 해야 했다.

- **전문 지식의 의존성**: 이미지에서 어떤 특징(선, 원, 질감 등)을 뽑을지 수학적으로 정의해야 했으며, 이는 고도의 전산학・수학적 지식을 필요로 했다.
- **막대한 비용과 시간**: 새로운 도메인(예: 숫자 인식 -> 얼굴 인식)으로 넘어갈 때마다 사람이 처음부터 다시 특징 추출기를 설계해야 했다.
- **범용성 부재**: 특정 데이터셋에 맞춰진 '수동 설계' 방식은 데이터가 조금 바뀌어도 성능이 급격히 떨어졌다.

### Fully-Connected(FC) Network의 문제

<img alt="Image" src="https://github.com/user-attachments/assets/d22e618e-4b34-4084-9638-c74de16f6b8a" width='100%' />

이미지를 단순히 한 줄로 세워(Flatten) 신경망에 넣는 방식은 두 가지 큰 벽에 부딪힌다.

1. **파라미터 수의 폭발 (Memory & Compute)**
    - **계산**: 32 x 32 픽셀 이미지를 100개의 뉴런이 있는 은닉층에 연결하면, 가중치(Weight)만 무려 **102,400개**(1,024 x 100)가 필요하다.
    - **결과**: 고해상도 이미지일수록 파라미터는 기하급수적으로 늘어나며, 이는 곧 메모리 부족과 학습 속도 저하로 이어진다.
2. **공간적 구조(Topology)의 상실**
    [Image showing a grid being flattened into a 1D vector, losing spatial relationships]
    - **왜곡에 취약**: FC 레이어는 픽셀 간의 거리를 무시한다. 숫자 '7'이 이미지 왼쪽에 있든 오른쪽 아래에 있든, FC 레이어 입장에서는 완전히 다른데이터로 인식된다.
    - **변형에 취약**: 이미지를 1픽셀만 옆으로 밀거나(Translation), 살짝 비틀어도(Distortion) 입력 벡터 전체의 숫자가 바귀어버려 인식에 실패한다.

#### 왜 공간적 구조(Topology)가 중요한가?

<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/2710687b-6e4a-4bb4-bc74-3365f5799750" />

LeNet-5가 올바르게 인식한 비정상적이거나, 왜곡되거나, 잡음이 심한 문자들의 예시.
출력 라벨의 회색 명암(grey-level)은 패널티(오류율 또는 불확실도) 를 나타내며, 색이 밝을수록 높은 패널티(더 큰 불확실성) 를 의미한다.

```m
"픽셀은 혼자 존재하지 않는다. 주변 픽셀과 함께 있을 때만 의미를 갖는다."
```

LeCun은 이 논문에서 **'이미지는 일반적인 데이터와 다르다'**는 점을 강조

- **인접 픽셀의 상관관계**: 이미지에서 특정 픽셀이 검은색이라면, 바로 옆 픽셀도 검은색일 확률이 높습니다. 이러한 **국소적 연관성**이 모여 Edge(선)나 Corner(모서리)를 형성한다.
- **FC 레이어의 맹점**: FC 레이어는 픽셀의 순서를 무작위로 섞어버려도 학습 결과가 동일하다. 즉, 이미지의 가장 큰 특징인 **'공간적 정보'를 스스로 버리고 시작하는 셈**이다.
- **CNN의 해결책**: CNN은 픽셀의 공간적 순서를 유지한 채 특징을 추출하므로, 픽셀들이 뭉쳐서 만드는 '모양' 그 자체를 학습할 수 있게 된다.

<br />

---

## 📌 핵심 아이디어

### 국소적 수용 영역 (Local Receptive Fields)

<img alt="Image" src="https://github.com/user-attachments/assets/44cc9073-381a-4202-8b95-b50e47c5e54c" width='100%' />

> 국소 수용 영역이란 신경망의 한 유닛(뉴런)이 입력 이미지 **전체(Global)**를 보는 것이 아니라, 아주 작은 **일부분(Local)에만 집중**하여 연결되는 구조를 말한다.
> 이 덕분에 모델은 이미지 왜곡(Distortion)이나 위치 변화에 더 강인한(Robust) 특징을 할 수 있게 되었다.

#### 왜 필요한가?
- **생물학적 근거 (고양이 시각 실험)**: 동물의 시각 피질 세포들은 이미지 전체가 아니라, **특정 위치와 방향의 선(Edge)**에만 반응
- **공간적 계층 구조**: 이미지는 점 -> 선 -> 면 -> 형체 순으로 복잡해진다. 처음부터 전체를 보면 복잡하지만, 작은 영역 (Edge, Corner)부터 차근차근 파악하면 훨씬 효과적으로 사물을 인식할 수 있다.
- **특징 검출의 효율성**: 픽셀 하나하나의 절대적인 값보다, 주변의 픽셀과의 **상대적인 관계(패턴)**를 파악하는 것이 특징 추출에 유리하다.

#### 예시
1. **완전 연결 (Fully Connected) 방식**
   - **구조**: 한 명의 감시관(뉴런)이 32 x 32 전체 화면을 한꺼번에 감시한다.
   - **연결 수**: 32 x 32 = **1,024개**
   - **비효율성**: 이미지 왼쪽 끝의 픽셀과 오른쪽 끝의 픽셀은 서로 큰 상관관계가 없을 확률이 높은데도, 모든 관계를 다 계산해야 하므로 연산 낭비가 심하다.
  
2. **국소 수용 영역 (Local Receptive Fields) 방식**
   - **구조**: 한 명의 감시관(뉴런)이 5 x 5 크기의 돋보기를 들고 특정 구역만 집중해서 본다.
   - **연결 수**: 5 x 5 = 25개
   - **효율성**: 한 뉴런이 처리해야 할 정보가 **약 40배(1024 vs 25)** 줄어든다. 이렇게 줄어든 부담 덕분에 더 많은 뉴런(필터)를 배치하여 다양한 특징을 동시에 찾아낼 수 있게 된다.

### 가중치 공유 (Shared Weights)

<img alt="Image" src="https://github.com/user-attachments/assets/b42d62e9-4f63-49f1-9f56-4713289dd89d" width='1500px' />

> 가중치 공유는 **이미지의 한 지점에서 유효한 특징(Edge 등)은 다른 지점에서도 유효하다**라는 가정에서 출발한다.
> **하나의 특징 검출기(filter)를 이미지 전체에서 재사용**

#### 왜 필요한가?
- **공간적 불변성 (Translation Invariance)**: '강아지 귀' 특징이 이미지 왼쪽 위에 있든 오른쪽 아래에 있든, 똑같은 '귀 검출 필터'로 찾아낼 수 있어야 한다.
- **효율성**: 이미지의 모든 픽셀마다 서로 다른 특징 검출기를 만드는 것은 비효율적이며 학습이 거의 불가능하다.
- **과적합(Overfitting) 방지**: 학습해야 할 변수(파라미터)가 줄어들면, 모델이 복잡해지는 것을 막고 일반화 성능이 올라간다.

#### 예시

```m
조건: 입력 이미지 28 x 28 / 필터 크기 5 x 5 / Feature Map 6개 생성
```

1. **가중치 공유가 없을 때 (전통적인 MLP 방식)**
   - 출력되는 Feature Map의 한 픽셀마다 각기 다른 5 x 5 = 25개의 가중치가 필요
   - 출력 크기(28 x 28) x 필터 크기(25) x Feature Map 개수(6)
   - **계산**: 784 x 25 x 6 = **117,600개**
   - **문제점**: "이미지 구석의 가로선"을 찾는 법과 "이미지 중앙의 가로선"을 찾는 법을 각각 따로 학습해야 하므로 데이터가 무수히 많이 필요하다.
  
2. **가중치 공유를 사용할 때 (CNN 방식)**
   - **하나의 Feature Map**은 하나의 필터(5 x 5)만 사용한다. 이 필터가 이미지 전체를 훑으며 (Sliding Window) 같은 가중치로 연산한다.
   - (필터 크기 25 + 편향 1) x Feature Map 개수(6)
   - **계산**: (25 + 1) x 6 = **156개**
   - **결과**: 파라미터 수가 **약 750배 감소**

### 서브샘플링 (Sub-sampling/Pooling)

<img alt="Image" src="https://github.com/user-attachments/assets/bd8f77d2-b790-41ff-98b7-99dae35f5407" width='100%' />

> 서브샘플링은 앞 단계(Convolution)에서 추출된 **지역적인 특징(Local Feature)들의 데이터를 요약하여, 더 추상적이고 전역적인 특징(Global Feature)로 변환**하는 과정이다.
> 현재 딥러닝에서는 주로 'Max Pooling'을 쓰지만, LeNet-5 당시에는 'Average Pooling' 방식을 사용했다.
> 서브샘플링을 거치면 이미지의 공간 해상도를 의도적으로 감소시켜, 사소한 변화(기울어짐, 떨림)에 상관없이 핵심 특징을 유지할 수 있는 **견고성(Robustness)**을 얻게 되고, 
> 따라서 인접한 여러 픽셀의 정보를 하나로 요약해준다고 볼 수 있다.

#### 왜 필요한가??
- **위치 불변성 (Translation Invariance)**: 
  - 사람이 숫자 '7'을 쓸 때, 수평선의 위치는 1~2픽셀 정도 위아래로 바뀔 수 있다.
  - 시스템이 "정확히 10행 5열에 선이 있어야 한다"라고 외우면 조금만 삐뚫어져도 인식을 못한다.
  - 서브샘플링은 **'정확한 위치는 몰라도 그 그천에 수평선이 있다는 사실이 중요'**라고 정보를 흐리게(Blur) 만들어 왜곡을 강하게 만든다.
- **연산량 감소**: 이미지 크기를 가로/세로 절반씩 줄이면 전체 데이터 양은 **1/4**로 줄어들어 계산 효율로 비약적으로 상승한다.

#### 특징
LeNet-5의 서브샘플링은 단순히 숫자를 고르는 요즘의 Max Pooling과 달리 조금 더 복잡한 구조를 가진다.

- **2x2 Average Pooling**: 인접한 2 x 2 픽셀 영역의 평균값을 취하여 하나의 대표 값으로 만든다.
- **None-overlapping (겹치지 않음)**: 2 x 2 윈도우가 겹치지 않게 이동하며(Stride 2) 특징 맵의 크기를 절반(가로/세로)으로 줄인다.
- **학습 가능한 파라미터**: LeNet-5에서 단순히 평균만 내는 것이 아니라, 평균값에 학습 가능한 가중치(trainable coefficient)를 곱하고 편향(bias)를 더한 후 활성화 함수를 통과시킨다. 이는 모델이 스스로 정보 압축률을 조절하도록 한다.

| **구분** | Fully-Connected (전통적 MLP) | CNN (LeNet-5 방식) |
|---|---|---|
| **입력 형태** | 1차원 벡터 (이미지를 한 줄로 펼침) | 2차원 그리드 (이미지 형태 유지) |
| **공간 정보** | 무시 (픽셀 간의 거리를 고려하지 않음) | 유지 (Topology 정보를 보존하며 학습) |
| **변형 저항성** | 위치 변화(Translation)나 왜곡에 매우 취약 | 위치가 바뀌어도 특징을 찾아내어 유연하게 대응 |
| **학습 효율** | 파라미터가 급증하여 메모리 소모 및 과적합 위험 | 가중치 공유를 통해 파라미터 최적화 및 일반화 우수 |

<br />

---

## 🏗️ LeNet-5 아키텍처

<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/7262d1a9-6ffe-462a-b900-6e5256f50676" />

> LeNet-5는 총 **7개의 층(layer)**로 구성된 **합성곱 신경망(CNN)** 아키텍처로, 입력으로부터 점진적으로 추상적인 특징(feature)을 학습하도록 설계되었다.

| Layer      | 유형 (Type)                          | Output 크기 | 세부 설명 및 특징                                                                 | 주요 역할                    | 파라미터 수     |
| ---------- | ---------------------------------- | --------- | -------------------------------------------------------------------------- | ------------------------ | ---------- |
| **Input**  | 이미지 (Image)                        | 32×32×1   | MNIST 28×28을 32×32로 확장. 중앙 정렬 및 여백 확보 (정규화: 배경 -0.1, 전경 1.175)             | 입력 이미지 제공                | -          |
| **C1**     | 합성곱 (Convolution)                  | 28×28×6   | 5×5 커널, 6개 Feature Map 생성. `tanh` 활성화 사용.                                  | 저수준 특징 추출 (edge, line 등) | **156**    |
| **S2**     | 서브샘플링 (Sub-sampling / Avg Pooling) | 14×14×6   | 2×2 영역 평균 후 학습 가능한 weight + bias 적용. Stride 2 (겹치지 않음).                    | 공간 축소 및 위치 불변성 확보        | **12**     |
| **C3**     | 합성곱 (Convolution)                  | 10×10×16  | 5×5 필터, 16개 Feature Map. S2의 일부 맵들과만 **부분 연결 (비대칭 구조)**                    | 중간 수준 특징 조합, 표현 다양성 확보   | **1,516**  |
| **S4**     | 서브샘플링 (Sub-sampling / Avg Pooling) | 5×5×16    | 2×2 Average Pooling. Feature Map 크기 절반 감소.                                 | 고수준 특징의 위치 불변성 강화        | **32**     |
| **C5**     | 합성곱 (Convolution)                  | 1×1×120   | 5×5 커널, 120개 Feature Map. S4의 전체 노드(5×5×16)와 연결 → **Fully Connected처럼 동작** | 고차원 특징 통합 및 압축           | **48,120** |
| **F6**     | 완전 연결 (Fully Connected)            | 84        | 120 입력 유닛 → 84 출력 유닛. 출력층 구조에 맞는 중간 표현 벡터 생성.                              | 출력층에 적합한 추상화된 표현         | **10,164** |
| **Output** | RBF Layer                          | 10        | Euclidean Radial Basis Function (RBF) 사용. 각 클래스 중심과의 거리 기반 분류.             | 숫자 0–9 인식                | **850**    |

- **총 7개의 layer**: (C: Convolution, S: Subsamplint, F: Fully-Connected)
- **활성화 함수**: Hyperbolic tangent (`tanh`)
- **입력 크기 (Input)**: 32 x 32 pixel 흑백 이미지(grayscale) (정규화: 흰색 -0.1, 검은색 1.175)

### 왜 32 x 32 인가?
MNIST의 기본 크기인 28 x 28보다 크게 설정한 이유는 **가장자리 여백(margin)** 때문이다.
- 글자를 중앙에 정렬하면 receptive field가 이미지의 중심부를 고르게 커버한다.
- Conv 필터의 경계 손실을 방지해, edge나 corner의 정보를 온전히 학습할 수 있다.

<br />

---

## 🧠 Layer별 세부 구조

### Layer C1 - 첫 번째 합성곱층
- Input: 32x32x1
- Output: 28x28x6
- Filter: 5×5, 6개
- 학습 파라미터: (5×5 + 1) × 6 = 156개
- 연결 수: 122,304

#### 역할
- 원시 픽셀로부터 기초적인 시각 패턴(edge, curve, corner) 검출
- 모든 필터는 동일한 크기의 **국소 수용 영역(Local receptive field)** 을 공유하지만, 학습된 가중치에 따라 서로 다른 패턴을 감지한다.

### Layer S2 - 서브샘플링 (평균 폴링)
- Input: 28×28×6
- Output: 14×14×6
- Filter: 2×2 (stride 2, non-overlapping)
- 학습 파라미터: (1 weight + 1 bias) × 6 = 12개

#### 연산
각 2x2 영역의 평균값을 취하고,
이를 학습 가능한 가중치(weight)와 편향(bias)에 곱한 후 sigmoid 활성화 함수를 통과시킨다.

#### 의의
- 위치 변화(translation)에 대한 불변성 강화
- 데이터 압축 -> 계산 효율 향상
- 이후 층이 더 넓은 receptive field를 확보하도록 유도

### Layer C3 - 두 번째 합성곱층 (핵심)
- Input: 14×14×6
- Output: 10×10×16
- Filter: 5×5, 16개
- 학습 파라미터: 1,516개
- 연결 수: 151,600

<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/15ea00be-fd50-4319-8b89-af70f658256e" />

#### 특징
C3의 S2의 feature map 전체에 **완전 연결되지 않는다.**
특정 C3 feature map은 S2의 일부 subset과만 연결되어 있다.

| **C3 Feature Map** | **연결된 S2 Maps** |
| -------------- | ----------- |
| 0–5            | 연속된 3개      |
| 6–11           | 연속된 4개      |
| 12–14          | 불연속 4개      |
| 15             | 전체 6개       |

#### 이유
- **파라미터 폭발을 방지**하면서 다양성을 확보
- 네트워크의 **대칭성(symmetric structure)** 을 일부러 깨뜨려 서로 다른 feature 조합을 학습하게 함 -> **표현력 향상**

### Layer S4 - 서브샘플링
- Input: 10×10×16
- Output: 5×5×16
- Filter: 2×2, stride 2
- 학습 파라미터: 32개
- 연결 수: 2,000

#### 역할
- S2와 동일한 average pooling 구조
- 위치 불변성 추가 확보
- C5로 전달되는 feature의 크기를 크게 줄여 **연산 효율 증가**

### Layer C5 - 합성곱이지만 Fully Connected처럼 동작
- Input: 5×5×16
- Output: 1×1×120
- Filter: 5×5, 120개
- 학습 파라미터: (5×5×16 + 1) × 120 = 48,120개

#### 핵심 포인트

> 이 레이어는 더 이상 '부분 연결'이 아닌 **전 연결(full connection)** 구조처럼 작동한다.
> 즉, 각 뉴런은 이전 층의 **전체 5x5x16 노드와 연결**된다.
> 이는 고차원 특징을 종합적으로 통합하는 역할을 한다.

### Layer F6 - 완전 연결층
- Input: 120
- Output: 84
- 학습 파라미터: (120 + 1) × 84 = 10,164개

#### 왜 84개인가?
- 원 저자는 **ASCII 문자 세트**와 관련된 표현 벡터를 고려해 84차원으로 설정했다고 언급.
- 이후 Output Layer에서 숫자 0~9 분류를 위한 입력 형태로 적합하게 만듦

### Output Layer - RBF (Radial Basis Function)
- Input: 84
- Output: 10
- 파라미터: (84 + 1) × 10 = 850개
- 활성화: 유클리드 거리 기반 RBF

#### 의의
- 일반적인 softmax 대신, 각 출력 뉴런이 **"목표 벡터와의 거리"**로 분류를 수행
- 이 방식은 **유사도 기반의 클래스 구분**을 가능하게 함

<br />

---

## 학습 방법 (Training Method)

| 항목                      | 설명                                                           |
| ----------------------- | ------------------------------------------------------------ |
| **Loss Function**       | MSE (Mean Squared Error) 사용 — 출력(RBF)과 타깃 벡터 간의 거리 최소화.      |
| **최적화 알고리즘**            | **Backpropagation (역전파)** + **Gradient Descent** 로 모든 계층 학습. |
| **정규화 (Normalization)** | 입력 이미지를 평균 0, 분산 1로 정규화.                                     |
| **활성화 함수**              | Hyperbolic tangent (`tanh`).                                 |
| **학습 전략**               | 학습 데이터에 랜덤한 **왜곡(distortion)** 적용 → 일반화 성능 향상 (Fig. 7).      |
| **수렴 (Convergence)**    | 약 50~60회(epoch) 반복 후 안정적인 수렴 (Fig. 5).                       |

## 학술적 의의

- **CNN 발전의 시초**: LeNet-5는 AlexNet, VGG, ResNet 등의 직접적 전신.
- **패러다임 전환**: “특징(feature)은 설계하는 것이 아니라 학습되는 것”이라는
현대 딥러닝의 핵심 개념을 최초로 실증.
- **Gradient-Based Learning의 실용화**: 경사하강 기반 학습이 대규모 이미지 인식에서도 효과적임을 증명.

> LeNEt-5는 **Gradient-Based Learning** 의 가능성을 처음으로 산업·학문 양쪽에서 입증한 모델이다.
> 손글씨 인식이라는 단일 문제를 넘어서, “기계가 스스로 특징을 학습하는 방식”을 개척했다는 점에서
> 오늘날 모든 CNN의 원형(prototype)으로 평가된다.

## 참고자료
- 원논문: [LeCun et al., "Gradient-Based Learning Applied to Document Recognition", 1998](http://vision.stanford.edu/cs598_spring07/papers/Lecun98.pdf)
- [Arc Lab's Blog 논문 요약](https://arclab.tistory.com/150)
- [Logical Scribbles 논문 리뷰](https://stydy-sturdy.tistory.com/4)
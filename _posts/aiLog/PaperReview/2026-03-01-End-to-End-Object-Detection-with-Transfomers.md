---
layout: post
related_posts:
  - /aiLog/paperReview
title:  "DETR 논문 리뷰: End-to-End Object Detection with Transformers"
date:   2026-03-01
categories:
  - paperReview
description: >
  논문 정보             
  저자: Nicolas Carion, Francisco Massa, Gabriel Synnaeve, Nicolas Usunier, Alexander Kirillov, Sergey Zagoruyko (Facebook AI)          
  학회: ECCV 2020               
  arXiv: https://arxiv.org/abs/2005.12872               
  GitHub: https://github.com/facebookresearch/detr
---
* toc
{:toc .large-only}

## 🏷️ 논문 개요

### 논문 제목의 의미

**"End-to-End Object Detection with Transformers"**

- **End-to-End (종단간 구성)**: 전통적인 Object Detection 파이프라인에서 사람이 설계한 수많은 중간 단계(앵커 생성, NMS 후처리 등)를 제거하고, 입력 이미지에서 출력 예측까지 단일 신경망이 직접 처리함을 의미.
- **Object Detection (객체 탐지)**: 이미지 속 객체의 위치(바운딩 박스)와 클래스 레이블을 동시에 예측하는 컴퓨터 비전의 핵심 태스크.
- **Transformers**: NLP에서 혁신을 일으킨 Transformer 아키텍처의 셀프 어텐션(Self-attention) 매커니즘을 도입하여, 이미지 내 요소들 간의 상호작용을 모델링하고 전역적인 문맥 정보를 탐지에 활용한다.

즉, 제목 자체가 **객체 탐지를 Transformer 기반 구조로 재정의하고, 완전한 end-to-end 학습을 수행하겠다** 는 노문의 핵심 주장을 압축적으로 표현하고 있다.

### 한 줄 요약

> DETR(DEtection TRansformer)은 객체 탐지(Object Detection)를 "집합 예측(set prediction)" 문제로 재정의하고, CNN + Transformer Encoder-Decoder 아키텍처와 Hungarian 매칭 기반의 이분 매칭(Biparite Matching) 손실을 통해 NMS・앵커와 같은 수작업 구성 용소(Component) 없이 End-to-End로 학습을 가능하게 한 최초의 Transformer 기반 객체 탐지 모델이다.

### Abstract

본 논문은 Object Detection을 **직접적인 집합 예측(Direct Set Prediction) 문제** 로 바라보는 새로운 방법론을 제안한다. 이 접근 방식은 탐지 파이프라인을 단순화하여, 기존에 사전 지식을 명시적으로 인코딩하는 Non-Maximum Suppression(NMS, 비최대 억제) 절차나 앵커 생성 같은 수많은 수작업 설계 구성 요소의 필요성을 효과적으로 제거한다.

새로운 프레임워크인 **DERT(DEtection TRansformer)** 의 핵심 요소는 다음 두 가지이다.

1. **이분 매칭(Biparite Matching)을 통한 집합 기반 전역 손실(Set-based Global Loss)**: 
각 예측이 유일한 Ground Truth에 대응하도록 강제한다.
2. **Transformer Encoder-Deocder 아키텍처**: 고정된 소규모의 학습된 Object Query들을 기반으로, 객체 간 관계와 전역 이미지 문맥을 추론하여 최종 예측 집합을 병렬로 직접 출력한다.

DETR는 개념적으로 간단하며, 다른 많은 최신 탐지기들과 달리 특수 라이브러리를 필요로 하지 않는다. DETR 은 COCO Object Detection 데이터셋에서 잘 최적화된 Faster R-CNN 기준 모델과 비슷한 정확도 및 런타임 성능을 달성한다. 또한 범주형 분할(panoptic segmentation)을 통합된 방식으로 생성하도록 쉽게 일반화될 수 있고, 경쟁적인 기준 모델들을 크게 능가함을 보인다.

## 🕰️ 연구 배경

전통적인 객체 탐지 모델들은 제안(Proposals), 앵커(Anchors), 혹은 윈도우 센터(Window centers)와 같은 초기 추측치를 대량으로 생성하고 이를 정제하는 간접적이 방식을 취해왔다. 기계 번역이나 음성 인식과 같은 복잡한 구조적 예측 작업에서는 이미 종단간 (End-to-End) 방식이 주류로 자리 잡았으나, 객체 탐지에서는 중복 예측을 처리하기 위한 중복 제거 로직의 복잡성과 사전 지식에 의존하는 설계 방식 때문에 이러한 철학의 도입이 상대적으로 늦어지게 되었다.

DETR을 이해하기 위해서는 다음 세 가지 배경 지식이 필요하다.

### 1. 기존 Object Detection 패러다임

현대 Object Detection은 크게 두 계열로 분류된다.

#### 2-Stage Detector 
Region Proposal Network(RPN)으로 후보 영역을 먼저 추출하고, 해당 영역에 대해 분류 및 회귀를 수행한다.

- R-CNN -> Fast R-CNN -> Faster R-CNN
- Region Proposal -> Classification
- 장점: 높은 정확도
- 단점: 복잡한 구조

#### One-stage detector
앵커(Anchor) 박스를 격자 형태로 미리 설정하고, 각 앵커에 대해 바운딩 박스 오프셋과 클래스를 직접 예측한다.

- YOLO, SSD, RetinaNet
- Anchor 기반 dense prediction
- 장점: 빠름
- 단점: anchor 튜닝 필요

> 두 방식 모두 **앵커** 와 **NMS** 라는 수작업 설계 요소에 읮ㄴ하며, 이 요소들의 설계 방식이 성능에 결정적인 영향을 미친다.

#### 기존 방식의 공통 특징
 
- Anchor box 필요
- IoU threshold 기반 positive/negative 정의
- NMS 필요
- heuristic이 매우 많음

### Transformer 아키텍처

Vaswani et al.(2017)이 제안한 Transformer는 Self-Attention 매커니즘을 핵심으로 하며, 시퀀스 내 모든 요소 간의 관계를 전역적으로 모델링한다. Encoder-Decoder 구조로 이루어져 있으며, NLP 분야의 번역・생성 태스크에서 혁신을 가져왔다.

- **Self-Attention**: 각 요소가 시퀀스 전체를 참조하여 자신의 표현을 갱신한다.
- **Cross-Attention**: Decoder가 Encoder의 출력을 참조하여 예측을 수행한다.
- **Positional Encoding**: 위치 정보가 없는 Attention 구조에 순서 정보를 부여한다.

- Self-attention 기반
- CNN과 달리 global context modeling 가능
- NLO -> Vision Transfomer로 확장

### 헝가리안 알고리즘과 이분 매칭

헝가리안 알고리즘(Hungarian Algorithm)은 이분 그래프에서 최소 비용 완전 매칭을 다항 시간 내에 찾는 알고리즘이다. DETR에서는 N개의 예측과 M개의 Ground Truth(N > M) 사이에서 최적의 일대일 대응을 찾는데 사용된다. 이 매칭은 각 Ground Truth에 정확히 하나의 예측만 대응되도록 보장하여, 중복 예측 없이 집합 형태로 탐지 결과를 출력할 수 있게 한다.

<br />

---

## 🔍 문제 인식: 기존 방식의 한계점

### 문제가 무엇인가?

기존 Object Detection 방법들은 다음과 같은 **수작업 설계 요소(Hand-crafted Components)** 에 심하게 의존한다.

1. **앵커(Anchor) 생성**: 객체가 있을 법한 위치와 크기를 미리 정의한 사각형들의 집합. 객체의 스케일, 종횡비, 개수를 사전에 설계해야 하며 이 설계에 따라 성능이 크게 달라진다.
2. **Non-Maximum Suppression(NMS)**: 동일 객체에 대해 중복으로 생성된 예측 바운딩 박스 주 가장 신뢰도가 높은 것만 남기는 후처리 과정. NMS의 임계값(threshold)을 직접 조정해야 하며, 최적값이 데이터셋마다 다르다.
3. **복잡한 파이프라인**: 제안 영역 생성 -> 특징 추출 -> 분류 및 회귀라는 다단계 과정이 완전한 end-to-end 학습을 어렵게 만든다.

> 이러한 요소들로 인해 학습 파이프라인이 복잡해지고, 각 구성 요소를 별도로 튜닝해야 하는 엔지니어링 부담이 크다.

<br />

---

## 🔬 문제 분석

- **앵커 의존성**: 앵커의 스케일・종횡비 설계에 따라 성능이 좌우되며, 소규모 객체 탐지에 특히 민감하다.
- **NMS 후처리**: 하이퍼파라미터 수동 조정이 필요하고, 밀집된 객체 상황에서 오탐(false suppression) 발생 가능
- **비 End-to-End**: 각 단계가 독립적으로 설계되어 있어 전체 파이프라인의 공동 최적화가 불완전하다.
- **병렬 처리 한계**: RNN과 분류 헤드가 순차적으로 처리되어 추론 속도 향상에 구조적 제약이 있다.

기존 방식에서 중복 예측 문제가 반복되는 근본 원인은 객체 탐지를 **'집합 예측(Set Prediction)'**으로 직접 다루지 못하고, 개별 윈도우나 앵커에 대한 독립적인 분류 문제**(회귀・분류 문제의 집합)**로 치환했기 때문이다. 이로 인해 하나의 객체에 다수의 긍정 응답이 발생하며, 이를 해결하기 위해 NMS와 같은 후처리 로직에 의존하게 된다. 중복 예측을 없애기 위한 NMS는 신경망 내부에서 미분 가능한 방식으로 학습되지 않고, 추론 시 규칙 기반으로 적용된다. 앵커 역시 기울기가 흐르지 않는 고정된 prior이므로, 데이터에 맞게 자동으로 최적화되지 않는다. 즉, 모델의 성능이 신경망의 학습 능력뿐만 아니라 수동으로 설정된 포스트 프로세싱 단계에 의해 크게 좌우되는 구조적 한계를 가지고 있었다.

<br />

---

## 💡 문제 해결 방법

### 핵심 아이디어

DETR은 Object Detection을 **N개의 객체를 직접 예측하는 집합 예측 문제** 로 재정의한다. 핵심 아이디어는 다음 두 가지이다.

1. **이분 매칭 기반 Set Loss**: 예측과 GT 사이에 헝가리안 알고리즘으로 최적 일대일 매칭을 찾고, 그 매칭에 기반하여 손실을 계산함으로써 NMS없이 중복 없는 예측을 달성한다.
2. **Transforemr를 이용한 전역 문맥 모델링**: Self-Attention이 이미지 전체에서 객체 간 관계를 모델링하여, 앵커 없이 전역적으로 객체의 위치와 클래스를 예측한다.

### 아이디어의 이론적 정립 및 구현

#### Object Detection Set Prediction Loss

디코더는 고정된 N개의 예측을 출력한다.(N은 이미지 내 최대 객체 수보다 충분히 크게 설정. Ground Truth 집합 y와 N개의 예측 집합 ŷ 사이에서 헝가리안 알고리즘으로 최적 순열 σ̂을 탐색한다.

#### 매칭 비용 (Matching Cost):

#### Hungarian Loss (최종 손실):

#### 바운딩 박스 손실 (Box Loss):








그림 1: DETR은 일반적인 CNN과 트랜스포머 아키텍처를 결합하여 최종 탐지 결과 집합을 (병렬적으로) 직접 예측합니다. 학습 중에는 이분 매칭(bipartite matching)이 예측값과 실제 박스를 고유하게 할당합니다. 일치하는 것이 없는 예측은 "객체 없음" (\emptyset∅\emptyset∅) 클래스 예측을 출력해야 합니다.


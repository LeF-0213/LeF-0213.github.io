---
layout: post
related_posts:
  - /aiLog/ai
title:  "Object Detection(객체 탐지)"
date:   2026-01-07
categories:
  - ai
description: >
  
---
* toc
{:toc .large-only}

## Object Detection 이란?

Object Detection(객체 탐지)은 이미지나 영상에서 특정 객체의 존재 여부를 확인하고, 해당 객체의 위치를 바운딩 박스 (bounding box)로 표시하는 기술이다. 이는 컴퓨터 비전에서 중요한 분야로, 이미지 내에서 여러 개의 객체를 동시에 탐지하고 분류할 수 있다. Object Detection은 주로 딥러닝 기반의 CNN(합성곱 신경망) 모델을 활용하며, 대표적인 알고리즘으로는 R-CNN 계열(Faster R-CNN), Mask R-CNN, YOLO(You Only Look Once), SSD(Single Shot MultiBox Detector) 등이 있다. 이러한 기술은 자율 주행, 보안 감시, 의료 영상 분석, 증강 현실 등 다양한 분야에서 활용된다.

### Object Detection 논문 추천 리스트

<img src='https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2Fb9KkNt%2FbtsP221rku2%2FAAAAAAAAAAAAAAAAAAAAAHtv4Fe8_PKqlr0fAszJ2HurvEpDQ23zmpUHuphhnWBJ%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1769871599%26allow_ip%3D%26allow_referer%3D%26signature%3DPWngSs9kuBZfYv7upUULqPUkwU8%253D' />

## 객체 탐지 전통적인 기법

객체 탐지(Object Detection)에서 전통적인 기법이란 딥러닝이 등장하기 이전에 사용되던 방식으로, 핸드크래프트된(hand-crafted) 특징 추출과 기계학습 분류기를 결합한 방법을 말한다. 대표적으로 HOG(Histogram of Oriented Gradeints), SIFT(Scale-Invariant Feature Transform) 같은 특징 추출기를 통해 이미지에서 경계・패턴을 뽑아내고, 이를 SVM(Suppor Vector Machine)이나 Adaboost 같은 분류기에 입력하여 객체 여부를 판별했다. 또한, 탐지 과정에서는 윈도우 슬라이딩 기법이나 선택적 탐색(Selective Search) 같은 방법을 이용해 후보 영역을 만들어냈다. 이 방식은 당시 좋은 성과를 냈지만, 특징을 직접 설계해야 하고 복잡한 배경이나 다양한 스케일 변화에 약하다는 한계가 있어 이후 딥러닝 기반 객체 탐지로 빠르게 대체되었다.

### 특징 추출 (Feature Extraction)

- 핸드크래프트 특징이라고 불리며, 사람이 규칙을 설계해서 이미지를 수치로 바꾼다.
- **HOG (Historgram of Oriented Gradients)**: 이미지의 밝기 변화(경계선, 윤곽선)를 방향 히스토그램으로 표현
- **SIFT (Scale-Invariant Feature Transform)**: 크기나 회전에 변해도 동일하게 잡히는 특징적인 검출
- SURF, LBP등도 사용됨

> 이 과정을 통해 "이 이미지는 경계선이 많다. 둥근 패턴이 있다." 같은 수치화된 특징 벡터를 얻는다.

### 분류기 학습 (Classifier Training)

- 추출된 특징 벡터를 머신러닝 모델에 넣어 객체인지 아닌지를 판별한다.
- SVM (Support Vector Machine), Adaboost, 랜덤 포레스트 등
- 예: 얼굴 탐지 -> 얼굴 특징(HOG) 추출 -> SVM이 "얼굴이다/아니다" 판별

### 후보 영역 탐색 (Region Proposal)

- 이미지를 그냥 한 번에 다 보는 게 아니라, **객체가 있을 법한 위치를 찾아내는 과정**이 필요했다.
- **슬라이딩 윈도우 (Sliding Window)**: 이미지를 작은 창(window)로 조금씩 옮겨가며 탐색
- **선택적 탐색 (Selective Search)**: 색·질감·경계선 등을 기준으로 객체 후보 영역 생성

<img src='https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FpZWeQ%2FbtsP20vNFH4%2FAAAAAAAAAAAAAAAAAAAAAJuSxdZxanTfJEWQGYjSRlMm6s3mF3rJ33u7HQNwDlcv%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1769871599%26allow_ip%3D%26allow_referer%3D%26signature%3D5ULZysZAw%252Bnd2gIzKGP7uGmSnDE%253D' />

## 🧭 객체 탐지 딥러닝 기반 기법 (Deep Learning-Based Object Detecton)

딥러닝 기반 객체 탐지(Object Detection)는 **탐지 단계의 구조**에 따라 크게 **1단계 탐지(One-Stage)**와 **2단계 탐지(Two-Stage)**로 구분된다.

> 과거에는 각 방식마다 명확한 장단점이 있었지만,        
> 이후 기술 발전으로 서로의 단점을 보완하며 **정확도는 높아지고**,      
> **실시간 적용성도 개선**되어          
> **현재는 두 방식이 모두 널리 사용되는 추세**이다.

### 1단계 탐지 (One-Stage Detection)

<img width="100%" alt="Image" src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FzKSnR%2FbtsMp9b6y9b%2FAAAAAAAAAAAAAAAAAAAAANUCdH2cBYlojdzdeCTpbDj0iOF2TK94AG0WZa4Q9yFA%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1769871599%26allow_ip%3D%26allow_referer%3D%26signature%3D55IGN6TlLHp9M8XKcg9%252BX4DA41M%253D" />

```m
"한 번에 모든 걸 예측한다."
```

#### 개념
1단계 탐지는 **한 번의 네트워크 추론(Forward Pass)** 만으로         
**객체의 위치(Localization) 와 클래스(Classification) 을 동시에 예측하는 방식** 이다.

입력 이미지를 **격자(Grid)** 또는 **앵커 박스(Anchor Box)** 단위로 나누고,          
각 셀(cell)에서 객체의 존재 여부(Objectness), 좌표(x, y, w, h), 클래스 확률을 바로 출력한다.


### 2단계 탐지 (Two-Stage Detection)

```m
"먼저 후보 영역을 찾고, 그다음에 세밀하게 분류한다."
```

<img width="100%" alt="Image" src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2Fbkud5b%2FbtsMpJYZsfl%2FAAAAAAAAAAAAAAAAAAAAAD5cYgg5afM2zg9Rj2EPACfhoJ4IuGwGaPooyn4m3nCF%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1769871599%26allow_ip%3D%26allow_referer%3D%26signature%3DfBWeIQGcEktTopUtiId2ylBlzak%253D" />

<img width="1536" height="1024" alt="Image" src="https://github.com/user-attachments/assets/b06884ff-ee89-4d55-8d95-52f8cbb685e1" />


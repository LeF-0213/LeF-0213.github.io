---
layout: post
related_posts:
  - /aiLog/ai
title:  "파이토치(PyTorch) - 딥러닝"
date:   2025-12-18
categories:
  - ai
description: >
  
---
* toc
{:toc .large-only}

# 생물학적 뉴런

생물학적 뉴런은 신경계를 구성하는 기본 단위로, 정보를 수집, 처리, 전달하는 기능을 담당한다. 뉴런은 크게 세 가지 주요 부분으로 나뉜다. 수상돌기(dendrite), 세포체(cell body), 축삭()

### 시그모이드

## CNN

### 스트라이드 (Stride)

스트라이드는 합성곱 연산에서 필터(커널)가 입력 데이터(이미지) 위를 이동하는 간격을 의미한다. 기본적으로 스트라이드 값이 1이면 필터가 한 칸씩 움직이며 모든 위치에서 연산을 수행한다. 스트라이드 값이 2 이상이면 필터가 더 큰 간격으로 이동하므로, 생성되는 출력 크기가 작아지고 연산량도 줄어든다. 스트라이드를 조절하면 특징 맵의 크기를 조정할 수 있어, 모델의 계산 효율성을 높이거나 더 큰 영역의 정보를 한 번에 처리할 수 있다. 다만, 스트라이드가 너무 크면 중요한 세부 정보가 손실 될 수 있으므로 적절한 값을 선택하는 것이 중요하다.

<img src='https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2Fb1g3su%2FbtsLNXCHsg5%2FAAAAAAAAAAAAAAAAAAAAAOxcELK03iP-OlhwIoGHpptycMHGHq9pRuOpTHJF9fds%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1767193199%26allow_ip%3D%26allow_referer%3D%26signature%3D8ioU4Pre7iKg%252BMo%252FkP6iUuqcFyw%253D' />

### 패딩 (Padding)

패딩은 합성곱 연산에서 입력 데이터(이미지)의 가장자리에 값을 추가하여 출력 크기를 조정하거나 경계 부분의 정보 손실을 방지하는 기법이다. 일

### 풀링 (Pooling)

풀링은 CNN에서 특정 맵의 크기를 줄이고, 중요한 정보를 요약하여 계산 효율성을 높이는 과정이다. 주로 사용되는 방법은 최대 풀링(Max Pooling)과 평균 풀링(Average Pooling)으로 

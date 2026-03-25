---
layout: post
related_posts:
  - /ai/basic
title:  "추천 알고리즘 (Recommendation-Algorithm)"
date:   2026-03-24
categories:
  - ai
description: >
   개념 이해 -> 수학적 원리 -> Python 구현
---
* toc
{:toc .large-only}

## 🗺️ 추천 알고리즘 큰 그림

![alt text](../../image/image.png)

# 협업 필터링 (Collaborative Filtering)

"나와 비슷한 사람이 좋아한 것을 나도 좋아할 것이다."
유저 - 아이템 상호작용 데이터(평점, 클릭, 구매)만으로 추천

## User-based Collaborative Filtering

유저 A 와 취향이 비슷한 유저들을 찾아서 (이웃 유저), 그들이 좋아했지만 A 는 아직 보지 않은 아이템을 추천한다.

### 핵심 3 단계

1. 유저-아이템 평점 행렬(Rating Matrix) 구성
2. 타겟 유저와 다른 유저들 간 유사도 계산
3. 유사도 가중 평균으로 미평가 아이템 점수 예측

- **장점:** 직관적, 구현 간단
- **단점:** 유저 수 많을수록 계산량 O($n^2$) 폭발, 희소 행렬 문제 (Sparsity), 콜드 스타트

## 📐 수식

### Cosine 유사도

두 유저가 **"얼마나 같은 방향의 취향을 가졌는가"** 를 측정한다.

$$sim(u,v) = \frac{\sum_{i \in I_{uv}} r_{ui} \cdot r_{vi}}{\sqrt{\sum_{i \in I_{uv}} r_{ui}^2} \cdot \sqrt{\sum_{i \in I_{uv}} r_{vi}^2}}$$


- u, v: 비교 대상이 되는 두 유저(User)입니다.
- $$

### Person 상관계수 - 평점 스케일 편향 제거 (권장)

$$sim(u, v) = \frac{\sum (r_{ui} - \bar{r}_u)(r_{vi} - \bar{r}_v)}{\sqrt{\sum (r_{ui} - \bar{r}_u)^2} \sqrt{\sum (r_{vi} - \bar{r}_v)^2}}$$

### 예측 평점

$$\hat{r}_{ui} = \bar{r}_u + \frac{\sum_{v \in N} sim(u, v) \cdot (r_{vi} - \bar{r}_v)}{\sum_{v \in N} |sim(u, v)|}$$

- $N(u)$: 타겟 유저 u의 Top-K 이웃 집합

## User-based CF

## Item-based CF

## Matrix Factorization (SVD, ALS)

## Neural Collaborative Filtering (NCF)

# 콘텐츠 기반 필터링 (Content-Based Filtering)

## TF-IDF + Cosine 유사도

## 아이템 임베딩 기반

# 하이브리드 (Hybrid)

## Weighted Hybrid

## Cascade Hybrid

# 최신 기법

## Two-Tower 모델

## GNN 기반 (LightGCN)

## 강화학습 기반

## LLM 기반

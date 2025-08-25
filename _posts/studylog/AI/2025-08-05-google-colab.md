---
layout: post
related_posts:
  - /studylog/ai
title:  "구글 코랩(Google Colab)이란?"
date:   2025-08-05
categories:
  - studylog
  - ai
description: >
  Google Colab의 기본 개념 정리
---
* toc
{:toc .large-only}

# 구글 코랩이란?

## 구글 코랩의 주요 특징
### 무료 GPU/TPU 사용
코랩은 기본적으로 CPU 외에도 GPU(그래픽 처리 장치)와 TPU(Tensor Processing Unit)를 무료로 제공한다.
GPU가 병렬처리를 가능하게 하기에 필요한 하드웨어가 없어도 대규모 딥러닝 모델을 훈련하거나 고속 연산을 처리하는데 매우 유용하다.
### 클라우드 기반 
모든 코드 실행과 데이터 처리가 구글 클라우드 서버에서 이루어진다. 따라서 로컬 컴퓨터의 성능에 의존하지 않기 때문에, 사양이 낮은 컴퓨터에서도 복잡한 작업을 처리할 수 있다.
### Jupyter 노트북과 호환

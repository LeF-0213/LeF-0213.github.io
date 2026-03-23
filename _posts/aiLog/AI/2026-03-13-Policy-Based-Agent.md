---
layout: post
related_posts:
  - /aiLog/ai
title:  "Policy-based Agent"
date:   2026-03-13
categories:
  - ai
description: >
    왜 이 알고리즘이 필요했는가
    Policy ・ Gradient ・ REINFORCE ・ Actor-Critic ・ A2C ・ TD Actor-Critic
---
* toc
{:toc .large-only}

# 알고리즘 발전의 흐름

{: .info-box}
각 알고리즘은 이전 방법의 문제점을 해결하기 위해 순서대로 등장했다.

{: .center}
```md
### Policy Gradient Theorem
이론적 토대 확립 - 환경 모델 없이도 정책을 학습할 수 있는 수학적 근거
                    ↓
### REINFORCE
첫 번째 실천 - 
                    ↓
### Actor-Critic
                    ↓
### A2C / A3C (Advantage Actor-Critic)

```

# Policy Gradient Theorem

Policy Gradient Theorem 은 정책 기반 강화학습에서 에이전트의 정책(policy)를 어떻게 개선해야하는지를 수학적으로 설명하는 정리이다.

{: .star-list}
- **정책(Policy)**: 에이전트가 어떤 상태($s$)에서 어떤 행동($a$)을 할지 결정하는 전략($\pi$).
- **경사/그래디언트(Gradient)**: 보상을 최대화하기 위해 정책의 파라미터($\theta$)를 어느 방향으로 수정해야 하는지 나타내는 기울기($\nabla$).
- **정리(Theorem)**: 환경의 전이 확률(물리 법칙)을 몰라도 샘플링만으로 이 기울기를 구할 수 있음을 수학적으로 증명한 법칙.

에이전트가 더 많은 보상을 얻도록 정책을 업데이트하려면, 선탹헌 행동의 로그 확률에 그 행동의 가치(Q 값 또는 Return)를 곱한 방향으로 정책 파라미터를 조정하면 된다.

- 높은 행동을 얻은 행동의 확률 → 높임
- 낮은 보상을 얻은 행동의 확률 → 낮춤

> 모델 없이 경험으로부터 직접 정책을 학습 가능          
> → REINFORCE, Actor-Critic 등의 이론적 기반

## 왜 Policy Gradient Theorem 이 필요한가?

{: .star-list}
- **가치 기반:** 행동의 가치(Q 값)를 계산해서 가장 큰 행동을 선택
- **정책 기반:** 행동을 선택하는 규칙 자체를 직접 학습

> 정책을 더 좋은 방향으로 바꾸기 위해 정확히 어떻게 업데이트해야 하는지 그 방향을 제시하는 것이 Policy Gradient Theorem 이다.

## 수식 분해

$$\nabla_\theta J(\theta) = \mathbb{E}_{\pi_\theta} \left[ \sum_{t=0}^{T} \nabla_\theta \log \pi_\theta(a_t|s_t) Q^{\pi_\theta}(s_t, a_t) \right]$$

{: .star-list}
- **∇θ​J(θ)**: 정책을 더 좋은 방향으로 바꾸기 위한 기울기. "어느 방향으로 수정하면 보상이 커질까?"
- **logπθ​(a∣s)**: 현재 상태에서 선택한 행동의 로그 확률. 행동 확률을 조절하는 수학적 도구
- **∇θ​logπθ​(a∣s)**: 파라미터를 조금 바꿨을 때 행동 확률이 얼마나 변하는지 (정책의 민감도)
- **Qπ(s,a)**: 상태 s 에서 행동 a 를 했을 때 앞으로 받을 것으로 기대되는 보상

> **log가 들어간 이유**         
> - 확률의 곱을 다룰 때 log를 취하면 덧셈으로 바뀌어 미분 계산이 훨씬 편해지기 때문이다.

# REINFORCE 알고리즘


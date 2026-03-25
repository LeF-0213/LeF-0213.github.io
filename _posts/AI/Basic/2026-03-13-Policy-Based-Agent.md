---
layout: post
related_posts:
  - /ai/basic
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
- 이론적 토대 확립 - 환경 모델 없이도 정책을 학습할 수 있는 수학적 근거
                    ↓
### REINFORCE
- 첫 번째 실천 - 에피소드 끝나고 한꺼번에 학습
- 문제: High Variance - 한 번의 운/불운이 학습을 흔든다.
                    ↓
### Actor-Critic
- 해결책: Critic 으로 실시간 평가 - 분산 감소
- 문제: Q(s,a) 직접 사용 - 기준점 없어 불안정
                    ↓
### A2C / A3C (Advantage Actor-Critic)
- 최종 해결: A(s,a) = Q(s,a) - V(s) - 평균 대비 얼마나 좋은가?
- A3C: 비동기 병렬 학습 / A2C: 동기 병렬 학습
```

<br/>

---

# Policy Gradient Theorem

정책 기반 강화학습을 하려면 한 가지 근본적인 질문에 답해야 한다.

> "환경이 어떻게 움직이는지 (P(s'⎮s,a)) 를 모르는데, 어떻게 정책을 개선할 수 있는가?"

Policy Gradient Theorem 은 이 질문에 대한 수학적 답이다. 환경 모델을 몰라도 실제 경험(샘플)만으로 정책을 개선하는 방향을 계산할 수 있음을 증명한다.

{: .star-list}
- **정책(Policy)**: 에이전트가 어떤 상태($s$)에서 어떤 행동($a$)을 할지 결정하는 전략($\pi$).
- **경사/그래디언트(Gradient)**: 보상을 최대화하기 위해 정책의 파라미터($\theta$)를 어느 방향으로 수정해야 하는지 나타내는 기울기($\nabla$).
- **정리(Theorem)**: 환경의 전이 확률(물리 법칙)을 몰라도 샘플링만으로 이 기울기를 구할 수 있음을 수학적으로 증명한 법칙.

에이전트가 더 많은 보상을 얻도록 정책을 업데이트하려면, 선탹헌 행동의 로그 확률에 그 행동의 가치(Q 값 또는 Return)를 곱한 방향으로 정책 파라미터를 조정하면 된다.

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

## 왜 log 를 사용하는가?

확률 $\pi(a⎮s)$ 를 직접 미분하면 계산이 복잡해지고, 확률의 곱이 등장할 때 처리가 어렵다. log를 취하면 두 가지 이점이 생긴다.

{: .star-list}
- **곱 → 합:** log(a x b) = log(a) + log(b). 확률의 곱을 합으로 바꿔 계산이 단순해짐
- **기대값 형태로 변환:** E[...] 형태로 만들 수 있어 실제 샘플로 추정 가능 - 환경 모델 불필요

> 경험 (s, a, r) 만으로도 기울기를 계산할 수 있다.
> - ∇θ​πθ​(a∣s) ← 직접 미분: 복잡하고 환경 모델 필요
> - ∇θ​logπθ​(a∣s) ← log 미분: = ∇θ​π / θ → 샘플로 추정 가능

<br/>

---

# REINFORCE 알고리즘

Policy Gradient Theorem 을 가장 단순하게 구현한 알고리즘이다.           
에피소드 전체를 실행한 뒤 각 시점의 Return(누적 보상)을 이용해 정책을 업데이트한다.

## 동작 원리

$$\nabla_\theta J(\theta) = \mathbb{E}_{\pi_\theta} \left[ \nabla_\theta \log \pi_\theta(a_t|s_t) G_t\right]$$

```md
### 1️⃣ 정책에 따라 에피소드 끝까지 실행
                ↓
### 2️⃣ 각 시점의 t의 Return Gt 계산
$$Gt = rt + γrt+1 + γ^2rt+2 + ...$$
                ↓
### 3️⃣ 정책 업데이트
$$\theta ← \theta + \alpha x ∇\thetalog\pi\theta(at⎮st) x Gt$$
                ↓
### 4️⃣ 반복
```

> 보상이 큰 행동은 확률을 높이고, 보상이 작은 행동은 확률을 낮추는 방향으로 학습한다.

## REINFORCE 의 문제점 - High Variance

### ⚠️ High Variance (높은 분산)

에피소드 전체의 Return 을 사용하기 때문에,
- 운 좋게 높은 보상을 받는 에피소드 → 모든 행동의 확률을 과도하게 올림
- 운 나쁘게 낮은 보상을 받는 에피소드 → 모든 행동의 확률을 과도하게 내림

> 결과: 학습이 불안정하고 느리다.


<br/>

---

# 구현 - CartPole-v1

```python
import gymnasium as gym
import torch
import torch.nn as nn
import torch.nn.functional as F
import torch.optim as optim
# 모델이 주는 점수들을 확률 분포로 바꾸고, 그 확률에 따라 랜덤하게 행동을 선택하게 한다.
from torch.distributions import Categorical 

# =============
# 하이퍼 파라미터
# =============
learning_rate = 0.0002
gamma = 0.98 # 미래 보상 할인율

# =============
# 신경망 구조
# =============
class Policy(nn.Module):
    def __init__(self):
        self.fc1 = nn.Linear(4, 128)    # 입력: 4 개 상태
        self.fc2 = nn.Linear(128, 2)    # 출력: 2 개 행동 확률
        self.optimizer = optim.Adam(self.parameters(), lr=learning_rate)

    def forward(self, x):
        x = F.relu(self.fc1(x))
        x = F.softmax(self.fc2(x), dim=0)   # 확률 합 = 1
        return x

    def put_data(self, item):
        self.data.append(item)

    # =============
    # 학습 로직
    # =============
    def train_net(self):
        # 누적 보상(Return)
        R = 0
        self.optimizer.zero_grad()

        # 마지막 스텝부터 처음 스텝까지 거꾸로 순회 (점화식 재사용)
        # Return 은 뒤에서부터 계산하면 편함
        for r, prob in self.data[::-1]:
            R = r + gamma * R           # Gt = Rt+1 + yGt+11
            loss = -torch.log(prob) * R # -log𝝅 * G : 보상 클수록 확률 높임
            loss.backward()             # - 부호: 좋은 행동의 확률을 최대화

        self.optimizer.step()
        self.data = []

# =============
# 메인 루프
# =============
def main():
    env = gym.make('CartPole-v1')
    pi = Policy()
    score = 0.0
    print_inerval = 20

    for n_epi in range(1000):
        s, _ = env.reset()
        done = False

        while not done:
            prob = pi(torch.from_numpy(s).float())
            m = Categorical(prob)           # 확률 분포 생성
            a = m.sample()                  # 확률적으로 행동 샘플링
            s_prime, r, done, _, _ = env.step(a.item())
            pi.put_data((r, prob[a]))       # (보상, 선택한 행동의 확률) 저장
            s = s_prime
            score += r
        pi.train_net()                      # 에피소드 종료 후 업데이트 

        if n_epi % print_interval == 0 and n_epi != 0:
            print("# of episode :{}, avg score : {}".format(n_epi, score/print_interval)
            score = 0.0
    env.close()

if __name__ == "__main__":
    main()
```

## 📊 학습 결과

```
# of episode :20, avg score : 19.0
# of episode :40, avg score : 23.25
...
# of episode :920, avg score : 148.7
# of episode :940, avg score : 130.95
# of episode :960, avg score : 118.0
# of episode :980, avg score : 129.7
```

```
# of episode :20, avg score : 20.55
# of episode :40, avg score : 22.15
...
# of episode :920, avg score : 126.5
# of episode :940, avg score : 165.3
# of episode :960, avg score : 149.35
# of episode :980, avg score : 180.95
```

### 분석

|에피소드|1회 실행|2회 실행|추세|
|-----|-------|------|------------|
|**#20**| 19.0 | 20.55 | 초반 낮은 점수  |
|**#200**| 23.1 | 37.4  | 완만한 상승 |
|**#500**| 52.75 | 60.5 | 꾸준한 성장 |
|**#780**| 123.1 | 122.75 | 고득점 진입 |
|**#980**| 129.7 | 180.95 | 지속 향상 |

<br/>

---

# Actor-Critic (Reducing Variance)

REINFORCE 의 High Variance(높은 분산) 문제를 해결하기 위해 등장했다. 에피소드가 끝날 때까지 기다리지말고, 매 스텝마다 Critic의 평가를 받아 즉시 학습하자는 아이디어이다.

### 해결책: Critic을 도입해 실시간으로 행동을 평가

{: .star-list}
- **REINFORCE:** 에피소드 종료 후 전체 Return Gt 사용 → 분산 큼
- **Actor-Critic:** 매 스텝 Critic의 TD Error 𝛿 사용 → 분산 줄어듦

## 역할 분담

### Actor

{: .star-list}
- **역할:** "무엇을 할까?"" - 행동 선택
- **학습 대상:** 정책 $\pi(a⎮s)$ - 행동 확률

### Critic

{: .star-list}
- **역할:** "얼마나 좋았나?" - 평가
- **학습 대상:** 가치 V(s) 또는 Q(s,a)

## TD Error - Critic 의 평가 신호

$$𝛿 = r + γ・V(s') - V(s)$$

|TD Error|의미|Actor 반응|
|----|------------|--------------|
|**𝛿 > 0 (양수)**| 예상보다 결과가 좋았다. | 방금 행동의 확률 높임 |
|**𝛿 < 0 (음수)**| 예상보다 결과가 나빴다. | 방금 행동의 확률 낮춤 |
|**𝛿 = 0**| 예측이 정확했다. | 업데이트 없음 |

## 전체 흐름

```md
                상태 s 관찰
                    ↓
        Actor: 𝝅(a⎮s) 로 행동 a 선택
                    ↓
        환경: 보상 r, 다음 상태 s' 반환
                    ↓
    Critic: 𝛿 = r + γV(s') - V(s) 계산
                    ↓
Actor 업데이트: 𝛿 > 0 이면 a 확률 ↑, 𝛿 < 0 이면 ↓
                    ↓
    Critic 업데이트: V(s)를 더 정확하게 수정
```

## REINFORCE vs Actor-Critic 비교

|항목|REINFORCE|Actor-Critic|
|-------|-------------|---------------|
|**업데이트 시점**| 에피소드 종료 후 | 매 스텝마다 즉시 |
|**사용하는 값**| 전체 Return Gt | TD Error 𝛿 |
|**분산**| 크다 (불안정) | 작다 (안정적) |
|**긴 에피소드**| 비효율적 | 효율적 |

<br/>

---

# 최적화의 정점 - Q / Advantage & A2C / A3C

## Q Actor-Critic 의 한계

{: .info-box}
Actor-Critic 에서 Q(s,a) 를 직접 사용하면 한 가지 문제가 생긴다.

### ⚠️ 기준점(Baseline)이 없다.

$$예: Q(s, 왼쪽) = 100, Q(s, 오른쪽) = 110$$

오른쪽이 좋긴 하지만, 둘 다 높은 값이라 왼쪽도 확률이 올라갈 수 있음

> 어떤 행동이 '평균보다' 좋은지 알 수 없어 업데이트가 불안정

## Advatage - 평균 대비 얼마나 좋은가?

{: .info-box}
Advantage 는 특정 행동이 해당 상태의 평균적인 행동보다 얼마나 더 좋은지를 나타낸다.

$$A(s,a) = Q(s,a) - V(s)$$

{: .star-list}
- **A(s,a) > 0:** 이 행동이 평균보다 좋음 → 확률 높임
- **A(s,a) < 0:** 이 행동이 평균보다 나쁨 → 확률 낮춤
- **A(s,a) = 0:** 평균과 동일 → 업데이트 없음

### Advantage 의 효과

$$Q(s,왼쪽)=100, V(s)=105 → A = -5 → 확률 낮춤$$
$$Q(s, 오른쪽)=110, V(s)=105 → A = +5 → 확률 높임$$

> 절대 점수가 아닌 '상대적 우위'로 판단 → 분산 감소, 학습 안정

실제 구현에서는 Q(s,a) 를 별도로 계산하는 대신 TD Error 𝛿 = r + γV(s') - V(s) 를 Advantage 의 근사값으로 사용한다.

## TD Actor-Critic - 매 스텝마다 Critic 업데이트

TD Actor-Critic 은 Critic 이 V(s)를 TD 학습으로 업데이트하고, 그 TD Error 를 Actor 의 학습 신호로 사용하는 방법이다. 에피소드가 끝나기를 기다리지 않고 매 스텝마다 즉시 업데이트할 수 있어 속도와 안전성이 향상된다.

### 코드 구현

```python
import grmnasium as gym
import torch
import torch.nn as nn
import torch.nn.functional as F
import torch.optim as optim
import numpy as np
from torch.ditributions import Categorical

# Hyperparameters
learning_rate = 0.0002
gamma = 0.98
# 한 번 학습하기 전 몇 단계 동안 데이터를 모을지 설정
n_rollout = 10

class ActorCritic(nn.Module):
    def __init__(self):
        super(ActorCritic, self).__init__()
        self.data = []

        self.fc1 == nn.Linear(4, 256)
        # 정책용 출력층, 0: 왼쪽, 1: 오른쪽
        self.fc_pi = nn.Linear(256, 2)
        # 가치용 출력층, V(s)
        self.fc_v = nn.Linear(256, 1)
        self.optimizer = optim.Adam(self.parameters(), lr=learning_rate)

    def pi(self, x, softmax_dim = 0):
        x = F.relu(self.fc1(x))
        x = self.fc_pi(x)
        prob = F.softmax(x, dim=softmax_dim)
        return prob

    def v(self, x):
        x = F.relu(self.fc1(x))
        v = self.fc_v(x)
        return v

    # transition 안에는 현재상태, 행동, 보상, 다음상태, 종료여부
    def put_data(self, transition):
        self.data.append(transition)

    def make_batch(self):
        s_lst, a_lst, r_lst, s_prime_lst, done_lst = [], [], [], [], []
        for transition in self.data:
            s, a, r, s_prime, done = transition
            s_lst.append(s)
            a_lst.append([a])
            # 보상 스케일을 줄여 학습을 안정화
            r_lst.append([r / 100.0])
            s_prime_lst.append(s_prime)
            # 종료 상태에서는 미래 가치를 더하지않게 하기 위함
            done_mask = 0.0 if done else 1.0
            done_lst.append([dome_mask])

        s_batch = torch.tensor(np.array(s_lst), dtype=torch.float)
        a_batch = torch.tensor(np.array(a_lst), dtype=torch.long)
        r_batch = torch.tensor(np.array(r_lst), dtype=torch.float)
        s_prime_batch = torch.tensor(np.array(s_prime_lst), dtype=torch.float)
        done_batch = torch.tensor(np.array(done_lst), dtype=torch.float)

        self.data = []
        return s_batch, a_batch, r_batch, s_prime_batch, done_batch

    def train_net(self):
        s, a, r, s_prime, done = self.make_batch()

        # self.v(s_prime): 다음 상태의 가치
        td_target = r + gamma * self.v(s_prime) * done
        # 현재 상태 가치 예측값이 실제 타깃보다 얼마나 작은지/큰지 계산
        delta = td_target - self.v(s)

        # ===========================================
        # Actor 업데이트: 𝛿 방향으로 선택한 행동의 확률 조정
        # ===========================================
        # 각 행이 하나의 상태
        # 각 열이 행동 0, 행동 1의 확률
        pi = self.pi(s, softmax_dim=0)
        # 실제로 선택했던 행동의 확률
        # pi = [[0.7, 0.3], [0.2, 0.9]]
        # a = [[0], [1]]
        pi_a = pi[a]
        # 델타값이 크다는 것은 전체적인 행동확률을 높이려고 학습, 낮으면 행동률 낮추려고
        # delta > 0: 좋은 결과. 그 행동 확률을 높이도록 학습
        # delta < 0: 나쁜 결과. 그 행동 확률을 낮추도록 학습
        # L1(MAE), L2(MSE)의 장점을 섞은 손실 함수: 이상치에 강함
        loss_pi = -torch.log(pi_a) * delta.detach() + F.smooth_li_loss(self.v(s), td_target.detach())

        # ===========================================
        # Critic 업데이트: V(s)를 td_target 에 가깝게 수정
        # ===========================================

```



---
layout: post
related_posts:
  - /aiLog/ai
title:  "강화학습(Reinforcement Learning, RL)"
date:   2026-03-03
categories:
  - ai
description: >
  
---
* toc
{:toc .large-only}

# 지도학습과 강화학습

## 지도학습 (Supervised Learning)

지도학습은 정답(라벨)이 포함된 데이터로 모델을 학습시키는 방식이다. 입력 X 와 그에 대응하는 정답 Y 를 함께 제공하여, 모델이 X -> Y 로 매핑하는 함수를 학습한다.

{: .star-list}
- **학습 방법:** 예측값과 실제 정답 사이의 오차(Loss)를 계산하고, 이를 최소화하도록 가중치를 업데이트
- **대표 문제:** 분류(Classification), 회귀(Regression)
- **활용 사례:** 이미지 분류, 스팸 메일 판별, 주가 예측

> "정답이 있는 데이터로 오차를 줄이며 학습한다."

## 강화학습 (Reinforcement Learning)

강화학습은 정답을 직접 주지 않고, 행동에 대한 보상(Reward)을 통해 학습하는 방식이다.

{: .star-list}
- **학습 구조:** 에이전트(Agent)가 환경(Environment)과 상호작용하며 행동(Action)을 선택
- **목표**: 받은 보상을 최대화하는 방향으로 정책(Policy)을 학습
- **활용 사례**: 게임 AI, 자율주행, 로봇 제어

> "정답 대신 보상을 통해 최적 행동 전략을 학습한다."

<br />

---

# 순차적 의사결정

에이전트가 한 번의 선택이 아니라 시간의 흐름에 따라 여러 번 행동을 선택하고, 그 결과가 이후 상태와 보상에 영향을 미치는 구조이다.

현재의 행동은 즉각적인 보상만 결정하는 것이 아니라, 미래의 상태 전이와 장기적인 누적 보상(Return)에까지 영향을 준다. 따라서 에이전트는 매 순간의 보상보다 장기적 보상을 최대화하도록 정책을 학습한다.

### 마르코프 결정 과정 (MDP) 구성 요소

- 상태 (State): 현재 환경의 상황
- 행동 (Action): 에이전트가 선택하는 행동
- 보상 (Reward): 행동의 결과로 받는 즉각적 피드백
- 상태전이 (Transition): 행동에 따른 상태 변화

> **마르코프 성질:** 과거 전체가 아닌 현재 상태만을 기반으로 의사결정을 한다고 가정한다.

<br />

---

# 리워드 (Reward)

리워드란, 에이전트가 특정 상태에서 어떤 행동을 했을 때 환경으로부터 받는 즉각적인 피드백 신호이다. 행동의 좋고 나쁨을 수치로 표현한 값으로, 양수는 바람직한 행동을, 음수는 바람직하지 않은 행동을 나타낸다.

강화학습의 목표는 단순히 현재 리워드를 최대화하는 것이 아니라, 할인율(y)을 고려한 미래 보상까지 포함한 **누적 리워드를 최대화** 하는 것이다.

## 즉각적 피드백(Immediate Signal)

특정 상태에서 행동을 했을 때 **즉시 주어지는** 수치형 피드백이다.
- 게임에서 점수 획득 → **+ 보상**
- 장애물 충돌 → **- 보상**

## 희소성 (Sparse Reward)

많은 환경에서 대부분의 시간 단계에서 보상이 0 이고, 
**특정 목표를 달성했을 때만** 보상이 주어진다.

> **예시**          
> 미로에서 출구에 도착했을 때만 + 1 부여          
> 희소할수록 학습이 더 어려워진다.

## 지연성 (Delayed Reward)

어떤 행동의 결과에 대한 보상이 즉시 나타나지 않고, 
**여러 단계를 거친 뒤 나중에 주어지는 경우** 이다.

> **예시**                            
> 체스에서는 한 수를 둔 직후 보상이 없고, 게임 종료 후 승패로 보상 부여        
> ⚠️ 신용 할당 문제 (Credit Assignment Problem): 어떤 행동이 최종 결과에 기여했는지 파악하기 어렵다.

<br />

---

# 에이전트 (Agent)

환경(Environment)과 상호작용하며 상태(State)를 관찰하고, 그에 따라 행동(Action)을 선택하고, 그 결과로 리워드(Reward)를 받아 학습하는 주체이다.

> 즉, 에이전트란 강화학습을 받는 주체

### 에이전트의 목표

단기적 보상이 아닌 미래 보상을 포함한 **누적 보상(Return) 을 최대화하는 정책(Policy)** 을 학습한다.
이를 위해 `**Exploration**` 과 `**Exploitation**` 을 균형있게 수행한다.

## 에이전트 유형

### 가치 기반 에이전트 (Value-Based Agent)

가치 함수(Value Function)를 학습하여 행동을 선택한다. 각 상태 또는 상태-행동 쌍의 가치를 계산하고, 가치가 가장 높은 행동을 선택한다.

{: .star-list}
- **대표 알고리즘:** Q-Learning, DQN

### 정책 기반 에이전트 (Policy-Based Agent)

행동 선택 규칙인 정책(Policy)을 직접 학습한다. 확률적으로 행동을 출력하는 정책을 최적화한다.

{: .star-list}
- **대표 알고리즘:** REINFORCE, Policy Gradient
- **장점:** 연속적인 행동 공간(예: 로봇 제어)에 적합

### 엑터-크리틱 에이전트 (Actor-Critic Agent)

가치 기반 정책 기반을 결합한 방식이다.
- Actor: 정책을 학습하여 행동을 선택
- Critic: 가치 함수를 학습하여 Actor 의 행동을 평가

{: .star-list}
- **대표 알고리즘:** A2C, A3C, PPO, SAC
- **특징**: 학습 안전성과 효율성이 높아 실무에서 많이 사용

### 모델 기반 에이전트 (Model-Based Agent)

환경의 동작 원리(상태 전이 확률, 보상 함수)를 학습하거나 알고 있다고 가정하고, 미래를 예측하며 계획(Planning)을 수행한다.

{: .star-list}
- **대표 알고리즘:** MCTS, Dyna-Q
- **장점:** 샘플 효율성이 높음

### 모델 프리 에이전트 (Model-Free Agent)

환경의 내부 구조를 모른 채, 경험을 통해 직접 정책이나 가치 함수를 학습한다.

{: .star-list}
- **대표 알고리즘:** DQN, PPO (대부분의 현대 강화학습 알고리즘)
- **특징:** 구현이 단순하지만 많은 데이터 필요

<br />

---

# Exploration (탐험)

에이전트가 현재까지 학습한 최적 행동만 반복하는 것이 아니라, 
더 나은 보상을 얻을 가능성이 있는 새로운 행동을 시도해보는 과정이다.

장기적인 누적 보상을 최대화하기 위해 필수적인 전략으로, 
Exploitation 과 균형을 이루어야 한다.

### ε-greedy 전략

일정 확률(ε)로 무작위 행동을 선택하여 새로운 가능성을 탐색한다.

> ⚠️ **주의**
> 탐험이 부족하면 -> 지역 최적해에 머물 수 있음
> 탐험이 지나치면 -> 학습이 불안정해질 수 있음
> 적절한 균형이 핵심!

<br />

---

# Exploitation (활용)

에이전트가 지금까지의 학습 결과를 바탕으로 현재 가장 높은 보상을 줄 것으로 예상되는 행동을 선택하는 과정이다.

단기적으로는 높은 성과를 보장하지만, 새로운 가능성을 탐색하지 않기 때문에 더 나은 전략을 발견하지 못하고 지역 최적해에 머무를 위험이 있다.

## Exploration vs Exploitation - 일상 속 예시

|:상황|:Exploitation(활용)|:Exploration(탐험)|
|-----|-----------------|-----------------|
|**점심 메뉴**|늘 가던 맛집 방문(안정적 만족)|새로운 식당 도전\n(더 맛있을 수도, 실패할 수도)|
|**공부 방법**|기존 성적이 잘 나왔던 방식 유지|스터디, 새 교재, AI 도구 등 새로운 방법 시도|

<br />

---

# Multi-Armed Bandit

{: .info-box}
카지노의 슬롯머신 여러 개 중 가장 돈을 많이 벌 수 있는 머신을 찾아야 하는 문제이다. 각 머신의 평균 보상을 모르는 상태에서, 직접 당겨보며 학습해야 한다.

## 실험 설계

### 환경 설계

- 슬롯머신 5개
- 각 머신의 서로 다른 평균 보상
(예: 0.2, 0.5, 0.1, 0.8, 0.3)
- 보상은 확률적으로 지급
(정규분포 또는 베르누이)

### 전략 구현

- Random 전략: 무작위로 머신 선택
- Greedy 전략: 현재까지 평균 보상이 가장 높은 머신만 선택
- ε - greedy 전략 (ε=0.1): 10% 확률로 무작위 선택, 90% 확률로 최선 선택

### 실험 조건

- 총 1,000 번 선택
- 누적 보상 기록
- 각 머신 선택 횟수 기록

### 결과 분석 항목

- 누적 보상 곡선
- 머신별 선택 비율
- ε값 변화 실험(0.1 vs 0.01)

## 실험 코드

```python
import numpy as np
import matplotlib.pyplot as plt

# ========== 1. 환경 설계 ==========
class SlotMachineEnv:
    def __init__(self, means=None, std=0.3):
        # 각 머신의 평균 보상 (모름 가정이지만, 환경은 이걸로 샘플링)
        self.means = means if means is not None else [0.2, 0.5, 0.1, 0.8, 0.3]
        self.n_arms = len(self.means)
        self.std = std  # 정규분포 표준편차

    def pull(self, arm):
        """arm(0~4)을 당기면 보상 1개 반환 (정규분포)"""
        reward = np.random.normal(self.means[arm], self.std)
        return max(0, min(1, reward))  # [0,1] 클리핑

# ========== 2. 전략 구현 ==========
def run_random(env, n_steps=1000):
    rewards = []
    counts = np.zeros(env.n_arms)
    for _ in range(n_steps):
        arm = np.random.randint(env.n_arms)
        r = env.pull(arm)
        rewards.append(r)
        counts[arm] += 1
    return np.array(rewards), counts

def run_greedy(env, n_steps=1000):
    rewards = []
    counts = np.zeros(env.n_arms)
    Q = np.zeros(env.n_arms)  # 추정 평균 보상
    for _ in range(n_steps):
        arm = np.argmax(Q)  # 항상 현재 추정 최고 머신만 선택
        r = env.pull(arm)
        rewards.append(r)
        counts[arm] += 1
        # 평균 갱신
        Q[arm] = Q[arm] + (r - Q[arm]) / counts[arm]
    return np.array(rewards), counts

def run_epsilon_greedy(env, n_steps=1000, eps=0.1):
    rewards = []
    counts = np.zeros(env.n_arms)
    Q = np.zeros(env.n_arms)
    for _ in range(n_steps):
        if np.random.random() < eps:
            arm = np.random.randint(env.n_arms)
        else:
            arm = np.argmax(Q)
        r = env.pull(arm)
        rewards.append(r)
        counts[arm] += 1
        Q[arm] = Q[arm] + (r - Q[arm]) / counts[arm]
    return np.array(rewards), counts

# ========== 3. 실험 조건 (1,000번, 누적 보상·선택 횟수) ==========
np.random.seed(42)
env = SlotMachineEnv(means=[0.2, 0.5, 0.1, 0.8, 0.3], std=0.3)
n_steps = 1000

r_rand, c_rand = run_random(env, n_steps)
r_greedy, c_greedy = run_greedy(env, n_steps)
r_eg1, c_eg1 = run_epsilon_greedy(env, n_steps, eps=0.1)
r_eg2, c_eg2 = run_epsilon_greedy(env, n_steps, eps=0.01)

# 누적 보상
cum_rand = np.cumsum(r_rand)
cum_greedy = np.cumsum(r_greedy)
cum_eg1 = np.cumsum(r_eg1)
cum_eg2 = np.cumsum(r_eg2)

# ========== 4. 결과 분석 ==========
fig, axes = plt.subplots(1, 2, figsize=(12, 4))

# 누적 보상 곡선
axes[0].plot(cum_rand, label="Random")
axes[0].plot(cum_greedy, label="Greedy")
axes[0].plot(cum_eg1, label="ε-greedy (ε=0.1)")
axes[0].plot(cum_eg2, label="ε-greedy (ε=0.01)")
axes[0].set_xlabel("Step")
axes[0].set_ylabel("Cumulative Reward")
axes[0].set_title("Cumulative Reward")
axes[0].legend()
axes[0].grid(True, alpha=0.3)

# 머신별 선택 비율 (ε-greedy 0.1 기준)
axes[1].bar(np.arange(env.n_arms), c_eg1 / c_eg1.sum(), color="steelblue", label="ε=0.1")
axes[1].set_xlabel("Machine")
axes[1].set_ylabel("Selection Ratio")
axes[1].set_title("Machine Selection Ratio (ε-greedy ε=0.1)")
axes[1].set_xticks(np.arange(env.n_arms))
plt.tight_layout()
plt.show()

# 선택 횟수 출력
print("Machine selection counts (ε=0.1):", c_eg1)
print("Machine selection counts (ε=0.01):", c_eg2)
print("True means:", env.means)
print("Best machine index:", np.argmax(env.means))
```

<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/4ecd58d0-2d0e-42cc-86b5-983602ab242e" />

## 핵심 질문 & 답변

### Greedy 전략이 실패할 수 있는 이유?

> 초기에 추정 Q가 모두 0이라, 처음 당긴 머신이 운 좋게 높은 보상을 받으면 그 머신만 계속 선택하게 됩니다. 
> 다른 머신을 충분히 탐색하지 않아 실제 최고 머신(예: 0.8)을 거의 탐색하지 못하고, 
> 국소적으로 좋아 보이는 머신에 갇혀(suboptimal) 누적 보상이 낮아질 수 있습니다.
> -> **지역 최적해(Local Optimum) 문제 발생**

### ε이 너무 크면 어떤 문제가 발생할 수 있나?

> 탐색(랜덤 선택) 비율이 커져서, 최적 머신을 이미 찾았어도 자주 다른 머신을 당깁니다. 
> 수렴 후에도 계속 탐색하므로 최적 머신 선택 비율이 낮고, 누적 보상이 이론적 최대보다 낮게 유지됩니다.
> -> **수렴 속도 저하, 전체 누적 보상 감소**

### ε이 너무 작으면 어떤 문제가 발생할 수 있나?

> 탐색이 거의 없어서 Greedy와 비슷한 문제가 생깁니다. 
> 초기 몇 번 시도에서 운이 나쁘면 잘못된 머신에 일찍 고정되고, 
> 최적 머신을 찾는 데 시간이 오래 걸리거나 아예 못 찾을 수 있습니다.
> -> 탐험 부족으로 인한 최적해 미달

### Exploration이 필요한 이유?

> 평균을 모르는 상태에서 “어느 머신이 가장 좋은지”를 알려면 각 머신을 여러 번 당겨봐야 추정이 가능합니다. 
> 탐색 없이 한두 번 시도한 머신만 계속 당기면, 
> 실제로는 좋은 머신을 시도해 보지 못해 영원히 찾지 못하게 됩니다. 
> 그래서 **탐험(Exploration)**과 **이미 좋다고 추정된 머신 활용(Exploitation)**을 섞는 ε-greedy가 필요합니다.
> -> **Exploration 이 없으면 Exploitation 만으로는 진정한 최적화가 불가능하다.**

## 심화 사례: 몬테주마의 복수 (Montezuma's Revenge)

Atari 게임 중 하나로, 강화학습에서 탐험 난이도가 매우 높은 사례이다.

<img src='https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FI7pUz%2FdJMcaioUTNp%2FAAAAAAAAAAAAAAAAAAAAADSN7mY-QLd6QilHakMGxXYnxAvaQhNJ5zSshgxU_aTb%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1774969199%26allow_ip%3D%26allow_referer%3D%26signature%3DVfkRudYPDkxVXV0QxFZOoFuolB0%253D' width='100%'/>

### 🎮 특징

- 중간 보상이 거의 없음
- 특정 순서의 복잡한 행동을 해야만 점수 획득 가능
- 잘못된 행동 시 즉시 게임 종료
- 무작위 탐험으로는 거의 보상에 도달하기 어려움

-> Sparse & Delay Reward 문제의 극단적 예시


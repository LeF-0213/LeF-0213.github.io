---
layout: post
related_posts:
  - /aiLog/ai
title:  "TD Learning(Temporal Diffrence Learning)"
date:   2026-03-10
categories:
  - ai
description: >
  Temporal Difference Learning - 매 스텝마다 학습하다
---
* toc
{:toc .large-only}

# TD Learning 이란?
{: .info-box}
TD Learning(Temporal Difference Learning) 은            
환경의 상태 전이 확률을 모르는 상황에서 에피소드가 끝날 때까지 기다리지 않고,           
현재 상태와 다음 상태의 가치를 이용해 상태 가치를 점진적으로 업데이트 하는 강화학습 방법이다.               

현재 상태의 가치 `V(s)`를 바로 다음 상태 `s'`의 가치 `V(s')`와 받은 보상 `r `을 이용해 조금씩 수정하며,         
이때 사용되는 값의 차이를 `TD Error` 라고 한다.

{: .star-list}
- **기반 알고리즘:** Q-Learning, SARSA 등 많은 강화학습 알고리즘의 기반
- **특징:** 모델 프리(Model-Free) - P(s'|s,a) 를 몰라도 학습 가능
- **장점:** 에피소드가 끝나기 전에도 매 스텝마다 즉시 업데이트 가능

<br />

---

# TD Learning 이 필요한 이유

강화학습에서는 어떤 상태가 좋은 상태인지 알고 싶다.                 
예를 들어 미로에서 어떤 칸이 출구에 가까운 좋은 칸인지 알고 싶다고 가정한다.

그런데 문제는 미래를 정확히 모른다는 것이다.

- 지금 이 칸이 좋은 칸인지
- 앞으로 어떻게 흘러갈지
- 결국 성공할지 실패할지

그래서 TD Learning 은 이렇게 생각한다.

> "지금은 정답을 모르니까,              
>  일단 다음 상태를 참고해서 현재 상태를 조금 수정하자."

# TD Learning 의 핵심 아이디어

예를 들어 현재 상태가 A 이고, 다음 상태가 B라고 한다.

### 직관적 예시

현재 상태 A 의 가치 = 3 (내가 추정한 값)

실제로 한 번 움직여보니:
- 보상 r = 1 을 받고
- 다음 상태 B 의 가치 = 5

> 'A는 추정한 값보다 더 좋은 상태'
> 왜냐하면 보상도 받고, 다음 상태도 꽤 좋았다.
> → A의 가치를 조금 올린다.
>
> **현재 추정값과 새로 관찰한 정보의 차이를 이용해 수정하는 것**, 이것이 TD Learning의 핵심이다.

<br />

---

# TD Learning 업데이트 수식

$$V(s) ← V(s) + \alpha x (r + γ・V(s') - V(s))$$

| 기호 | 의미 |
|-----|------------|
| **V(s)** | 현재 상태 s 의 추정 가치 |
| **r** | 실제로 받은 즉시 보상 |
| **γ(gamma)** | 할인율 - 미래 보상의 중요도 (0~1) |
| **V(s')** | 다음 상태 s'의 추정 가치|
| **r + y・V(s')** | TD Target - 새로 관찰한 정보로 계산한 목표값 |
| **TD Error (δ)** | TD Target - V(s): 예측과 실제의 차이 |
| **\alpha (alpha)** | 학습률 - 오차를 얼마나 반영할지 (0~1) |

### TD Error 해석
{: .info-box}
`TD Target = r + γV(s')`          ← 새로운 관찰로 얻은 목표값
`TD Error δ = TD Target - V(s)`   ← 예측과 목표의 차이

- `δ > 0` : 현재 추정보다 실제가 더 좋음 → V(s) 올림
- `δ < 0` : 현재 추정보다 실제가 더 나쁨 → V(s) 내림
- `δ = 0` : 완변학 예측, 업데이트 없음

<br />

---

# Monte Carlo vs TD Learning

|항목|Monte Carlo|TD Learning|
|---|-----------|-----------|
| **업데이트 시점** | 에피소드 종료 후 | 매 스텝마다 즉시 |
| **사용하는 값** | 전체 누적 보상 G (실제값) | r + γV(s') (추정값) |
| **에피소드 종료 필요** | 필요 (종료 후에만 학습) | 불필요 (진행 중에도 학습) |
| **긴 에피소드** | 느리고 비효율적 | 효율적으로 처리 가능 |
| **편향(Bias)/분산(Var)** | 낮은 편향, 높은 분산 | 낮은 분산, 다소 높은 편향 |

> "이 차이 때문에 두 방법의 성격이 완전히 달라진다."

<br />

---

# 실전 예제 - 술 취한 사람 (1 차원)
{: .info-box}
1 차원 `Home-A-B-C-Bar` 환경에서 TD Learning으로 상태 가치를 추정한다. Monte Carlo와 같은 환경이지만 업데이트 방식이 다르다.

## 💻 코드

```python
import random

V = {
    "Home": 1.0,
    "A": 0.0,
    "B": 0.0,
    "C": 0.0,
    "Bar": -1
}

alpha = 0.1
gamma = 1.0
episodes = 50000

states = ['Home', 'A', 'B', 'C', 'Bar']

for ep in range(episodes):
    state = random.choice(states)
    while state not in ["Home", "Bar"]:
        idx = states.index(state)
        next_state = states[idx - 1] if random.random() < 0.5 else states[idx + 1]

        reward = 0

        # V(s) = V(s) + alpha * (reward + gamma * V(s') - V(s))
        V[state] = V[state] + alpha * (reward + gamma * V[next_state] - V[state])

        state = next_state

for state, value in V.items():
    print(f'{state}: {value:.4f}')
```

### 📊 수렴 결과

```
Home: 1.0000
A: 0.4757
B: 0.2703
C: -0.4931
Bar: -1.0000
```

## Monte Carlo vs TD - 같은 문제, 다른 업데이트

### 업데이트 방식 비교
{: .start-list}
- **Monte Carlo:**              
에피소드 종료 후 전체 경로의 Return 한번에 업데이트
- **TD Learning:**              
매 스텝마다         
V(A) ← V(A) + $\alpha$ x (0 + 1.0xV(B) - V(A))          
V(B) ← V(B) + $\alpha$ x (0 + 1.0xV(C) - V(B))              
... 이동할 때마다 즉시 업데이트

<br />

---

# 실전 예제 - GridWorld 4x4 (TD)
{: .info-box}
4x4 격자 안에서 에이전트가 무작위로 움직이며 TD Learning 방식으로 각 칸의 상태 가치를 추정한다.

## 환경 설정

|항목|내용|
|-------|-------------------|
| **격자 크기** | 4 x 4 (총 16개 상태) |
| **시작 위치** | (0, 0) - 좌상단 |
| **목표 위치** | (3, 3) - 우하단, 도착하면 에피소드 종료 |
| **이동 방향** | 상・하・좌・우 4방향 무작위 선택 |
| **벽 처리** | 격자 밖으로 이동하려 하면 제자리 유지 |
| **보상 (R)** | 이동할 때마다 -1 (목표에 빨리 도달할수록 유리) |
| **할인율 γ** | γ=1.0 (미래 보상 전부 반영) |
| **학습률 $\alpha$** | $\alpha$=0.001 |
| **에피소드 수** | 50,000 번 반복 |

### 🗺️ GridWorld 격자
{: .info-box}
(0,0) (0,1) (0,2) (0,3)
(1,0) (1,1) (1,2) (1,3)
(2,0) (2,1) (2,2) (2,3)
(3,0) (3,1) (3,2) (3,3)  ← 목표 지점

## 💻 코드

```python
import random
import numpy as np
```

```python
class GridWorld():
    def __init__(self):
        self.x = 0
        self.y = 0

    def step(self, a):
        if a == 0:
            self.move_left()
        elif a == 1:
            self.move_up()
        elif a == 2:
            self.move_right()
        elif a == 3:
            self.move_down()

        reward = -1
        done = self.is_done()
        return (self.x, self.y), reward, done

    def move_right(self):
        self.x += 1
        if self.x > 3
            self.x = 3
    
    def move_left(self):
        self.x -= 1
        if self.x < 0:
            self.x = 0

    def move_up(self):
        self.y -= 1
        if self.y < 0:
            self.y = 0

    def move_down(self):
        self.y += 1
        if self.y > 3:
            self.y = 3

    def is_done(self):
        if self.x == 3 and self.y == 3:
            return True
        else:
            return False

    def get_state(self):
        return (self.x, self.y)

    def reset(self):
        self.x = 0
        self.y = 0
        return(self.x, self.y)
```

```python
class Agent():
    def __init__(self):
        pass

    def select_action(self):
        coin = random.random()
        if coin < 0.25:
            action = 0
        elif coin < 0.5:
            action = 1
        elif coin < 0.75:
            action = 2
        else:
            action = 3

        return action
```

```python
def main():
    env = GridWorld()
    agent = Agent()
    data = [[0 for _ in range(4) for _ in range(4)]]
    gamma = 1.0
    reward = -1
    alpha = 0.001

    for k in range(50000):
        done = False
        while not done:
            x, y = env.get_state()
            action = agent.select_action()
            (x_prime, y_prime), reward, done = env.step(action)
            x_prime, y_prime = env.get_state()

            # TD Update (MC 와 달리 에피소드 종료 전에도 즉시 업데이트)
            data[x][y] = data[x][y] + alpha*(reward+gamma*data[x_prime][y_prime]-data[x][y])
        env.reset()

    for row in data:
        print(row)

if __name__ == '__main__':
    main()
```

### 📊 결과

```
[-61.099661308879334, -59.012937274350655, -55.459214214059976, -51.99028796277632]
[-58.95784953209073, -56.7174431236241, -51.38893630217367, -44.38032231201669]
[-55.913250062963186, -51.24827745238006, -43.06934063183317, -30.824289367502676]
[-52.90372508994275, -46.675816558160065, -31.902347250018842, 0]
```

<br />

---

# 실전 예제 - GridWorld 5x5 술취한 사람 문제

## 환경 설정

|항목|내용|
|------|----------------|
| **격자 크기** | 5x5 (총 25개 상태) |
| **Home (0, 0)** | 좌상단-도착 보상 +1, 종료 |
| **Bar (4, 4)** | 우하단-도착 보상 +1, 종료 |
| **보상 (R)** | 이동할 때마다 -1 고정 (종료 시 별도 처리)|
| **할인율 γ** | γ=1.0 |
| **학습률 $\alpha$** | $\alpha$=0.001 |
| **에피소드 수** | 50,000 번 반복 |

### 🗺️ GridWorld 격자
술취한 사람이 아래와 같은 격자 위에 서 있습니다.

```
+-----+-----+-----+-----+-----+
| H   | A   | B   | C   | BAR |
+-----+-----+-----+-----+-----+
| D   | E   | F   | G   | I   |
+-----+-----+-----+-----+-----+
| J   | K   | L   | M   | N   |
+-----+-----+-----+-----+-----+
| O   | P   | Q   | R   | S   |
+-----+-----+-----+-----+-----+
| T   | U   | V   | W   | X   |
+-----+-----+-----+-----+-----+
```

## 💻 코드

```python
def main():
    env = GridWorld()
    agent = Agent()
    data = [[0 for _ in range(5)] for _ in range(5)]
    gamma = 1.0
    alpha = 0.01
    data[0][0] = 1.0
    data[4][4] = -1.0

    for k in range(50000):
        done = False
        while not done:
            x, y = env.get_state()
            action = agent.select_action()
            (x_prime, y_prime), reward, done = env.step(action)
            if done:
                td_target = reward
            else:
                td_target = reward + gamma * data[x_prime][y_prime]
            data[x][y] = data[x][y] + alpha * (td_target - data[x][y])
        env.reset()
        data[0][0] = 1.0
        data[4][4] = -1.0

    for row in data:
        print([round(v, 4) for v in row])

if __name__ == "__main__":
    main()
```

### 📊 결과

```
[1.0, -21.5602, -34.047, -39.8735, -42.1448]
[-25.4605, -31.4977, -36.8447, -39.4567, -39.8776]
[-35.5666, -37.4161, -37.8893, -36.7085, -34.8177]
[-40.9613, -40.015, -36.4465, -31.0814, -22.9497]
[-42.8679, -40.2825, -34.0787, -21.7874, -1.0]
```

<br />

---

# 랜덤 벽 GridWorld 에서 TD Learning 구현
{: .info-box}
랜덤하게 생성되는 벽이 존재하는 GridWorld 환경을 만들고, 에이전트가 랜덤 정책으로 움직일 때 TD Learning을 이용해 상태가치 V(s)를 학습하는 프로그램 구현

## 환경 설정

|항목|내용|
|-------|------------------------|
| **격자 크기** | 4x4 |
| **시작 위치** | (0, 0) - 좌상단 |
| **목표 위치** | (3, 3) - 우하단, 도착 시 종료 |
| **벽 생성** | 매 에피소드마다 2~3개 랜덤 생성 |
| **벽 제약** | 제자리 유지(격자 밖 이동도 동일 |
| **벽 이동 시** | 시작 위치, 목표 위치, 중복 위치 제외 |
| **보상 (R)** | 일반 이동 -1 고정 |
| **할인율 γ** | γ=1.0 |
| **학습률 $\alpha$** | $\alpha$=0.001 |
| **에피소드 수** | 5,000 번 반복 |

## 💻 코드

```python
import numpy as np
import random

class RandomWallGridWorld:
    def __init__(self, size=4):
        self.size = size
        self.start_pos = (0, 0)
        self.goal_pos = (3, 3)
        self.actions = [0, 1, 2, 3] # 상하좌우
        self.dw = [0, 0, -1, 1] # col 변화 (좌, 우 관련)
        self.dh = [-1, 1, 0, 0] # row 변화 (상, 하 관련)
        self.v_table = np.zeros((size, size))
        self.alpha = 0.1 # 학습률
        self.gamma = 0.9 # 할인율
        self.walls = []

    def reset_env(self):
        # 매 에피소드마다 벽을 랜덤하게 생성 (2개 또는 3개)
        self.walls = []
        possible_positions = [(r, c) for r in range(self.size) for c in range(self.size) 
                            if (r, c) != self.start_pos and (r, c) != self.goal_pos]
        num_walls = random.choice([2, 3])
        self.walls = random.sample(possible_positions, num_walls)
        return self.start_pos

    def step(self, state, action):
        # 행동에 따른 다음 상태와 보상 반환
        r, c = step
        nr, nc = r + self.dh[action], c + self.dw[action]

        # 격자 밖으로 나가거나 벽을 만나는 경우 제자리 유지
        if nr < 0 or nr >= self.size or nc < 0 or nc >= self.size or (nr, nc) in self.walls:
            next_state = (r, c)
        else:
            next_state = (nr, nc)

        reward = -1
        done = (next_state == self.goal_pos)
        return next_state, reward, done

    def train(self, episodes=1000, max_steps=200):
        for _ in range(episodes):
            state = self.reset_env()
            done = False
            steps = 0

            while not done and steps < max_steps:
                action = random.choice(self.actions)
                next_state, reward, done = self.step(state, action)

                # TD Update: V(s) = V(s) + alpha * [r + gamma * V(s') - V(s)]
                td_target = reward + (0 if done else self.gamma * self.v_table[next_state[0], next_state[1]])
                self.v_table[state[0], state[1]] += self.alpha * (td_target - self.v_table[state[0], state[1]])

                state = next_state
                steps += 1

    # 실행 및 결과 출력
    env = RandomWallGridWorld()
    env.train(episodes=5000)
    print("학습된 상태 가치 V(s):")
    print(np.round(env.v_table, 2)) 
```

### 📊 결과

```
학습된 상태 가치 V(s):
[[-9.79 -9.75 -9.59 -9.16]
 [-9.77 -9.76 -9.45 -8.16]
 [-9.68 -9.44 -8.29 -5.96]
 [-9.49 -9.01 -5.62  0.  ]]
```

## 일반 환경 vs 랜덤 벽 환경 - 상태 가치 차이 분석

### 경로의 불확실성과 우회 비용

|관점|일반 환경|랜덤 벽 환경|
|-----|----------|----------|
|**최단 경로**| 항상 열려 있음 | 에피소드마다 막힐 수 있음 |
|**V(s) 패턴**| 목표에 가까울수록 선형적으로 높아짐 | 전체적으로 더 큰 음수(우회 비용 반영) |
|**평균 이동 횟수**| 상대적으로 적음 | 벽으로 인해 3~4칸 우회 → 더 많아짐 |

### 상태 가치의 수렴 특성

|관점|일반 환경|랜덤 벽 환경|
|-----|----------|----------|
|**환경 특성**| 결정론적(Deterministic) | 확률론적(Stochastic) - 벽 위치 변동 |
|**수렴 양상**| 매끄럽게 특정 값으로 수렴 | TD 오차가 지속 발생, 평균 난이도로 수렴 |
|**길목 주변 상태**| 안정적인 가치 유지 | 벽이 자주 생기면 가치 급격히 하락 |

### 기대 보상의 하락

보상이 매 이동마다 -1 이므로, V(s)는 목표까지의 평균 거리의 음수값과 비례한다.

- 벽이 존재하면 평균 거리가 늘어남 → 모든 상태의 가치가 일반 그리드 대비 **하향 평준화**
- 특히 시작 지점 (0, 0) 근처에 벽이 자주 생성되면 초반 탐색 비용이 커져 시작 지점 가치 크게 하락
- 벽이 자주 막히는 길목 주변 칸은 가치가 급격히 낮아지는 패턴 관찰

> **기대 보상 비교**
> - 일반 환경 V(0, 0) ≈ -N      (N: 평균 최단 이동 횟수)
> - 벽 환경 V(0, 0) ≈ -(N+k)    (k: 평균 우회 비용)
> → 벽 환경이 항상 더 큰 음수값을 가짐              
> → V(s) 값의 패턴이 균일하지 않고 벽 생성 위치에 따라 불규칙해짐

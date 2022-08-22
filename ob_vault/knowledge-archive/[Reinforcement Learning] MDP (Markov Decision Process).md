---
date: 2022-08-22 10:08
lastmod: 2022-08-22 10:08
tags:
- 101
- Machine Learning
- Deep Learning
- Reinforcement Learning
---

> [!info] [COMPM050/COMPGI13] Reinforcement Learning

# MDP (Markov Decision Process)

## Markov Processes

### Introduction to MDPs

- RL의 Fully Observable Env.를 Markov Decision Process(이하 MDP)라 함
- 현재의 State가 process를 완전히 표현하는 것
- 거의 대부분의 RL 문제는 MDP 형태로 만들 수 있음

### Markov Property

- 미래는 현재 이전의 과거와는 독립적임
	- $\mathbb{P}[S_{t+1} | S_t] = \mathbb{P}[S_{t+1} | S_1, ..., S_t]$
- State는 History의 모든 연관 정보를 담고 있으므로 State가 주어지면, History는 무의미함
- State Transition Matrix
    - Markov State $s$와 후속 Sate $s^\prime$의 전이 확률은 $\mathcal{P}_{ss^\prime}=\mathbb{P}[S_{t+1}=s^\prime | S_t = s]$로 정의됨
- Markov Chains

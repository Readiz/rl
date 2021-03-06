---
layout: page
group: "Chapter. 3"
title: "3. Stochastic Process (SP)"
---

- 이번 장은 사실 그냥 넘어가도 큰 문제는 없다. (그래도 그냥 넘기진 말자)
    - 따라서 이해가 되지 않더라도 가볍게 읽고 넘기도록 하자.
- 앞서 살펴 본 동적 프로그래밍에서는 보상(reward) 또는 비용(cost)이 포함된 모델이 사용되었다.
    - 가방 문제 예제에서는 아이템마다 가중치 \\(v\_i\\) 가 존재했었다는 걸 잊지말자.
- 이를 좀 더 확장해서 이후 부터는 MP 에서도 보상(reward) 또는 비용(cost)이 포함된 모델을 확인할 것이다.
    - 즉 확률 전이 모델에 보상 개념이 추가된 모델을 사용하게 될 것이다.
    - 그냥 이를 Stochastic Process(SP)라고 한다.
    - 실제 자주 사용되는 용어는 아니니 무시해도 된다.
- 일단 \\(v\_k\\) 상태에서 \\(m\_k\\) 전이를 선택하여 상태 \\(v\_{k+1}\\) 로 전이하는 과정을 다음과 같이 표현해 보자.

$$v_{k+1} = T(v_k, m_k)$$

- 상태 \\(v\_0\\) 에서 \\(m\_0\\) 전이를 선택하였을 때의 비용을 정의해보자. 이제 비용함수 \\(C\\) 를 살펴본다.

$$C_{v_0, T(v_0, m_0)} = C_{v_0, v_1} = C(v_0, m_0)$$

- 이는 상태 \\(v\_0\\) 에서 전이 \\(m\_0\\) 를 선택하여 전이하는 경우에 발생하는 비용을 나타내는 표기법이다.
- 이제 이를 이용하여 \\(\{m\_0,m\_1,...,m\_{n-1}\}\\) 까지의 이동이 발생하는 경우의 총 비용을 계산한다.

$${C}_n(v) = \sum_{t=0}^{n-1} C(v_t, m_t)$$ 

- 잘 보면 시작부터 현재 상태 \\(v\_n\\) 까지 도달하기 위한 모든 비용을 합한 것임을 알 수 있다.
    - 물론 중간에 어떤 전이를 선택하느냐에 따라 최종 누적 비용은 달라지게 될 것이다.
- 이러한 식에서 전체 이동 비용을 최소화하는 작업을 고려하면 어떻게 될까?
    - 이를 식으로 기술하면 다음과 같다.

$$f_n(v) = \min_{m_0,...,m_{n-1}} C_n(v)$$

- 계산하기 편리한 식으로 좀 정리해보자.

$$f_n(v) = \min_{m_0,...,m_{n-1}} C_v(v)=\min_{m_0}\left[\min_{m_1,...,m_{n-1}}C_n(v)\right]=\min_{m_0}\left[\min_{m_1,...,m_{n-1}}\sum_{t=0}^{n-1} C(v_t, m_t)\right]\\
= \min_{m_0}\left(\min_{m_1,...,m_{n-1}}\left[C(v_0, m_0)+\sum_{t=1}^{n-1} C(v_t, m_t)\right]\right)\\
= \min_{m_0}\left(C(v_0, m_0) + \min_{m_1,...,m_{n-1}}\left[\sum_{t=1}^{n-1} C(v_t, m_t)\right]\right)\\$$ 

- 결국 다음과 같은 재귀 식으로 만들어낼 수 있다.

$$f_n(v) = \min_{m_0}\left[C(v_0, m_0) + f_{n-1}(T(v, m_0))\right]$$

- 이러한 과정을 multi-stage decision process 라고 한다.
- \\(m\_0\\) 는 첫번째 액션을 취하는 과정이다.
    - 따라서 이 때의 전체 비용(cost)은 첫번째 액션을 취한 후 이 때의 비용과 그 이후에 발생하는 비용의 합이다.
- 이제 이 식으로부터 최적의 비용을 구하기 위한 액션 \\(m\_0^*\\) 의 식을 먼저 유도해보자.

$$m_0^* \in {\arg\min}_{m_0}\left[C(v_0, m_0) + f_{n-1}(T(v_0, m_0))\right]$$

- 이를 보면 \\(m\_0^*\\) 를 결정하는 문제에 \\(f\_{n-1}\\) 이 영향을 준다는 것을 알 수 있다.
    - 무슨 말인고 하니 맨 처음의 상태에서 최적의 행동을 선택하는 문제는 그 이후에 발생할 행동들에 영향을 받게 된다는 의미.
- 다시 \\(f\_{n-1}\\) 에 대한 최적의 결정은 무엇일까?

$$f_{n-1}(v) = \min_{m_1}\left[C(v_1, m_1) + f_{n-2}(T(v_1, m_1))\right]$$

$$m_1^* \in {\arg\min}_{m_1}\left[C(v_1, m_1) + f_{n-2}(T(v_1, m_1))\right]$$

- \\(f\_{n-1}\\) 도 \\(f\_{n-2}\\) 에 영향을 받게 된다. 결국 이 문제는 재귀적인 특성을 가지고 있다.
    - \\(m\_0^\*\\) 를 구하기 위해서는 \\(m\_1^\*\\) 를 구해야 하고, 다시 \\(m\_1^\*\\) 를 구하기 위해서는 \\(m\_2^\*\\) 를 구해야 한다.
- 이 식을 좀 일반화하자. 결국 앞서 살펴보았던 동적 프로그래밍 방식을 기술한 것과 차이가 없다.

$$f_k(v) = \min_{m_{N-k}}\left[C(v, m_{N-k}) + f_{k-1}(T(v, m_{N-k}))\right]$$

- 복잡한 식이 나와서 좀 당황할 수도 있지만 결국 하고자 하는 이야기는 앞서 설명한 DP와 다를 것이 없다는 것을 이해하는 것이 중요하다.
    - DP 문제를 그냥 식으로 기술한 것.
    - 다만, 비용(Cost)에 관해서 이렇게 재귀적인 형태로 문제를 풀어낼 수 있다는 것이 중요한 점이다.
    - 사실 이번 장은 이 정도만 이해하고 넘어가면 된다. (즉, 이런 재귀식으로 비용 식을 풀어낼 수 있다는 점만 이해하면 된다.)
    
## Discount Factor

- 추가적으로 비용 함수에 Discount Factor 라고 불리는 값을 추가할 수도 있다.

$${C}_n(v) = \sum_{t=0}^{n-1} \gamma^t C(v_t, m_t)$$ 

$$ \gamma \in (0, 1]$$

- 특별한 것은 없고 프로세스가 진행될 수록 일정 비율의 상수 값이 곱해지는 것이다.
    - 만약 \\(\gamma\\) 가 \\(1\\) 이라면 앞에서 본 것과 동일한 것이고,
    - 만약 \\(\gamma\\) 가 \\(1\\) 보다 작은 실수 값이라면 비용의 발생 정도가 점점 작아지는 것이라 생각할 수 있다.

- Discount factor 를 원 식인 \\(f_k\\) 에도 추가로 기술할 수 있다.

$$f_k(v) = \min_{m_{N-k}}\left[C(v, m_{N-k}) + {\gamma}^k f_{k-1}(T(v, m_{N-k}))\right]$$

- 그런데 왜 갑자기 Discount factor 를 도입하는 것일까?
    - 상황에 따라 사용자가 스텝 횟수를 정하지 않고 적당한 수준으로 반복 후 멈추기를 원할 수도 있다.
    - 실제 보상이나 비용이 시간이 지날수록 작아지는 문제들이 주어질 수 있다.
- 이런 단순한 이유 외에도 다음과 같은 여러가지 이유로 Discount factor 가 도입된다.
    - 무한히 반복되어 도는 것을 막기 위해.
    - 현재를 기준으로 미래에 대한 불확실성
        - 현재 가치가 미래 가치보다 높다.
        - 미래는 생각도 않고 현재만 쫒는 우매한 인간에게 적합한 모델링
    - 뭐, 어차피 이 값을 \\(1\\) 로 하는 경우 도입하지 않은 것과 동일한 효과이므로 일반화 측면에서 그냥 식에다 추가해 놓아도 전혀 문제는 없다.
- 걱정하지 말자. 이와 관련된 내용은 이후에 좀 더 자세히 살펴볼 것이다.

## RL과의 연관성

- 사실 이번 장의 제목은 조금 잘못 되었다.
- SP는 그냥 확률(stocastic) 모델을 사용하는 Process를 의미하는 것이므로 MDP 등도 SP라고 해도 큰 문제는 없다.
- 그냥 MC -> DP -> MDP 로 넘어가는 단계에서 좀 짜친 몇 가지 개념들을 다루기 위해 추가한 chapter라고 생각하자.
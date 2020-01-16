---
layout: post
title: 선형대수 Orthogonality, Orthogonal Complement, Orthogonal Basis
tags: [Linear Algebra]
use_math: true
---
안녕하세요! 이번 포스트에서는  **직교성(Orthogonality)**, **직교여공간(Orthogonal Complement)**, **직교기저(Orthogonal Basis)** 에 대해 제가 배웠던 내용을 정리하고자 합니다. 바로 시작하겠습니다 😊

### 1. 직교(orthogonal)의 정의
먼저 직교의 정의는 다음과 같습니다.
> **Def.** n차원의 실수 공간의 두 벡터 u, v에 대하여, $u\cdot v=0$이면 두 벡터는 직교(orthogonal)한다.

내적(inner product)에 대해서는 일단 ``실수 공간에서 두 벡터를 input으로 하고, 스칼라를 output으로 하는 함수`` $u\cdot v$로만 정의를 하고 진행하겠습니다! 후에 실수 공간을 넘어서 inner product에 대해 조금 더 구체적으로 포스팅하려고 합니다.

$$inner\space product: u\cdot v=u^Tv, (u,v\in \mathbb{R^n})$$

돌아와서, 직교의 정의에 의하면, 두 벡터의 내적 값이 0이 되면, 두 벡터는 서로 **직교** 한다고 말합니다. 사실 이 글을 보시는 분들 중에 직교의 정의를 모르시는 분들은 없을 것 같아 바로 넘어가겠습니다^^

<br>

### 2. 직교여공간(Orthogonal Complement)
자, 직교의 정의는 분명 두 벡터 간의 관계에 대한 정의였습니다. 그렇다면 한 벡터와 부분공간(subspace)이 직교한다는 건 무슨 뜻일까요?`` 어떤 벡터 v가 어떤 부분공간 W의 모든 벡터와 직교한다면 v는 W와 직교한다고 말합니다.`` 이 때, $W$와 직교인 모든 벡터들을 모아 놓은 집합을 바로 **W의 직교여공간(orthogonal complement)** 이라고 부르고, $W^⊥$와 같이 표기합니다.

$$W^⊥=\{\boldsymbol z | \boldsymbol z⊥W\}$$

다시, n차원의 실수 공간의 부분공간 $W$가 있을 때, 직교여공간 $W^⊥$은 다음의 두가지 사실(Fact)이 있습니다.
1. 어떤 벡터 $\boldsymbol x$가 $W^⊥$안에 존재하기 위한 필요충분조건은 $\boldsymbol x$가 $W$를 생성(span)하는 모든 벡터와 직교라는 것이다.
2. $W^⊥$는  $\mathbb{R}^n$의 부분공간이다.

1의 경우는 결국 $\boldsymbol x$가 $W$의 모든 벡터와 직교한다는 것을 의미하므로 정의와 일맥상통합니다. 2의 경우 직관적으로 생각해볼까요? 어떤 벡터 $\boldsymbol z$가 $W$와 직교한다면, $\boldsymbol z$에 어떠한 상수를 곱해도 여전히 $W$와 직교할 것입니다. 또 한, $W$와 직교하는 두 벡터가 있을 때, 이 두 벡터를 더해서 생긴 평면 역시 $W$와 직교할 것입니다. 따라서 부분공간이 되기 위한 조건을 만족합니다.

이제 이와 관련하여 중요한 정리가 있는데, 이는 다음과 같습니다.
> 어떠한 mxn 행렬 $A$에 대하여, $Row(A)^⊥=Nul(A)$  ,   $Col(A)^⊥=Nul(A^T )$ 이다.

혹시 행렬의 **Row space** 와 **Column space**, **Null space** 가 가물가물하신 분들은 아래 정의를 다시 한번 상기해주세요!
- $Row(A)$: $A$의 행(row)들이 생성하는 공간 = $A$의 행들의 모든 선형결합들의 집합
- $Col(A)$: $A$의 열(column)들이 생성하는 공간 = $A$의 $image$   
즉, $A$에 의해 선형변환된 벡터는 $A$의 열들의 선형결합이라는 것을 의미합니다. 한편, $A$의 열과 $A^T$의 행은 같겠죠? 따라서, 자명하게 $Col(A)=Row(A^T)$ 입니다!
- $Nul(A) = \{x\in \mathbb{R}^n  | Ax=0\}$

다시 돌아와서 위 정리가 왜 그런지 살펴봅시다. 간단하게 $Row(A)^⊥=Nul(A)$가 되는 것을 증명해보겠습니다! 어떤 벡터 $\boldsymbol x$가 $A$의 $null\space space$안에 있다고 합시다. $(x\in Nul(A))$

$$\boldsymbol x\in Nul(A)\iff A\boldsymbol x=0\iff \boldsymbol x ⊥Row(A) \iff \boldsymbol x \in Row(A)^⊥$$

이 때, $Row(A)=Col(A^T)$이므로, $Nul(A)=Col(A^T )^⊥$ 와 같이도 나타낼 수 있습니다.

여기까지 두 벡터, 벡터와 부분공간, 두 부분공간 간의 직교 개념에 대해서 알아보았습니다.

<br>

### 3. 직교 기저(Orthogonal Basis)
이제 직교 기저의 개념에 대해서 알아보고자 합니다. 직교 기저는 앞으로 계속 나오게 될 orthogonal projection을 이해하는데 필요한 필수 개념입니다!
> **Def.** $\mathbb{R^n}$의 부분공간인 $W$의 직교 기저(orthogonal basis)는 $W$의 기저(basis)를 이루는 벡터들이 서로 직교하는 기저를 말한다.

결국 벡터들이 서로 선형 독립인 것을 넘어서 직교하는 성질이 추가된 기저를 말합니다! 이러한 직교성이 추가되면 다른 기저들에 비해 갖게 되는 장점이 있는데요, 바로 ``선형결합의 가중치를 매우 쉽게 구할 수 있다``는 것입니다. 다음의 정리를 봅시다.

>**Thm.**  $\{u_1,u_2,⋯,u_p \}$를 $\mathbb{R^n}$의 부분공간 $W$의 직교 기저라고 하자. $W$ 내의 벡터 $\boldsymbol y$는 다음과 같이 선형결합으로 나타내어 지고, 가중치는 다음과 같이 주어진다.
$$y=c_1 u_1+⋯+c_p u_p,\quad c_j=\frac{(𝑢_𝑗\cdot 𝑦)}{(𝑢_𝑗\cdot 𝑢_𝑗 )},j=1,⋯,p$$

간단하게 증명해볼까요?

$$ \begin{aligned}
(pf)\quad y&=𝑐_1 𝑢_1+𝑐_2 𝑢_2+\cdots +𝑐_𝑝 𝑢_𝑝\\
𝑢_𝑗\cdot 𝑦&=𝑢_𝑗\cdot (𝑐_1 𝑢_1+𝑐_2 𝑢_2+⋯+𝑐_𝑝 𝑢_𝑝) &= 𝑐_𝑗 (𝑢_𝑗\cdot 𝑢_𝑗) \\
∴𝑐_𝑗&=\frac{(𝑢_𝑗\cdot 𝑦)}{(𝑢_𝑗\cdot 𝑢_𝑗 )}, j=1,\cdots,p
\end{aligned}$$

따라서, 모든 가중치를 위와 같이 표현해서 다시 y를 나타내면 다음과 같습니다.

$$y=\frac{y\cdot u_1}{u_1\cdot u_1}u_1+\frac{y\cdot u_2}{u_2\cdot u_2}u_2+⋯+\frac{y\cdot u_p}{u_p\cdot u_p}u_p$$

이때, 특별한 제한조건을 추가해봅시다. 조금 더 간편하게 보기 위해 $u_j\cdot u_j=1,∀j$, 즉모든 $u_j$ 벡터들의 ``길이가 1이라고 제한``해봅시다. 그러면 $y$는 다시 다음과 같이 표현됩니다.

$$ \begin{aligned}
y&=\frac{u_1\cdot y}{u_1\cdot u_1}u_1+\frac{u_2\cdot y}{u_2\cdot u_2}u_2+⋯+\frac{u_p\cdot y}{u_p\cdot u_p}u_p\\
&=(u_1\cdot y)u_1+(u_2\cdot y)u_2+\cdots+(u_p\cdot y)u_p\\
&=(u_1^T y)u_1+(u_2^T y)u_2+\cdots+(u_p^T y)u_p\\
&=𝑢_1 (𝑢_1^T  𝑦) +𝑢_2 (𝑢_2^T  𝑦) +⋯+𝑢_𝑝 (𝑢_𝑝^T  𝑦)\\
&=(𝑢_1 𝑢_1^T) 𝑦 +(𝑢_2 𝑢_2^T) 𝑦 +⋯+(𝑢_𝑝 𝑢_𝑝^T  )𝑦\\
&=𝑃_1 𝑦 +𝑃_2 𝑦 +⋯+𝑃_𝑝 𝑦
\end{aligned}$$

4번째 줄에서 $u_j u_j^T$꼴 보이시나요? $u_j$가 nx1 벡터라고 하면, $u_j u_j^T$는 **nxn 행렬** 이 되고 이를 $P_j$로 명명하였습니다. 이렇게 벡터 $y$는 행렬들의 선형 결합으로도 표현 가능한 것입니다. 이에 대해서는 Orthogonal Projection에 대한 글에서 조금 더 구체적으로 설명하고자 합니다. 한편, 길이가 1이라는 조건을 추가한 벡터 $u_j$를 **단위 벡터** 라고 하고, $\{u_1,u_2,\cdots,u_p\}$가 직교 기저인데, 각 벡터들이 단위벡터인 경우, 이를 **직정 기저(Orthonormal Basis)** 라고 부릅니다.

이번 글은 여기까지였구요! 이제 다음 글에서 본격적으로 이 가중치들이 무엇을 의미하는지에 대해 설명하고자 합니다. 다음에는 Orthogonal Projection에 대해 포스팅하겠습니다. 감사합니다 :)




<br>

참고:  $\small Linear\space Algebra\space and\space its\space Applications\space 5th\space edition\space (David C.Lay, Stephen R.Lay, Judi J.McDonald)$

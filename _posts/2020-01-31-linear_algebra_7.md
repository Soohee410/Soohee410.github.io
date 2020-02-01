---
layout: post
title: 선형대수 Inner Product Space
tags: [Linear Algebra]
use_math: true
---
안녕하세요! 이번 포스트에서는  **Inner Product Space** 에 대한 내용을 정리하고자 합니다. 바로 시작하겠습니다 😊

<br>

## 1.  Inner Product와 Inner Product Space

<br>
지금까지는 내적을 다음과 같이 **실수공간** 에서 두 벡터 간의 내적으로 정의해왔습니다.

$$u, v \in \mathbb R^n\quad \mapsto \quad u\cdot v \in \mathbb R$$

이번 포스트에서는 실수공간을 넘어 다양한 벡터 공간에서 내적을 정의하는 것을 설명하고자 합니다. 물론 내적의 요체(?), ``두 벡터를 input으로 하고 스칼라 값을 output으로 한다``는 것은 변하지 않습니다! 어떠한 벡터 공간 $V$에 두 벡터 $u, v$가 있다고 합시다. 벡터 공간을 확장했으니, 내적 값을 $u\cdot v$가 아닌 $<u,v>$로 표기하겠습니다.

$$u, v \in V\quad \mapsto\quad <u, v> \in \mathbb R$$

물론 계산했더니 스칼라 값이 나왔다고 모두 내적인 것은 아니고 추가 조건들이 필요합니다. 이와 관련하여 내적의 정의는 다음과 같습니다.

<img src="/assets/inner1.png" width="690px">

예를 들어, 2차식의 집합 $\mathbb P_2$ 벡터공간에 대하여, 내적을 다음과 같이 정의할 수 있습니다.

$$<u,v>=\int_0^1 u(x)v(x) dx \quad for\space u(x),v(x)\in \mathbb P_2$$

$u(x)=x,\space v(x)=x^2$라고 한다면, 이들의 내적 값은 다음과 같이 계산될 것입니다.


$$<u,v>=\int_0^1 (x)(x^2) dx = \int_0^1 x^3 dx = \frac{1}{4}  $$

이렇게, 다항식의 집합 또한 내적 공간이 될 수 있습니다.

<br>

## 2. 내적 계산 가능의 의미

내적을 계산할 수 있다는 것은 어떤 의미를 가지게 될까요? 이전 포스트에서 내적을 이용해서 무엇을 했는지를 생각해봅시다. 내적을 이용해서 각 벡터의 길이(length)를 구했고, [직교(Orthogonality)](https://soohee410.github.io/linear_algebra_3)를 정의했으며, [정사영(Orthogonal Projection)](https://soohee410.github.io/linear_algebra_4)도 구했고, [그람슈미트 과정(Gram-Schmidt Process)](https://soohee410.github.io/linear_algebra_5)도 적용했습니다. 즉, 이와 같은 것들을 이제 실수 공간이 아니더라도 다양한 내적 공간에서 모두 할 수 있게 됩니다!  
교재에 의하면, 응용 수학(Applied Mathematics) 분야에서 흔한 문제 중 하나는 함수를 원소로 하는 벡터 공간 $V$에 관한 것이라고 합니다.  예를 들어, $V$의 특정 부분공간 $W$에서 특정 함수 $f \in V$에 가장 근사하는 함수 $g\in W$를 찾는 경우를 생각해봅시다. 앞서 [Least Squares Solution](https://soohee410.github.io/linear_algebra_6)에서 포스팅한 것과 같이,  가장 근사하다는 것의 수리적인 의미는 $\lVert f-g \rVert$를 최소화하는 것이고, 이를 최소화하는 함수 $g\in W$는 $W$에 $f$를 정사영시킨 것입니다. 이 문제는 예시를 보면서 이해해 봅시다.

**Problem)** 3차 다항식의 집합 $\mathbb P_3=\lbrace a_0+a_1x+a_2x^2+a_3x^3 \vert a_0, a_1, a_2,a_3\in \mathbb R \rbrace$에서의 내적이 다음과 같이 주어졌다고 하자.

$$<f,g>=\int_{-1}^1 f(t)g(t) \space dt$$

이 때, 다음을 최소화하는 $a,b,c \in \mathbb R$를 구해라.

$$\int_{-1}^1 \vert x^3-a-bx-cx^2\vert ^2 \space dx$$

자, 어떻게 풀어야 할 지 감이 오시나요? 식 $a+bx+cx^2$를 $g(x)\in \mathbb P_2$라 하고, $x^3$을 $f(x)\in \mathbb P_3$이라고 하겠습니다. 즉, 문제는 **$<f-g, f-g> = \lVert f-g \rVert ^2$ 을 최소화하는 $g(x)\in \mathbb P_2$를 구하는 것** 입니다!
따라서 이 때의 $g=Proj_{\scriptsize\mathbb P_2} x^3$ 가 됩니다. 이 문제를 해결하는 간단한 절차는 다음과 같습니다.

1. $\mathbb P_3$의 부분공간 $\mathbb P_2$의 기저를 $\lbrace 1,x,x^2 \rbrace$ 로 잡는다. 그람 슈미트를 이용해서 이 기저로부터 직교 기저를 도출한다.
2. $\mathbb P_2$의 직교 기저를 이용해서  $Proj_{\scriptsize\mathbb P_2} x^3$ 을 구한다.

물론 그람슈미트와 정사영 구하는 과정 모두 해당 내적공간의 내적 $<f,g>$을 이용해서 구하면 됩니다!

<br>

여기까지 내적 공간에 대한 내용이었습니다. 저는 정말 처음에 이 부분을 배우면서 실수 공간을 넘어 다양한 벡터 공간에서 내적을 정의하는 게 신세계 같았습니다! 다음에 포스팅할 내용에서도 실수 범위를 넘어 복소수 범위에서의 행렬을 잠깐 언급하고자 합니다. 다음에는 대칭행렬(Symmetric matrix)에 대해 포스팅하겠습니다. 감사합니다 :)

<br>

---
$Reference.$  
- $\small Linear\space Algebra\space and\space its\space Applications\space 5th\space edition\space (David C.Lay, Stephen R.Lay, Judi J.McDonald)$
- 고려대학교 김홍중 교수님의 수업

<br>

---
layout: post
title: "[선형대수] Quadratic Forms, Principal Axes Theorem, Positive Definite Matrix"
tags: [Linear Algebra]
use_math: true
---
안녕하세요! 이번 포스트에서는  **이차 형태(Quadratic Forms)** 에 대한 내용과 **양의 정부호(Positive Definite)** 의 의미를 정리하고자 합니다. 바로 시작하겠습니다 😊
<br>

## 1.  Quadratic Forms

<br>
먼저 이차형태(Quadratic Forms)의 정의부터 알아보겠습니다.

>**Def.** 어떠한 nxn 대칭행렬 $A$에 대해, $\mathbb {R^n}$의 **이차형태** 는  $\mathbb {R^n}$에서 정의된 함수 $Q$를 의미하는 것으로, 벡터 $x\in \mathbb {R^n}$에서의 함숫값은 $Q(x)=x^\intercal Ax$으로 계산된다. 이 행렬 $A$를 **matrix of the quadratic form** 이라고 일컫는다.

즉, 대칭행렬 $A$에 대해, $\small Q(x)=x^\intercal Ax$의 형태를 이차형태라고 하는데요. 예를 들어 벡터 $\small \boldsymbol x=\begin{pmatrix} x_1 \cr x_2\end{pmatrix}$에 대하여 다음과 같은 대칭행렬들이 있을 때, 이차형태를 계산하면 다음과 같습니다.

- $\small A=\begin{bmatrix} 1 & 0\cr 0&2 \end{bmatrix}$일 때,  $\small x^\intercal Ax=\begin{bmatrix}x_1&x_2\end{bmatrix} \begin{bmatrix} 1 & 0\cr 0&2 \end{bmatrix} \begin{bmatrix}x_1\cr x_2 \end{bmatrix}=x_1^2+2x_2^2$
- $\small A=\begin{bmatrix} 2 & 2\cr 2&4 \end{bmatrix}$일 때, $\small x^\intercal Ax=\begin{bmatrix}x_1&x_2\end{bmatrix} \begin{bmatrix} 2 & 2\cr 2&4 \end{bmatrix} \begin{bmatrix}x_1\cr x_2 \end{bmatrix}=2x_1^2+4x_1x_2+4x_2^2$

이제 이 이차형태들을 임의의 상수 $c$에 대하여 그림으로 나타내면 아래 그림과 같을 것입니다. 첫번째 이차형태의 경우 축들을 기준으로 타원형 그래프가 **표준 위치(Standard Position)** 에 있고, 두번째 이차형태의 경우 그래프가 사선 방향의 타원형이네요. 그런데 이 이차형태들의 함숫값들 중에서 ``원점과의 거리가 가장 긴 좌표를 찾는 문제``를 생각해 봅시다. 첫번째의 경우 특별한 계산과정이 필요도 없이 $x_2=0$일 때의 좌표가 문제의 답이 된다는 것을 알 수 있습니다.  반면, 두번째의 경우 이 문제에 대한 답을 구하는 과정은 조금 더 복잡합니다. 두번째 이차형태의 경우에는 cross-product 항 $4x_1x_2$이 존재하기 때문입니다. 여러가지 행렬들을 계산해보면, 대각 행렬의 경우 cross-product 항이 존재하지 않고, 대각행렬이 아닌 대칭행렬의 경우에는 cross-product 항이 존재한다는 것을 알 수 있습니다.
<img src= '/assets/quad1.png' width='500px'>

돌아와서 두번째 이차형태의 경우 이 문제에 대한 답을 어떻게 찾을 수 있을까요? 아래 그림처럼 축을 바꿔보면, 새로운 축에 대해서는 그래프가 표준 위치에 놓이게 되는 것을 볼 수 있습니다. 이 새로운 축 $y_1, y_2$에 대해서는 두번째 이차형태가 cross-product항을 가지지 않게 되어, 여러가지 최적화 문제에 대해서도 계산이 훨씬 쉬워질 것입니다. 그렇다면 이 새로운 축은 어떻게 구하면 될까요? 위에서 대각행렬의 경우 이차형태가 cross-product 항을 가지지 않는다는 것을 알게 되었는데요. 그렇다면 행렬의 대각화를 이용해서 대각행렬로 만들어주는 아이디어를 어떻게 이용하면 되지 않을까요? 이 과정은 **변수 변환(change of variable)** 을 이용합니다.
<img src= '/img/quad2.png' width='500px'>

<br>

## 2. Change of Variable in a Quadratic Form

<br>

$\boldsymbol x$가 $\mathbb R^n$의 어떠한 변수 벡터라고 합시다. 이제  $\mathbb R^n$의 새로운 변수 벡터 $\boldsymbol y$에 대하여 다음을 만족하는 invertible matrix $P$가 존재한다고 하겠습니다.

$$\small \boldsymbol x=P\boldsymbol y \quad or\quad \boldsymbol y=P^{-1}\boldsymbol x $$

위와 같은 식을 **변수 변환** 했다고 합니다! $P$의 열들을 $u_1,u_2,\cdots,u_n$이라 하고, 이 열들로 결정되는 기저를 $\small B=\lbrace u_1,u_2,\cdots,u_n\rbrace$라 할 때, $\boldsymbol y$는 $\boldsymbol x$가 기저 $B$에 의해 표현될 때 $\boldsymbol x$의 좌표벡터(coordinate vector)인 것입니다. ($\small i.e.\space \boldsymbol y=\begin{bmatrix}\boldsymbol x\end{bmatrix}_B$)

$$\small x=Py=\begin{bmatrix} \boldsymbol{u_1}& \cdots &\boldsymbol{ u_n}\end{bmatrix} \begin{pmatrix}y_1\cr\vdots\cr y_n\end{pmatrix}=y_1\boldsymbol{u_1}+\cdots +y_n\boldsymbol{u_n}$$

이제 이것을 이차형태 $\small x^{\intercal}Ax$에 대입하면 다음과 같이 될 것입니다.

$$\small x^{\intercal}Ax=(Py)^{\intercal}A(Py)=y^{\intercal}P^{\intercal}APy=y^{\intercal}(P^{\intercal}AP)y$$

그러면 새로운 이차형태의 행렬이 $\small P^{\intercal}AP$이 되는 것을 확인할 수 있습니다. 이 때, $A$는 대칭행렬이기 때문에, [이전 포스트](https://soohee410.github.io/hermitian_matrix)에서 했던 정리에 의하여 $A$는 ``직교대각화가 가능``합니다. 따라서, $\small P^{\intercal}AP$가 대각행렬 $D$가 되게 하는 orthogonal matrix $P$가 존재합니다. [대각화 포스트](https://soohee410.github.io/diagonalization)에서 했듯이, 이 때의 $P$는 각 열들이 $A$의 고유벡터인 행렬이고, $D$는 대각요소가 각 고유벡터에 해당하는 $A$의 고유값들인 대각행렬입니다.

$$\small \begin{aligned} x^{\intercal}Ax=y^{\intercal}Dy&=\begin{pmatrix} y_1,& \cdots, & y_n\end{pmatrix} \begin{pmatrix} \lambda_1 & &0\cr & \ddots & \cr 0 & & \lambda_n \end{pmatrix}\begin{pmatrix} y_1 \cr \vdots \cr y_n\end{pmatrix}\\ &= \lambda_1 y_1^2+\lambda_2 y_2^2+\cdots +
\lambda_n y_n^2 \end{aligned}$$

예를 들어, nxn 대칭행렬 $A$의 고유벡터들이 $u_1, u_2, \cdots, u_n$이고, 고유값들이 $\lambda_1, \lambda_2,\cdots,\lambda_n$일 때, 이차형태 $\small Q(x)=x^{\intercal}Ax=y^{\intercal}Dy$에 대하여, $y=\boldsymbol e_1$이면 어떻게 될까요? 벡터 $x$는 다음과 같이 고유벡터 $u_1$을 의미하고,

$$\small x=Py=\begin{bmatrix} \boldsymbol u_1 & \cdots & \boldsymbol u_n \end{bmatrix}\begin{pmatrix}1\cr 0\cr \vdots \cr 0\end{pmatrix} = \boldsymbol u_1$$

$\small Q$는 $\boldsymbol x=\boldsymbol u_1$일 때 $u_1$에 해당하는 고유값 $\lambda_1$을 출력할 것입니다.

$$\small \begin{aligned} Q(\boldsymbol x)=x^{\intercal}Ax=y^{\intercal}Dy&=\lambda_1 y_1^2+\lambda_2 y_2^2+\cdots +\lambda_n y_n^2\\ &=\lambda_1+0+\cdots+0=\lambda_1 \end{aligned}$$

혹은 반대로 $\boldsymbol x$을 알고 있을 때 $\boldsymbol y$는 다음과 같이 구할 수 있습니다.

$$y=P^{-1}x=P^{\intercal}x$$

이차형태의 변수변환에서의 x와 y의 관계가 조금 이해되시나요? 이제 이 모든 것을 정리하는 정리가 바로 **Principal Axes Theorem** 입니다.

> **The Principal Axes Theorem.** $A$가 nxn 대칭행렬이라고 하자. 그러면 이차형태 $\small x^{\intercal}Ax$를 cross-product 항이 존재하지 않는 이차형태 $\small y^{\intercal}Dy$로 변환시키는 직교 변수변환 $\small \boldsymbol x=P \boldsymbol y$이 존재한다.

이 정리에서 $P$의 열들이 바로 이차형태 $\small x^{\intercal}Ax$의 **Principal Axes** 입니다. 그리고 새로운 벡터 y는 principal axes로 이루어진 orthonormal basis로 표현된 좌표계에서 $x$의 좌표벡터를 의미합니다. 어떠한 행렬이 대각행렬이면, 이차형태 그래프는 표준 위치에 놓이게 된다는 것을 기억하실 것입니다. 따라서, principal axes를 찾는다는 것은 ``그래프를 표준 위치에 놓이게 하는 새로운 좌표계를 찾는다``는 것을 의미합니다.

<br>

## 3. Classifying Quadratic Forms

<br>
이차형태의 함숫값들의 부호에 따라 이차형태를 분류할 수 있는데요. 먼저 정의부터 알아봅시다.

> **Def.** 이차형태 $Q$는,  
a. 모든 $\boldsymbol x \not=0$에 대하여 $\small Q(\boldsymbol x)>0$이면, **positive definite** 이다.  
b. 모든 $\boldsymbol x \not=0$에 대하여 $\small Q(\boldsymbol x)<0$이면, **negative definite** 이다.  
c. $\small Q(\boldsymbol x)$가 양의 값, 음의 값을 모두 가지면, **indefinite** 이다.

또한, 모든 $\boldsymbol x$에 대해 $\small Q(\boldsymbol x)\ge 0$이면, $Q$는 **positive semidefinite** 이라고 하고,  $\small Q(\boldsymbol x)\le 0$이면, $Q$는 **negative semidefinite** 이라고 말합니다. 이렇게 이차형태의 함숫값들의 부호에 따라 이차형태를 다양하게 분류한다는 것을 알게 되었습니다. 다만 모든 0이 아닌 벡터 $\boldsymbol x$에 대해 $\small Q(\boldsymbol x)$를 알아야 한다는 것인데, 너무 번거롭겠죠? 실제로 이차형태를 판별하는 다양한 방법이 있지만, 그 중에서도 쉬운 방법은 바로 고유값들의 부호를 확인하는 것입니다!

> **Thm.** $A$가 nxn 대칭행렬이라고 하자. 그러면 이차형태 $\small x^{\intercal}Ax$가,  
a. positive definite이기 위한 필요충분조건은 $A$의 고유값들이 모두 양수인 것이다.  
b. negative definite이기 위한 필요충분조건은 $A$의 고유값들이 모두 음수인 것이다.  
c. indefinite이기 위한 필요충분조건은 $A$가 양의 고유값, 음의 고유값을 모두 가지는 것이다.

왜 그럴까요? 위에서 **Principal Axes Theorem** 에 의하여, 다음을 만족하는 직교변환 $\boldsymbol x = P\boldsymbol y$가 존재합니다. 이 때, 모든 0이 아닌 $\boldsymbol x$와 모든 0이 아닌 $\boldsymbol y$ 사이에는 일대일 대응이 존재하기 때문에, $\small Q(x)$의 부호는 결국 $A$의 고유값들의 부호에 달려있다고 할 수 있습니다!

$$\small Q(\boldsymbol x)=x^{\intercal}Ax=y^{\intercal}Dy=\lambda_1 y_1^2+\lambda_2 y_2^2+\cdots +\lambda_n y_n^2$$

그렇다면 여기서 유추할 수 있는 성질이 있는데요. 어떤 대칭행렬 $A$의 이차형태 $\small x^{\intercal}Ax$가 **positive definite** 이라면, ``행렬 A는 가역(invertible)``이라는 것입니다!  $\small x^{\intercal}Ax$가 **positive definite** 이면, $A$의 고유값들이 모두 양수라는 것을 의미하고, 고유값들이 모두 양수이면 행렬식 또한 양수가 되어 $A$가 가역이라는 것을 의미하게 됩니다.

$$\small det(A)=\lambda_1\lambda_2\cdots \lambda_n>0 \implies A: invertible$$


<br>


여기까지 이차형태와 변수변환, 그리고 이차형태 판별에 대한 내용이었습니다! 조금 이해가 되셨나요? 통계학에서는 특히 **공분산 행렬**이 대칭행렬이라는 것을 이용하여, 특히 positive definite 행렬이라 가정하고 다양한 연산을 하게 됩니다. 다음에는 이차형태에서의 Constrained Optimization에 대해 포스팅하겠습니다. 감사합니다 :)

<br>

---
$Reference.$  
- David C.Lay · Stephen R.Lay · Judi J.McDonald, Linear Algebra and its Applications, 5th edition, Pearson
- 고려대학교 김홍중 교수님의 수업

<br>

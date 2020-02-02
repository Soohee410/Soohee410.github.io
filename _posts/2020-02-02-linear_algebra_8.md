---
layout: post
title: 선형대수 Hermitian Matrix, Diagonalization of Symmetric Matrix, Spectral Decomposition
tags: [Linear Algebra]
use_math: true
---
안녕하세요! 이번 포스트에서는  **에르미트행렬(Hermitian Matrix)**, **대칭행렬(Symmetric Matrix)** 의 특징과 대칭행렬에서의 **대각화**, 마지막으로 **스펙트럴 분해(Spectral Decomposition)** 에 대한 내용을 정리하고자 합니다. 바로 시작하겠습니다 😊

<br>

## 1.  Hermitian Matrix

<br>
먼저 대칭행렬(Symmetric Matrix)이 무엇인지부터 알아봅시다. 사실 대칭행렬이 무엇인지 모르는 사람은 없을 것이라 생각합니다! 말 그대로 대각 요소(diagonal entry)를 기준으로 마주보고 있는 요소들이 같은, 즉 대칭의 형태를 가진 행렬을 대칭행렬이라고 합니다.

$$\small A: Symmetric \space Matrix \quad if\space A^{\intercal}=A$$

 이제 본격적으로 대칭행렬의 특성들에 대해서 배울 건데, 그 전에 사고의 확장(?)이 필요합니다...! 실수 범위를 넘어서 더 큰 범위, 복소수 범위를 생각해봅시다. [저번 포스트](https://soohee410.github.io/linear_algebra_7)에서 복소수 공간의 내적에 대해서 기억하시나요? 복소수 공간에서 두 벡터 $u,v$의 내적은 $\bar u^{\intercal}v$로 계산하고, 이것은 $u^{\sf H}v$로 표기한다고 언급했었는데요. 행렬도 똑같습니다! 어떤 행렬 $A$에 대해, $\small A^{\sf H}=\bar A^{\sf T}$로 계산됩니다. 이 때, $\small A^{\sf H}=A$이면, 이 행렬을 **에르미트 행렬(Hermitian Matrix)** 라고 일컫습니다. 다시 말해서, 대각을 기준으로 서로 마주보고 있는 요소끼리 켤레복소수인 행렬을 에르미트 행렬이라고 합니다.

$$\small  A: Hermitian\space Matrix \quad if\space  A^{\sf H}=A$$

예를 들어, 행렬 $\small A=\begin{pmatrix}1 & {1+i} \\ {1-i} & 1 \end{pmatrix}$는 $\small A=\bar A^{\sf T}$를 만족하기 때문에 에르미트 행렬이라고 할 수 있습니다. 혹시 눈치가 빠르신 분들은 벌써 아셨을 수 있지만, 이 행렬 $A$에서 허수부, 즉 $i$가 없다면 어떻게 되나요? $A$는 실수만 남게 되고 대칭행렬이 됩니다! 즉, ``대칭행렬은 에르미트 행렬 중에서 모든 요소가 실수인 특별한 케이스``라고 할 수 있습니다. 다시 말해서 (실수의) 대칭행렬은 에르미트 행렬입니다. 이제 에르미트 행렬의 대표적인 세가지 특징을 보려고 합니다!

**첫째**, $\small A^{\sf H}=A$이면, $A$의 대각 요소는 항상 실수이다.  
직관적으로 생각해도, $A$의 대각요소 중 복소수인 것이 존재한다면, $\bar A^{\sf T}$와 같을 수 없겠죠?

**둘째**, $\small A^{\sf H}=A$이면, $A$의 고유값은 실수이다.

$$\small \begin{aligned}(pf)\qquad \qquad Ax &=\lambda x \\
x^{\sf H}Ax &=x^{\sf H}\lambda x=\lambda x^{\sf H}x \implies \lambda &=\frac{x^{\sf H}Ax}{x^{\sf H}x} \end{aligned} $$

$$\small \begin{aligned}  x^{\sf H}x&=\bar x_1x_1+\cdots +\bar x_nx_n\in \mathbb{R},\\
\space (x^{\sf H}Ax)^{\sf H} &= x^{\sf H}Ax,\quad  x^{\sf H}Ax:scalar \implies x^{\sf H}Ax\in\mathbb{R} \end{aligned}$$

$$\small\therefore  \lambda =\frac{x^{\sf H}Ax}{x^{\sf H}x}\in \mathbb{R} $$

**셋째**, $\small A^{\sf H}=A$이면, 서로 다른 고유공간의 고유벡터들은 직교한다.

$$\small(pf)\qquad\qquad Let \space Av_1=\lambda_1 v_1,\space Av_2=\lambda_2v_2,\space \lambda_1\not =\lambda_2 $$

$$\small \begin{aligned}\lambda_1(v_1^{\sf H}v_2)&=(\lambda_1v_1)^{\sf H}v_2=(Av_1)^{\sf H}v_2\\ &=v_1^{\sf H}A^{\sf H}v_2=v_1^{\sf H}(A^{\sf H}v_2)\\ &=v_1^{\sf H}\lambda_2v_2=\lambda_2(v_1^{\sf H}v_2)\\
\therefore v_1^{\sf H}v_2&=0
\end{aligned} $$

<br>
잘 이해되셨나요? 대칭행렬도 에르미트 행렬이기 때문에, 위 특성들을 모두 가지고 있습니다. 특히 3번째 특성은 교재에 정리로 나와있습니다.

> **Thm.** A가 대칭이면, 서로 다른 고유공간의 고유벡터들은 서로 직교한다.

선형대수 첫 번째 포스트, [Eigenvalue and Eigenvector](https://soohee410.github.io/linear_algebra_1)에서 원래는 ``서로 다른 고유공간의 고유벡터들은 서로 선형독립이다``라는 정리를 기록했었는데요. 만약 대칭행렬일 경우에는 서로 다른 고유공간의 고유벡터들이 선형독립인 것을 넘어서 직교한다는 성질이 추가된 것입니다! 이 성질을 고려했을 때 대각화는 그럼 어떻게 변하게 되는지 알아봅시다.
<br>

## 2. Diagonalization of Symmetric Matrix
<br>

대각화가 가능하기 위한 조건이 뭐였는지 기억하시나요? [대각화 포스트](https://soohee410.github.io/linear_algebra_2)에서 nxn행렬이 대각화가 가능하기 위해서는 ``n개의 선형독립인 고유벡터가 필요하다``고 언급했었는데요. 이번엔 어떤 행렬 $A$가 대칭행렬이고 n개의 서로 직교하는 고유벡터가 존재한다고 해봅시다. 이 고유벡터들을 각각 길이가 1이라는 조건을 추가하겠습니다. 서로 직교하면 독립하기 때문에 당연히 대각화가 가능할 것입니다.  이 때, 각 열이 고유벡터인 행렬 $P$는 **Orthogonal Matrix** 가 될 것입니다.

$$\small  A=PDP^{-1}, \space (D:Diagonal, P^{-1}=P^{\intercal})$$

이렇게, 어떠한 행렬이 Orthogonal Matrix $P$와 대각행렬 $D$의 결합, $\small A=PDP^{-1}=PDP^{\intercal}$으로 표현될 수 있다면, 이 행렬은 **직교 대각화가 가능(Orthogonally Diagonalizable)** 하다고 말합니다.

이제 또 다시 중요한 물음을 해야하죠. 언제 직교 대각화가 가능할까요? 이와 관련하여 놀라운 정리가 있습니다.

> **Thm.** 정방행렬 $A$가 직교 대각화가 가능하기 위한 필요충분조건은 $A$가 대칭행렬인 것이다.

대부분 어떤 정방행렬을 보자마자 이 행렬이 대각화가 가능한지 아닌지 바로 알 수가 없습니다. 하지만, 위 정리에 의하면 주어진 행렬이 대칭행렬일 때에는 그 행렬은 무조건 대각화, 그것도 직교 대각화가 가능하다는 것입니다! 또 한, 직교대각화가 가능하면 대칭행렬입니다.

$$\small A^{\intercal} = (PDP^{\intercal})^{\intercal}=PDP^{\intercal}=A$$

안타깝게도 이 놀라운 정리의 증명은 비교적 길고 까다로워서 이 포스트에서는 생략하도록 하겠습니다 ~~ㅠㅠ~~  

<br>

## 3. Spectral Decomposition
<br>

어떠한 행렬 $A$의 고유값들의 집합은 때로 **A의 스펙트럼(Spectrum of $A$)** 이라고 불리고, 고유값에 대한 다음의 설명을 **스펙트럴 정리(Spectral Theorem)** 이라고 일컫습니다.

> **Spectral Theorem for Symmetric Matrices** : nxn 대칭행렬 $A$는 다음의 성질들을 가진다.  
a.  $A$는 n개의 실수인 고유값을 가지고 있다.  
b. 각 고유값 $\lambda$에 대한 고유공간의 차원은 $\lambda$의 개수(multiplicity)와 같다.  
c. 고유공간은 상호 직교한다.  
d. $A$는 직교 대각화가 가능하다.

앞서 설명했던 내용을 정리하는 정리라고 할 수 있을 것 같습니다 :)

한편, 어떤 대칭행렬 $A$를 직교 대각화했다고 합시다. 그러면 다음과 같이 표현할 수 있습니다.

$$\small \begin{aligned} &A=PDP^{\intercal}=\begin{bmatrix} u_1&\cdots&u_n\end{bmatrix} \begin{bmatrix}\lambda_1 & &0\\ & \ddots& \\ 0& &\lambda_n \end{bmatrix} \begin{bmatrix}u_1^{\intercal}\\ \vdots\\u_n^{\intercal}\end{bmatrix} \\
&=\begin{bmatrix}\lambda_1u_1 &\cdots &\lambda_n u_n \end{bmatrix} \begin{bmatrix}u_1^{\intercal}\\ \vdots\\u_n^{\intercal}\end{bmatrix} \\ &=\lambda_1u_1u_1^{\intercal}+\lambda_2u_2u_2^{\intercal} +\cdots \lambda_nu_nu_n^{\intercal}  \end{aligned}$$

위 식에서 마지막 줄을 보면 $A$의 고유값들, 즉 스펙트럼에 의해 A가 다양하게 쪼개진 것을 확인할 수 있습니다. 위와 같이 행렬 $A$를 표현한 것을 $A$의 **스펙트럴 분해(Spectral Decomposition)** 라고 합니다. 또 한, 자주 언급되었지만 각 nxn행렬 $u_ju_j^{\intercal}$는 **투영행렬(Projection Matrix)** 입니다. 예를 들어 어떠한 벡터 $\boldsymbol x$를 행렬 $A$에 의해 선형변환 시킨다면 다음과 같이 될 것입니다.

$$A\boldsymbol x=\lambda_1u_1u_1^{\intercal}\boldsymbol x+\lambda_2u_2u_2^{\intercal}\boldsymbol x +\cdots \lambda_nu_nu_n^{\intercal}\boldsymbol x $$

이 때, 각 $(u_ju_j^{\intercal})\boldsymbol x $ 는 벡터 $\boldsymbol x $를 벡터 **$u_j$에 의해 생성되는 부분공간으로의 정사영** 을 의미한다는 점에서 $u_ju_j^{\intercal}$는 투영행렬이고, 각 투영행렬 $u_ju_j^{\intercal}$의 rank는 1입니다.

<br>

여기까지 에르미트행렬, 대칭행렬, 스펙트럴 정리에 대한 내용이었습니다! 다음에는 2차형태(Quadratic Forms)에 대해 포스팅하겠습니다. 감사합니다 :)

<br>

---
$Reference.$  
- $\small Linear\space Algebra\space and\space its\space Applications\space 5th\space edition\space (David C.Lay, Stephen R.Lay, Judi J.McDonald)$
- 고려대학교 김홍중 교수님의 수업

<br>

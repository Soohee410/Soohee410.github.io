---
layout: post
title: "[선형대수] Least-Squares Solution, Normal Equation"
tags: [Linear Algebra]
use_math: true
---
안녕하세요! 이번 포스트에서는  **최소제곱해(Least Squares Solution)** 과 **정규방정식(Normal Equation)** 에 대해 선형대수 과목에서 제가 배웠던 내용을 정리하고자 합니다. 바로 시작하겠습니다 😊
<br>

## 1.  The Best Approximation Theorem

<br>
본격적으로 최소제곱해(Least Squares Solution)에 대해 배우기 전에 알아야 할 [정사영(Orthogonal Projection)](https://soohee410.github.io/orthogonal_projection)의 중요한 성질이 있습니다! 바로 **The Best Approximation Theorem** 이라는 것인데, 이는 다음과 같습니다.

<img src="/assets/least1.png" width="700px">

위 정리에 의하면, 어떤 부분공간 $W$에서 $\mathbb R^n$의 벡터 $y$와의 거리 차이(error)가 가장 작은 벡터는 $W$에 $y$를 정사영시킨 벡터 $\hat y$입니다. 그렇다면, 어떠한 경우에는, 예를 들면 $y$를 알 수가 없거나 $y$를 구하기 위한 비용이 클 때, $y$ 대신 $\hat y$으로 대체할 수 있을 것입니다! 많은 분야에서 어떤 찾고자 하는 벡터가 있을 때, 그 보다 낮은 차원에서 그 벡터와 가장 가까운 대체 백터를 찾는 데에 관심이 있습니다. 대표적으로 통계학이 그렇다고 할 수 있는데요. 통계학에서는 우리가 찾고자 하는 어떠한 모수(parameter)에 대하여, 우리가 관측할 수 있는 범위 내에서 그 모수를 충분히 잘 설명하는 대체 통계량을 찾고자 합니다. 또 한, 어떠한 데이터에 대해, 그 데이터를 충분히 잘 설명하는 하나의 회귀 모델을 추정하여 데이터를 간략화하는 것도 이와 같은 맥락이라고 할 수 있습니다. 아무튼 위와 같은 정리를 통해, ``실수 공간에서는 모 벡터와, 그 보다 낮은 차원에서 가장 가까운 벡터를 찾기 위해서는 Orthogonal Projection을 이용하면 된다``는 것을 알게 되었습니다!

<br>

## 2. Least-Squares Solution

<br>
이제 다음과 같은 식이 있다고 해봅시다.

$$\small \begin{aligned} 1x_1+3x_0&=2 \\
2x_1+5x_0&=3 \end{aligned}$$

이 식들을 행렬로 표현해볼까요?

$$\small \begin{gathered}  \begin{pmatrix}1&3\\2&5\end{pmatrix} \begin{pmatrix}x_1 \\x_0\end{pmatrix}=\begin{pmatrix}2\\3 \end{pmatrix} \implies \small A\boldsymbol x=\boldsymbol b \\ \begin{pmatrix} A=\begin{pmatrix}1&3\\2&5\end{pmatrix},\boldsymbol x=\begin{pmatrix}x_1 \\x_0\end{pmatrix}, \boldsymbol b=\begin{pmatrix}2\\3\end{pmatrix} \end{pmatrix}\end{gathered} $$

따라서, 위 두 식을 풀려면 $Ax=b$를 만족하는 $x$를 찾으면 됩니다. 이 때, $Ax$는 자명하게 $A$의 열공간(Column Space)에 속합니다. 그런데 $b$는 $A$의 열공간에 반드시 속하리라는 보장이 없습니다. $b$가 $Col(A)$에 속한다면, $Ax=b$를 만족하는 $x$를 구할 수 있을 것입니다. 하지만, $b$가 $Col(A)$에 속하지 않는다면 $Ax=b$를 만족하는 $x$를 구할 수 없게 되겠죠. 그럼 이 경우에는 어떻게 해야할까요? 아마 차선책은 ``식을 완전히 만족하는 해는 못 구하더라도, 그 해에 가장 근사하는 해를 구하는 것``일 겁니다! 즉,  $Ax$가 $b$와 최대한 가깝게 되는 $x$를 구해야 할 것입니다. 다시 말해서, $\lVert b-Ax \rVert$를 최소화하는 $x$를 구해야 합니다. $\lVert b-Ax \rVert$를 최소화하는 해 $x$를 바로 **최소제곱해(Least Squares Solution)** 이라 하고, 이것의 정의는 다음과 같습니다.   

> **Def.** $A$가 mxn행렬이고 $b \in \mathbb R^m$일 때, $Ax=b$의 최소제곱해(Least-Squares Solution)은 다음을 만족하는 $\hat x \in \mathbb R^n$이다.    
$\lVert b-A\hat x\rVert≤\lVert b-Ax\rVert,for\space all\space x\in \mathbb R^n $

즉, $b$가 $A$의 열공간에 존재하지 않을 때 최소제곱해(Least Squares Solution)는, $A$의 열공간에서 $b$와 가장 가까운 점$(Ax)$이 되게끔 하는 $x$입니다. 그러면 이제 $A$의 열공간 밖에 있는 $b$와 가장 가까운 $Ax$가 대체 무엇인지 알아야합니다. 다시 말해서, 특정한 부분공간 내에서 다른 차원에 있는 어떠한 벡터와 가장 가까운 것을 찾아야 하는 것인데, 그런데 이 맥락 어디서 많이 보지 않았나요? 바로 위에서 배운 정사영의 중요한 성질을 이용하면 될 것입니다! **Best Approximation Theorem** 에 의해, $A$의 열공간에 존재하면서 $b$와 가장 가까운 것을 $\hat b$라고 한다면, $\hat b$는 **$A$의 열공간에 대한 $b$의 정사영(orthogonal projection onto column space of A)** 이 됩니다.

$$\hat b = proj_{col(A)}b$$

<img src="/assets/least2.png" width="360px">

$\hat b$는 A의 열공간 안에 존재하므로, 식 $Ax=\hat b$는 이제 풀리게 됩니다! 이 때의 해가 $\hat x$이 $Ax=b$의 **최소제곱해** 가 됩니다.

$$A\hat x =\hat b$$

<br>

## 3. 정규방정식(Normal Equation)

<br>
자, 위에서 정사영을 이용해서 최소제곱해를 구할 수 있다는 것을 알게 되었습니다. 그런데 행렬에 따라서 매번 정사영을 구하고 그 뒤에 최소제곱해를 구하는 과정이 비효율적일 수 있습니다. 실제로 최소제곱해를 구하는 방법은 여러가지가 있는데요. 먼저 대표적인 방법인 **정규방정식(Normal Equation)** 에 대해 설명하고자 합니다. 물론 모두 정사영의 연장선 상에서의 풀이 방법입니다.  
먼저 $Ax=b$에서 벡터 $b$는 **Orthogonal Decomposition Theorem** 에 의해 다음과 같이 쪼개질 수 있습니다. (Orthogonal Decomposition Theorem에 대해서는 [여기]( https://soohee410.github.io/orthogonal_projection)를 참고해주세요!)

$$\small b=\hat b + (b-\hat b),\quad  \hat b \in Col(A),\space (b-\hat b)\in Col(A)^{\perp}$$

이 때, $b-\hat b$가 $A$의 열공간에 직교하는 것을 이용하여 다음과 같이 전개할 수 있게 됩니다.

$$\small \begin{aligned}  &  b-\hat b \perp  Col(A)  \\
&\implies b-A\hat x \perp Col(A) \\
&\implies b-A\hat x \perp a_j \space for\space all\space j=1, \cdots, n\\
&\implies a_j\cdot (b-A\hat x)=0 \space for\space all\space j=1, \cdots, n\\
&\implies a_j^{\intercal}(b-A\hat x)=0 \space for\space all\space j=1, \cdots, n \\
&\implies A^{\intercal}(b-A\hat x)=0 \\
&\quad \therefore A^{\intercal}A\hat x=A^{\intercal}b  \end{aligned}$$

즉, 위 식 $A^TAx=A^Tb$을 $Ax=b$의 **정규방정식(Normal Equation)** 이라고 부릅니다.
>**Thm.** $Ax=b$의 최소제곱해들의 집합은 **정규방정식(Normal Equation)** $A^{\intercal}Ax=A^{\intercal}b$의 해들의 집합$(\not = ∅)$이다.

위 정리에 의하면, 정규방정식의 해들의 (공집합이 아닌) 집합이 $Ax=b$의 최소제곱해들의 집합이 됩니다.

<br>

## 4. 행렬 A가 선형독립이라면?

<br>
지금까지는 행렬 $A$에 어떠한 조건도 없었습니다. 이제 특정한 조건을 하나 추가해볼 건데요. 어떤 mxn행렬 $A$의 ``열들이 서로 선형독립(linearly independent)``이라고 합시다.  그러면, $A^{\intercal}A$는 선형독립인 정사각행렬(nxn)이 될 것입니다. 이제 $\hat x$이 $Ax=b$의 최소제곱해라고 합시다. 그러면 3번에서 위 정리에 의해 $\hat x$은 정규방정식을 만족할 것입니다.

$$\small A^{\intercal}A\hat x=A^{\intercal}b$$

이 때, 행렬 $A^{\intercal}A$은 선형독립인 정사각행렬이므로 역행렬이 존재할 것이고, 위 식은 다음과 같이 유일(unique) 최소제곱해가 존재하게 됩니다.

$$\small \hat x=(A^{\intercal}A)^{-1}A^{\intercal}b$$

한편, A의 열들이 선형독립이라는 것을 이용해서 또 다른 방법을 도입하려고 합니다. 바로 [QR분해](https://soohee410.github.io/linear_algebra_5)입니다! QR분해를 곁들인(?) 최소제곱해에 관한 정리는 다음과 같습니다.

> **Thm.** mxn 행렬 A의 열들이 선형 독립이고, A=QR로 분해가 가능하다고 하자. 그러면, 각 $b\in \mathbb{R^m}$에 대하여, 방정식 $Ax=b$는 다음의 유일한 최소제곱해를 가진다.   
$$\hat x = R^{-1}Q^{\intercal}b$$

간단하게 증명해볼까요? 유일 최소제곱해 $\hat x$는 다음과 같이 전개됩니다.

$$\small \begin{aligned} \hat x&=(A^{\intercal}A)^{-1}A^{\intercal}b \\
&=((QR)^{\intercal}QR)^{-1}(QR)^{\intercal}b\\
&=(R^{\intercal}Q^{\intercal}QR)^{-1}R^{\intercal}Q^{\intercal}b\\
&=(R^{\intercal}R)^{-1}R^{\intercal}Q^{\intercal}b\\
&=R^{-1}(R^{\intercal})^{-1}R^{\intercal}Q^{\intercal}b\\
&=R^{-1}Q^{\intercal}b \end{aligned} $$


<br>

여기까지 **Least Squares Solution** 과 **Normal Equation** 에 대한 기록이었습니다! 이번 포스트에서 다룬 내용은 특히나 통계학을 공부하시는 분들에게는 매우 친숙한 내용일거라 생각합니다. 저 역시 회귀분석(Linear Regression)과 같은 과목에서, 방정식을 $X\boldsymbol\beta=\boldsymbol y$로 두고 정규방정식을 이용해서 회귀계수 $\small \hat{\boldsymbol\beta} =(X^{\intercal}X)^{-1}X^{\intercal}\boldsymbol y$을 구하는 것을 배웠었는데요. 선형대수 과목에서 이 챕터를 배우면서 좀 더 어떤 근거로 이렇게 구할 수 있는지에 대해서 알게 었던 것 같습니다! 다음 포스트에서는 [Inner Product Space](https://soohee410.github.io/inner_product_space)에 대해서 정리하고자 합니다. 감사합니다 :)

<br>

---
$Reference.$  
- David C.Lay · Stephen R.Lay · Judi J.McDonald, Linear Algebra and its Applications, 5th edition, Pearson
- 고려대학교 김홍중 교수님의 수업

<br>

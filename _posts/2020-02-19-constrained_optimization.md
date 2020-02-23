---
layout: post
title: "[선형대수] Constrained Optimization"
tags: [Linear Algebra]
use_math: true
---
안녕하세요! 이번 포스트에서는  **Constrained Optimization** 에 대한 내용을  정리하고자 합니다. 바로 시작하겠습니다 😊
<br>

## 1.  Constrained Optimiztion of  Quadratic Forms
<br>

이번 포스트에서 다룰 이차형태의 최적화 문제에 대한 내용은 저번 포스트 주제였던 [이차 형태]((https://soohee410.github.io/quadratic_form))의 바로 연장선이기 때문에, 저번 포스트 내용을 잘 알고 계시는 분들께서는 비교적 자명하게 느껴지실 거라 생각합니다! 이번 포스트의 주제는 **대칭행렬** $A$가 주어졌을 때, 벡터 $\boldsymbol x$가 **단위벡터(unit vector)** 라는 제한조건 하에서, 이차형태 $\small \boldsymbol x^{\intercal}A\boldsymbol x$의 최댓값(최솟값)과 이를 최대화(최소화)하는 $\boldsymbol x$를 찾는 것입니다.

$$\small \min_{ \lVert\boldsymbol x\rVert=1} \boldsymbol x^{\intercal}A\boldsymbol x =\space ? \quad or\quad \max_{ \lVert\boldsymbol x\rVert=1} \boldsymbol x^{\intercal}A\boldsymbol x =\space ? $$

 그렇다면 이 최적화 문제는 어떻게 구하면 될까요? 일단 이와 관련하여 정리 하나를 짚고 가겠습니다!

>**Thm.** nxn 행렬 $A$가 대칭행렬이고, n개의 고유값 $\small\lambda_1\ge\lambda_2\ge \cdots \ge\lambda_n$과, 이들 각각에 해당하는 단위 고유벡터 $\small \boldsymbol u_1, \cdots, \boldsymbol u_n$이 있다고 하자.   
이 때, $\small \boldsymbol x^{\intercal}A\boldsymbol x$의 최댓값은 행렬 $A$의 고유값 중 가장 큰 것($\lambda_1$)이고, 이를 최대화하는 $\boldsymbol x$는 $\lambda_1$에 해당하는 단위 고유벡터 $\boldsymbol u_1$이다. 한편, $\small \boldsymbol x^{\intercal}A\boldsymbol x$의 최솟값은 $A$의 고유값 중 가장 작은 것($\lambda_n$)이고, 이를 최소화하는 $\boldsymbol x$는 $\lambda_n$에 해당하는 단위 고유벡터 $\boldsymbol u_n$이다.

 위 정리에 의하면, 벡터 $\boldsymbol x$가 단위벡터라는 제한조건이 있는 상황에서 이차형태 $\boldsymbol x^{\intercal}A\boldsymbol x$의 최대값이 $A$의 가장 큰 고유값이고, $\boldsymbol x$는 그 고유값에 해당하는 고유벡터라는 것입니다! 이제 왜 그런지 살펴보겠습니다. 저번 포스트에서 **변수 변환** 을 이용해서 다음과 같이 이차형태의 행렬 $A$를 대각행렬 $D$로 바꾸었습니다. 또, 이것의 의미는 연산의 편리함을 위해, ``이차형태에서 cross-product 항이 없도록``, 그래프가 **표준 형태(standard position)** 가 되게 하는 새로운 좌표계를 찾은 것이라고도 했습니다.

 $$\small \boldsymbol x^{\intercal}A\boldsymbol x = \boldsymbol y^{\intercal}D\boldsymbol y \quad when\space \boldsymbol x=P\boldsymbol y$$

이 때, $P$가 orthogonal matrix이기 때문에, $\small P^{\intercal}P=I$입니다. 따라서 다음과 같이 벡터 $\boldsymbol x$의 길이와 $\boldsymbol y$의 길이가 같은 것을 확인할 수 있습니다.

$$\small \lVert \boldsymbol x\rVert=\small \lVert P\boldsymbol y\rVert=\small \lVert \boldsymbol y\rVert$$

특히, $\boldsymbol x$가 단위벡터라는 제한조건이 존재하기 때문에, 둘다 길이가 1이겠네요!

$$\small \lVert \boldsymbol x\rVert=\small \lVert \boldsymbol y\rVert=1$$

따라서, 우리가 구하고자 하는 최적화 문제는 다음과 같이 표현할 수 있습니다. $\small \boldsymbol y^{\intercal}D\boldsymbol y$의 경우 cross-product 항이 없기 때문에 최적화 문제에 대한 해결이 더욱 간편합니다. 따라서, $\small \boldsymbol y^{\intercal}D\boldsymbol y$의 최대값, 최소값을 찾아서 문제를 해결하면 될 것입니다.

$$\small \max_{ \lVert\boldsymbol x\rVert=1} \boldsymbol x^{\intercal}A\boldsymbol x =\max_{ \lVert\boldsymbol y\rVert=1} \boldsymbol y^{\intercal}D\boldsymbol y, \quad \min_{\lVert\boldsymbol x\rVert=1} \boldsymbol x^{\intercal}A\boldsymbol x =\min_{ \lVert\boldsymbol y\rVert=1} \boldsymbol y^{\intercal}D\boldsymbol y$$

이 때, $\small\boldsymbol y^{\intercal}D\boldsymbol y$의 하한선은 다음과 같이 $\lambda_n$이 될 것입니다.

$$\small\begin{aligned}\boldsymbol y^{\intercal}D\boldsymbol y&=\lambda_1y_1^2+\lambda_2y_2^2+\cdots+\lambda_ny_n^2\cr &\ge\lambda_ny_1^2+\lambda_ny_2^2+\cdots+\lambda_ny_n^2\cr
&=\lambda_n(y_1^2+y_2^2+\cdots+y_n^2)\cr &=\lambda_n\lVert \boldsymbol y\rVert^2=\lambda_n\end{aligned}$$


마찬가지로, $\small\boldsymbol y^{\intercal}D\boldsymbol y$의 상한선은 다음과 같이 $\lambda_1$이 될 것입니다.

$$\small\begin{aligned}\boldsymbol y^{\intercal}D\boldsymbol y&=\lambda_1y_1^2+\lambda_2y_2^2+\cdots+\lambda_ny_n^2\cr &\le\lambda_1y_1^2+\lambda_1y_2^2+\cdots+\lambda_1y_n^2\cr
&=\lambda_1(y_1^2+y_2^2+\cdots+y_n^2)\cr &=\lambda_1\lVert \boldsymbol y\rVert^2=\lambda_1\end{aligned}$$


그렇다면 이제 $\small \boldsymbol y^{\intercal}D\boldsymbol y$가 언제 가장 작은 값 $\lambda_n$이 되는지 살펴봅시다. 자명하게 $\small\boldsymbol y=\boldsymbol e_n$일 때가 될 것입니다. 이 때, $\boldsymbol x$는 다음과 같이 고유벡터 $\small \boldsymbol u_n$이 됩니다. 물론 이 때 $\small\boldsymbol{u_n}$은 단위벡터입니다.

$$\small\boldsymbol x=P\boldsymbol e_n=\begin{bmatrix}\boldsymbol u_1 & \boldsymbol u_2 &\cdots&\boldsymbol u_n \end{bmatrix}\begin{pmatrix} 0\cr\vdots\cr 1\end{pmatrix}=\boldsymbol u_n$$

또 한, $\small \boldsymbol y^{\intercal}D\boldsymbol y$가 가장 큰 값 $\lambda_1$이 되는 경우는 $\boldsymbol y=\boldsymbol e_1\small$이  될 것이고, $\boldsymbol x$는 다음과 같이 고유벡터 $\boldsymbol u_1$이 될 것입니다. 마찬가지로 $\small\boldsymbol{u_1}$은 단위벡터입니다.

$$\small\boldsymbol x=P\boldsymbol e_1=\begin{bmatrix}\boldsymbol u_1 & \boldsymbol u_2 &\cdots&\boldsymbol u_n \end{bmatrix}\begin{pmatrix} 1\cr\vdots\cr 0\end{pmatrix}=\boldsymbol u_1$$

따라서 정리하면 다음과 같은 결론이 도출됩니다.

$$\small\begin{aligned} \max_{ \lVert\boldsymbol x\rVert=1} \boldsymbol x^{\intercal}A\boldsymbol x=\max_{ \lVert\boldsymbol y\rVert=1} \boldsymbol y^{\intercal}D\boldsymbol y=\lambda_1, \space when \space \boldsymbol x=\boldsymbol{u_1}, \boldsymbol y=\boldsymbol{e_1}
\cr \min_{\lVert\boldsymbol x\rVert=1} \boldsymbol x^{\intercal}A\boldsymbol x=\min_{ \lVert\boldsymbol y\rVert=1} \boldsymbol y^{\intercal}D\boldsymbol y=\lambda_n, \space when \space \boldsymbol x=\boldsymbol{u_n}, \boldsymbol y=\boldsymbol{e_1}\end{aligned}$$

<br>

## 2. 제한조건의 추가(1)
<br>

 한편 어떠한 벡터가 단위벡터가 아니라면, 그 벡터를 자기 자신의 길이로 나누면 단위벡터로 만들 수 있을 것입니다. 따라서 이 문제는 다음과 같이도 표현될 것입니다.

$$\small\max_{ \lVert\boldsymbol x\rVert=1} \boldsymbol x^{\intercal}A\boldsymbol x =\max_{\boldsymbol x\not =0} \frac {\boldsymbol x^{\intercal}A\boldsymbol x}{\boldsymbol x^{\intercal} \boldsymbol x}$$

왜일까요? $\small\boldsymbol x^{\intercal} \boldsymbol x=\begin{Vmatrix}\boldsymbol x\end{Vmatrix}^2$을 이용하면 단위벡터를 표현하는 방식이 다음과 같이 되기 때문입니다.

$$\small \big(\frac{\boldsymbol x}{\lVert \boldsymbol x \rVert}\big)^{\intercal}A\big(\frac{\boldsymbol x}{\lVert \boldsymbol x \rVert}\big)=\frac{\boldsymbol x^{\intercal} A \boldsymbol x}{\lVert \boldsymbol x \rVert ^2}=\frac{\boldsymbol x^{\intercal} A \boldsymbol x}{\boldsymbol x^{\intercal}\boldsymbol x} $$

<br>이제 예제를 하나 보겠습니다.

$$\small maximize\quad \frac{x_1^2-8x_1x_2+x_2^2}{x_1^2+x_2^2}
\quad for\space \boldsymbol x=\begin{pmatrix}x_1\cr x_2\end{pmatrix}\not=\begin{pmatrix}0\cr 0\end{pmatrix}$$

어떻게 풀어야 할지 감이 오시나요? 저 식이 결국 우리가 내내 했었던, $\small\max\lbrace\boldsymbol x^{\intercal}A\boldsymbol x:\lVert\boldsymbol x\rVert=1 \rbrace$을 구하라는 말과 똑같습니다! 분자에 해당하는 이차형태로부터 대칭행렬 $A$를 구하면, $\small A=\begin{pmatrix}1&-4\cr -4&1\end{pmatrix}$이 되고, 이 행렬의 가장 큰 고유값을 구하면 될 것입니다.

<br>그렇다면, 이번엔 다음의 예제를 봅시다. 드디어 포스트 제목답게 제약조건이 조금 변경되었습니다.

$$\small maximize\quad Q(\boldsymbol x)=x_1x_2
\quad when\space x_1^2+4x_2^2=4 $$

일단 저희가 배운 내용은 ``단위벡터 하에서`` 이차형태를 최대화하는 것 뿐입니다. 따라서, 저 제한조건을 1로 만들어주기 위해 양변을 4로 나누고 다음과 같이 치환합니다.

$$\small\begin{gathered}\frac{x_1^2}{4}+x_2^2=1\cr
y_1:= \frac{x_1}{2},\space y_2:=x_2 \implies x_1=2y_1, \space x_2=y_2 \end{gathered}$$

따라서 문제는 다음과 같이 새로운 벡터로 표현할 수 있게 됩니다.

$$\small maximize\quad Q(\boldsymbol y)=2y_1y_2\quad when\space y_1^2+y_2^2=1$$

따라서, 이제 $\small Q(\boldsymbol y)$로부터 행렬 $A$를 구하면, $\small A=\begin{pmatrix} 0&1\cr 1&0\end{pmatrix}$이 될 것이고, 위에서 배운 정리에 의하여 가장 큰 고유값이 문제의 답이 될 것입니다.

<br>

## 3. 제한조건의 추가(2)
<br>

먼저 다음의 문제를 하나 보겠습니다. 벡터 $\small\boldsymbol x'=(x_1,x_2,x_3)$에 대해,

$$\small\begin{gathered} maximize\quad 5x_1^2+6x_2^2+7x_3^2+4x_1x_2-4x_2x_3\cr when\space \boldsymbol x^{\intercal}\boldsymbol x=1, \boldsymbol x \perp \boldsymbol{u_1}\end{gathered}$$

이번에는 벡터 $\boldsymbol x$가 첫번째 고유벡터 $\boldsymbol u_1$과 직교한다는 조건이 추가되었습니다. 이 최적화 문제는 어떻게 풀어야 할까요? 이와 관련하여 다음의 정리가 있습니다.

>**Thm.** 대칭행렬 $A$에 대하여, $\lambda_1$은 $A$의 가장 큰 고유값이고, $\small\boldsymbol{u_1}$은 $\lambda_1$에 해당하는 단위 고유벡터라고 하자.  
 $\small\boldsymbol x^{\intercal}\boldsymbol x=1, \boldsymbol x \perp \boldsymbol{u_1}$의 제약조건 하에서 $\small \boldsymbol x^{\intercal}A\boldsymbol x$의 최대값은 두번째로 큰 고유값 $\lambda_2$이고, 이 때의 $\boldsymbol x$는 $\lambda_2$에 해당하는 단위 고유벡터 $\small\boldsymbol{u_2}$이다.

즉, $\boldsymbol x$가 $\small\boldsymbol u_1$이 안 된다면,  그 다음 $\small\boldsymbol u_2$가 벡터 $\boldsymbol x$가 된다는 것인데요. 이것은 어느정도 자명하게 느껴지실 것도 같습니다! $P$는 orthogonal matrix입니다. 따라서 각 열들끼리는 서로 직교입니다. 한편, 아래와 같이 벡터 $\boldsymbol x$는 $P$의 열들의 선형결합으로 표현되고, $\small y_1, \cdots, y_n$은 선형결합에서 각각의 **가중치** 에 해당합니다.

$$\small\begin{aligned}\boldsymbol x = P\boldsymbol y&=\begin{bmatrix}u_{11} &\cdots & u_{n1}\cr \vdots&\ddots&\vdots\cr u_{1n}&\cdots&u_{nn }\end{bmatrix}\begin{bmatrix}y_1\cr \vdots\cr y_n
\end{bmatrix}=\begin{bmatrix} u_{11}y_1+\cdots+u_{n1}y_n\cr\vdots\cr u_{1n}y_1+\cdots+u_{nn}y_n\end{bmatrix}\cr &=\begin{bmatrix} u_{11}y_1\cr \vdots\cr u_{1n}y_1\end{bmatrix}+\cdots+\begin{bmatrix} u_{n1}y_n\cr \vdots\cr u_{nn}y_n\end{bmatrix}=y_1\boldsymbol{u_1}+\cdots+y_n\boldsymbol{u_n}\end{aligned}$$

그런데, 이 벡터 $\boldsymbol x$가 벡터 $\boldsymbol u_1$과 직교한다는 것은 $\small\boldsymbol u_1^{\intercal} \boldsymbol{x}=0$이라는 것이고, 이는 가중치에 해당하는 $y_1$이 0이 된다는 것을 의미할 것입니다.

$$\small \begin{aligned}\boldsymbol u_1^{\intercal} \boldsymbol{x} &=y_1\boldsymbol{u_1^{\intercal}} \boldsymbol{u_1}+\cdots+y_n\boldsymbol{u_1^{\intercal}} \boldsymbol{u_n}\cr
&=y_1\boldsymbol{u_1^{\intercal}} \boldsymbol{u_1}+0+\cdots+0
\cr &=y_1\cdot 1
\cr\implies y_1&=0
\end{aligned}$$

따라서, 이제 위 1번에서 한 증명을 똑같이 하면 됩니다. 다만 이 때에는 $y_1$이 0이기 때문에 상한선이 바뀌게 됩니다.

$$\small\begin{aligned}\boldsymbol y^{\intercal}D\boldsymbol y&=\lambda_1y_1^2+\lambda_2y_2^2+\cdots+\lambda_ny_n^2\cr
&=0+\lambda_2y_2^2+\cdots+\lambda_ny_n^2\cr &\le\lambda_2y_2^2+\cdots+\lambda_2y_n^2\cr
&=\lambda_2(y_2^2+\cdots+y_n^2)\cr &=\lambda_2\lVert \boldsymbol y\rVert^2=\lambda_2\end{aligned}$$

따라서, $\small \boldsymbol y^{\intercal}D\boldsymbol y$가 가장 큰 값 $\lambda_2$가 되는 경우는 $\small\boldsymbol y=\boldsymbol e_2\small$이  될 것이고, $\boldsymbol x$는 다음과 같이 고유벡터 $\small\boldsymbol u_2$가 될 것입니다.

$$\small\boldsymbol x=P\boldsymbol e_2=\begin{bmatrix}\boldsymbol u_1 & \boldsymbol u_2 &\cdots&\boldsymbol u_n \end{bmatrix}\begin{pmatrix} 0\cr 1\cr\vdots\cr 0\end{pmatrix}=\boldsymbol u_2$$

이제 3번에서 처음 던졌던 위 문제로 돌아가면, $\small Q(\boldsymbol x)=5x_1^2+6x_2^2+7x_3^2+4x_1x_2-4x_2x_3$를 통해 행렬 $A$를 구하고, 이 행렬 $A$에서 두번째로 큰 고유값 $\lambda_2$가 바로 그 답이 되는 것입니다!

$$\small \begin{aligned} Q(\boldsymbol x) &= 5x_1^2+6x_2^2+7x_3^2+4x_1x_2
-4x_2x_3\cr &\implies A=\begin{bmatrix}5&2&0\cr 2&6&-2\cr 0&-2&7 \end{bmatrix}\end{aligned}$$

$$\small\therefore \max_{\lVert \boldsymbol x\rVert=1}\boldsymbol{x^{\intercal}}A\boldsymbol x=\lambda_2\quad when\space \boldsymbol x\perp \boldsymbol{u_1}
$$

<br>

여기까지 이차형태의 제약조건이 있는 최적화 문제에 대한 내용이었습니다! 조금 이해가 되셨나요? 다음에는 Singular Value Decomposition에 대해 포스팅하겠습니다. 감사합니다 :)

<br>

---
$Reference.$  
- David C.Lay · Stephen R.Lay · Judi J.McDonald, Linear Algebra and its Applications, 5th edition, Pearson
- 고려대학교 김홍중 교수님의 수업

<br>

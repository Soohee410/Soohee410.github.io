---
layout: post
title: "[Linear Algebra] RNN에서 orthogonal matrix를 initializer로 쓰는 이유"
date:   2021-08-11
image: '/assets/img/covers/linear-algebra.jpg'
tags: [Linear Algebra]
use_math: True
---
캐글 필사 코드에서 RNN의 kernel initializer를 orthogonal로 사용하였는데 이 부분이 궁금해서 찾아보다가 [흥미로운 글](https://smerity.com/articles/2016/orthogonal_init.html)을 발견했는데 이해하기 쉽게 설명되어 있어 일부를 번역하였다! 목차는 다음과 같다.
>1. RNN의 문제점
2. Eigenvalues
3. orthogonal matrix이란?
4. init들의 시각화

## 1. RNN의 문제점

딥러닝의 기초적인 동작은 **matrix multiplication(행렬 곱셈)을 반복**하는 것이다. 그 결과 계산할수록 행렬 값은 기하급수적으로 변하고 값이 빠르게 사라지거나 치솟아오를 수 있어(무한으로 감) 안정성에 문제가 생긴다. RNN은 weight를 반복해서 곱하기 때문에 기울기 소실(gradient vanishing)이 생길 수 있다. 이를 해결하기 위한 initializer가 `orthogonal matrix`(직교 행렬)이다. 

## 2. Eigenvalues

eigenvalue를 설명하기 위해 피보나치 행렬을 예로 들어보자. 피보나치 행렬 F는 $F = Q \Lambda Q^{-1}$로 행렬 분해가 가능하다. 행렬 분해로 나타낸 행렬 F을 n제곱했을때 계산이 더 간단해지는 장점이 존재한다. 우선 F를 제곱한다면 다음과 같다.

<br>

$$F^2 = (Q \Lambda Q^{-1})(Q \Lambda Q^{-1})$$

$$= Q \Lambda (Q^{-1}Q) \Lambda Q^{-1}$$

$$=Q \Lambda^2 Q^{-1}$$

<br>
따라서 F를 n제곱했을때 `Λ만 n제곱`하면 된다. 이로 인해 행렬 곱셈을 반복했을 때 사라지거나 너무 증가하는 문제가 발생하게 된다. 피보나치 행렬을 n제곱하게 되면 하나의 eigenvalue는 증가하는 반면 하나의 eigenvalue는 사라지는 것을 볼 수 있다. 따라서 행렬을 반복적으로 곱했을 때 eigenvalue에 따라 다음과 같은 결과를 확인할 수 있다.

- 모든 eigenvalue의 절대값이 1보다 작으면 $F^n$은 소실된다.
- 모든 eigenvalue의 절대값이 1이면 $F^n$은 일정한 norm을 유지한다.
- 어떤 eigenvalue의 절대값이 1보다 크면 폭발한다.

## 3. orthogonal matrix이란?

행벡터와 열벡터가 유클리드 공간의 `정규 직교 기저`를 이루는 실수 행렬을 의미한다. 정규 직교 기저란 벡터의 크기가 1이고 서로 수직인 기저 벡터라고 할 수 있다. 수식으로 보자면 행렬 A가 있을 때 아래의 식을 만족한다.

<br>

$$A^{-1}=A^{T} or AA^{T}=A^{T}A$$

<br>
여기서 직교하기 때문에 각 벡터간의 $\theta$가 $90°$가 되고 두 벡터간의 cosine similarity($\|\|a\|\|\|\|b\|cos{\theta}$)는 $cos90°$로 0이 된다. 즉, `서로 독립적인 벡터`들이란 의미이다. 이 orthogonal matrix의 가장 중요한 특성은 `모든 eigenvalue의 절대값이 1`이라는 것이다. 그 결과 matrix를 여러번 곱해도 소실되거나 폭발하지 않게 된다. eigenvalue가 1 또는 -1일 경우 매우 지루한 행렬이 생성된다. 그러나 복소수의 영역까지 확장하면 여러번 곱할 때 훨씬 더 흥미로운 결과를 얻을 수 있다. eigenvalue가 복소수인 경우에도 실수인 행렬이 생성될 수 있다. 간단한 예로는 2x2 rotation 행렬이다. rotatoin 행렬 R은 실수로 이루어진 행렬이지만 eigenvalue들과 eigenvector들은 복소수이다.

<br>

$$R = \left(\begin{matrix} 0 & 1 \\ -1 & 0\\ \end{matrix} \right)$$

$$\Lambda = \left(\begin{matrix} li & 0 \\ 0 & -li\\ \end{matrix} \right)$$

$$Q = \left(\begin{matrix} 0.707 & 0.707 \\ 0.707i & -0.707i\\ \end{matrix} \right)$$

<br>
## RNN을 위한 orthogonal init

orthogonal init에 집중하기 위해 **단순한 RNN 모델**을 보자. 단순하게 하기 위해 input, bias, activation function가 없고 $h_0$의 initial hidden state가 indentify matrix라고 가정한다.

<br>

$$h_t=f(Wh_{t−1}+Vx_{t})=f(Wh_{t−1})=Wh_{t−1}$$

$$h3=W(W(Wh_0))=W^3h_0=W^3I=W^3$$

<br>
RNN 내에서 gradient가 소실되면 정보가 backpropagation되지 않으므로 training이 정지된다. 반대로 gradient가 폭발하면 gradient update가 크게 변동하므로 training이 수렴되지 않을 수 있다. 위의 이유로 RNN에서 weight matrix를 초기화할 때 random uniform이나 random normal initialization을 사용하는 것은 일반적이지 않다. `orthogonal matrix`를 사용하면 폭발하거나 소실되지 않아 더 효과적으로 backpropagation할 수 있다.

## 4. init들의 시각화

아래의 영상은 64개의 time step에 걸쳐 반복된 행렬 곱셈을 수행한 결과를 시각화한 것이다. RNN이 수백, 수천 개의 time step을 실행하는 것이 드문 일은 아니다.

### 작은 eigenvalue로 인한 소실 행렬

<video controls loop>
    <source src="/assets/video/orthogonal-matrix/eigenvalue_vanish.m4v" type="video/mp4">
</video>

### 1보다 큰 eigenvalue로 인한 폭발 행렬

<video controls loop>
    <source src="/assets/video/orthogonal-matrix/eigenvalue_explode.m4v" type="video/mp4">
</video>

### orthogonal 행렬

<video controls loop>
    <source src="/assets/video/orthogonal-matrix/eigenvalue_orthogonal.m4v" type="video/mp4">
</video>

 ---

$Reference.$
- [https://smerity.com/articles/2016/orthogonal_init.html](https://smerity.com/articles/2016/orthogonal_init.html)
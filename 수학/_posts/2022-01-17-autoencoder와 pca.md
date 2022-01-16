---
title: autoencoder와 PCA
---


입력으로 \\(n\\)-벡터를 받으면 함수값으로 \\(l\\)-벡터(\\(n>l\\))를 내는 함수 \\(f(\mathbf{x})\\)와 거꾸로 입력으로 \\(l\\)-벡터를 받으면 함수값으로 \\(n\\)-벡터를 내는 함수 \\(g(\mathbf(c})\\)가 있다 하자. 



(1) 먼저, 처음의 입력 \\(\mathbf{x}_0\\)과 \\(g(f(\mathbf{x}_0))\\)의 값의 차이를 최소로 하는 함수 \\(f\\)와 \\(g\\)가 어때야 하는지를 생각해보자.


일단, 문제를 간소화 하기 위해 \\(g(\mathbf{c})= D\mathbf{c}\\) (단, \\(D = \begin{bmatrix} \mathbf{d}_1 & \cdots & \mathbf{d}_l \end{bmatrix}, \mathbf{d}_1, \cdots, \mathbf{d}_l 는 모두 orthonormal인 \\(n\\)-벡터) 꼴이라 하자.

그리고 \\(f(\mathbf{x})\\)를 \\(F(f) = \int \| \mathbf{x} - g(f(\mathbf{x})) \|_{2}^2 d\mathbf{x} \\) 값을 최소로 하는 함수라 하자. 

한편 입력으로 함수를 받아 함수값으로 스칼라값을 내는 함수를 범함수(functional)라 하는데, 위 식의 \\(F(f)\\)가 이러한 범함수라 할 수 있다. 범함수의 최솟값은 변분법(calculus of variations)으로 구할 수 있으며, 변분법으로 \\(F(f)\\)의 최솟값을 구하려면 다음을 만족하는 \\(f\\)를 구하면 된다.

\\(\nabla_f \| \mathbf{x} - g(f(\mathbf{x})) = 0 \\)

이 식을 정리하면 \\(f(\mathbf{x}) = D^T \mathbf{x}\\) 일 때 \\(F(f)\\)가 최솟값을 가짐을 알 수 있다.



(2) \\(f\\)와 \\(g\\)의 \\(D\\) 행렬이 어때야 하는지를 구해보자.

먼저, \\(X\\)와 \\(R\\)을 다음과 같이 정의하자.

$$

X = 

\begin{bmatrix} \mathbf{x}_1^T \\ 
\vdots \\
\mathbf{x}_m^T \end{bmatrix},


R = 

\begin{bmatrix} g(f(\mathbf{x}_1))^T \\ 
\vdots \\
g(f(\mathbf{x}_m))^T \end{bmatrix}

= 

\begin{bmatrix} \mathbf{x}_1^T D D^T \\ 
\vdots \\
\mathbf{x}_m^T D D^T \end{bmatrix}

= XDD^T

$$

이때, \\(E = X-R = X-DD^T\\)로 정의하면, 여기서 찾고자 하는 \\(D\\)는 \\(E\\)의 Frobenius norm(\\(\|E\|_F\\))을 최소로 하는 \\(D\\)이다. 

그런데 \\(\|E\|_ F\\)를 정리하면 \\(-\mathrm{tr}(D^T X^T X D) = - \sum_{i=1}^l \mathbf{d}_i^T X^T X \mathbf{d}_i\\)를 얻는다. 

이 식은 각 \\(\mathbf{d}_i\\)에 대한 \\(X^T X\\)의 이차형식의 합이므로, 각 이차형식을 최대로 하는 벡터(=\\(X^T X\\)의 고유벡터들)를 \\(\mathbf{d}_i\\)로 하면 최적의 \\(D\\)를 구함을 알 수 있다.

따라서, \\(\mathbf{d}_1, \cdots, \mathbf{d}_l\\)이 \\(X^T X\\)의 가장 큰 \\(l\\)개의 고유값에 해당하는 고유벡터들일 때 최적의 \\(D\\)를 얻는다.
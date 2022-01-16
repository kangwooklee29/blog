### 1. range, null space

\- \\(n\\)개의 벡터로 이루어진 어떤 집합 \\(A\\)가 있을 때, **그 집합의 원소 벡터들의 linear combination으로 만들 수 있는 모든 벡터의 집합**을 \\(A\\)의 **span**이라 한다. 

\- \\(m \times n\\) 크기의 어떤 행렬 \\(A\\)에 대하여 다음이 정의된다.

  - \\(m \times n\\) 행렬에 \\(n\\)-벡터를 곱하면 결과로 \\(m\\)-벡터를 얻는다. \\(A\\)에 임의의 \\(n\\)-벡터를 곱하여 얻은 **가능한 모든 \\(m\\)-벡터의 집합**(\\(\left\\{ \mathbf{v} \mid \mathbf{v}=  A \mathbf{x}, \, \mathbf{x} \in \mathbb{R}^n \right\\} \\))을 \\(A\\)의 열공간(column space) 또는 **range**라 한다. \\(col(A)\\) 또는 \\(\mathcal{R}(A)\\)로 쓴다.

    - \\(A \mathbf{x}\\)은 \\(A\\)의 열벡터들의 linear combination으로 표현되는 벡터이므로, \\(A\\)의 range를 \\(A\\)의 열벡터들로 이루어진 집합의 span이라고 생각할 수도 있다. 

    - linear system \\(A\mathbf{x} = \mathbf{b}\\) 가 해가 존재한다면 \\(\mathbf{b} \in col(A)\\) 이고, 해가 존재하지 않는다면 \\(\mathbf{b} \notin col(A)\\) 이다. 

  - \\(A\\)에 어떤 \\(n\\)-벡터를 곱하여 결과로 zero vector를 얻을 수 있다 할 때, **zero vector를 얻을 수 있게 하는 모든 \\(n\\)-벡터의 집합**(\\(\left\\{ \mathbf{x} \mid  A \mathbf{x} = \mathbf{0} \right\\} \\))을 \\(A\\)의 **null space**라 한다. \\(\mathcal{N}(A)\\)로 쓴다.


\- 어떤 행렬 \\(A\\)에 대하여 다음이 성립한다.

- \\(\mathcal{R}(A^T) \cap \mathcal{N}(A)\\)의 원소는 오로지 \\(\mathbf{0}\\)뿐임이 알려져 있다.

- 모든 \\(n\\)-벡터는 \\(\mathcal{R}(A^{T})\\)의 원소와 \\(\mathcal{N}(A)\\)의 원소의 합으로 표현할 수 있음이 알려져 있다. 

- 어떤 벡터가 어떤 벡터 집합의 모든 원소 벡터와 직교한다면 '그 벡터가 그 벡터 집합과 직교한다'라고 말하며, 어떤 벡터 집합 \\(X\\)에 대하여 \\(X\\)와 **직교하는 모든 벡터를 모아놓은 벡터 집합**을 \\(X\\)의 직교여공간(orthogonal complement)이라 한다. \\(\mathcal{R}(A^T)\\)와 \\(\mathcal{N}(A)\\)는 서로 직교여공간 관계임이 알려져 있다.


### 2. projection, 최소제곱법, 선형회귀

#### 1) projection과 최소제곱법

\- \\(m \times n\\) 크기의 어떤 행렬 \\(A\\)과 \\(\mathcal{R}(A)\\)에 속하지 않는 어떤 \\(m\\)-벡터 \\(\mathbf{b}\\)가 있을 때, \\(\mathcal{R}(A)\\)의 원소 벡터 중 \\(\mathbf{b}\\)와 가장 거리가 가까운(L2 norm이 가장 작은) 원소 벡터를 \\(\mathbf{b}\\)의 \\(A\\)에 대한 projection이라 하며, \\(\mathrm{Proj}(\mathbf{b}; A)\\)로 쓴다. 

- linear system의 관점에서 보면, \\(A\mathbf{x} = \mathbf{b}\\)는 해가 없는 linear system이며 \\(\mathrm{Proj}(\mathbf{b}; A) = \bar{\mathbf{b}}\\)로 쓰면 linear system \\(A \bar{\mathbf{x}} = \bar{\mathbf{b}}\\) 의 해 \\(\bar{\mathbf{b}}\\)를 \\(A\mathbf{x} = \mathbf{b}\\)의 근사해라고 할 수 있다. 이러한 근사해를 최소제곱해(least squares solution)이라 하며, 최소제곱해를 구하는 것을 최소제곱법이라 한다.

\- 어떤 inconsistent한 linear system \\(A\mathbf{x} = \mathbf{b}\\)의 최소제곱해(=\\(A \bar{\mathbf{x}} = \bar{\mathbf{b}}\\)의 해)는 **이 linear system의 양변 각 항의 앞쪽에 \\(A\\)의 전치행렬 \\(A^T\\)를 곱한 새로운 linear system \\(A^T A \bar{\mathbf{x}} = A^T \mathbf{b}\\)의 해 \\(\bar{\mathbf{x}}=(A^T A)^{-1}  A^T \mathbf{b}\\)와 같다.** 


- 미분의 개념을 사용하여 이를 증명할 수 있다. 

  \\(\|A\mathbf{x}-\mathbf{b}\|_{2}^{2} = (A\mathbf{x}-\mathbf{b})^T (A\mathbf{x}-\mathbf{b}) = \mathbf{x}^T A^T A \mathbf{x} - 2\mathbf{b}^T A \mathbf{x} + \mathbf{b}^T \mathbf{b}\\) 이므로,

  \\(\nabla \|A\mathbf{x}-\mathbf{b}\| _{2}^{2} = 2A^T A \mathbf{x} - 2 A^T \mathbf{b} \\) 를 얻는다. 
  
  여기서 \\(\mathbf{x}\\)에 \\((A^T A)^{-1}  A^T \mathbf{b}\\)를 대입해야 \\(\nabla \|A\mathbf{x}-\mathbf{b}\| _{2}^{2} \\)가 0이 됨을 알 수 있다.


\- \\(\mathrm{Proj}(\mathbf{b}; A) = A(A^{T} A)^{-1}A^{T}\mathbf{b}\\) 임을 간단히 유도할 수 있다.

- \\(\mathrm{Proj}(\mathbf{b}; A) = \bar{\mathbf{b}}\\) 로 두고 \\(\bar{\mathbf{x}}\\)를 \\(A \bar{\mathbf{x}} = \bar{\mathbf{b}}\\)의 해라 하면, 

  앞서 보았듯 \\(\bar{\mathbf{x}} = (A^T A )^{-1} A^T \mathbf{b}\\) 를 만족시킨다. 

  그런데 \\(\bar{\mathbf{b}} = A \bar{\mathbf{x}}\\) 이므로, 

  \\(\mathrm{Proj}(\mathbf{b}; A) = A(A^{T} A)^{-1}A^{T}\mathbf{b}\\) 를 얻을 수 있다.
  



#### 2) 선형회귀

\- 선형회귀(linear regression)은 n개의 m차원 좌표가 주어졌을 때 이 좌표들을 '가장 잘 설명하는' linear equation을 구하는 방법이다. 주어진 좌표 \\(\mathbf{x}_i\\) 를 

$$ \begin{bmatrix} x_{i, 1} & \cdots & x_{i, n} \end{bmatrix} $$

로 쓸 수 있고 구하고자 하는 linear equation이 \\(a_0 + a_1 x_1 + \cdots + a_{n-1} x_{n-1} = x_n\\) 이라면, 이 n개의 좌표들을 가장 잘 설명하는 linear equation의 계수 \\(a_i\\)는 다음 linear system의 최소제곱해이다.

$$ \begin{bmatrix} 1 & x_{1, 1} & \cdots & x_{1, n-1} \\
\vdots & \vdots & \vdots & \vdots \\
 1 & x_{n, 1} & \cdots & x_{n, n-1} \end{bmatrix} 
 \begin{bmatrix} a_0 \\
 \vdots \\
 a_{n-1} \end{bmatrix} =
 \begin{bmatrix} x_{1, n} \\
 \vdots \\
 x_{n, n} \end{bmatrix}
$$


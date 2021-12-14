### 1. linear system

\- \\(a_{1,1} \ x_1 + a_{1,2} \ x_2 = b\\) 와 같이 미지수 \\(x_i\\)에 관한 1차 방정식을 linear equation이라 하며, 이러한 linear equation 유한개의 집합을 linear system이라 한다.

\- \\(n \times n\\) 행렬 \\(A\\), 열벡터 $$ \mathbf{x} = \begin{bmatrix} x_1 \\
 \vdots \\ 
 x_n \end{bmatrix} $$, 열벡터 \\( \mathbf{b} \\) 로 썼을 때, 모든 linear system은 \\( A \cdot \mathbf{x} = \mathbf{b} \\) 꼴로 표현할 수 있다.

- \\(A^{-1} \\) 이 존재하지 않는다면 \\( A \\)는 singular하다고 한다. 이 경우 이 linear system은 해가 없거나 무수히 많다.

- 해가 있으면 이 linear system은 consistent하다고 하며, 해가 없다면 반대로 inconsistent하다고 한다.


### 2. 가우스 소거법

\- 가우스 소거법은 행렬곱 식으로 표현된 linear system의 해를 구하는 알고리즘으로, 열벡터 \\(\mathbf{x}\\)의 계수인 행렬에 ERO(elementary row operation)를 수행하는 작업을 반복하여 해를 구한다.

#### 1) ERO

(1) 행렬의 i번째 행의 모든 성분에 똑같은 상수를 곱한다.

- 이 연산은 단위행렬에서 i번째 행의 원소를 그 상수로 바꾼 행렬에 이 행렬을 곱하는 연산을 수행해도 같은 결과를 얻는다.

(2) 행렬의 i번째 행의 각 성분을 j번째 행의 각 성분에 더한다.

(3) 행렬의 i번째 행과 j번째 행을 서로 맞바꾼다.

- 이 연산은 단위행렬에서 i번째 행과 j번째 행을 맞바꾼 행렬(이러한 행렬을 흔히 \\(\mathbf{P}\\)로 쓴다)에 이 행렬을 곱하는 연산을 수행해도 같은 결과를 얻는다.

#### 2) forward elimination과 back substitution

(1) 첫 번째 행을 이용하여, 두 번째 행부터 마지막 행까지 모두 첫 번째 성분을 0으로 만드는 ERO를 수행한다. (=각 행의 첫 번째 성분을 0으로 만들기 위하여, 첫 번째 행의 각 성분에 임의의 상수를 곱한 값을 그 행의 각 성분에서 뺀다. 만약 첫 번째 행의 첫 번째 성분이 0이라면 첫 번째 성분이 0이 아닌 다른 행과 맞바꾸는 연산을 먼저 수행한다.)

(2) 'i번째 행을 이용하여 i+1번째 행부터 마지막 행까지 모두 i번째 성분을 0으로 만드는 ERO를 수행'하는 작업을 두 번째 행부터 마지막 행까지 반복한다. (이를 앞에서부터 차례대로 수행한다 하여 forward elimination이라 한다.)

\- 위 단계를 반복하면 기존 행렬곱 식이 

$$ \begin{bmatrix} 

M_{1,1} & M_{1,2} & \cdots &  M_{1,n} \\ 
0 & M_{2,2} & \cdots & M_{2,n} \\
\vdots & \vdots & \cdots & \vdots \\
0 & 0 & \cdots & M_{n,n}
\end{bmatrix} \begin{bmatrix} x_1 \\ x_2 \\ \vdots \\ x_n \end{bmatrix} = \mathbf{c} $$ 

와 같은 형태가 된다. 맨 마지막 행을 통해 \\(x_n\\) 의 값을 알 수 있고, 이 값을 그 바로 윗식에 대입하여 \\(x_{n-1}\\)의 값을 알 수 있으며 이러한 과정을 반복하여 모든 \\(x_i\\)의 값을 알 수 있다. (이러한 방식을 back substitution이라 한다.)

\- forward elimination을 통해 다음과 같은 사실을 알 수 있다.

(a) 그 linear system의 rank를 알 수 있다. 

- 어떤 linear system에서 n개의 linear equation이 주어졌을 때 각각을 상수배 하더라도 **다른 방정식과 동일해지지 않는 방정식의 개수**를 그 linear system의 rank라 한다. 

- forward elimination을 수행하면 열벡터 \\(\mathbf{x}\\)의 계수 행렬의 어떤 행의 모든 성분이 0인 행(그에 대응되는 등호 우변의 열벡터의 성분 포함)이 나오는 경우가 있다. 이는 다른 행을 상수배 하면 그 행과 모든 성분이 정확히 일치하여 ERO 과정에서 사라지게 된 것이다. linear system의 rank는 이처럼 열벡터 \\(\mathbf{x}\\)의 계수 행렬 중 모든 성분이 0인 행을 제외한 행의 수와 같다. 

(b) 그 linear system에 해가 존재하는지, 해가 무수히 많은지 여부를 알 수 있다.

- 미지수가 n개인 linear system인데 rank가 n보다 작다면 그 linear system은 해가 무수히 많은 것이다. 

- 열벡터 \\(\mathbf{x}\\)의 계수 행렬의 어떤 행의 모든 성분이 0이지만 그에 대응되는 등호 우변의 열벡터의 성분은 0이 아니라면 그 linear system은 inconsistent한 것이다.


### 3. LU 분해

다음과 같은 행렬을 lower triangular matrix라 한다.

$$L = \begin{bmatrix} 

l_{1,1} & 0 & \cdots &  0 \\ 
l_{2,1} & l_{2,2} & \cdots & 0 \\
\vdots & \vdots & \cdots & \vdots \\
l_{n,1} & l_{n,2} & \cdots & l_{n,n}
\end{bmatrix}  $$ 



다음과 같은 행렬을 upper triangular matrix라 한다.


$$U = \begin{bmatrix} 

u_{1,1} & u_{1,2} & \cdots &  u_{1,n} \\ 
0 & u_{2,2} & \cdots & u_{2,n} \\
\vdots & \vdots & \cdots & \vdots \\
0 & 0 & \cdots & u_{n,n}
\end{bmatrix}  $$ 


\- 정사각행렬을 \\(LU\\) 꼴로 인수분해하는 것을 LU 분해라 하며, 모든 정사각행렬은 \\(PLU\\) 꼴로 인수분해가 가능함이 알려져 있다. 

\- 정사각행렬을 LU 분해 할 수 있다는 것은 매우 빠른 시간 안에 linear system의 해를 구할 수 있음을 뜻한다. 정사각행렬이 항상 LU 분해가 가능하다고 했으므로 행렬곱 식 \\( A \cdot \mathbf{x} = \mathbf{b} \\) 은  \\( L \cdot \left( U \mathbf{x} \right) = \mathbf{b} \\) 으로 쓸 수 있는데, 이는 곧 back substitution을 두 번 수행하는 것만으로 이 linear system의 해를 구할 수 있음을 뜻한다. 즉, \\(A\\)의 LU 분해를 알고 있다면 **가우스 소거법을 사용하지 않고 O(n) 시간 안에 이 linear system의 해를 구할 수 있는 것**이다. (이는 \\( A\\)의 값은 변하지 않고 우변의 열벡터의 값이 수시로 변하는 문제에서 해를 빠른 시간 내에 업데이트 해야 하는 경우 등에서 매우 중요하다.)




### 4. linear combination의 관점에서 linear system의 해 구하기

#### 1) linear combination과 linear system

\- 행렬 \\(A\\)의 각 성분 \\(a_{i, j}\\)에 대해 열벡터 

$$ \mathbf{a}_j =  \begin{bmatrix}  a_{1, j} \\ \vdots \\ a_{m, j} \end{bmatrix} $$ 

로 정의할 때, linear system \\(A \mathbf{x} = \mathbf{b}\\)를

$$ A  \mathbf{x} = \begin{bmatrix} \mathbf{a}_1 & \cdots & \mathbf{a}_n \end{bmatrix} \begin{bmatrix} x_1 \\ \vdots \\ x_n \end{bmatrix} = \mathbf{a}_1 x_1 + \cdots + \mathbf{a}_n x_n = \mathbf{b}$$

으로 쓸 수 있다. 이는 마치 열벡터 \\(\mathbf{b}\\)를 서로 다른 n개의 벡터에 각각 가중치를 곱한 값으로 표현한 것과 같은 꼴이다. 이처럼 어떤 열벡터를 서로 다른 벡터들에 각각 가중치를 곱한 값으로 표현하는 것을 linear combination이라 한다.

\- linear system에서 '해를 구한다'라는 것을 linear combination 관점에서 보면, '서로 다른 n개의 성분 벡터를 조합해 특정 열벡터와 같아지게 하는 가중치 값의 조합을 구한다'라는 말과 같은 뜻을 가짐을 알 수 있다.

\- 한편 \\( x_1 \mathbf{a}_1 + \cdots + x_n \mathbf{a}_n  = \mathbf{b} \\) 이 식을 잘 보면, 마치 n차원 공간상의 좌표 \\(\mathbf{x}\\)를 각 축의 벡터가 각각 \\(  \mathbf{a}_1 , \cdots, \mathbf{a}_n \\)인 n차원 좌표계에 투영시켜 \\(\mathbf{b}\\) 라는 새로운 좌표로 변환시킨 것으로 볼 수도 있다. 즉 \\( A \cdot \mathbf{x} = \mathbf{b} \\) 라는 식은 \\(\mathbf{x}\\) 라는 좌표를 \\(A\\)라는 좌표계를 통해 \\(\mathbf{b}\\)라는 좌표로 변환한 것으로 볼 수 있는 것이다. 이러한 관점에서 linear system을 보면, 좌표계 \\(A\\)와 이 좌표계를 통해 변환된 좌표 \\(\mathbf{b}\\)를 알 때 원래의 좌표 \\(\mathbf{x}\\)를 구하는 과정으로 볼 수 있다.


#### 2) column space

\- 행렬 \\(A\\)에 **가능한 모든 가중치 조합 열벡터** \\(\mathbf{x}\\)를 곱해 만든 행렬곱 \\(A\mathbf{x}\\)을 모은 집합을 열공간 \\(col(A)\\)라 한다. 

- linear system \\(A\mathbf{x} = \mathbf{b}\\) 가 consistent 하다면 \\(\mathbf{b} \in col(A)\\) 이고, inconsistent 하다면 \\(\mathbf{x} \notin col(A)\\) 이다.


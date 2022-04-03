### 1. 문제 정의

\- classification 문제가 어떤 사진에 어떤 물체가 있는지 여부를 판별하는 문제라 하면, localiazation 문제는 그 사진에서 그 물체가 어떤 위치에 있는지, 얼만큼의 영역을 차지하는지를 구하는 regression 문제의 하나라고 할 수 있다. object detection은 사진 안에서 찾고자 하는 모든 물체의 존재 여부와 있을 경우 그 위치와 차지하는 영역의 크기를 모두 구하는 문제라 할 수 있다.

\- supervisied learning을 할 때 주어지는 정답 결과 벡터는 다음과 같이 구성된다.

- 물체의 존재 여부

- 물체의 중심점의 좌표(x, y)

- 물체가 차지하는 영역의 가로, 세로 길이

- 물체의 정답 클래스가 표시돼 있는 one-hot code



### 2. object detection 분야의 발전사

\- 2012년 deep learning이 성과를 내기 이전만 해도 object detection 분야는 연구 역사가 길었음에도 불구하고 발전에 정체가 있어 기술적 한계를 넘기 어려울 거라는 예상이 지배적이었으나, deep learning 기술이 발전하면서 급속도로 발전했다. 

\- object detection 관련하여 가장 먼저 등장한 deep learning 알고리즘은 RCNN(Regions with Convolutional Neuron Networks features)로, 2014년 발표되었다. RCNN은 먼저 물체의 위치를 파악하고 그 다음에 그 물체의 클래스를 분류하는 'two-stage detection' 구조를 갖는다. 
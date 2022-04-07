### 1. semantic segmentation

\- 주어진 이미지를 픽셀 단위로 클래스를 분류하는 문제를 푸는 것을 semantic segmentation이라 한다. (이미지 내 서로 다른 객체가 서로 다른 객체인 것까지 인지할 필요는 없고 클래스가 같은지 다른지만 알아내도 충분하다.) 인물 사진을 찍었을 때 인물과 배경을 구분하는 문제, 자율주행 차량이 카메라로 외부 정보를 받았을 때 차량과 차도와 인도와 인물을 구분하는 등의 문제를 풀 때 중요한 문제가 된다. 또, 사람의 얼굴이나 전신 이미지를 보고 각 부위별 특징을 파악한 다음에 얼굴 표정변화나 사람의 자세를 정확히 파악하는 문제를 풀 수도 있다. 

\- semantic segmentation은 같은 클래스에 속하는 픽셀이라면 실제 각 물체가 서로 구분되는 별개의 물체라 하더라도 같은 클래스의 픽셀로 분류한다. 이때 만약 object detection을 한 후 각 region에 대해 semantic segmentation을 하면 이 문제를 해결할 수 있다. (이를 instance segmentation라 한다.)

\- 신경망에 입력으로 들어온 이미지는 신경망을 거쳐 최종적으로 클래스 개수만큼 채널이 있고, 채널마다 0 또는 1의 값을 갖는 행렬들로 변환된다. (하나의 채널에 하나의 행렬이 있고 행렬 각 성분의 값이 클래스명이 되는 식으로 처리하지 않는데, 그렇게 처리하면 연산량이 늘기 때문이다.) 이미지의 한 픽셀마다 그 픽셀이 각 클래스로 분류될 수 있는지에 관한 정보를 얻게 되며, 여기서 argmax 연산을 통해 최종적으로 그 픽셀을 여러 클래스 중 어느 하나로 분류하게 된다.

\- 처음 신경망에 입력으로 들어온 이미지는 encoder를 거쳐 저해상도 feature map을 내고, 이는 다시 decoder의 입력이 되어 다시 최종적으로 입력으로 들어온 이미지와 같은 해상도를 갖는 행렬로 결과를 낸다. 이처럼 encoder와 decoder를 거쳐 해상도가 줄어들었다 늘어나는 식으로 처리하는 이유는, (1)feature map을 통해 변환된 형태로 처리하는 것이 물체의 특징을 더 잘 감지해 처리하며 (2)저해상도 행렬이 연산 속도가 더 빠르기 때문이다.


### 2. FCN(fully convolution network)

\- 기존 CNN 알고리즘들은 모두 가장 마지막에는 FC layer를 사용하여 classification을 수행했는데, 이렇게 할 경우 (1)입력 이미지의 어떤 픽셀이 그 이미지를 특정 클래스로 분류하게 하는지에 관한 정보 없이 그냥 정답 클래스만 나타난다는 점, (2)입력 이미지의 크기가 고정된다는 점 등의 단점이 있다. FC layer를 사용하지 않고('fully convolution network') 약간 다른 관점에서 접근할 경우 기존 CNN 알고리즘으로 이 두 문제를 해결할 수 있다.

\- 입력 이미지를 신경망을 통과시키면 각 클래스에 해당하는 heatmap과 같은 꼴을 갖는 행렬을 클래스 하나마다 얻는다. 여기서 heatmap이 밝게 나타나는 부분이 '그 부분에 해당하는 픽셀은 그 클래스에 해당한다'라고 분류함으로써 semantic segmentation 문제를 풀 수 있다. 또, 이런 알고리즘은 입력 행렬을 flatten해서 고정된 크기 입력을 받는  FC layer로 넣는 게 아니므로 입력 이미지의 크기가 고정돼 있을 필요가 없다.

\- 다만 이와 같은 방식으로 입력 이미지를 처리할 경우 마지막에 얻는 이미지의 크기는 해상도가 낮아져 있으므로, 처음 입력으로 넣은 이미지에 대응시키려면 어떻게든 크기를 키울 필요가 있다. 이때 단순히 보간법(bilinear interpolation)으로 크기를 키운 다음 빈칸에 숫자를 채워넣으면 어떻게 크기는 맞출 수 있으나(upsampling) 정보 손실이 일어났다는 문제는 여전히 남아 있다.

\- 크기를 키운 다음 빈칸에 숫자를 채워넣을 때, 이와 같은 보간법을 사용하지 않고 앞에서 신경망을 완전히 거치지 않은 중간 결과물을 이용하여 convolution 연산을 거꾸로 해서 숫자를 채워넣으면(transpose convolution) 정보 손실 문제를 다소 완화할 수 있다. (가장 최근의 중간결과물뿐 아니라 그 앞의 중간결과물을 더 사용하면 정보 손실 문제를 더 크게 완화할 수 있다. 이를 multiscale prediction이라 한다.)


### 3. PSPNet(pyramid scene parsing network)

\- PSPNet은 입력으로 들어온 이미지를 feature map으로 변환한 후 이를 다시 아주 낮은 해상도의 feature map부터 여러 해상도의 feature map으로 변환하는 pooling을 수행한 후 이들 feaure map을 이어붙인 것을 다시 다음 신경망의 입력으로 하는 구조를 갖는다. 여러 해상도의 feature map을 동시에 사용하면 기존 FCN 등의 알고리즘에서 사용한 것보다 더 높은 해상도의 feature map도 사용하게 되므로, FCN에서 한 것과 같이 낮은 해상도에서의 semantic segmentiation 이후 transpose convolution으로 고해상도에서의 semantic segmentation을 유추하지 않고 고해상도의 semantic segmentation을 높은 정확도로 얻을 수 있다.



### 4. Mask RCNN

\- instance segmentation을 수행하는 알고리즘으로, 구체적으로 Faster RCNN으로 object detection을 한 다음 FCN으로 semantic segmentation을 수행하는 구조를 갖고 있다. 

\- object detection 이후 semantic segmentation을 할 때는 object detection을 통해 얻게 된 물체의 boundary box 정보를 통해 이미지에서 semantic segmentation을 수행할 영역의 범위를 얻어온다. 이때 바로 원본 이미지에서 boundary box 영역만큼 잘라내 그걸 다시 base network에 넣는 게 아니라, base network를 거쳐 feature map 상태가 된 것에 대하여 boundary box 영역만큼 잘라내 semantic segmentation을 수행한다. 근데 feature map은 원본 이미지에 비해 해상도가 훨씬 낮으므로, 이 feature map에 대하여 boundary box 영역만큼 잘라내게 되면 실제 원본 이미지에서 잘라내는 것보다 정보가 손실되거나 불필요한 정보가 포함되는 일이 일어날 수 있다. 따라서 단순히 feature map을 자른 후 이에 대해 semantic segmantation을 수행하는 게 아니라, feature map을 보고 '만약에 높은 해상도 상태에서 boundary box를 잘라냈더라면 그때의 feature map의 값은 어떻게 구성될지'를 유추하여(보간법을 사용한다) 그 feature map에 대하여 semantic segmentation을 수행한다. 이렇게 새 feature map을 구하는 과정을 RoI align이라 한다.
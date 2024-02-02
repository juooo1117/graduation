# Graduation Project - FaceDetection
주제: 초상권 침해 방지를 위한 동영상 얼굴 모자이크 시스템


## ✏️ 프로젝트 소개
  - 동영상 공유 플랫폼을 이용한 컨텐츠 공유가 활발해 지면서 초상권 침해 문제가 발생하고 있다.
  - 타인의 초상권 침해를 방지하기 위해서, 촬영 후 편집 작업을 통해 특정인물을 제외한 불특정 인물의 얼굴이나 신체를 모자이크하는 방법을 사용할 수 있으나, 이 과정은 번거롭다는 불편함이 존재한다.
  - 영상 모자이크 처리는 이미 상용화되어 있으나, 사용자가 영상 속에서 모자이크 처리할 부분을 직접 선택해 주어야 하는 번거로움이 있다.
  - 따라서 별도로 등록한 인물의 얼굴을 인식하여, 동영상 내 등장하는 모든 인물과 대조한 뒤, 일치하지 않는 불특정 다수의 얼굴을 자동으로 모자이크 처리하는 시스템을 구현하였다.


## 📖 연구 활용 이론 분석

#### 1. Convolutional Neural Network
   - CNN은 이미지 인식에 좋은 성능을 가지고 있어 가장 널리 사용되는 알고리즘이다.
   - Convolution과 Pooling을 반복해 이미지가 가진 특징(Feature)을 추출하고, 완전 연결 계층(Fully connected layers)을 통해 입력된 이미지를 분류하기 위한 학습을 수행한다.
   - CNN은 학습에 필요한 전체 파라미터 수를 감소하여 모델의 학습 속도를 빠르게 하는 장점이 있다.


#### 2. dlib Library 
   - dlib 라이브러리를 활용하여 얼굴 탐색 시, 보통 HOG(Histogram of Oriented Gradients) 기법을 활용하거나 학습된 CNN 모델을 사용한다.
   - 성능 향상을 위하여 CNN 학습을 사용하고자 ‘dlib_face_recogni tion_resnet_model_v1.dat’를 활용하였다.
   - 동영상과 사진 속의 얼굴을 인식하기 위해서 아래의 세 과정을 거친다.
   - 128개의 숫자는 딥러닝의 결과이므로 각 숫자가 가진 의미는 없지만, 같은 사람의 얼굴을 입력했을 때 비슷한 숫자의 나열이 생성되는 특징을 가진다.
<p align="center">
  <img src="https://github.com/juooo1117/Graduation_FaceDetection/assets/95035134/d5cf61cc-2bf0-4b53-bf70-c4561d751763" width="650" height="100">
</p>


#### 3. 얼굴 간의 거리(Similarity) 구하기
   - 사진을 이용해 face encoding 해둔 값과 동영상에서 새로 인식한 얼굴의 face encoding 값을 비교하는 방법을 통해서 가장 비슷한 얼굴을 가진 사람을 찾는다.
   - 즉, 동영상 속 등장하는 얼굴들의 face encoding 값과 이미 가지고 있는 값을 비교하여 가장 거리가 가까운 값을 찾는 것이다.
   - 이때 face encoding 값은 128차원의 벡터 형태이므로 두 값 사이의 거리 계산을 위해서 유클리드 거리(Euclidean distance) 공식을 이용한다.
   - 따라서 두 face encoding 거리는 0~1 사이의 값이며, 이 값을 통해 두 얼굴의 유사도를 파악할 수 있다.


#### 4. 유사도(Similarity Threshold)
   - 유클리드 거리를 통해 계산한 두 face encoding 값의 차이에 따라서 동일 인물인지 아닌지를 판단하므로, 판단의 기준이 될 차이 값을 미리 정의해 두어야 한다.
   - 따라서 해당 값을 유사도(Similarity Threshold)로 정의하여 이 값보다 작으면 동일 인물이라고 판단하고 이 값보다 크면 서로 다른 인물이라고 판단한다.


#### 5. 모자이크(Mosaic) 처리
   - 이미지에서 얼굴을 탐지한 후 해당 영역의 높이와 넓이를 축소하여 다시 원래 사이즈대로 확대하면 그 과정에서 이미지의 픽셀이 깨지면서 화질이 비정상적으로 저하된다. 
   - 해당 원리를 이용하여 원하는 얼굴 영역에만 모자이크 처리가 되도록 하였다.


#### [참고] System Flow
<p align="center">
  <img src="https://github.com/juooo1117/Graduation_FaceDetection/assets/95035134/3c2e6e3b-a95f-4c21-8709-3ca16894e36f" width="600" height="400">
</p>



## 📖 System Flow-Chart
<p align="center">
  <img src="https://github.com/juooo1117/Graduation_FaceDetection/assets/95035134/fca063d0-2ae5-459f-9796-9dfc612fc8ff" width="600" height="700">
</p>



## 📝 결과분석

#### 1. 처리결과
   - 원래는 단순히 모자이크가 적용된 결과로 출력되나, 시스템이 잘 적용되었는지 검증하기 위해 인식된 얼굴 영역과(초록 네모 박스 부분) 얼굴 영역별로 계산된 facial landmark의 차이 결과를 숫자로 함께 표기하여 출력하였다.
   - 결과를 살펴보면 입력된 동영상 속 얼굴에 해당하는 영역이 잘 검출되어 있는 것을 확인할 수 있다.
   - ‘wonyoung’, ‘rei’에 해당하는 인물은 facial landmark 계산 결과가 지정한 similarity threshold(0.45)보다 작기 때문에 모자이크에서 제외
   - 나머지 인물은 해당 값을 초과하기 때문에 모자이크 처리

<p align="center">
  <img src="https://github.com/juooo1117/Graduation_FaceDetection/assets/95035134/f434cb12-8ef7-4ff0-b6b4-c2f5b77f4597" width="400" height="300">
</p>


   |    |모자이크 시스템 시연 영상|
   |----|-------------------|
   |영상주소|https://youtu.be/wUQJ7UE-fGU|
   |포함내용|- 사용자 입력 데이터(동영상, 모자이크 제외할 인물사진) <br> - 프로그램 코드 구현 및 결과 <br> - 모자이크 처리된 결과 동영상|  


#### 2. 시스템 성능 비교 및 향상
   - 모자이크 처리 시스템의 성능을 높이기 위해서 동영상 속 얼굴 간의 유사성을 계산하여 동일 인물인지 판단 기준으로 삼는 유사도(Similarity Threshold) 값을 조절하는 방법을 사용하였다.
   - Similarity Threshold 값보다 작으면 시스템은 사용자가 입력한 특정 인물의 사진과 동일 인물이라고 판단하여 모자이크 처리를 적용하지 않게 된다.
   - 얼굴 판별 성능을 가장 높이기 위해서 Similarity Threshold 값을 0.3, 0.45, 0.6으로 나누어서 실행해 보고 해당 값에서 출력된 결과물을 통해 시스템의 성능을 비교하였다.
   - 0.3으로 지정했을 때는 거의 대부분의 얼굴을 다른 사람이라고 판단하여 모자이크 처리를 하였고, 0.6일 때는 대부분의 얼굴을 같은 인물이라고 판단하여 모자이크에서 제외되었다.
   - 가장 잘 판별한 경우의 Similarity Threshold 값은 0.45임을 확인하였으므로, 0.45로 값을 정의하여 구현하였다.


#### 3. 한계점
   - 동영상 속 얼굴을 인식하여 동일 인물인지 판별하고 모자이크를 적용하기 위해 작성한 코드 구현에 사용되는 시간이 오래 걸리는데, 이는 GPU 성능이 좋지 않아서 발생한 문제이다.
   - 측면 얼굴이나 너무 멀리 떨어져 있는 경우 인식이 제대로 되지 않아 facial landmark의 판별이 어려워, 제대로 모자이크 처리가 되지 못하는 문제가 발생하는 것을 확인하였다. (이목구비의 좌표값을 구하여 유사도를 측정하는 방식이기 때문에 측면 얼굴에 대해서는 유사도 측정이 불가)
   - 대량의 데이터 셋을 바탕으로 얼굴을 학습하는 딥러닝 기법을 적용한다면 측면 얼굴이나 멀리 떨어져 있는 경우에도 처리가 가능할 것이다.



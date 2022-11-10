# CP1_Traffic-Accident

여 차량이 현재 처한 상황의 위험도를 Sigmoid 함수를 사용하여 0~1 사이로 출력
-> 딥 러닝 기반 차량 위기 감지 시스템을 제안

진행 방향에 따라서 Reference Line을 설정하고, ROI 내에서 검출된 물체가 Reference Line에 포함될 때 후보 위험군으로 판단

OpenCV라이브러리를 사용하여 차선을 탐지한 후 측면 충돌이 일어날 수 있는 끼어들기 상황에 대해 위험상황이라고 보여주는 시각화 모델

차량의 긴급상황을 정확하게 판단하기 위하여 차량 위기 상황 판단 모듈은 사전에 DNN 모델을 학습시켜 야만 한다.
차량 위기 상황 판단 모듈은 DNN을 Back-propagation 방식으로 학습시킨다.
DNN의 학습을 위해 사용되는 트레이닝 데이터는 직접 수집된 정상 주행데이터와 교통사고 통계 분석 데이터를 이용한 차량의 사고 데이터를 기반으로 생성된다. 
생성된 트레이닝 데이터 중 정상 주행데이터의 출력은 모두 0으로 설정되며, 사고 데이터는 0~1 사이의 출력을 지정한다


## 차선을 탐지하기 위해 처음에 기존의 이미지를 흑백화면으로 변환하는 Gray Scale 작업을 진행한다.
이 작업을 통해 백색 부분을 제외한 나머지 부분을 모두 까맣게 변환시킨다. 그 뒤 백색 부분의 채워진 부분을 제거하여 테두리 부분만 남긴 뒤, RoI(Region of Interest) 범위에 해당하는 영역만 남긴 뒤 모두 제거하는 Canny Edge Detection 작업을 진행한다. 그 뒤 남은 부분에 Hough Space작업을 통해 직선을 그어 차선을 탐지한 결과를 보여준다.

## 물체 인식 정보와 OpenCV라이브러리를 통해 나온 차선을 바탕으로 끼어들기 상황에 대한 위험 시나리오를 구성한다. 
인식된 물체의 클래스에 대해 물체의 경계 박스(bounding box)와 차선의 중첩된 영역 비율에 대한 조건을 다르게 하여 위험 탐지를 정의하였다.
경계 박스의 크기가 커질수록 위험을 탐지하는 비율도 더 크게 적용

Faster R-CNN 신경망과 Resnet 모델을 적용하여 물체의 위치를 탐지 <br>
OpenCV 라이브러리를 이용하여 프레임에 찍힌 차량의 차선을 탐지하여 표시

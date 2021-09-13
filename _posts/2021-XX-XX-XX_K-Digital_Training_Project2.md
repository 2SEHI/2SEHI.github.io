ROI자동 탐지 기법으로 Selective Window(matchTemplate)를 이용한 Selective Search방식을 사용해 보려고 합니다.

OpenCV에  Selective Window(matchTemplate)는 이미 있지만, 이미지 사이즈와 탐지하고자 하는 객체 사이즈의 비율을 모르는 경우에는 객체 추출이 어렵다는 단점이 있어서 이미지를 컬러로 인식하고, 색깔별로 영역을 묶고 ROI를 추출하는 Selectvie Search방식을 이용합니다.

로 객체 탐지
객체가 겹쳐지는 것은 NMS(비극대값 억제)를 적용(TensorFlow, Keras 및 OpenCV를 사용하면 CNN 이미지 분류기를 객체 감지기로 전환할 수 있습니다.
)



 **Python 및 TensorFlow, Keras 및 OpenCV와 같은 라이브러리를 사용하여 딥 러닝 분류기를 객체 감지기로 바꾸는 단계.**

![image](https://user-images.githubusercontent.com/58774664/132818185-4be060d1-32d5-44bd-acb5-e2fa80764eee.png)
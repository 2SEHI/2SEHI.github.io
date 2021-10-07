# R-CNN



## 1) 객체 인식

lpin 의 전력량계량기 OCR 인식 프로젝트에서 데이터에 전력량계량기의 배경에 다른 객체가 포함되어 전력량계량기를 찾는 것이 쉽지 않습니다. 그래서 모든 데이터마다 가지고있는 공통적인 특징에 대하여 객체 탐지를 하여 문자를 읽어야하는 영역을 찾으면 좋겠다는 생각이 들었습니다. 

<img src="/assets/images/75_K-Digital_Training_Project_2.png" alt="75_K-Digital_Training_Project_2" style="zoom:150%;" />



## 2) 찾고자하는 객체

공통적인 특징은 G-Type 과 전력량계량기의 8-segment 부분이 있었습니다.

<img src="/assets/images/75_K-Digital_Training_Project_1.png" alt="75_K-Digital_Training_Project_1" style="zoom:75%;" />



### G-Type을 찾고자하는 대상이 될 때 예상되는 문제점

G-Type 을 기준으로 찾았을 때 예상되는 문제가 몇가지 있었는데 첫번째는 다른 타입의 전력량계량기 이미지가 들어오면 영역을 찾을 수 없다는 것이었습니다. lpin으로부터 G-Type 의 전력량계량기 이미지데이터만 받아서 애초에 다른 타입은 훈련조차 할 수 없긴 하지만 또 다른 문제로는 Selective Search 를 이용하여 G-Type의 영역을 찾으려면 비교하는데 시간이 너무 오래걸린다는 단점이 있습니다. 



따라서 어떤 Type의 전력량계량기 이미지가 들어와도 찾을 수 있고 크기도 작지 않은 8-segment영역에 대해 훈련데이터셋을 만들고 모델을 훈련하고자 하였습니다.



## 3) 객체의 좌표추출

모델에 이미지에 대한 8-segmentation 각 이미지에 대해 selectROI를 이용하여 8-segment 영역의 좌표를 파일로 만들었습니다. 그런데 원본 이미지의 크기가 너무 커서 화면에 이미지가 전부 나타나지 않아 가로 길이를 700으로 맞춰서 좌표를 추출하였습니다. 

[훈련데이터셋 만드는 소스코드]()

- 사용이 불가능한 이미지를 제거하여 789개의 이미지를 사용했습니다.



## 4) 훈련데이터셋 생성

![image-20211004170413313](/assets/images/75_K-Digital_Training_Project_4.png)



## 5) R-CNN(VGG16)모델 훈련

selective search에 해당하는 region proposal 만큼 CNN을 돌려야 하며 큰 저장 공간이 필요하여 훈련데이터셋을 생성하고 모델을 바로 훈련시키면 훈련 도중에 저장 공간이 모자라 멈춰버립니다. 그리고 무엇보다도 예측할 때도 입력 이미지 데이터에 대해 2000개의 region proposal 를 추출하고 이미지로 저장하기 때문에 약 20분 넘는 시간이 소요 되어 사용이 불가능할 정도였습니다.



# 다른 모델 알고리즘 알아보기

R-CNN 과 Fast R-CNN의 단점을 보완한 Faster R-CNN을 알아보았습니다.

- Fast R-CNN : R-CNN과 다른 점은 region proposal 에 대해 이미지로 저장하지 않고 좌표를 저장합니다. 
  하지만 R-CNN 와 마찬가지로 Selective Search 를 CNN 외부에서 연산하여 속도적인 문제가 아직 남아있습니다.



그래서 Fast R-CNN 을 발전시킨 Faster R-CNN 모델을 사용해야합니다.

# Faster R-CNN

- Selective Search 를 CNN 내부에서 연산하며, GPU 를 활용하여 수행하기 때문에 빠릅니다.
- 또한 region proposal을 수행하는 RPN 모듈과 Fast R-CNN 모듈을 사용하는데 Fast R-CNN 모듈의 conv 층을 공유하여 계 Fast R-CNN 모듈의 계산을 공유합니다.
- RPN에서 iou가 0.7이상인 레이블은 positive, iou 0.3이하인 것은 negative 라고 학습합니다.

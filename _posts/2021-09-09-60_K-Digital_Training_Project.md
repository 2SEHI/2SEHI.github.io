---
title:  "[전력계량기OCR인식] 2021/9/9 문자영역 contouring"

date: 2021-09-09
last_modified_at: 2021-09-09

use_math: true
comments: true

categories:
  - K-Digital Project

---
참고 사이트 : https://d2.naver.com/helloworld/8344782



# 문자특징 추출 모델

	- 화면을 임의로 분할하거나 일정 비율로 분할해 CNN에 인식시키는 방법도 있겠지만 텍스트가 있다고 추정되는 영역만 바로 추출해서 CNN에 인식시키는 것이 더 효율적인 방법이라고 생각됨

- 주어진 이미지에서 색깔(color)을 기반으로 그룹을 지은 덩어리를 추출하는 것
- 이미지 자체에서 윤곽선을 위주로 한 그룹을 추출해 낼 수 있다면 텍스트 덩어리도 손쉽게 찾을 수 있을 것이다.









## 경계 그룹 찾기(Contour)

**findContours**를 이용하여 연속된 점을 탐지하여 사물의 형태를 탐지하는 것

이미지를 GrayScale로 변환하고 contour해야됨.



## 코딩순서 계획

```
GrayScale변환
임계치(Adaptive Threshold)
Morph Gradient
Morph Close
Long Line Remove
contour
원본 이미지에서 자동으로 Contour 영역을 추출
```

인공지능을 학습할 때 사용할 데이터는 텍스트(또는 텍스트로 분류되는 것)가 있는 이미지와 텍스트가 아닌 것이 있는 이미지, 2가지로 만들면 됩니다.  텍스트가 아닌 것은 AI가 글자로 오해할 수 있는 사물입니다. 예를 들어 아파트 창문, 한옥의 창살 등이 글자와 비슷한 형상을 가지고 있기 때문에 글자로 오인될 수 있습니다.



### 임계치

Global Threshold는이미지 전체에 임계가 적용되어 버려지는 영역이 많아지므로 Adaptive Threshold를 적용해야함

- Adaptive Threshold
  - 이미지는 영역을 분할하고 임계값을 자동으로 조정해 얻은 흑백 이미지
- Global Threshold
  - 이미지 전체에 임계가 적용돼 다음 예의 Global 이미지에서처럼 버려지는 영역이 많아지므로 Adaptive Threshold를 적용해야함

![60_K-Digital_Training_Project_1adaptiveThreshold](\assets\images\60_K-Digital_Training_Project_1adaptiveThreshold.jpg)

### 영역제외

- 사각형 영역의 크기가 어느 정도 이상이거나 밀도가 어느 정도 이상이 아닌 영역은 제외한다



### Morph Gradient

테두리(외곽선)를 더 정확하게 추출하는 과정 - dilate 또는 erode

- dilate 와 erode

<img src="\assets\images\60_K-Digital_Training_Project_2dilate_erode.png" alt="60_K-Digital_Training_Project_2dilate_erode" style="zoom:67%;" />



### Morph Close

커널 크기를 9 x 5픽셀

- 끊어진 점이 있는 선을 제거
- Morph Close를 적용하지 않으면 글자가 한덩이가 아니라 여러개로 쪼개질 것임
  ![60_K-Digital_Training_Project_4close](\assets\images\60_K-Digital_Training_Project_4close.jpg)



### Long Line Remove - HoughLinesP이용

불필요한 세로선을 제거

![60_K-Digital_Training_Project_5HoughLinesP](\assets\images\60_K-Digital_Training_Project_5HoughLinesP.jpg)

### contouring - findContours메소드

- CHAIN_APPROX_SIMPLE 설정의 경우

![60_K-Digital_Training_Project_6contour](\assets\images\60_K-Digital_Training_Project_6contour.jpg)

- CHAIN_APPROX_NONE 설정 
  - RETR_EXTERNAL 과 RETR_TREE

<img src="\assets\images\60_K-Digital_Training_Project_7rect_contour.png" alt="60_K-Digital_Training_Project_7rect_contour" style="zoom:60%;" />





### 모델 학습

 Contour 영역을 추출하여 Morph Gradient가 적용된 이미지를 사용하며 학습과 질의를 할 때는 윤곽선이 정확(정밀)해야 하기 때문에 테두리가 뭉그러지지 않게 임계와 Morph Close를 적용하지 않는다.

학습 모델은 Inception, [VGG](http://www.robots.ox.ac.uk/~vgg/research/very_deep/) 등 어떤 모델 사용해도 무방하다.



## 결론

소스코드 : [ROIExtraction.py](https://github.com/2SEHI/OCR-Text-Detection/tree/main/preprocessing/ROIExtraction.py)


임계치를 여러방면으로 바꾸어 보았지만 이 방법으로 입력되는 사진의 크기가 달라지면 임계치도 그에 따라 달라져 문자영역만 뭉치는게 힘든 것 같아서 ROI를 추출하는 다른 방식을 알아봐야겠습니다.


---
title: "[전력계량기OCR인식] 2021/9/21 Selective Search - 수정중"

date: 2021-09-21
last_modified_at: 2021-09-21

use_math: true
comments: true

categories:
  - K-Digital Project
tags:
  - 전력계량기 OCR인식
---

[https://learnopencv.com/selective-search-for-object-detection-cpp-python/](https://learnopencv.com/selective-search-for-object-detection-cpp-python/)



# Selective Search for Object Detection



## 1.Object Detection(객체 탐지) vs. Object Recognition(객체 인지)

### 1) Object Detection

**객체 인지 알고리즘(Object Recognition)**은 이미지에 존재하는 객체를 식별합니다.입력된 전체 이미지와 출력이미지 레이블과 이미지에 존재하는 객체 클래스의 확률의 정보를 가집니다.



### 2) Object Recognition

반면, 객체 탐지 알고리즘은 이미지에 존재하는 객체뿐만 아니라, 이미지내의 객체가  가진 bounding box(x,y,width, height) 도 출력합니다. 객체탐지 알고리즘의 핵심은 객체인지 알고리즘에 있습니다. 



## 2.segmentation

영상에서 의미있는 부분으로 구별해내는 기술을 의미하며, threshold나 edge에 기반한 단순 구별뿐만 아니라 개체의 경계선까지 추출하여 영상을 의미있는 영역으로 나눠주는 작업을 해줍니다.



## 3.Classification 와 Detection

Classification은 전체 영상에서 특정 객체의 존재 여부만 알아내면 되고, Detection은 위치까지 파악하여 개체 주위에 bouding box로 구별을 해주어야 합니다.



## 4.Sliding Window Algorithm

## 5.Region Proposal Algorithms

## 6.Selective Search?



# Similarity(유사도 측정방법)

SelectiveSearch는 **4가지 요소 Color (색상), Texture(텍스처), Size(크기), Shape(모양) **들의 유사도 측정방법을 사용합니다.



## Color **Similarity**

## Texture Similarity

## Size Similarity

## Shape Compatibility





### OpenCV를 이용한 Selective Search 

[소스 코드](https://github.com/2SEHI/OCR-Text-Detection/blob/main/preprocessing/5_CreateSelectiveSearch.py)

다음은 OpenCV의 createSelectiveSearchSegmentation라는 메소드에 기본 파라미터를 설정하여 Search Segmentation Object를 추출해본 이미지입니다.

이를 활용하여, 색상을 이용한 전력량부분의 유사도 측정, Type 문자영역의 유사도 측정에 활용해 볼 수 있을 것 같습니다.

![image-20210923102412286](\assets\images\70_K-Digital_Training_Project.png)


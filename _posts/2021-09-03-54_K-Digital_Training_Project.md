---
title:  "[전력계량기OCR인식] 2021/9/3 주요객체 추출"

date: 2021-09-03
last_modified_at: 2021-09-03

use_math: true
comments: true

categories:
  - K-Digital Project
---



# 전기계량기 OCR 인식을 통한 전력량 도출

아직 회사로부터 데이터를 받지 못해 인터넷에서 찾은 E-type의 전력량 계량기 이미지를 이용하여 배경사진으로부터 주요객체(전력량 계량기)를 추출하기 위한 전처리 작업을 하였습니다.

### 샘플 이미지

![54_K-Digital_Training_Project_1](\assets\images\54_K-Digital_Training_Project_1.jpg)









### 주요 객체를 추출하기 위해 해본것

위의 샘플 이미지를 이용하여 주요 객체를 추출을 시도해보았습니다. 사용해본 함수는 GaussainBlur, AdapTreshold, dilate, threshold입니다.

1. 주요 객체와 배경 영역 구분
   1. dilate를 이용하여 객체내의 불필요한 내용을 침식시킴
   2. cv2.threshold 를 이용하여 주요객체와 배경영역을 구별함
2. 주요 객체 추출



### 잡음 제거 필터링

야간에 스마트폰 플래쉬를 이용하여 전력량 계량기사진을 찍으면 반사광 노이즈가 생기므로 객체를 추출하는데 방해가 되지 않도록 잡음을 제거하기 위한 처리가 우선 필요하다고 생각했습니다.



### AdaptThreshold 

AdaptThreshold 는 8세그먼트부분이 유독 어두워서 문자열 추출할 때 쓰면 좋을 것 같습니다


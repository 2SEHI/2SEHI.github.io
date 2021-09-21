---
title: "[전력계량기OCR인식] 2021/9/15 계량기사진 정보기록"

date: 2021-09-15
last_modified_at: 2021-09-15

use_math: true
comments: true

categories:
  - K-Digital Project
tags:
  - 전력계량기 OCR인식
---





## 1. 데이터 정보

### 1) 계량기 사진

- 984장

- 파일 형식 : jpg

- 각도 : 거의 정방향에 가까움

- Type종류 : Type명이 보이는 계량기는 모두 G-Type

- 사진오류 

  - 계량기 사진이 아닌 형태가 4장 존재
  - Type명이 가려진 사진이 40여장 존재

  

### 2) 모뎀 사진

- 984장
- 파일 형식 : jpg
- 각도 0도~180도로 회전 필요



### 3) 그 외

- 파일명의 하이픈(_)을 기준으로 계량기 사진과 모뎀 사진이 매칭 됨.
  - 예를 들어 계량기 이미지 '**847207D64AF9**\_P1134.jpg'와 모뎀 이미지 '**847207D64AF9**\_P2134.jpg'의 '**847207D64AF9**'부분이 일치하며 두 이미지의 배경이 같은 곳임



## 2. 오늘 한 것

어제 해본 matchTemplate는 크기, 방향, 회전 변환에는 잘 작동하지 않고, 속도가 느리다는 단점이 있어서 'G-Type'이라는 타입 이미지 크기, 방향, 각도를 변경해가면서 Template matching을 해야합니다. 그래서 결국 한글 ocr모델에 전력량 계량기의 글자 정보를 추가 훈련하여 문자탐지를 하거나, 타입 이미지가 아닌, 전력량 8-segment디스플레이를 찾아서 문자영역을 특정해야 할 것 같습니다.



### 1) 모델 만드는 방법 알아보기

- 참고 : [딥러닝을 활용한 한글문장 OCR 프로젝트](https://medium.com/@sunwoopark/%EB%94%A5%EB%9F%AC%EB%8B%9D%EC%9D%84-%ED%99%9C%EC%9A%A9%ED%95%9C-%ED%95%9C%EA%B8%80%EB%AC%B8%EC%9E%A5-ocr-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-hclt-2019-bb9d17622412)



#### - 데이터 셋 만들기

- 참고 사이트에 따르면, [폰트제공 사이트](https://www.font.co.kr/yoonfont/free/main.asp), AI Hub의 한글 이미지 + 우리에게 필요한 글씨 이미지 (전력량계량기의 글씨 이미지)에 대한 데이터셋을 파일로 만들어야 합니다.
- 데이터 셋을 만들어도 데이터 셋 파일을 불러 훈련을 하려면GPU가 필요한데 학원 컴퓨터로는 내장 GPU밖에 존재하지 않아 훈련을 데이터셋 불러오는것을 실패합니다. 알아본 정보가 맞는지 내일 선생님께 여쭤보고 확인해 볼 것입니다.



#### - ocr 모델 훈련

- [데이터셋 생성](https://github.com/Belval/TextRecognitionDataGenerator)
- [한글 OCR모델](https://github.com/parksunwoo/ocr_kor)


---
title:  "[전력계량기OCR인식] 2021/9/13 멘토링 3회차 - 객체탐지"

date: 2021-09-13
last_modified_at: 2021-09-13

use_math: true
comments: true

categories:
  - K-Digital Project
tags:
  - 전력계량기 OCR인식
---


# 이번주에 해볼 것



#### 1. cv2.matchTemplate()

- 샘플이미지로 해봄. 성능 향상을 위해선 더 많은 계량기 ROI 이미지를 이용해야 함
- 회전, 크기 변화 시 적용 어려움
- 계량기 타입별로 별도의 훈련이 필요함



#### 2. cv2.CascadeClassifier()

- 시도해볼만함
- 성능 확인해보고 이용 여부 판단해 볼 예정
- 이용 시 계량기 ROI 영역을 이용하여 분류기 훈련 필요



#### 3. Selective Search

- window sliding과 달리 이미지 픽셀의 컬러, 무늬, 크기, 형태에 따라 유사한 region을 계층적 그룹핑하여 계산하는 방식임

- 모든 계량기 이미지에 대해 일괄된 전처리를 수행하여 일정한 특징을 검출할 수 있는지 확인 필요함

- 현재까지는 이미지 파일의 색상 공간을 hsv로 읽고 saturation으로 배열 변환했을 때, 

  액정 영역에서 특정 값이 나타날 수 있는지 확인하는 중



# 피드백



### 변하지 않는 데이터 부분을 템플릿매칭의 templ로 이용할 것
- 동일한 위치, 동일한 크기
- templ 사이즈와 전체 계량기 사이즈 비율 맞춰서 계량기 도출하기


### 다른 방법
- 계량기 테두리가 perspective transformation로 안 잡히는 경우를 해결하기 위해  다른 방법 이용
- 방법1
	- 계량기 영역 따지 말고, template match를 이용하여 원본 이미지에서 계량기 좌표 부분만 읽어들이기
- 방법2
	- 전력량 액정 영역만 다시 잘라내서 별도의 binarization 진행




### 그 외 질답
- 모든 이미지에 공통적으로 적용되는 임계값
	- 직접 탐색하기 어려움
	- **OTSU threshold 이용**
	- 이진화 후 Contour 돌려보기
	- 계량기 영역 좌표 받아서 사용




- 투시변환 방법
	- 전체 데이터와 각 데이터 부분의 뒤틀림이 다를 수 있음
	- 데이터 영역을 각각 따로 읽어낸 다음 투시변환 -> OCR 수행할 것




-  cv2.matchTemplate
	- src -> src
	- src -> dst
	- 방향, 각도, 크기 받아올 수 있음





- Contour 대신 WaterShade 써서 이미지 segmentation 돌리기




- 영수증 케이스 찾아보기
  - [https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=tommybee&logNo=221380482319](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=tommybee&logNo=221380482319)
  - [https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=tommybee&logNo=221380482319](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=youseok0&logNo=221566757079)



# 오늘 한 것

멘토링받은 내용 중의 영수증 케이스를 참고하여 문자영역 contouring을 해보았습니다.

소스코드 : [https://github.com/2SEHI/OCR-Text-Detection/blob/main/preprocessing/4_MatchTemplate.py](https://github.com/2SEHI/OCR-Text-Detection/blob/main/preprocessing/4_MatchTemplate.py)



## 1.Type이름영역의 ROI추출하기

- matchTemplate함수는 template이미지가 영상에서 이동하며, 매칭 방법에 따라 계산하여 결과를 반환하고, 매칭 결과를 탐색하여 template위치를 찾습니다.
- 그런데 이때 주의할 점은 template이미지와 영상속의 매칭이미지의 크기가 같아야해서 나중에는 타입별로 Type이름영역의 template이미지를 저장해두고 비율을 바꿔가며 매칭을 해봐야 합니다.
- 일단 오늘은 비율을 찾았다는 전제 하에 일단 template이미지를 영상으로부터 selectROI를 이용해 추출했습니다.



<img src="\assets\images\64_K-Digital_Training_Project-Mentoring3_1.png" alt="64_K-Digital_Training_Project-Mentoring3_1" style="zoom: 50%;" />

## 2.문자영역 추출

Type이름영역과 문자영역의 비율을 계산하여, 문자영역 추출하였습니다.



## 3.이미지 전처리

Otsu Thresholding(이진화)  -> 팽창,침식 -> close 를 이용하여 문자영역이 한 줄로 이어지도록 하였습니다



## 4.contouring

외곽선 점들의 좌표를 근사화하여 box를 그렸습니다.



![64_K-Digital_Training_Project-Mentoring3_2](\assets\images\64_K-Digital_Training_Project-Mentoring3_2.png)





지금까지 전력량 계량기의 영역자체를 찾아보려고 했었는데 계량기 `Type이름영역`을 match하여 문자영역을 추출할 수 있었습니다. 또한 모폴로지 연산의 close를 이용할 때 가로의 직사각형으로 해볼 생각을 하지 못했었습니다. `Type이름영역`의 크기를 바꿔가며 matchTemplate로 matching한다면 꽤 괜찮은 방법인 것 같습니다.



# 다음으로 해볼 것

- "G-Type"영역의 template크기는 달라지는데 matchTemplate를 어떻게 적용시킬 것인지?
- 임계치를 사진마다 어떻게 공통적으로 적용시킬 것인지?
- 추출한 문자영역의 투시변환하기
- 8세그먼트영역의 문자인식
- 문자인식후 어떻게 필요한 정보를 어떻게 구별해 낼 것인지?

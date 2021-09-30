---
title: "[전력계량기OCR인식] 2021/9/28~30 모델 훈련, Server구현, Android 구현"

date: 2021-09-29
last_modified_at: 2021-09-30

use_math: true
comments: true

categories:
  - K-Digital Project
tags:
  - 전력계량기 OCR인식

---


# 9/28

R-CNN 방식의 모델은 만들었지만, 하나의 이미지 데이터의 객체를 탐지하는데에 시간이 약 10분정도로 너무 오래 걸린다는 문제가 발생했습니다. 탐지를 할 때마다 입력 이미지에 대해 2000개의 Selective Search이미지를 모두 출력하기 때문에 너무 오래 걸렸던 것이었습니다.또 한 훈련이미지마다 2000개의 Selective Search이미지를 모두 메모리에 저장하기 때문에 메모리 초과 문제도 있었습니다.  더 좋은 모델훈련방법이 없을까 여러 논문이나 글을 읽으며 탐지모델에 대해 공부해본 결과 제가 사용한 R-CNN의 경우는 Selective Search 이미지와 레이블을 이용하여 모델을 훈련하지만 Fast R-CNN의 경우에는 Selective Search 이미지가 아닌 좌표를 이용하여 모델을 훈련시켜서 Fast R-CNN에 대한 모델 훈련을 해보고 있습니다.





# 9/29

오늘은 Server쪽을 구현하였습니다.

## 1.DB구현

exerd를 이용하여 전력량계량기 테이블, 모뎀 테이블, 전처리이미지파일 테이블 생성하였습니다. 조회페이지에서 지역선택을 하여 필터링해주는 처리도 구현하고 싶어서 지역코드도 넣었지만 다음주까지는 시간이 없어서 일단 컬럼만 만들어 놓고 나중에 구현할 것입니다.

- exerd 파일  :[https://github.com/2SEHI/OCR-Text-Detection/tree/main/db](https://github.com/2SEHI/OCR-Text-Detection/tree/main/db)

  ![image-20210929192437800](\assets\images\73_K-Digital_Training_Project_1.jpg)

- 테이블 생성 sql : [https://github.com/2SEHI/OCR-Text-Detection/blob/main/db/CreateTableSQL.sql](https://github.com/2SEHI/OCR-Text-Detection/blob/main/db/CreateTableSQL.sql)

- 샘플데이터  sql : [https://github.com/2SEHI/OCR-Text-Detection/blob/main/db/InsertSampleData.sql](https://github.com/2SEHI/OCR-Text-Detection/blob/main/db/InsertSampleData.sql)



## 2.Flask WebService

전체 소스 : [https://github.com/2SEHI/OCR-Text-Detection/tree/main/server/ElectricityOCRServer](https://github.com/2SEHI/OCR-Text-Detection/tree/main/server/ElectricityOCRServer)

### 1) project 생성 : ElectricityOCRServer

pycham에 WebServer용 프로젝트를 생성했습니다.



### 2) 패키지 설치

- flask 
- pymysql : mysql 연동 패키지



### 3) 디렉토리 구조


```
ElectricityOCRServer
├── 📂common						# 공통 처리 폴더
│	├──📄db.py						# db connect
├── 📂model							# 모델 예측 및 인식처리
│	├──📄model.py					# 이미지 전처리, 모델 예측 및 인식처리
│	├──📑ieeercnn_vgg16_1.h5		# 모델 파일
├── 📂static						# 안드로이드에서 등록한 전력량, 모뎀, 전처리과정 이미지를 저장하고 있을 폴더
├── 📂templates    					# 테스트용? 필요없음?
├──📄app.py 						# 초기 실행 파일
```



### 4) 조회 테스트

현재 sql을 python 파일에 하드코딩 하여 웹페이지상에 json데이터가 출력되는 부분까지 했는데, sql을 json파일로 만들어서 읽어들이도록 하는 것이 개발하기 쉽고 리빌드를 안해도 되서 더 편리할 것같습니다.

```sql
select electricity_meter_tb.serial_cd , 
	electricity_meter_tb.typename , 
	electricity_meter_tb.electricity_save_date, 
	modem_tb.modem_cd
from modem_tb
join electricity_meter_tb 
on modem_tb.serial_cd = electricity_meter_tb.serial_cd;
```

![image-20210929202727987](\assets\images\73_K-Digital_Training_Project_2.jpg)





# 9/30

## 1.Android 조회 화면 테스트

테스트 용으로 전력량계량기 테이블 리스트를 Android에 전송하여 제조사코드와 전력량계량기 사진을 리스트에 출력하는 것까지 해보았습니다.

내일은 이제 전력량계량기와 모뎀 테이블을 join 하여 리스트에 출력하는 것을 해볼 것입니다.

![image-20211001011003764](\assets\images\73_K-Digital_Training_Project_3.jpg)










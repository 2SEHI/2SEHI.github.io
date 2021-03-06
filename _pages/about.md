---
permalink: /about/
layout: single
title: "반갑습니다! 제 이름은 이세희입니다"
---



- 저는 도쿄에서 2016년 9월부터 2020년 10월까지 SI 시스템 개발자로 4년간 일했으며, 

  AI에 흥미를 가지고 도전하고자 과감히 한국으로 돌아왔습니다.

- 한국으로 돌아와 중앙정보처리학원에서 2021년 4월부터 2021년 10월까지 

   [빅데이터 기반 AI 응용 솔루션 개발자 전문과정]을 수료하였습니다. [📌훈련과정에서 배운 내용 저장소](https://github.com/2SEHI/K-Digital_Lecture)

- 저는 기록과 분석을 좋아하며, 효율적인 판단력을 가지고 비즈니스 인사이트를 도출해내는 개발자가 되고 싶습니다.

<br>

# 🚩Skill Set

- Language 
  - Python, Java

- OS
  - Linux

- Framework
  - Django, Flask, Spring, Struts, OpenCV 

- Database
  - Oracle, MySQL, PostgreSQL

- IDE
  - Eclipse, IntelliJ, STS, Pycharm, Colab, Android Studio, DBeaver, eXERD 

- Data Analysis
  - Scikit-learn, Scipy, Tensorflow, Keras, Pytorch 

- Front-End
  - JavaScript, HTML/CSS, JQuery

- Version Control
  - Git, SVN



<br>

# 📍개인 참가 프로젝트

중앙정보처리학원  K-Digital Traning  - [빅데이터 기반 AI 응용 솔루션 개발자 전문과정] 에서 진행한 프로젝트입니다.

<br>

## [1. 💻디스플레이 센서의 불량품 요인 분석](https://github.com/2SEHI/DisplaySensor-Anomaly-Analysis)

- 목적 : 800여 개의 디스플레이 생산 공정에 대해 1년간 1시간 단위로 수집된 불량 감지 센서 데이터를 분석하여 불량에 요인이 되는 공정을 찾아냈습니다.
- 분석 도구 및 환경 : Python3, Colab, Jupyter, Pandas, Seaborn, Scikit-learn, Numpy, Matplotlib, Scipy, Slack, Github, Windows10
- 데이터 전처리, PCA 주성분 분석, XGBoost, RandomForest Classifier를 이용하여 불량에 요인이 되는 피처를 찾아냈는데 모든 팀원이 다같이 진행을 하였고  XGBoost, SVM, RandomForest 의 모델링 수행부분만 2명씩 파트를 나누어 진행했습니다. 그 중에서 저는 RandomForest Classifier 를 수행했습니다.



<br>

## [2. ⚡전력량계량기 OCR 추출 ](https://github.com/yujapie/ElectricityOCRServer)

- 목적 : 전력량 계량기 기기정보와 모뎀 바코드 정보의 기재 시간을 줄이고 기재 내용의 정확도를 높이기 위해 OCR인식을 통해 계량기 제조번호와 모뎀 식별코드의 기재를 자동화하였습니다.
- 배경 : 전력량계량기로부터 전력량 데이터를 한전 고압서버로 전송하기 위해 통신 모뎀을 설치하는데 사람이 전력량 계량기의 정보와 모뎀에 부착되는 모델 식별 바코드를 일일히 손으로 적는것은 시간적 소요와 오타가 발생
- 개발 내용 : 전력량계량기와 모뎀을 촬영하면 OCR인식을 통해 읽어들인 정보들을 데이터베이스에 자동으로 저장되도록 함.
- 개발 도구 및 환경 : Python3, Android Studio(java), Json, MySQL, Flask, OpenCV, TensorOCR, Numpy, Pycharm, DBeaver, Eclipse, Google Drive, Slack, eXERD, Window10
- 저는 팀에서 이미지 전처리, Android 의 조회, 상세화면 구현, Server 구축, README 작성, PPT작성을 맡았으며, 성공하지는 못했지만, 객체탐지를 위한 R-CNN 모델의 전이학습을 통한 훈련도 수행했습니다. 
- 난이도가 높았던 작업 :   R-CNN 모델을 이용한 객체탐지에 성공하지 못한 이유는 입력 이미지에 대해  predict를 할 때 2000개의 Selective Search 이미지를 모두 저장하여 model 예측결과와 일일이 비교하여 한장의 이미지에서 객체를 추출하는데 약 20분이 넘게 소요되었기 때문입니다. 더 좋은 모델훈련방법이 없을까 여러 논문이나 글을 읽으며 탐지모델에 대해 공부해본 결과 제가 사용한 R-CNN의 경우는 Selective Search 이미지와 레이블을 이용하여 모델을 훈련하지만 Fast R-CNN의 경우에는 Selective Search 이미지가 아닌 좌표를 이용하여 모델을 훈련시켜서 Fast R-CNN에 대한 모델 훈련을 해보고 있습니다.

<br>



# 🔧경력

<br>

## 💡NCB

- 기간 : 2016/09 ~ 2018/09

- SI 개발

<br>

### 현장1 - 비지니스 지원 시스템

- 기간 : 2016/10 ~ 2017/05
- NTT 에서 진행한 법인/ 개인 고객을 대상으로 하는 비지니스 지원 시스템
- Java, postgreSQL, Spring Framework, SVN
- 개발 파트 : 제가 맡은 기능은 타 시스템으로부터 법인/ 개인 고객의 18가지 종류의 개설 정보를 조회하는 기능이었습니다. 이 기능은 법인 정보, 주소 등을 입력하면, 타 시스템으로부터 정보를 주고받기 위한 xml형식으로 변환하고, 조회결과를 Client에 전달하기 위한 데이터보정을 했습니다. 그리고 그 과정에서 설계서도 수정하였습니다.


- 실무경험을 한 현장으로 시스템이 커서 처음엔 업무 내용을 파악하는게 가장 힘들었지만 설계서나 매뉴얼 등이 잘 정리되어 있어서 금새 적응했습니다.  프로젝트 내의 코딩 규약도 있었기 때문에 실무경험이 처음이었던 제게 많은 도움이 되었습니다.



### 현장2 - 요코하마 은행 콜센터 업무 지원 시스템

- 기간 : 2017/06 - 2018/07
- Java, Oracle, Spring Framework
- 콜센터직원의 고객응대업무를 지원하는 시스템
- 기존에 사용하던 오래된 시스템을 마이그레이션하기 위해 전반적인 기능의 DB접속부분과 SQL수정, 테이블 이행, 기능 수정 등을 수행하였습니다. 단위 테스트부터 인수 테스트까지 진행했습니다.
- 개발된지 약 10여년이 지난 기존 시스템을 보면서 해서는 안되는 코딩방법에 대해 실감할 수 있었습니다.
  - 예를들어, 기존 시스템에는 불필요한 소스와 주석이 많았고 처음 프로그래밍을 배울 때부터 mybatis를 이용하여 소스와 SQL를 분리하였기 때문에 SQL 하드코딩에 대한 단점을 느끼지 못했었는데 소스내에 SQL 하드코딩 을 보며 수정이 매우 힘들었습니다.
- 가장 힘들었던 부분은 엄격한 보안때문에 외부 인터넷이 차단되어 있고 사무실 내에서 핸드폰을 볼 수 없어서 모르는 내용에 대해 사무실 밖에서 검색하고 종이에 적어와 해결해야 했습니다. 

<br>

## 💡ISS

- 기간 : 2018/10 ~ 2020/10

- 이전 회사 - NCB의 같은 계열사로 이직하여 프로젝트를 진행하면서 신입 채용과 JAVA 교육을 맡기도 했습니다.

<br>

### 현장 - Docomo Systems

[**프로젝트 기록**](https://magical-goldenrod-6ed.notion.site/Docomo-Systems-Projects-6364910272254815849d867e33f7396e)

- 기간 : 2018/09 ~ 2020/10
- 일본 최대 통신사 Docomo의 사내 시스템인 dDreams에서 기지국 설비업무파트를 담당했습니다. 
- 주로 프로젝트 기획같은 상위공정업무를 진행했습니다. 
  - 예를들어, 개발 요건에 대해 비용 대비 효과가 있는지, 시스템과 업무 사양 등을 검토하여 프로젝트 개발 승인을 받은 후에 설계 리뷰, 테스트, 릴리즈 등을 진행하였습니다.
  - 그 밖에도 SOX법 감사 대응, QA/고장 대응 등의 업무도 진행했습니다.
- 회의, 테스트, 서류 작업등의 모든 일정을 주도적으로 잡아서 진행하였습니다. 
- 고객이 필요하다고 무작정 개발하는 것이 아니라, 개발 요건이 나타나게 된 배경과 업무에 대한 이해 그리고 명확한 개선 목적이 있어야, 효율적인 개발을 할 수 있다는 것을 배웠습니다.
- 일본에서 회사를 그만둘 때 Docomo 일본인 부장님께서 저에게 업무에 소질이 있고, 언제든 일본에 돌아오면 일자리를 알아봐주겠다며 명함을 건내주셔서 감사하고 보람을 느꼈습니다.



# Contact.

- <a href="mailto:mail@hyunseob.me">이메일</a>

- [블로그](https://2sehi.github.io/)
- [깃허브](https://github.com/2sehi)


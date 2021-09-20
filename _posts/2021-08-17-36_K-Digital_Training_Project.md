---

title:  "[디스플레이센서 이상요인 분석] 2021/8/17 Random Forest모델의 graphviz시각화"
excerpt: "Random Forest모델의  graphviz시각화할 경우, samples과 value의 개수가 안맞는 이유에 대하여"

date: 2021-08-17
last_modified_at: 2021-08-17

use_math: true
comments: true

categories:
  - K-Digital Project
tags:
  - 디스플레이센서 이상요인 분석
---

## Random Forest모델의 graphviz시각화할 경우,  samples과 value의 개수가 안맞는 이유에 대하여

### 1. 문제점
- RandomForestClassiffier로부터 하나의 DecisionTreeClassifier를 추출하여, graphviz로 트리 구조를 시각화해 봤는데 한 노드 내의 samples과 value의 개수가 같아야 한다고 생각했지만 달랐습니다. 문제를 해결하기 위해 DecisionTree에서의 samples와 value의 의미에 대해 다시 알아보았습니다.

<br>

#### Tree구조 시각화 결과

<div style="text-align:center"><span>Samples과 value의 개수가 다름</span><br><img src="\assets\images\36_K-Digital_Training_Project_1.png" alt="36_K-Digital_Training_Project_1" style="zoom:50%;" /></div>



#### graphviz 시각화 소스

```python
# RandomForest의 3번 인덱스 DecisionTree모델을 설정
estimator_5p  = rf_estimator_5p.estimators_[3]

# 3번 인덱스 DecisionTree모델에 대한 Tree구조 시각화
# .dot 파일로 export
export_graphviz(estimator_5p,
                feature_names=X_data.columns,
                out_file='2_randomForest_LRD_5p_MinMax.dot',
                filled = True, # class별 color 채우기
                rounded=True # 박스의 모양을 둥글게
                )

# 생성된 .dot 파일을 .png로 변환
from subprocess import call
call(['dot', '-Tpng','2_randomForest_LRD_5p_MinMax.dot', '-o', working_dir + '2_randomForest_LRD_5p_MinMax.png', '-Gdpi=600'])
```

<br>

### 2. samples과 value의 의미
- samples : 현 규칙 노드에 해당하는 데이터 건수. DecisionTree구조의 samples는 중첩되지 않은 고유한 sample데이터 개수
- value : 클래스 값 기반의 데이터 건수

<br>

samples와 value의 의미를 알았으니 RandomForest의 알고리즘을 알아보겠습니다.

<br>

### 3. Random Forest
- RandomForest는 앙상블 학습 유형 중의 하나인 배깅 유형으로 DecisionTree알고리즘을 여러 개 만들어서 보팅으로 최종 결정하는 알고리즘입니다. 이 때 RandomForest는 각각의 DecisionTree가 학습하는 데이터 세트를 전체 데이터에서 일부 중첩되게 샘플링하는데 이를 부트스트래핑 분할 방식이라고 합니다.
- RandomForest는 모델 생성시  bootstrap방식을 default로 적용하고 있습니다. 다음과 같이 각각의 Decision Tree알고리즘마다 데이터를 추출하여 학습하고 있습니다. 

<div style="text-align:center"><sapn>bootstrap=True의 경우, 매번 다른 데이터로 샘플링</sapn><br><img src="\assets\images\36_K-Digital_Training_Project_2.png" alt="36_K-Digital_Training_Project_2" style="zoom:50%;" /></div>





- 그런데 ```bootstrap=False```옵션을 주게 되면 매번 같은 원본데이터를 이용하여 DecisionTree알고리즘을 수행하게 됩니다.

<div style="text-align:center"><sapn>bootstrap=False의 경우, 매번 같은 데이터로 샘플링</sapn><br><img src="\assets\images\36_K-Digital_Training_Project_3.png" alt="36_K-Digital_Training_Project_3" style="zoom:50%;" /></div>

<br>
아래 사이트에서 Random Forest알고리즘과 트리구조에 대해 자세하게 알 수 있었습니다. 
<br>
[참고 사이트](https://machinelearningmastery.com/bagging-and-random-forest-ensemble-algorithms-for-machine-learning/)

*You need to pick data with replacement. This mean if sample data is same training data this mean the training data will increase for next smoking because data picked twice and triple and more. The best thing is pick 60% for training data from sample data to make sure variety of output will occurred with different results. I repeat. Training data must be less than sample data to create different tree construction based on variety data with replacement.*

<br>

<span>
매번 데이터를 교체하여 추출해야하는데 sample데이터가 학습데이터와 일치한다면 데이터가 두번, 세번 그리고 그 이상 선택되어 단계 학습을 할 때마다 학습데이터는 증가할 것입니다. 가장 좋은 방법은 sample 데이터로부터 60%의 학습데이터를 추출하여 다양한 결과가 나오도록 하는 것입니다. 다시 말하자면, 데이터를 교체하여 다양한 데이터를 기반으로 한 다른 트리구조를 만들어야 하기 때문에 학습 데이터는 sample데이터보다 적어야 합니다. 
</span>

- 확실하지는 않지만 실제로 매 단계마다 value의 개수 대비 samples의 개수를 계산해보면 약60%정도입니다.(60~63%언저리..)
- ```bootstrap=True```와```max_samples=float```옵션을 설정해주면 상위 단계 노드의 데이터로부터 %퍼센트 비율의 데이터를 샘플링하게 됩니다. (value적용)

<br>

### bootstrap=True(default)

<div style="text-align:center"><sapn>samples와 value의 개수가 다름</sapn><br><img src="\assets\images\36_K-Digital_Training_Project_4.png" alt="36_K-Digital_Training_Project_4" style="zoom:100%;" /></div>

<br>

### bootstrap=True and max_samples=0.5

<div style="text-align:center"><sapn>samples와 value의 개수가 다름. value의 개수가 전체 데이터(6516행)의 절반</sapn><br><img src="\assets\images\36_K-Digital_Training_Project_4.png" alt="36_K-Digital_Training_Project_4" style="zoom:100%;" /></div>

<br>

### bootstrap=False

<div style="text-align:center"><sapn>samples와 value의 개수가 같음</sapn><br><img src="\assets\images\36_K-Digital_Training_Project_4.png" alt="36_K-Digital_Training_Project_4" style="zoom:100%;" /></div>

<br>

### 결론

- DecisionTree구조의 samples는 중첩되지 않은 고유한 sample데이터 개수이고, value는 각 클래스에 해당하는 훈련 데이터의 개수입니다!
- **RandomForest수행후 트리구조 시각화할 때 samples와 value의 개수를 같게 하고 싶나요?**
그렇다면 RandomForest 모델 생성시 ```bootstrap=False``` 옵션을 추가하세요! 다만, 이 옵션을 설정하면 같은 데이터로 DecisionTree를 여러번 수행하여 RandomForest의 특징을 잃게 될 것입니다.

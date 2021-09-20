---

title:  "[디스플레이센서 이상요인 분석] 2021/7/31 LEFT, RIGHT컬럼 구별 시도"
excerpt: "공장 생산 라인이 LEFT, RIGHT로 나누어져 있어,  LEFT, RIGHT 라인에 나누어 분류모델링을 해보기 위해 컬럼명 구별을 시도하였습니다"

toc: true
toc_sticky: true

date: 2021-07-31
last_modified_at: 2021-08-07

use_math: true
comments: true

categories:
  - K-Digital Project
tags:
  - 디스플레이센서 이상요인 분석
---

## 오늘 한 것

1. 컬럼명에 다음과같이 왼쪽, 오른쪽 생산라인의 센서처럼 보이는 컬럼들이 존재하여 나중에 왼쪽, 오른쪽컬럼명을 나누어 모델링을 해보면 어떨까 싶어서 컬럼명 분리를 시도하였습니다.

```
D_AB5_R_UL
D_AB6_L_UL
```

혹은

```
TMP.TIN..BAY.10.LEFT.1TI30209.PV
TMP.TIN..BAY.1.RIGHT.1TI30202.PV
```



- 그런데 L 이 포함된 컬럼이 LEFT 뿐만아니라 LOWER의 의미를 가지기도 하고, 다음과 같이 문자열길이가 반드시 같지는 않아서 정확히 컬럼명을 분리하는것은 무리일 것 같습니다.

```
HOOD.N2.TOP.HEATER.L..1JI39007.PV
HOOD.N2.HEATER.R..1JI39008.PV
```



- 그래서 일단은 컬럼명 분리는 미루고 분산으로 인한 이상치 제거를 해보려고 합니다.
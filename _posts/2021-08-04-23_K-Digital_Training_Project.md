---

title:  "[공정이상요인분석] 2021/8/4 VIF계수 구하기"
excerpt: "VIF계수 30이하로 줄이기"

date: 2021-08-04
last_modified_at: 2021-08-07

categories:
  - K-Digital Project
---

## 오늘 한 것

- 어제 밤에 실행시켜놓은 VIF계수 구하는 함수를 실행시켜놓았는데 아침에 보니 컬럼 3개를 20가지의 조합으로 제거해도 VIF계수가 계속해서 낮아지지 않았습니다.
- 컬럼4개, 5개, 6개....의 컬럼 제거 조합을 계속해서 추가해야 하나 고민을 하다가 나무플래닛 이사님께 VIF계수가 상승하지 않게 컬럼을 제거해 나가는 방법에 대해 여쭤봤는데 상승해도 제거해야한다고 하셨습니다. 시간이 많았다면 VIF계수가 높아지지 않도록 계속해서 컬럼의 조합을 찾아 제거했겠지만 시간관계상 이사님의 조언대로 하기로 하고 VIF계수가 상승하더라도 계속 컬럼을 제거했습니다.
- 그랬더니 382개의 컬럼이 179개까지 줄어들었습니다. 그리고 오른쪽 컬럼을 제거하고 나니 150개까지도 줄어들고 상관관계 히트맵을 그려보니 상관관계가 꽤 많이 줄어든 것을 볼 수 있었습니다.

<div style="text-align:center"><img src="\assets\images\23_K-Digital_Training_Project_1.png" alt="img" style="zoom: 28%;" /><img src="\assets\images\23_K-Digital_Training_Project_2.png" alt="img" style="zoom: 28%;" /></div>
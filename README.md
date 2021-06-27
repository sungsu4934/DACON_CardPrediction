# Dacon주관: 신용카드 사용자 연체 예측 AI 경진대회 
https://dacon.io/competitions/official/235713/overview/description
 - 대회개요: 신용카드 신청자가 제출한 개인정보를 토대로 신용카드 연체 사용자 예측 (정형데이터)
 - Metric: Logloss
 - Tool: python
 - Rank: Private (108/714, 상위 15.1%), Public (130/714, 상위 18.2%)

## Feature Enginnering
 - 직업종류 결측치 처리: 근무일수변수를 활용하여 근무일수가 0일인 사람은 직업이 없는 것으로 값 대체, 그 외 Nan은 말하기 부끄러운 직업군으로 판별(이 또한 유의미한 변수로 파악)
 - 직업종류 등 인코딩: (약 30개)를 one-hot-encoding하기에는 curse-of-dimension일 수 있다고 판단. 각 직업군별 신용카드 연체정도의 평균값을 구해 이를 토대로 인코딩
   > 직업종류, 소득종류, 거주형태, 교육수준 도 이와 마찬가지로 진행
  
 - 자동차, 부동산 여부 구분: 두 가지 모두 고가재산으로 둘 다 갖고 있는지, 한 가지만 갖고 있는지, 둘 다 없는지로 구분
 - 나이변수 구간화: 10대, 20대 등 bin을 10으로 구간화
 - 근로변수 구간화: 4년차이하(사회초년생), 5년차에서 9년이하, 10년차에서 20년차이하, 20년차에서 30년차 이하 등으로 구분
 - 아이들 수: 0명, 1명 또는 2명, 3명 이상으로 구분
 - 결혼여부: 미혼, 결혼 및 동거, 결혼 및 미동거로 구분
 
 - 거주형태 및 소득형태에 대해 빈도수 변수 생성
 - Category 변수의 소득에 대한 최저, 평균, 최대 소득값에 대한 변수 생성
   > 거주형태, 교육형태 등 4개 변수
 - ['car','reality','work_phone','phone','email'] 들 중 몇개에 속하는지 파악
 - kmean 기반 파생변수: kmean기반 군집생성 및 elbow method를 통해 구한 4개의 군집 중심에서 해당 object가 얼마나 떨어져있는가에 대하여 변수 생성
###
## Modeling
 - 최종 활용된 모델: lightgbm
 - 절차
  > 1) AutoLGB을 통한 최적 파라미터 searching
  > 2) lightgbm 모델을 5-fold 방식으로 생성 
###
## 시행착오
 - 시행착오로 활용된 모델: Lightgbm, DNN
 - 시행착오 정리
  > 1) AutoEncoder를 활용한 Feature engineering
  > 2) TargetEncoding 활용한 Feature engineering
  > 3) Lightgbm의 변수를 한 두개씩 바꾸면서 모델을 형성한 후 스태킹 앙상블
  > 4) DNN을 활용한 Modeling
  
## 총평
 - 여러 Feature Engineering과 딥러닝, 스태킹 앙상블 등을 활용해보았으나 결론적으로 Lightgbm 단독 모델이 더 우세한 성능

## 추후 todo
 - lightgbm 뿐만 아니라 xgboost, catboost 등 다른 부스팅 계열 실험(실제 1등 모델의 경우 catboost였음)
 - 정형데이터 딥러닝 모델인 TabNet에 대한 학습. 비교적 최신 모델인 TabNet에 대한 논문 리뷰
 - AutoEncoder에 대한 원천적 원리와 더 좋은 feautre를 생성하기 위한 방법 생각

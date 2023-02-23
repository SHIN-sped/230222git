# 230222git
###### 230222git.git

git.git
git

git

git
---
***
# 1
# 2
# 3
# 4

1. ol orderlist
2. ul unorderlist
3. ol ul
ul
ol

* ol order list
* ul unorder list
  * ol ul

**ol**
_ul_
***older list***
~~unorder list~~


# 메인토픽
* numeric, categorical, ordinal, datetime, coordinate type feature에서의 preprocessing, generation 방법
* Missing value의 처리방법


# Numeric features
# Preprocessing
## feature scaling

* Tree based model은 scaling이 필요 없음.
* Nearest Neighbor, linear model, Neural Network : scaling 필수

## 방법

* MinMax scaling : scale만 달라지고, 분포는 그대로 유지
* StandardScaler : 평균 0, 표준편차 1인 분포로 transformation

## outliers

* linear model은 특히 outlier에 취약함.
* lower bound와 upper bound를 이용하여 제거해야함


## 방법 winsorization

* 1 percentile, 99 percentile upperbound, lowerbound = np.percentile(x,[1,99]) y = np.clip(x, upperbound, lowerbound)

## Rank

* rank processing은 다양한 값의 간격을 동일하게 만들어 줌.

* outlier가 있을 경우, MinMaxScaler보다 좋은 효과를 보임 why? outlier를 다른 값과 가까이에 위치하게 만들기 때문

* linear model, KNN, NN 에 좋은 효과

## 방법 from scipy.stats import rankdata rankdata([0, 2, 3, 2], method = 'min','max','dense','ordinal') More about preprocessing

* log 변환
* 루트 변환
non tree based model에 특히 NN에 좋다
다양한 feature preprocessing을 진행한 후에 모델을 mix하면 좋은 영향을 준다.

# Feature generation
사전지식 ⇒ 구글링과 관련 문헌들을 살펴보며 domain 습득
EDA를 통해서 ⇒ 뒤에서 배움
# Categorical and ordinal features

# Ordinal Feature
* 티켓 등급, 자격증 등급, 교육수준 등 order가 있는 category feature


## encoding method

* label encoding : tree model에서 좋음, non tree model에서는 보통 안좋음

## 방법
 label encoder는 알파벳 순서
factorize는 값 등장 순서
sklearn.preprocessing.LabelEncoder pd.factorize()
* frequency encoding : 값의 분포에 대한 정보. tree/non tree 다좋음 ※ 같은 frequency를 지니는 값이 있을 때는 rank 하고 나서 encoding하자.

# categorical feature

* One-hot encoding(OHE)
* linear model, KNN, NN에 좋음
 
값이 많은 feautre를 ohe하면 대부분의 값이 0인 matrix생성
이 때 sparse matrix 으로 변환

# Datetime and coordinates
# date and time
* periodicity
* day, second, minute, hour seq in week, month, season, year
데이터에서 반복적인 패턴을 알아낼 수 있음.

* Time since
어떤 순간으로부터의 time delta
(e.g.) since UTC 00:00:00 1970-01-01

중요한 순간으로부터의 time delta
다음 휴일까지 며칠 남았는지, 마지막 휴일로부터 며칠 지났는지 등등

* Difference between dates

* 어떤 두 event사이의 date diff를 구하는 것.
* 회원가입 후 구매까지 걸린 일수 등등
* Datetime feature generation이후 preprocessing 필수.

# Coordinates
중요한 장소들과의 거리

* 병원, 학교 등등
* 이런 장소가 딱히 없다면, data에서 그런 point를 뽑아내라
* map을 격자로 나누고, 한 격자 내에 특정 point를 잡은 후, 격자내 다른 데이터와의 거리를 구한다
* data point들로 clustering하고, centroid를 중요한 장소로 간주.
* 어떤 장소의 주변으로 통계량 계산

* 어떤 장소 주변의 아파트 개수
* 평균 부동산 가격 등
* rotated coordinates

* 축을 잘 rotate하면 여러번의 split대신 한번의 split으로 분류 가능
* tree model에서 좋은 성능
* 22.5, 45 도 등 여러번 시도 필요
 

# Handling Missing Values
* histogram을 그렷는데 이런 모양이면 주최측에서 임의로 na를 채운것.
 

왼쪽은 na를 -1로 대체, 오른쪽은 na를 mean값으로 대체

# fillna 방법

999, -1
linear model이나 NN에는 안좋음
mean, median
linear model이나 NN에 좋음
## feature generation
1. isnull feature
2. feature engineering은 na를 채우기 전에 해야함. na를 채우고 하면 분포에 영향을 준다.


# Exploratory data analysis EDA가 무엇인가?
* data를 살펴보면서 "잘 이해하고, 익숙해 지는 것"
* data에 대해 직관을 만든다.
* feature에 대한 가설을 수립한다 ⇒ better score
* 재미있는 insight 발견 ⇒ better score

# Building intuition about the data
# EDA step

* domain 지식 얻기
* 데이터가 말이 되는지 체크
* 데이터가 어떻게 생성되었는지 히애

# domain 지식 얻기

* 분야가 다양하기 때문에 아주 자세하게는 알필요 X
* 우리의 목적이 정확하게 무엇인지
* 우리가 가진 데이터는 어떤 데이터인지
* 보통 어떻게 그 문제를 해결하는지
* 구글링, 위키피디아 등등 search

# 데이터가 말이 되는지 체크

* 데이터가 domain 관점에서 말이 되는지 봐야함.(e.g.) age 변수에 값이 300.

* 이해가 안될 때는 discussion을 활용

*  휴먼에러가 아닌, logic이 이상해서 나오는 반복적인 오류
* ⇒ feature generation 가능

## 데이터가 어떻게 생성되었는지 이해

* train / test 데이터가 어떤 방식으로 sampling되었는지

⇒ 바탕으로 train / validation split 해야함(상당히 중요)

* train/test가 각각 다른 방식으로 생성되었다면, tr을 vl로 이용 불가.
* (e.g.) tr의 period > tst 의 period, tr row 개수 < tst row 개수
* ⇒ 다르게 sampling된 것.


 
 
# Exploring anonymized data Anonymized data란
* feature이름에 아무 의미가 없음(x00,x01,x02...)
* test는 hash되어 있음.

우리가 할 수 있는 것.
* 개별 feature 탐색
 * column의 의미를 추측(어려운 경우 대다수)
 * column의 타입을 추측(이거라도 하면 좋음)
  1. dtype이 float이면 numeric 변수
  2. dtype이 integer이면 event counter, label encoded, unixtime 등등

* feature간의 관계 알아보기
 * feature pair의 관계 찾기
 * feature group 찾기
 
Visualizations
앞서 다룬 내용들을 잘 할수 있게 해주는 도구

# 개별 feature 탐색
## Histogram

* 많은 값들이 0에 쏠려있는 경우, log를 취하면 좋음.

* log를 취해서, na를 mean으로 채웠다는 것을 발견함.


왼쪽이 log 변환 전, 오른쪽이 log 변환 후
 

Plots


가로 선이 선명하면, 중복 값이 많은 것.
가로선은 보이고, 세로선은 안보인다 ⇒ shuffled well
class label 별로 시각화
데이터가 전혀 섞여 있지 않음을 확인
feature간의 관계 탐색
우리의 목적은 데이터를 해독하는 것이 아니라, feature를 만들어 내는 것.

어떤 feature를 만들 수 있을 지에 주목하여 탐색.

 

Scatter plots


가장 왼쪽 부터 그림 1,2,3,4 
1. classification 일 때 label 구분하여 시각화

1. regression일 때 heatmap or point size 로 구분

 

2. train / test set의 분포가 같은지 다른지 확인할 때 유용

2. test가 class0과는 분포가 겹치지만 나머지는 다른 구역에 분포함.

2. overfitted feature일 가능성

 

3. X1과 X2의 관계 X1 + X2 < 1

3. 위 관계를 바탕으로 feature generation 가능

3. X1-X2, X1/X2.

 

4. 여러개의 삼각형이 보임.

4. 각 삼각형에 point가 포함되는 지 여부로 feature generation

 

 

feature 간의 distance 구하기

상관계수
X1이 X2보다 값이 큰 횟수
distinct combination 개수 등
Feature group 만들기

비슷한 feature들로 group을 만든다

feature 평균을 내서 sorting하면 group 을 찾을 수 있다
group을 찾아낸 후, group별 feature generation 유용함.


feature들의 평균을 계산한 후 sorting하여 plot한 모습
 
Dataset cleaning 과 체크 해야할 것들
# duplicated and constant feature

## constant feature

* 모델에 도움 X, 메모리만 낭비하므로 지워야함
* train에서는 constant, test에서는 non-constant ⇒ 그래도 지워야함
* train에서는 non-constant, test에서는 constant ⇒ vl 으로 simulate 후 결정

## duplicated feature

* numeric feature는 단순 비교
* categorical feature는 pd.factorize하여 비교

https://outyj.tistory.com/8?category=846181

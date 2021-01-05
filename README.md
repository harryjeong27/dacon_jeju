# dacon_jeju
dacon project of visualizing Jeju disaster spent during May - Aug

# Table of Contents
1. [Problem Definition & Domain Research](#ProblemDefinition&DomainResearch)  
    1.1 [Problem Definiton](#problemdefinition)  
    1.2 [Data](#data)  
    1.3 [Domain Research & Questions](#domain)  
2. [Acquire training and testig data : Data Loading](#dataloading)  
    2.1 [Package Loading & Basic Setting](#package)  
    2.2 [Data Loading](#loading)  
3. [Data Analyze (EDA) & Preprocessing (Wrangle, Cleanse)](#EDA&Wrangling)  
    3.1 [Analyze by describing data (Quick-view)¶](#quick)  
    3.2 [Assumption in 5-fundamental ways](#assumption)  
    3.3 [Analyze by pivoting features](#pivoting)  
    3.4 [Analyze by visualizing data in 5 ways](#visual)  
    3.5 [Wrangle data](#wrangle)  

# 1. Problem Definition & Domain Research<a name="ProblemDefinition&DomainResearch"></a>
## 1.1 Problem Definiton<a name="problemdefinition"></a>
### 1.1.1 Topic
- 공간 정보를 활용한 탐색적 데이터 분석 경진대회
### 1.1.2 Background
- 공간 정보를 활용하여 올 5~8월까지의 제주 지역 데이터를 분석하여 다양한 인사이트 발굴
### 1.1.3 Purpose
- 공간 정보를 활용한 탐색적 데이터 분석 및 시각화
- 공간 정보에 대한  일반인의 관심을 제고할 수 있는 인사이트 발굴  
### 1.1.4 Host  
- 국토연구원, Dacon

## 1.2 Data<a name="data"></a>
### 1.2.1 Dictionary
- YM : 기준년월  
- SIDO : 지역대분류명  
- SIGUNGU : 지역중분류명  
- FranClass : 소상공인구분  
  - 영세 : 연매출 3억 이하
  - 중소 : 5억 이하
  - 중소1	: 10억이하
  - 중소2	: 30억이하
  - 일반(대형) : 30억초과 
- Type : 업종명  
- Time : 시간대  
  - 새벽 : 2~6	
  - 오전 : 6~11	
  - 점심 : 11~15	
  - 오후 : 15~18	
  - 저녁 : 18~22	
  - 심야 : 22~02	
  - x : 무승인거래 (별도 승인없이 결제되는 건(SMS자동결제, 기내 면세점 등))
- TotalSpent : 총사용금액 (재난지원금 사용금액 포함)
- DisSpent : 재난지원금 사용금액  
  - negative value : refund
- NumOfSpent : 총 이용건수  
- NumOfDisSpent : 총 재난지원금 이용건수  
- POINT_X, POINT_Y : X,Y 좌표  

## 1.3 Domain Research & Questions<a name="domain"></a>
- What is Disspent?
  - Why
    - 코로나19 위기 극복을 위한 정부의 한시적인 지원제도 
    - 국민생활 안정과 경제회복 지원을 목적으로 함
  - When & what
    - first : from 20/04/20 to 20/05, cash 412억원
    - second : from 20/08/24 to 20/10/11, cash 648억원
    https://www.chosun.com/national/regional/jeju/2020/11/16/NMNHCRFVSBBSVI7Q6ZTINA6SLY/
  - where
    - offline only
    - not allowed : 대형마트, 백화점, 유흥업종, 위생업종, 골프장/노래방 등 레저업종, 카지노/복권방 등 사행업종, 성인용품점, 귀금속, 면세, 보험, 교통/통신료 등 자동이체 건
- Questions
  - 재난지원금 사용처 제한으로 인한 소비의 이동
    - 골프장/노래방 등 레저업종 혹은 대형마트 혹은 백화점 혹은 유흥업종 혹은 면세에 사비를 쓰고, 꾸준히 비용발생하던 타 업종에 재난지원금 사용가능  
      How? 전체 사용금액 변화체크, 사용제한 업종별 사용금액 변화체크, 사용가능 업종별 사용금액 변화체크
  - 국민의 소비가 재난지원금 지원 이전과 비교하여 높아졌는가?
    - 100만원 쓰던 사람이 130만원 썼는지? => 국민소비 증가 / 자영업자 매출증가
      How? 전체 사용금액 변화체크
    - 100만원 쓰던 사람이 사비 70, 재난지원금 30쓰고 남은 돈은 저축했는지? => 국민소비 답보 / 자영업자 매출증가 혹은 답보
      How? 전체 사용금액 변화체크, 재난지원금 사용금액 체크
  - 기간별 변화
  - 시간별 변화
  - 업종별 변화
  - 업종규모별
  - 지역별 변화
  - 각 컬럼의 상관관계 체크 -> 이를 활용한 시각화

# 2. Acquire training and testig data : Data Loading¶<a name="dataloading"></a>
## 2.1 Package Loading & Basic Setting<a name="package"></a>
## 2.2 Data Loading<a name="loading"></a>

# 3. Data Analyze (EDA) & Preprocessing (Wrangle, Cleanse)<a name="EDA&Wrangling"></a>
## 3.1 Analyze by describing data (Quick-view)¶<a name="quick"></a>
### 3.1.1 Check columns (name)
    df.columns.values
### 3.1.2 Check feature type
1) Categorical
- Categorical
- Ordinal
2) Numerical
- Continous
- Discrete
    df.head()
    df.tail()
### 3.1.3 Check errors or typos
    plt.figure(figsize = (12, 8))
    sns.boxplot(data = df_all[spent], color = 'red')
    plt.show()
### 3.1.4 Check blank, null or empty values & data types
- integer or floats or strings(objects)
    df.info()
    import missingno
    missingno.matrix(df);
### 3.1.5 Check distribution of numerical feature values
    df.describe()
## 3.2 Assumption in 5-fundamental ways<a name="assumption"></a>
>We arrive at following assumptions based on data analysis done so far. We may validate these assumptions further before taking appropriate actions.

### 3.2.1 Correlating.
Correlating. One can approach the problem based on available features within the training dataset. Which features within the dataset contribute significantly to our solution goal? Statistically speaking is there a correlation among a feature and solution goal? As the feature values change does the solution state change as well, and visa-versa? This can be tested both for numerical and categorical features in the given dataset. We may also want to determine correlation among features other than survival for subsequent goals and workflow stages. Correlating certain features may help in creating, completing, or correcting features.
- correlation btw dependant variable and each explanatory variable

### 3.2.2 Completing.
Completing. Data preparation may also require us to estimate any missing values within a feature. Model algorithms may work best when there are no missing values.
- ex) there is no missing values

### 3.2.3 Correcting.
Correcting. We may also analyze the given training dataset for errors or possibly innacurate values within features and try to corrent these values or exclude the samples containing the errors. One way to do this is to detect any outliers among our samples or features. We may also completely discard a feature if it is not contribting to the analysis or may significantly skew the results.
- ex) Q_Ques may be dropped as it contains relative number of time (each Q_Time's total is same) & it is hard to find the relation btw Answering time & Reliability.
- ex) W_Ques may be dropped as we could not find any relation with voted or other features.
- ex) some of features in human group may be dropped as it does not have any relation with voted or others: hand, engnat, familysize.

### 3.2.4 Creating.
Creating. Can we create new features based on an existing feature or a set of features, such that the new feature follows the correlation, conversion, completeness goals.
- ex) We may create a new feature called mach_score based on the concept of the maki test.
- ex) We may create a new feature called tp_score based on the concpet of TIPI test.

### 3.2.5 Classifying.
Classifying. We may want to classify or categorize our samples. We may also want to understand the implications or correlation of different classes with our solution goal.
- ex) 10s are more likely not to have voted. (under the voting age)
- ex) The educated are more likely to have voted.
- ex) The people with High mach_score are more likely to have voted.

## 3.3 Analyze by pivoting features & Visualization<a name="pivoting"></a>
### 3.3.1 Macro 1 (key vs spent)
- 목표 없이 key feature와 spent feature 간의 관계를 개괄적으로 살피면서 특성을 찾아본다.  
spent = ['totalspent','disspent','numofspent','numofdisspent', 'tot_dis', 'num_tot_dis']  
key = ['sigungu', 'ym', 'type','time','franclass','dong','category']  
- all period
  - 'sigungu' vs spent
  - 'ym' vs spent
  - 'category' vs spent
  - 'type' vs spent
  - 'time' vs spent
  - 'franclass' vs spent
  - 'dong' vs spent

### basic
  - totalspent : 700,426,010,461 (7천 4억 2천만원)
  - disspent : 35,733,656,647 (357억 3천만원)
  - numofspent : 21,917,930 (2천 1백 9십만 회)
  - numofdisspent : 1,242,134 (124만 회)
  - tot_dis : 664,692,353,814 (6천 6백 4십억 6억 9천만원)
  - num_tot_dis : 20,675,796 (2천 6십 7만 회)  
  *Q1* : tot_dis, num_tot_dis에 따른 월별 변화량 => 사비 사용금액의 추이 확인하기
  - totalspent / numofspent : 31,957원 (회당 평균사용액)
  - disspent / numofdisspent : 28,768원 (회당 평균재난지원금사용액)  
  *Q2* : 회당 사용금액에 따른 업종 비교 => 제품/서비스가 고액인 업종에서는 많이 쓰이지 못했을 것이다.
  
### 'totalspent'
- 'totalspent' of 'ym' : Aug > July > June > May
  - 관광업 중심의 제주도의 경우 5 -> 8월은 성수기로 가는 단계이기에 'totalspent'가 늘었다고 판단됨. 하지만, 작은 차이임
- 'totalspent' of 'time' & 'time_cut' : 점심 > 저녁 > 오후 > 오전 > 심야 > 새벽
  - 점심, 저녁 시간대가 높으며, 활동이 적은 심야, 새벽 시간이 확실히 적음
- 'totalspent' of 'sigungu' : 제주(73.7%) > 서귀포시(26.3%)
- 'totalspent' of 'dong' : 상위 10개동 (연동>노형동>용담이동>이도이동>애월읍>서귀동>일도이동>한림읍>조천읍>안덕면)  
  *Q3* : 상위권 동은 아무래도 관광상권이 아닐까?  
  *Q4* : 재난지원금은 도민만 사용할 수 있으므로, 재난지원금만으로 봤을때 도민들이 어디서 재난지원금을 많이 사용했는지 볼 수 있음
- 'totalspent' of 'category' : 외식/주점(25.8%), 마트/편의점(유통)(24%), 여행/숙박(10.9%), 주유/자동차(6.9%), 의료(6.5%)
  - 교통/통신, 금융 등 재난지원금 사용이 막혀 있는 분야는 아무래도 금액사용비율이 매우 적음
- 'totalspent' of 'type' : 일반한식(17.1%), 슈퍼마켓(7.8%), 편의점(5.6%), 면세점(4.8%), 주유소(4.8%) 등
  - 상위권은 면세점을 제외하고는 재난지원금 사용가능 업종, 음식/생활용품 업종
- 'totalspent' of 'type' : 일반(43%), 영세(22.4%) 양극단이 대부분 차지  
  *Q5* : 재난지원금 사용이 늘어날수록 영세 비율이 높아지는지 체크할 필요 있을듯

### numofspent
- 'totalspent'와 거의 비슷한 흐름

### disspent
- 'disspent' of 'ym' : May(67.7%), June(27.1%), July(3.8%), Aug(1.5%)
  - 재난지원금이 지급된 5월에 압도적으로 사용함
- 'disspent' of 'time' : 시간별 재난지원금 사용금액은 'totalspent'와 매우 유사
  - 재난지원금이든, 개인돈이든 사용시간대는 비슷
- 'disspent' of 'sigungu' : 'totalspent'와 비슷
- 'disspent' of 'dong' : 상위 10개동은 'totalspent'와 약간 다른 양상
(노형동>연동>이도이동>일도이동>서귀동>애월읍>일도일동>도남동>동홍동>한림읍)
  *Q6* : 관광상권의 영향으로 보임
- 'disspent' of 'category' : 마트/편의점(유통)(36.3%), 외식/주점(22.5%), 주유/자동차(8.2%), 의료(7.9%), 레저/스포츠(4.6%)
  - 'totalspent'와 비교하여 재난지원금이 사용불가한 면세점 등은 하위권으로 이동
- 'disspent' of 'type' : 일반한식(16.9%), 슈퍼마켓(14.8%), 농축협직영매장(7.3%), 편의점(5.7%), 주유소(5.5%)
  - 'totalspent'와 시각적 비교가 필요해보임
- 'franclass'도 비슷함

### numofdisspent
- 'disspent'와 비슷한 흐름

### tot_dis
- 'totalsepnt'와 비슷한 흐름

### 3.3.2 Macro 2
- 목표 없이 key vs times, spent feature 간의 관계를 개괄적으로 살피면서 특성을 찾아본다.  
times = ['ym', 'time', 'time_cut']  
spent = ['totalspent','disspent','numofspent','numofdisspent', 'tot_dis', 'num_tot_dis']  
key = ['type', 'dong','category']  

### category
- 'category' vs times + spent
  - 'ym' + spent
    - 5->8월로 갈수록 관광객이 증가하므로, 여행/숙박, 외식/주점, 마트/편의점(유통)은 'totalspent'가 증가하는 모습을 볼 수 있음
    - 'numofspent'도 같이 증가하는 흐름, 여행/숙박에서는 'numofspent'증가량에 비해 'totalspent'증가량이 매우 큰데, 이것은 여름성수기로 들어갈수록 요금이 높아지는 것을 보여주는 것으로 보임
    - 'disspent'는 'totalspent'와 비슷한 흐름으로 사용했음이 보이며, 한가지 특징은 재난지원금이 사용될 수 있는 업종임에도 불구하고 사용이 매우 적은 여행/숙박을 보았을 때 도민들은 여행/숙박에는 재난지원금을 포함한 돈을 잘 사용하지 않는 것으로 보임
    *Q7* : 위와 같은 방식으로 재난지원금 혜택을 본 업종을 판별할 수 있을 듯
    - 재난지원금은 5월에 압도적으로 쓰였으며, 대부분 마트/편의점(유통), 외식/주점에서 사용하였고, 7, 8월도 같은 흐름이었다.
    *Q8* : 5월에 지나치게 많이 쓰여 6, 7, 8월에는 소비진작 효과가 미미하지 않았을까?
    - 마트/편의점(유통), 여행/숙박, 외식/주점은 5->8월에 사용횟수 증가가 두드러짐.
  - 'time_cut' + spent
    - spent에 상관 없이 비슷한 양상을 보이며, 큰 특이점을 찾을 수 없었음

### type
- 'type' vs times + spent
  - 'ym' + spent
    - 202005기준 상위 15개만 색인
    - 보통 8월로 갈수록 사용금액이 높아지나 스포츠레저용품은 반대인 모습을 보임
    *Q9* : 왜 스포츠/레저용품은 8월로 갈수록 줄어들까? 이미 그전에 많이 구매해서 그런건 아닐까?
    - 'disspent'를 보면, 농협직영매장, 농협하나로클럽에서 재난지원금을 많이 사용한 것을 볼 수 있다.
  - 'time_cut' + spent
    - 거래량이 제일 많은 점심 거래량 순으로 시각화하였으며, 업종의 특성에 따른 거래량이 보인 것 같다. ex) 저녁이 점심에 비해 식비 평균사용금액이 높음

### dong
- 'dong' vs times + spent
  - 'ym' + spent
    - 7월의 연동, 노형동, 이도이동, 8월의 용담이동, 애월읍, 한림읍, 조천읍, 성산읍, 안덕면, 구좌읍은 사용금액이 갑자기 상승하는 경향이 있음
    *Q10* : 해당기간에 해당동에서 축제가 있었던 건 아닐까? 혹은 관광지인가?
    - 전체사용금액의 경우 월이 지날수록 커지는 추세였으나 재난지원금은 다른 형상이며, 5월이 작년과 비교했을때 총사용금액이 높아졌는지 볼 필요가 있을듯 혹은 5월에 재난지원금으로 원래 쓰던 돈을 많이 세이브했으므로 6, 7, 8월에 더 사용했다고 볼 수도 있을듯
    *Q11* : 노형동에서 가장 많은 재난지원금이 쓰인 것으로 보아 주민이 가장 많이 사는 곳인지 볼 필요가 있음
  - 'time_cut' + spent
    - 심야, 새벽에 가장 많은 사용금액이 나오는 동은 연동
    *Q12* : 연동엔 도대체 뭐가 있는 걸까?
    - 연동은 전체사용금액에서도 심야, 새벽 사용률이 상위권이었는데 재난지원금에서도 그런 것을 보니 주민도 많이 살고 관광객도 많이 오는 지역으로 생각됨

### 3.3.3 Resolving assumptions & questions

### Checkpoint 1. 월별 재난지원금 사용비율
- 사용비율체크 (전체, 월별, 업종별, 지역별, 업종규모별)
- 전체 by 파이차트
  - 전체 소비 중 재난지원금 사용비율은?
  - 5.1%이며, 전체 재난지원금 412억원(1차) 중 357억 3천만원이 사용되었다. 소진율은 86.7%다. 총 사용건수 중 재난지원금 사용건수는 5.7%이다.
- 월별 by 누적막대그래프 (비율)
  - 월별 전체사용금액 중 재난지원금 사용비율은?
  - 월별 각각 14.3, 5.8, 0.7, 0.3%이며, 5월에 대부분이 소비되었다.
  - 사용된 재난지원금 중 월별 사용된 재난지원금 비율은 67.7%, 27.1%, 3.8%, 1.5%로 5월에 약 67%, 6월에 약 27%로 전체의 95% 가량이 소진되었다.
  - 재난지원금으로 인해 5월 소비가 많이 진작되었다. 6, 7, 8월은? (인구 및 관광객 숫자 체크)
- 2020년 제주도 내 인구현황 & 관광객현황에 따른 분석
  - 인구현황
    - 월별 인구 수는 5-8월 소량 증가추이지만 약 67만명 (5월이 가장 적음)
    - 시별 인구 수는 제주시가 약 49만, 서귀포시가 약 18만으로 제주시가 약 2.7배 많음
    - 동별 인구 수는 노형동이 5만4천명으로 1위이며, 가장 적은 지역은 제주시 추자면으로 1696명이 살고 있음.
    - 지도에 표시하기 ***
  - 입국객현황 (관광객으로 추정)
    - 월별 제주도 입국객 수는 5-8월 간 매월 증가하며 5월 79만, 6월 89만, 7월 102만, 8월 114만명을 기록했다. (5월이 가장 적음)
    - 증가율은 12.5%, 14.6%, 11.7%로 7월에 가장 가파르게 증가하였다.
- 결론  
  1) 총 재난지원금 약 412억원(1차) 중 약 357억 3천만원이 사용되었고, 소진율은 86.7%였다.
  2) 월별 전체사용금액 중 재난지원금 사용비율은 5월이 14.3%로 가장 높았으며, 사용된 재난지원금 중 월별 사용된 재난지원금 비율은 5월이 67.6%, 6월이 27%로 전체의 95%를 소진하였다.
  3) 5-8월 제주도 내 인구 수를 도민 현황 및 관광객 현황 데이터를 바탕으로 본 결과 5->8월에 계속 증가하는 모습을 보였다.
  4) 위 1-3)에 따라 인구가 가장 적었던 5월에 총사용금액이 높았던 이유는 재난지원금 지급이 큰 원인인 것으로 보인다.

### Checkpoint 2. 업종별 재난지원금 사용비율
- 업종별 by 수평누적막대그래프 (비율)
  - 주로 외식/주점, 마트/편의점(유통), 주유/자동차, 의료 업종에서 재난지원금이 소비되었으며, 6~8월은 재난지원금이 거의 소진되어 사용되지 않았다.
  - 5월과 재난지원금 사용금액이 거의 없는 8월의 소비를 분석해보면 어느 업종/지역/규모업종의 재난지원금이 사용되었는지 확인 할 수 있을 것이다.
    - 재난지원금이 많이 사용된 외식/주점, 마트/편의점(유통)은 매월 거의 비슷하게 소비된 것을 보아 5, 6월에 사용할 사비를 재난지원금을 이용하였거나 재난지원금 사용을 위해 소비를 한 것으로 보인다.
    - 의료, 레저/스포츠의 경우, 재난지원금이 사용된 5, 6월은 높은 사용금액, 7, 8은 전보다 낮은 사용금액을 보이는 것으로 보아 재난지원금이 지원된 시기에 의료 서비스를 더 많이 받은 것으로 보이며, 세부항목을 들여다볼 필요성이 있다. *
    - 쇼핑/패션을 보아도 5, 6월에 재난지원금으로 인해 소비가 많이 진작된 것을 볼 수 있다.
  - 'disspent'는 'totalspent'와 비슷한 흐름으로 사용했음이 보이며, 한가지 특징은 재난지원금이 사용될 수 있는 업종임에도 불구하고 사용이 매우 적은 여행/숙박을 보았을 때 도민들은 여행/숙박에는 재난지원금을 포함한 돈을 잘 사용하지 않는 것으로 보인다.
  - 위 내용을 보았을 때 재난지원금 혜택을 본 업종 : 외식/주점, 마트/편의점(유통), 의료, 레저/스포츠, 쇼핑/패션
  - 위 업종 위주로 세부업종 살펴보기
    - 외식/주점에서 가장 많이 사용된 세부업종은 일반한식이며, 압도적이다.
    - 마트/편의점(유통)에서 가장 많이 사용된 세부업종은 슈퍼마켓이며, 그 다음으로 편의점, 농축협직영매장, 농협하나로클럽이 눈에 띈다.
    - 의료에서 가장 많이 사용된 세부업종은 약국이며, 그 다음으로 의원, 종합병원, 치과의원이 있다.
    - 레저/스포츠에서는 스포츠레져용품이 제일 많이 사용되었다.
    - 쇼핑/패션에서는 정장이 가장 많이 사용된 세부업종이었으며, 그 다음으로 스포츠의류, 기타잡화가 있다.
- 결론
  - 위 결과에서 상위권에 속하는 업종들은 타 업종에 비해 재난지원금 혜택을 크게 보았고, 이 업종들에 대해서는 재난지원금 효과가 있었다고 판단된다.
  - 다만, 5월에는 소비진작이 확실히 보였으나, 5월에 재난지원금을 사용함으로써 세이브한 금액을 이월사용했다는 부분은 없거나 파악하기 어려워 보인다. 그 이유는 5월 재난지원금으로 반짝 사용금액이 높아졌던 업종은 6-8월에 다시 낮아졌으며, 5월에 높고 6-8월까지 높음을 유지한 업종은 도민/관광객 수의 증가로 인한 것일 가능성이 꽤 높기 때문이다.

### Checkpoint 3. 지역별 재난지원금 사용비율
- 지역별 by 가로막대그래프
  - 주로 노형동, 연동, 이도이동, 일도이동 지역에서 재난지원금이 소비되었으며, 해당지역의 전체 소비 중 재난지원금 사용비율은 30% 이상이다.
  - 해당지역의 재난지원금 사용이 높은 이유는 거주도민과 밀접한 연관이 있어 보인다. 재난지원금 사용금액순과 유사하게 인구분포를 보인다. (지도*)
  - 5월 사용금액보다 8월 사용금액이 높은게 일반적이지만, 5월 사용 재난지원금이 높은 동은 대체로 8월 사용금액과 5월 사용금액이 비슷한 양상을 보인다.
- 결론
  - 1) 재난지원금은 도민이 많이 살고 있는 지역에서 대부분 사용된 것으로 보인다. ex) 전체 도민의 약 8%가 살고 있는 노형동에서 전체 재난지원금의 10% 가량이 사용됨.
  - 2) 5월 재난지원금으로 인해 동별 소비도 8월에 가까운 수준으로 올라갔음을 볼 수 있다.

### Checkpoint 4. 업종규모별 재난지원금 사용비율
- 업종규모별 
  - 총사용금액은 일반, 영세, 중소1, 중소2, 중소 순으로, 재난지원금은 일반, 영세, 중소1, 중소2, 중소 순으로 많이 사용되었다.
  - 전체 재난지원금 중 재난지원금 사용비율은 일반이 40.4% 가장 많이 사용되었고, 영세 23.7%, 중소1 13.4%, 중소2 13.1%, 중소 9.41% 순으로 나타났다.
  - 좀 더 세부적으로 보기 위해 전체 사용금액 중 재난지원금 사용비율을 비교해보았더니, 중소1, 중소2, 영세가 5.4%대를, 중소가 4.8%, 일반이 4.7%로 나타났다.
    - 즉, 전체 재난지원금 사용은 일반이 가장 높았으나, 전체 사용금액에 비해서는 일반이 가장 낮음을 볼 수 있다. 이는 일반이 일반적으로 금액이 많이 쓰였음을 보여주고, 동시에 영세와 중소에서 작은 차이이지만 그래도 재난지원금이 잘 활용되었다는 점을 보여준다.
  - 좀 더 세부적으로 보기 위해 월별 비교를 해보았다.
    - 도민/관광객 수가 점차 늘어남에도 5월에 각 업종규모들이 높은 수준을 보여주었으며, 일반을 제외한 업종규모에서 특히 그랬다.
  - 조금 더 자세히 보기 위해 업종규모별 재난지원금이 많이 사용된 업종의 세부업종별 재난지원금 사용분포를 살펴보았다.
    - 외식/주점의 경우 일반한식이 제일 많았는데, 그 중에 영세의 비중이 가장 높았다. 다른 세부업종인 서양음식, 일식회집 등도 영세에서 가장 많은 재난지원금 사용을 보여주었다.
    - 마트/편의점(유통)의 경우, 일반의 사용이 많았는데, 슈퍼마켓, 농축협 관련 세부업종 등이 그랬다.
    - 의료의 경우, 약국과 의원이 비율이 높았는데, 약국의 경우 일반과 중소2가 높았고, 의원의 경우 중소1과 중소2가 높았다. 종합병원은 일반이 거의 유일했다.
    - 주유/자동차의 경우, 일반에서 대부분이 사용되었음을 볼 수 있다.
  - 그렇다면 업종규모별 어떤 업종, 세부업종이 있는지 얼마나 있는지 확인해보자.
    - 영세의 경우, 외식/주점이 40%이상을 차지했다.
    - 일반의 경우, 외식/주점, 마트/편의점(유통)이 대부분을 차지했다.
    - 중소, 중소1, 중소2도 비슷한 모습을 보였다.
    - 세부업종의 경우, 영세는 일반한식, 서양음식이, 일반은 일반한식, 서양음식, 편의점, 주유소, 중소는 일반한식, 편의점이 가장 많은 비율을 보여주었다.
- 결론
  - 1) 업종규모별 총사용금액, 재난지원금 사용금액은 업종규모별 개수와 같은 순으로 많았다. (일반>영세>중소1>중소2>중소)
  - 2) 전체 재난지원금 중 재난지원금 사용비율만 보자면 일반에서 너무 많은 재난지원금이 쓰였다고 볼 수 있으나, 업종규모별 전체 사용금액 중 재난지원금 사용비율과 월별 업종규모별 전체 사용금액을 보면, 영세 및 중소에서 일반보다 5월에 재난지원금 효과가 컸음을 알 수 있다.
  - 3) 업종규모별 재난지원금이 어떤 세부업종이 쓰였는지를 보았을 때, 외식/주점에서는 영세 및 중소에 많이 사용되었고, 마트/편의점에서는 일반에, 의료에는 일반 및 중소, 주유/자동차의 경우는 일반에 많이 사용되었음을 볼 수 있다. 이를 통해 외식/주점에서는 재난지원금이 영세 및 중소에 큰 힘을 주었으나, 타 업종에서는 일반에 다소 많은 재난지원금이 사용되었음을 볼 수 있다.
  - 4) 재난지원금이 많이 사용된 업종(외식/주점, 마트/편의점)은 개수가 많기 때문에 상대적으로 사용될 확률이 높았다는 점을 발견했다.
  - 위를 바탕으로 재난지원금은 지점수가 많은 곳(외식/주점, 마트/편의점 등)에서 주로 사용되었으며, 일반에서 가장 많이 사용되었지만, 영세 및 중소 업종이 5월에는 소비진작 효과를 더 받았으며, 외식/주점에서는 영세 및 중소가 큰 혜택을, 나머지에서는 일반이 큰 혜택을 보았음을 확인하였다.

### Checkpoint 6. 공간정보 활용

### 4. Result
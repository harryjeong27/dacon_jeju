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
    - first : from 20/04/20 to 20/05, cash 550억원
    - second : from 20/08/24 to 20/10/11, cash 698억원
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

## 3.3 Analyze by pivoting features<a name="pivoting"></a>
### Dependant variable vs each Explanatory variable
- To confirm some of our observations and assumptions, we can quickly analyze our feature correlations by pivoting features against each other.
- We can only do so at this stage for features which do not have any empty values.
ex) It also makes sense doing so only for features which are categorical (human), ordinal (Q_Ques, TP_Ques, age_group) or discrete (familysize) type.

### Summary
- ex) The voted rate is 0.55. (24898 voted, 20634 not voted out of 45532).
- ex) education, age_group, married are strongly related to the voted rate. (classifying)
- ex) engnat, gender, hand, race, religion, urban, familysize are not clearly related to the voted rate. (completing for familysize, creating)

## 3.4 Analyze by visualizing data in 5 ways<a name="visual"></a>
>Confirming some of our assumptions using visualizations for analyzing the data.

### 3.4.0 Heatmap
>Check correlation btw human features
- ex) (+) Relation : education & married, voted & married, voted & education
- ex) (-) Relation : mach_score & married, mach_score & chin

### 3.4.1 Correlating based on feature types
ex)
- mach_score
   - 56점 이상 높아지면 점점 not voted가 많아짐 => classifying
- tp_score : 큰 의미 없어보임
- age_group + mach_score
   - 10s are the most, but most did not vote and they tend to have high mach_score
   - 40s, 50s, 60s mostly voted and they tend to have relatively low mach_score
   - It seems high mach_score provoke low vote rate and the low is opposite.
- Married might divide into two groups at [0.0, 1.0], [2.0, )
- Education must be an important feature and is already well-grouped.

## 3.5 Wrangle data<a name="wrangle"></a>
>We have collected several assumptions and decisions regarding our datasets and solution requirements. So far we did not have to change a single feature or value to arrive at these. Let us now execute our decisions and assumptions for correcting, creating, and completing goals.

### 3.5.1 Correcting by dropping features
This is a good starting goal to execute. By dropping features we are dealing with fewer data points. Speeds up our notebook and eases the analysis.
- Based on our assumptions and decisions we want to drop **'engnat, gender, hand, race, religion, urban, familysize'**  features.
- Based on our assumptions and decisions we also want to drop **'Q_Ques', 'Q_Time', 'W_Ques', **  features.

### 3.5.2 Creating new feature extracting from existing
- age, mach, married may be considered to create new feature by banding.

- mach_score
   - 56점 이상 높아지면 점점 not voted가 많아짐 => classifying
- tp_score : 큰 의미 없어보임
- age_group + mach_score
   - 10s are the most, but most did not vote and they tend to have high mach_score
   - 40s, 50s, 60s mostly voted and they tend to have relatively low mach_score
   - It seems high mach_score provoke low vote rate and the low is opposite.
- Married might divide into two groups at [0.0, 1.0], [2.0, )
- Education must be an important feature and is already well-grouped.

### 3.5.3 Completing a numerical continuous feature (NA)
Now we should start estimating and completing features with missing or null values. 
- familysize seems it has outlier -> but we already eliminated.
   
We can consider three methods to complete a numerical continuous feature.   
1. A simple way is to generate random numbers between mean and [standard deviation](https://en.wikipedia.org/wiki/Standard_deviation).
2. More accurate way of guessing missing values is to use other correlated features. In our case we note correlation among Age, Gender, and Pclass. Guess Age values using [median](https://en.wikipedia.org/wiki/Median) values for Age across sets of Pclass and Gender feature combinations. So, median Age for Pclass=1 and Gender=0, Pclass=1 and Gender=1, and so on...
3. Combine methods 1 and 2. So instead of guessing age values based on median, use random numbers between mean and standard deviation, based on sets of Pclass and Gender combinations.

### 3.5.4 Completing a categorical feature (NA)
- There is no NA.

### 3.5.5 Create new feature combining existing features
- We created mach_score & tp_score above.
- We created mach_age above.

### 3.5.6 Converting categorical feature to numeric
- age_group

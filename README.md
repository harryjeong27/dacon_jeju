< Proejct 2. Dacon_Jeju : Visualization >
=========================================
> dacon project of visualizing Jeju disaster spent during May - Aug


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
4. [Result](#result)
    4.1 [Summary]
    4.2 [Opinion]

# 1. Problem Definition & Domain Research<a name="ProblemDefinition&DomainResearch"></a>
## 1.1 Problem Definiton<a name="problemdefinition"></a>
### 1.1.1 Topic
- 공간 정보를 활용한 탐색적 데이터 분석 경진대회
### 1.1.2 Background
- 공간 정보를 활용하여 올 5~8월까지의 제주 지역 데이터를 분석하여 다양한 인사이트 발굴
### 1.1.3 Purpose
- 공간 정보를 활용한 탐색적 데이터 분석 및 시각화
- 공간 정보에 대한 일반인의 관심을 제고할 수 있는 인사이트 발굴  
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

# 2. Acquire training and testig data : Data Loading¶<a name="dataloading"></a>
## 2.1 Package Loading & Basic Setting<a name="package"></a>
    import os
    import warnings
    warnings.filterwarnings('ignore')
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
```
df.head()  
df.tail()
```
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
> We arrive at following assumptions based on data analysis done so far. We may validate these assumptions further before taking appropriate actions.

### 3.2.1 Correlating.
> Correlating. One can approach the problem based on available features within the training dataset. Which features within the dataset contribute significantly to our solution goal? Statistically speaking is there a correlation among a feature and solution goal? As the feature values change does the solution state change as well, and visa-versa? This can be tested both for numerical and categorical features in the given dataset. We may also want to determine correlation among features other than survival for subsequent goals and workflow stages. Correlating certain features may help in creating, completing, or correcting features.
- nothing in this project

### 3.2.2 Completing.
> Completing. Data preparation may also require us to estimate any missing values within a feature. Model algorithms may work best when there are no missing values.
- nothing in this project

### 3.2.3 Correcting.
> Correcting. We may also analyze the given training dataset for errors or possibly innacurate values within features and try to corrent these values or exclude the samples containing the errors. One way to do this is to detect any outliers among our samples or features. We may also completely discard a feature if it is not contribting to the analysis or may significantly skew the results.
- Let's drop 'sido'

### 3.2.4 Creating.
> Creating. Can we create new features based on an existing feature or a set of features, such that the new feature follows the correlation, conversion, completeness goals.
- We may create a feature called 'singlespent' by totalspent/numofspent.
- We may create a feature called 'singledis' by disspent/numofdisspent.
- We may create a feature called 'address' by using new data set. (http://data.nsdi.go.kr/dataset/15145)
- We may create a feature called 'category' by categorizing type feature.
- We may create a feature called 'time_cut' by categorizing time feature.

### 3.2.5 Classifying.
> Classifying. We may want to classify or categorize our samples. We may also want to understand the implications or correlation of different classes with our solution goal.
- nothing in this project

## 3.3 Analyze by pivoting features & Visualization<a name="pivoting"></a>
### 3.3.1 Macro 1 (key vs spent)
- key feature와 spent feature 간의 관계를 개괄적으로 살피면서 전반적인 특성을 파악한다.
  - spent = ['totalspent','disspent','numofspent','numofdisspent']  
  - key = ['sigungu', 'ym', 'type','time','franclass','dong','category', 'time_cut']  

#### 1) basic info
#### 2) totalspent
#### 3) numofspent
#### 4) disspent
#### 5) numofdisspent

### 3.3.2 Macro 2
- key vs times & spent feature 간의 관계를 개괄적으로 살피면서 특성을 파악한다.  
  - times = ['ym', 'time', 'time_cut']  
  - spent = ['totalspent','disspent','numofspent','numofdisspent', 'tot_dis', 'num_tot_dis']  
  - key = ['type', 'dong','category']  

#### 1) category vs times + spent
#### 2) type vs times + spent
#### 3) dong vs times + spent

### 3.3.3 Resolving assumptions & questions
- see deeply all the assumptions & questions we mentioned above and conclude.

### Checkpoint 1. 월별 재난지원금 사용비율
> 월별 추이를 통한 인사이트 도출
#### 1) 재난지원금 관련 비율
#### 2) 월별 재난지원금
#### 3) 2020년 제주도 내 인구현황 & 관광객현황에 따른 분석
#### 4) 정리 및 해석

### Checkpoint 2. 업종별 재난지원금 사용비율
> 업종별 추이를 통한 인사이트 도출
#### 1) 월별 업종별 총사용금액과 재난지원금 사용액 비교
#### 2) 재난지원금이 많이 사용된 업종의 세부업종 살펴보기
#### 3) 정리 및 해석

### Checkpoint 3. 지역별 재난지원금 사용비율
> 지역별 추이를 통한 인사이트 도출
#### 1) 재난지원금 사용을 많이 한 지역과 인구가 많은 지역
#### 2) 5월 지역별 총사용액과 재난지원금 사용액 그리고 8월 지역별 총사용액 비교
#### 3) 정리 및 해석

### Checkpoint 4. 업종규모별 재난지원금 사용비율
> 업종규모별 추이를 통한 인사이트 도출 (plotly 활용)
#### 1) 업종규모별 총사용액 및 재난지원금 사용액 비교
#### 2) 월별 업종규모별 총사용액 및 재난지원금 사용액 비교
#### 3) 업종규모별 업종 내 세부업종별 재난지원금 사용액 비교
#### 4) 업종규모별 업종 및 세부업종 분포    
#### 5) 정리 및 해석

### Checkpoint 5. 공간정보 활용
#### 1) 인구분포 Heatmap (202006 기준)
#### 2) 업종분포 Marker (202005 기준)
#### 3) 정리 및 해석

# 4. Result<a name="result"></a>
## 4.1 Summary<a name="summary"></a>
## 4.2 Opinion<a name="opinion"></a>
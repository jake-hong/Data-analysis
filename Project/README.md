# 넷플릭스 전세계 컨텐츠 분석(2021.04.03)

## 개요

#### 💡 주제 및 목표
  ##### 프로젝트 주제
  - **넷플릭스 콘텐츠 변화**를 주제로 1997년부터 2021년 까지 넷플릭스에 등록된 데이터를 활용하여 **넷플릭스의 콘텐츠 변화, 코로나 시대로 인한 콘텐츠 변화**분석하고 시각화
  - Kaggle에서 제공하는 Netflix Movies and TV Shows 데이터를 활용하여 분석하였습니다.
  - 🔗data source : [Kaggle Netflix Data](https://www.kaggle.com/shivamb/netflix-shows)

  ##### 프로젝트 목적
   1️⃣ 성장중인 넷플릭스 콘텐츠 확인
   - 넷플릭스는 계속 성장중인 회사로 성장할 수록 어떤 콘텐츠가 추가 되는지 형태를 파악하고자 한다.

   2️⃣ 코로나로 인한 콘텐츠 변화 확인
   - 코로나로 인해 콘텐츠 제작이 얼마나 변화하였는지 확인


## 💡데이터 처리 과정
![데이터 분석 과정](https://user-images.githubusercontent.com/68861542/114046075-80b01480-98c3-11eb-870b-08ce27b2fc59.png)
  #### 🏭환경설정
   - Python 3.6 이상의 버젼 설치가 필요하고 전체코드를 실행하려면 아래의 라이브러리 설치 필요
   - 해당 파일들은 Colab에서 작성되었습니다.
  
  ```
  pip install pandas
  pip install numpy
  pip install seaborn
  pip install matplotlib
  pip install missingno
  pip install warnings
  pip install folium
  pip install plotly
  pip install scikit-learn
  ```
  #### ✂️ 결측치 처리
  - 결측치가 가장 많은 director행과 사용하지 않는 3개의 열(show_id, cast, description) 열을 삭제 한 후 결측치가 존재하는 행을 전부 다 지워서 7265개 데이터가 남았다. 

<img width="40%" alt="결측치 제거전" src="https://user-images.githubusercontent.com/68861542/114172250-608a5f00-9970-11eb-9de8-78d16547656d.png"><img width="40%" alt="결측치 제거후" src="https://user-images.githubusercontent.com/68861542/114172265-62ecb900-9970-11eb-9c5d-c476aba271b5.png">

  #### ➡️ 결측치 변환
  - 날짜를 연,월,일로 나누어서 추가
   ```
   df['date_added'] = pd.to_datetime(df['date_added'])
   df['year']= df['date_added'].dt.year
   df['month']= df['date_added'].dt.month
   df['day']= df['date_added'].dt.day
   ```
   
   - 콘텐츠를 등급별로 분류
   ```
   ages = df['rating_age'].astype(int)
   bins = [0, 3, 11, 17, 19] # 4개 영역으로 카테고리화
   age_labels = ['All', 'Children' ,'Teens', 'Adults']
   rating_cat = pd.cut(ages, bins, labels = age_labels)
   ```
   
 
## 💡데이터 분석 과정
  #### 1. 전체 데이터 분석
   1). 넷플릭스에 TV Show와 Movie의 비율을 연도별로 나타낸 그래프
   - 전체적으로 Movie의 비율이 TV Show의 비율보다 높다.
    <img width="851" alt="연도별 유형변화" src="https://user-images.githubusercontent.com/68861542/114182694-e234b980-997d-11eb-995d-b3f3ba337e83.png">

   2). 나라별로 등록된 넷플릭스 콘텐츠 수
   - 압도적으로 미국에 등록된 콘텐츠 수가 많고 뒤이어 여러나라들이 비슷하게 콘텐츠가 등록 되어 있다. 
   - 한국도 콘텐츠 수로 상위권에 위치해 있다.
   ![20210409_215841](https://user-images.githubusercontent.com/68861542/114183633-d85f8600-997e-11eb-8721-040f940866e0.png)
   
   - 지도로 표시
   ![나라별 콘텐츠 수 지도](https://user-images.githubusercontent.com/68861542/114183677-e4e3de80-997e-11eb-8a6e-46dfd9448621.png)
   
   3). 요일별 넷플릭스 콘텐츠 출시 비율
   - 콘텐츠 등록은 금요일에 압도적으로 많으며 주말에 사람들의 관심을 끌 수 있도록 출시하는 것으로 예상된다
   <img width="1026" alt="요일별 콘텐츠 생산" src="https://user-images.githubusercontent.com/68861542/114184228-8e2ad480-997f-11eb-9448-4b548eb27dec.png">
   
   4). 연도별로 등록된 넷플릭스 콘텐츠 수
   - 2015년 부터 넷플릭스 콘텐츠 생산은 기하급수적으로 늘어나는 추세를 보인다.
   - 2019년에 최고치를 찍고 코로나 영향인지 약간 줄어들었음을 알 수 있다.
   <img width="1179" alt="연도별 컨텐츠 생산 변화" src="https://user-images.githubusercontent.com/68861542/114184310-a864b280-997f-11eb-9de7-1f9fbb3468b0.png">
   
   5). 등급별 콘텐츠 변화
   - 성인 콘텐츠와 청소년들의 콘텐츠가 압도적으로 많고 어린이들을 위한 콘텐츠는 많지가 않다.
   <img width="1094" alt="등급별 컨텐츠 변화" src="https://user-images.githubusercontent.com/68861542/114184841-f7aae300-997f-11eb-8e2a-477eb3cd42b9.png">
   
   6). 장르별 Movie 콘텐츠 분석
   - 드라마 형식의 독립영화와 세계영화 부분이 가장 진한것으로 보아 가장 많은 장르인 것을 알 수 있다.
   <img width="918" alt="히트맵" src="https://user-images.githubusercontent.com/68861542/114185120-51aba880-9980-11eb-85a4-a4390db08276.png">
   
   7) 장르별 TV Show 콘텐츠 분석
   - 다큐멘터리 시리즈 비율이 높은편으로 나타났고 공상과학과 판타지 콘텐츠를 다룬 액션과 모험 시리즈 비율이 높은것으로 알 수 있다.
   <img width="942" alt="히트맵1" src="https://user-images.githubusercontent.com/68861542/114185129-54a69900-9980-11eb-8eb7-f61fbffc86fc.png">
   
  #### 2. 한국 데이터 분석
   1). 한국와 한국을 제외한 모든 국가의 콘텐츠 비율 알아보기
   - 한국은 TV Show의 비율이 압도적으로 나타나지만 이외의 국가에서는 Movie의 비율이 대체적으로 크다.
   - 전 세계와 한국은 정 반대의 유향 분포를 보이고 있다.
   <img width="709" alt="한국과 외국 콘텐츠 비교" src="https://user-images.githubusercontent.com/68861542/114185644-eadabf00-9980-11eb-9a60-397ffcf9e80b.png">
   
   2). 한국과 이외국가 등록 연도별 컨텐츠 비율
   - 한국은 연도별로 콘텐츠 출시 비율이 크게 차이나지만 외국에는 대체적으로 증가하는 추세를 보이고 있다.
   <img width="40%" alt="한국콘텐츠" src="https://user-images.githubusercontent.com/68861542/114186009-50c74680-9981-11eb-91b5-e707679eec49.png"><img width="40%" alt="세계콘텐츠" src="https://user-images.githubusercontent.com/68861542/114186021-54f36400-9981-11eb-97b0-ea71bbb4d6fe.png">
  
   3). 한국의 장르별 콘텐츠 분석
   - 한국은 TV Show의 비율이 높은 만큼 한국에서 제작한 것들과 로맨틱 Tv show 비율이 높다.
   <img width="636" alt="한국 히트맵" src="https://user-images.githubusercontent.com/68861542/114186528-ed89e400-9981-11eb-80f1-3bd9a7a12b92.png">
   
  #### 3. COVID-19와의 관계
   1) 가설 1: 집에 있는 시간이 증가하면 지속성 있는 시리즈 물을 더 많이 보지 않을까?
    ![newplot (2)](https://user-images.githubusercontent.com/68861542/114187270-c253c480-9982-11eb-86dc-440edcdc22d5.png)
     
   2) 가설 2: 넷플릭스에 선정적인 콘텐츠가 많은데 COVID-19에도 유지가 될 수 있을까?
    <img width="40%" alt="코로나전후 콘텐츠비교" src="https://user-images.githubusercontent.com/68861542/114187452-fa5b0780-9982-11eb-9738-d00a3a66dca3.png"><img width="40%" alt="상위30개 콘텐츠 연도별" src="https://user-images.githubusercontent.com/68861542/114187552-18286c80-9983-11eb-8f03-c897f9c18eec.png">

   3) 가설 3: COVID-19 대유행으로 넷플릭스에 등록하는 콘텐츠가 줄지 않았을까?
    <img width="549" alt="2020년 콘텐츠 생산" src="https://user-images.githubusercontent.com/68861542/114187630-2d050000-9983-11eb-966c-c2962edf8fac.png">
    

## 💡결론 
  1. 결론 : TV Show의 비율이 소폭 상승한 것으로 볼 때 COVID 19의 영향이라고는 볼 수 없고 **2020년에 처음으로 콘텐츠 등록 변화가 일어난 점에 대해 주목할 것**
  2. 결론 : 성인 기준의 콘텐츠 비율이 유지될 것이라고 예상한 것과 달리 COVID-19로 인해 사람들이 집에서 보내는 시간이 증가함에 따라 가족 모두가 시청할 수 있는 **전체 관람가와 어린이들의 콘텐츠 비율이 소폭 상승**
  3. 결론 : 예상대로 **COVID 19의 대유행으로 제작을 미루어 콘텐츠 수가 감소**하였지만 사람들의 활동이 줄어들고 개인의 시간이 증가함에 따라 점차적으로 콘텐츠 등록 수를 회복 중임


## ⭐함께한 사람들
- 김도훈
- 박재용
- 이하경
- 최정근
- 홍성은

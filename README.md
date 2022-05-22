# 주제 : 따릉이 대여량에 영향을 미치는 요인 분석과 요인에 따른 대여량 예측


## 분석 배경 및 목적
<img src ="https://user-images.githubusercontent.com/105912035/169677587-86dcf3b8-d80f-44c4-9119-5b3093cbdeee.png" width="400" heigth="400"/> <img src ="https://user-images.githubusercontent.com/105912035/169677588-0e91b296-5dad-448a-ab6f-8677d0a9cbb7.png" width="400" heigth="400"/> 
<img src ="https://user-images.githubusercontent.com/105912035/169680407-0d42c94c-a957-4f02-bd7f-f8a5c485c3bb.png" width="800" heigth="400"/> <br>

코로나 19가 확산하면서, 서울시민들은 사람들이 많이 몰리는 버스나 지하철 이용을 피하기위해 비대면 교통수단인 따릉이를 즐겨 찾으면서 사용자가 급증한것으로 보인다.<br>   
따릉이 사용이 급증함에 따라서, 서울시도 자전거수와 대여소를 대폭 늘렸다고 한다. 하지만 실제로 서울과학기술대학교 근처에 있는 따릉이 대여소를 찾아가보면, 대여 가능한 따릉이가 없을때가 있었다.<br>
이와 같이 대여소와 자전거를 확충함에도 몇몇 대여소들은 자전거의 부족함을 확인할 수 있다. 그리하여 따릉이 대여량에 영향을 미치는 요인들이 무엇인지 궁금하게 되었다.<br>

<img src ="https://user-images.githubusercontent.com/105912035/169682242-4521b6f0-6355-4f21-a627-48f1091f4a17.png" width="600" heigth="600"/>
<기사출처> https://www.news1.kr/articles/?4579584 <br>
공공자전거 무인대여시스템은 2015년 서울시에서 시작되어 대전(타슈), 세종(뉴 어울링), 창원(누비자), 광주(타랑께)에 설치되면서, 점차 전국적으로 확대되고있다. 얼마전 위에 기사에 따르면 22년 초, 부산 기장군에서 처음으로 공공자전거 무인대여시스템을 도입했다. 이처럼 부산 이외의 다른 도시에도 공공자전거 무인대여시스템을 도입할 때, 우리가 파악한 요인들을 통해 대여량 예측을 하여 다른 도시에서 대여소를 설치할 때, 근거자료로 도움이 될 수 있을것같다고 생각하여 모델을 만들어보기로 하였다.  


## 데이터획득
#### 변수 설정
따릉이 대여소와 대여/반납<br>
(3개 feature : return_day, rent_day, rental_stop)<br>
<span style ='color:blue'>토지특성인 시설물 수</span><br>(12개 feature : large_store(대규모점포), company, hospital, restaurant, park, cafe, house, bank, car, school, bus_stop, subway_station)<br>
대중교통(버스, 지하철)이용에 따른 승객 승하차 수<Br>(4개 feature : bus_geton_people, bus_getoff_people, subway_geton_people, subway_getoff_people)<br>
지역특성인 하루 평균 연령대별/성별 생활인구<br>
(18개 feature : M_child, M_teenager, M_20, M_30, M_40, M_50, M_60, M_over70, W_child, W_teenager, W_20, W_30, W_40, W_50, W_60, W_over70, M_sum, W_sum)<br>
총 37개의 feature가 있다.

#### 데이터 획득 경로(독립변수)
지하철역 위치 : https://data.seoul.go.kr/dataList/OA-21232/S/1/datasetView.do  
지하철 승하차 인원 : https://data.seoul.go.kr/dataList/OA-21224/S/1/datasetView.do (21.7 데이터)  
버스정류장 위치 : https://data.seoul.go.kr/dataList/OA-15067/S/1/datasetView.do (22.3.29 데이터)  
버스 승하차 인원 : https://data.seoul.go.kr/dataList/OA-21225/S/1/datasetView.do (21.7 데이터)  
은행 : https://data.seoul.go.kr/dataList/10129/S/2/datasetView.do (21.7 데이터)  
회사 : https://data.seoul.go.kr/dataList/103/S/2/datasetView.do (19년 데이터)   
병원 : https://www.data.go.kr/data/15069540/fileData.do (19년 데이터)   
학교 : https://www.data.go.kr/data/15049381/fileData.do (21.9 데이터)  
자동차 등록현황 : https://data.seoul.go.kr/dataList/OA-21236/F/1/datasetView.do (21.12 데이터)  
연령대별 생활인구 : http://data.seoul.go.kr/dataList/OA-14991/S/1/datasetView.do (21.7 데이터)   
음식점 : https://data.seoul.go.kr/dataList/10154/S/2/datasetView.do (20년 데이터)   
카페 : https://www.data.go.kr/data/15083033/fileData.do (22.3 데이터)  
아파트 수(주택수) : https://data.seoul.go.kr/dataList/10585/S/2/datasetView.do (20년 데이터)  
대규모 점포수 :　https://data.seoul.go.kr/dataList/OA-16096/S/1/datasetView.do (22년 데이터)  
공원 존재여부 : https://data.seoul.go.kr/dataList/OA-394/S/1/datasetView.do (21.5 데이터)  
#### 데이터 획득 경로(종속변수)
따릉이 대여이력 : https://data.seoul.go.kr/dataList/OA-15182/F/1/datasetView.do# (21.7 데이터 사용)  
  
---> 여러 데이터를 모으다보니 데이터 시점이 맞지 않는 경우가 발생하는 한계가 있었다. 데이터의 시점을 최대한 21년 7월로 통일시켰다. 


#### 데이터 획득 경로(독립변수)  



## 분석 과정(EDA, 전처리, 모델링, 후처리, 검증 등)
### 1.EDA(Clustering, 시각화)

### 2.전처리

### 3.모델링

### 4.후처리

### 5.검증

## 기대 효과:
공공자전거가 없는 지역에 새로 설치할 때 동일한 feature들을 이용해 추후 공공자전거 추가 입지 선정에 있어 근거 자료로 활용될 것으로 기대됨.
일부 대여소에서 발생하는 공공자전거 공급부족과 과잉반납과 같은 문제에 대응해 더욱 효율적인 공공자전거 시설 공급에 기여할 것으로 기대된다.
-한계점 및 추후 개선 방안 등:동별로 묶어서 보는거보다, 대여소별로 예측을 할 수 있었으면 더 실용성있는 분석일것같다?

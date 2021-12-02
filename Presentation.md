# 최종 제출 모델

## Final_1
Public:  / Private: <br>

### 1차 전처리
<code>임대건물구분</code>
- 상가 데이터 제거

<code>환승역 수</code>
- 특정 단지코드에서 10분 거리에 지하철이 없는 지역의 <code>환승역 수</code>를 모두 0으로 대체

<code>전용면적</code>
<code>전용면적</code> 단위를 다음과 같이 축소
- 10~19 -> 15
- 20~29 -> 25
- ...

<code>공급유형</code>
dummy화한 뒤에 공공분양, 공공임대(5년), 장기전세는 0으로 대체


#### 파생변수 생성
- <code>지역x세대당인구</code>: <code>지역별 등록차량수</code> × <code>세대당 인구수</code> <br>
<code>등록차량수</code>를 지역별로 평균을 낸 값에 단지코드별 정보를 더해주기 위해 <code>세대당_인구</code>를 곱해서 생성

- <code>공가비율</code>: <code>공가수</code>/<code>총세대수</code>

- <code>연령별 인구수</code>: <code>연령별_비율</code> × <code>전용면적별세대수</code>

### 2차 전처리(1차원 병합)
- 최종 예측값은 단지코드 기준이지만 단지코드별로 unique하지 않은 column이 존재하기 때문에 병합이 필요함 
- 전용면적별 임대료, 임대보증금의 차이가 크기 때문에 단지코드 기준으로 평균을 내는 것은 정보손실 우려가 있음

∴ 단지코드, 전용면적, 공급유형을 기준으로 평균을 내서 병합 <br>

### 3차 전처리
#### 파생변수 생성
- <code>y1</code>: <code>등록차량수</code> × <code>ratio</code>
- <code>단지내주차면수_new</code>: <code>단지내주차면수</code> × <code>ratio</code>
 <code>단지내주차면수</code>와 <code>등록차량수</code>간의 correlation이 높기 때문에 단지코드별 전용면적세대수의 총합 대비 전용면적세대수 비율을 곱해 생성

- <code>y2</code>: <code>y1</code>/<code>단지내주차면수_new</code>
최종 target

#### 이상치 제거
<code>y2</code> 기준 이상치로 의심되는 단지코드 'C1722' 제거

### 모델링
Step 1. <code>y2</code>를 target으로 두고 학습 및 예측
Step 2. 예측한 <code>y2</code>에 <code>단지내주차면수_new</code>를 다시 곱해 <code>y1</code> 예측값을 계산
Step 3. 단지코드별 예측을 위해 <code>y1</code> 예측값을 단지코드별로 합하고, 실제 <code>y1</code> 역시 동일한 과정을 거친 뒤에 MAE 계산

### 사용한 변수
- <code>전용면적</code>&nbsp;&nbsp;&nbsp;<code>전용면적별세대수</code>&nbsp;&nbsp;&nbsp;<code>총세대수</code>&nbsp;&nbsp;&nbsp;<code>공가수</code>&nbsp;&nbsp;&nbsp;

- <code>지하철역</code>&nbsp;&nbsp;&nbsp;<code>버스정류장</code>&nbsp;&nbsp;&nbsp;<code>위도</code>&nbsp;&nbsp;&nbsp;<code>경도</code>&nbsp;&nbsp;&nbsp;<code>subway_dist</code>&nbsp;&nbsp;&nbsp;<code>환승역 수</code>&nbsp;&nbsp;&nbsp;

- <code>총인구수</code>&nbsp;&nbsp;&nbsp;<code>세대당_인구</code>&nbsp;&nbsp;&nbsp;<code>남/여비율</code>&nbsp;&nbsp;&nbsp;<code>남/여_0-19세</code>&nbsp;&nbsp;&nbsp;<code>남/여_20-39세</code>&nbsp;&nbsp;&nbsp;<code>남/여_40-69세</code>&nbsp;&nbsp;&nbsp;<code>남/여_70세이상</code>&nbsp;&nbsp;&nbsp;
- <code>지역별 등록차량수</code>&nbsp;&nbsp;&nbsp;<code>지역x세대당인구</code>&nbsp;&nbsp;&nbsp;<code>공가비율</code>&nbsp;&nbsp;&nbsp;
- <code>0-19 인구수</code>&nbsp;&nbsp;&nbsp;<code>20-39 인구수</code>&nbsp;&nbsp;&nbsp;<code>40-69 인구수</code>&nbsp;&nbsp;&nbsp;<code>70세이상 인구수</code>&nbsp;&nbsp;&nbsp;
- <code>공공분양</code>&nbsp;&nbsp;&nbsp;<code>공공임대(10년)</code>&nbsp;&nbsp;&nbsp;<code>공공임대(50년)</code>&nbsp;&nbsp;&nbsp;<code>공공임대(5년)</code>&nbsp;&nbsp;&nbsp;<code>공공임대(분납)</code>&nbsp;&nbsp;&nbsp;<code>국민임대</code>&nbsp;&nbsp;&nbsp;<code>영구임대</code>&nbsp;&nbsp;&nbsp;<code>장기전세</code>&nbsp;&nbsp;&nbsp;<code>행복주택</code>&nbsp;&nbsp;&nbsp;
- <code>임대료</code>&nbsp;&nbsp;&nbsp;<code>임대보증금</code>

### 후처리
주최 측 오류였던 3개 단지코드를 제외하기 위해 0으로 대체

---

## Final_2
Public: 109 / Private: 110 <br>


---

## Addition_1
Public: / Private: <br>

<code></code>

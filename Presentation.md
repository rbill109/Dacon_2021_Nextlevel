# Final.ipynb
Public:  109 / Private: 110<br>

---

### 1차 전처리
#### 변수별 전처리
`임대건물구분` <br>
- 상가 데이터는 제외

`전용면적` <br>
단위를 다음과 같이 통일
- 10~19 -> 10
- 20~29 -> 20
- ...

#### 결측치 처리
`임대료`, `임대보증금` <br>
- NA는 0으로 대체

#### 파생변수 생성
`공가비율`: `공가수`/`총세대수` <br>

`연령별_비율` <br>
- age_gender_info 데이터를 사용해 여자와 남자의 연령비율을 합해 생성

---

### 2차 전처리(1차원 병합)
- 최종 예측값은 단지코드 기준이지만 단지코드별로 unique하지 않은 변수(`자격유형`, `임대보증금`, `임대료`, `전용면적별세대수`, `전용면적`, `공급유형`)가 존재하기 때문에 병합이 필요함 
- 전용면적별로 `임대료`, `임대보증금`의 차이가 크기 때문에 단지코드 기준으로 평균을 내는 것은 정보손실 우려가 있음

∴ `단지코드`, `전용면적`, `공급유형`을 기준으로 `전용면적별세대수`는 합하고, `임대료`, `임대보증금`는 평균을 내서 병합 
<br>
-> `임대료`, `임대보증금`에서 NA 발생 시 `지역`, `전용면적`, `공급유형`을 기준으로 평균을 낸 값으로 대체

--- 

### 3차 전처리
#### 변수별 전처리
`공급유형`, `전용면적` <br>
- dummy화한 뒤에 공공분양, 공공임대(5년), 장기전세는 0으로 대체


#### 파생변수 생성
- `전용면적별세대수_비율`: `전용면적별세대수`/`총세대수` <br>
아파트 총세대수와 해당 면적에 속한 세대수 비율

- **`면적별_등록차량수`**(target): `등록차량수` × `ratio`, 
`면적별_단지내주차면수`: `단지내주차면수` × `ratio` <br>
`단지내주차면수`와 `등록차량수`간의 correlation이 높기 때문에 `전용면적별세대수_비율`을 곱해 생성 <br>
단지코드별로 값이 동일한 `등록차량수`만으로는 전용면적별세대수에 따른 임대료, 임대보증금 차이 같은 정보를 반영해주지 못하므로 `전용면적별세대수_비율`을 곱해 이를 반영

- `연령별 인구수`: `연령별_비율` × `전용면적별세대수_비율` <br>
특정 단지코드의 전용면적별 인구분포를 나타냄 <br>
기존 `연령별_비율`은 제거

#### 이상치 처리
- `y2` 기준 이상치로 의심되는 단지코드 'C1722' 제거

---

### 모델링


### 사용한 변수
- `면적별_단지내주차면수`&nbsp;&nbsp;&nbsp;`공가비율`&nbsp;&nbsp;&nbsp;`총세대수`&nbsp;&nbsp;&nbsp;`임대료`&nbsp;&nbsp;&nbsp;`임대보증금`&nbsp;&nbsp;&nbsp;
- `공공분양`&nbsp;&nbsp;&nbsp;`공공임대(10년)`&nbsp;&nbsp;&nbsp;`공공임대(50년)`&nbsp;&nbsp;&nbsp;`공공임대(분납)`&nbsp;&nbsp;&nbsp;`국민임대`&nbsp;&nbsp;&nbsp;`영구임대`&nbsp;&nbsp;&nbsp;`장기전세`&nbsp;&nbsp;&nbsp;`행복주택`&nbsp;&nbsp;&nbsp;
- `면적_10`&nbsp;&nbsp;&nbsp;`면적_20`&nbsp;&nbsp;&nbsp;`면적_30`&nbsp;&nbsp;&nbsp;`면적_40`&nbsp;&nbsp;&nbsp;`면적_50`&nbsp;&nbsp;&nbsp;`면적_60`&nbsp;&nbsp;&nbsp;`면적_70`&nbsp;&nbsp;&nbsp;`면적_80`&nbsp;&nbsp;&nbsp;
- `0~19세_인구수`&nbsp;&nbsp;&nbsp;`20~39세_인구수`&nbsp;&nbsp;&nbsp;`40~69세_인구수`&nbsp;&nbsp;&nbsp;`70세이상_인구수`

### 후처리
주최 측 오류였던 3개 단지코드를 제외하기 위해 0으로 대체

---

# modeling1.ipynb
Public: / Private: <br>

**※ Final과 동일하되 추가한 사항만 기재**
### 외부 데이터
- `위도`, `경도`, `subway_dist`, `환승역 수` <br>
수집 및 전처리 과정은 [Here](https://github.com/rbill109/Dacon_2021_Nextlevel/tree/master/source_code/ProcessedData/External3)을 참고

- `세대당_인구`, `남/여비율`, `남/여_0~19세`, `남/여_20~39세`, `남/여_40~69세`, `남/여_70세이상` <br>
수집 및 전처리 과정은 [Here](https://github.com/rbill109/Dacon_2021_Nextlevel/tree/master/source_code/ProcessedData/External2)을 참고

### 1차 전처리
#### 변수별 전처리
`환승역 수` <br>
- 특정 단지코드에서 10분 거리에 지하철이 없는 지역의 `환승역 수`를 모두 0으로 대체

`전용면적` 단위를 다음과 같이 통일
- 10~19 -> 15
- 20~29 -> 25

#### 파생변수 생성
`지역x세대당인구`: `지역별 등록차량수` × `세대당_인구수` <br>
- `등록차량수`를 지역별로 평균을 낸 값에 단지코드별 정보를 더해주기 위해 `세대당_인구`를 곱해서 생성

---

### 3차 전처리
#### 변수별 전처리
 `전용면적` <br>
dummy화 X

### 사용한 변수
- `총세대수`&nbsp;&nbsp;&nbsp;`공가수`&nbsp;&nbsp;&nbsp;`공가비율`&nbsp;&nbsp;&nbsp;`임대료`&nbsp;&nbsp;&nbsp;`임대보증금`
- `지하철역`&nbsp;&nbsp;&nbsp;`버스정류장`&nbsp;&nbsp;&nbsp;`위도`&nbsp;&nbsp;&nbsp;`경도`&nbsp;&nbsp;&nbsp;`subway_dist`&nbsp;&nbsp;&nbsp;`환승역 수`&nbsp;&nbsp;&nbsp;
- `총인구수`&nbsp;&nbsp;&nbsp;`세대당_인구`&nbsp;&nbsp;&nbsp;`남/여비율`&nbsp;&nbsp;&nbsp;`남/여_0~19세`&nbsp;&nbsp;&nbsp;`남/여_20~39세`&nbsp;&nbsp;&nbsp;`남/여_40~69세`&nbsp;&nbsp;&nbsp;`남/여_70세이상`&nbsp;&nbsp;&nbsp;`지역별 등록차량수`&nbsp;&nbsp;&nbsp;`0~19 인구수`&nbsp;&nbsp;&nbsp;`20~39 인구수`&nbsp;&nbsp;&nbsp;`40~69 인구수`&nbsp;&nbsp;&nbsp;`70세이상 인구수`&nbsp;&nbsp;&nbsp;
- `공공분양`&nbsp;&nbsp;&nbsp;`공공임대(10년)`&nbsp;&nbsp;&nbsp;`공공임대(50년)`&nbsp;&nbsp;&nbsp;`공공임대(5년)`&nbsp;&nbsp;&nbsp;`공공임대(분납)`&nbsp;&nbsp;&nbsp;`국민임대`&nbsp;&nbsp;&nbsp;`영구임대`&nbsp;&nbsp;&nbsp;`장기전세`&nbsp;&nbsp;&nbsp;`행복주택`&nbsp;&nbsp;&nbsp;
- `전용면적`&nbsp;&nbsp;&nbsp;`전용면적별세대수`&nbsp;&nbsp;&nbsp;
- `지역x세대당인구`&nbsp;&nbsp;&nbsp;





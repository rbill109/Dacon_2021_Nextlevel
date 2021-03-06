# 아파트 단지 내 적정 주차수요 예측 프로젝트
[데이콘](https://dacon.io/competitions/official/235745/overview/description) 주차수요 예측 AI 경진대회 출품작

## Goal 
#### **정형 데이터를 활용한 아파트 단지 내 주차수요 예측** <br>
법정주차대수와 장래주차수요* 중 큰 값에 따라 결정되는 적정주차대수를 예측하기 위한 모델 제안 <br>
\* <code>'주차원단위'와 '건축연면적'을 기초로 산출됨</code>


### Overview
한국주택토지공사 빅데이터센터의 임대주택 관련 데이터를 바탕으로 임대주택 단지 내 적정 주차수요를 예측 <br>

**[Problem]** <br>
신규 주택부지 인근의 유사 단지를 피크 시간대에 방문해 주차된 차량대수를 세는 방법으로 산정되는 '주차원단위'로 인해 **과대/과소 산정**이 우려되는 장래주차수요 산정 방식을 개선해야 함

**[Solution]** <br>
임대주택과 거주 관련 데이터를 활용해 RandomForest와 Catboost 기반 예측 모델 개발

**[Key point]** <br>
- 정보 손실을 막기 위해 target 변수를 재구조화

- - -

### Timeline
2021.06 ~ 2021.07

### Team
Next Level / 유은영, 남승지, 조유림, 조유민

### Role
데이터 전처리 및 교차 검증

### Presentation
제출 모델에 대한 설명은 [이곳](https://github.com/rbill109/Dacon_2021_Nextlevel/blob/master/Presentation.md)을 참고

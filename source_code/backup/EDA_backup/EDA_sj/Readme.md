0717
유민이와 카페스페이스에서 만났다. 


#### 1. 데이터를 나눠서 모형 fitting 기준 찾기 
  1) 단지내주차면수
  2) 단지내주차면수/총세대수
- 방법 (예.단지내주차면수): 단지내주차면수 기준으로 sorting 후 특정 단지내주차면수를 기준으로 2개로 split 했을때, 첫번째 그룹 등록차량수의 분산 * 첫번째그룹 length + 두번째 그룹 등록차량수의 분산 * 두번째 그룹 length 를 계산함. 이 값이 가장 작아지는 단지내주차면수로 데이터 split. 
- 단지내주차면수 기준으로 split 시 7:3 (0.68:0.32)로 나뉘고, 단지내주차면수/총세대수 기준으로 split 시 약 1:1로 나뉜다. 


#### 2. 단지명에 휴먼시아가 들어간 단지코드, 단지명에 주공이 들어간 단지코드 비교

- 휴먼시아는 별 차이가 없어서 필요없다고 판단. 
- 주공 들어간지 유무는 애매하다. 


#### 3. 유민이의 세팅과 은영이의 세팅이 달라 비교가 어렵다.. 


#### 4. 단지내주차면수 혹은 세대당 단지내주차면수가 큰 경우, 
특히 데이터를 나눴을 때 주차면수가 큰 쪽은 아무래도 등록차량수가 크다. 등록차량수가 작은쪽은 촘촘, 큰쪽은 간격이 커서 주차면수가 큰쪽의 mae가 클수밖에 없을 수도 있다.. 등록차량수에 log 취해주는 것이 대안이 될 수 있을까? skew 를 보정하는 것이 의미가 있을까?

#### 5. modeling 2 : 119/56/113(제출)
상가 임대료/임대보증금: quantile 유림이꺼 -> 세대별주차면수로 나눠서 똑같이 캣부로 하면 117.81

#### 6. 세대별주차면수 0.83 기준으로 나눠서 modeling
유림이 modeling2 방법 그대로에서 
+ 세대별주차면수 기준으로 나눴을 때... 
- 등록차량수에 log를 취하니 유민이 best model 코드에서 mae가 모두 0.25, 0.21가 나오고 GB+CB 조합이 나와서 -> 제출 123
- 


유림이 modeling2 방법 그대로에서 
- 둘다 회귀 + quantile 25%,75% 뺌 + exp() -> 제출:134
- 둘다 gb+ quantile 25%,75% 뺌 + exp() -> 제출:119 (이때 유민이 best model 코드에서는, GB + CB 조합 말함)

데이터가공방식은 옛날승지방식 (파생변수추가가 안된 방식)
- group1: rf(튜닝함), group2: lm  -> 제출: 123


# KOELECTRA를 활용한 스팀 게임 리뷰 감성 분석

<div> <img src = "https://github.com/yeon0306/steam_game/assets/112537146/9515bfdc-4a13-443a-95bb-880740466568" width="800"></div>


# 1. 개요
이번 프로젝트에서는 한국어 자연어 처리 모델인 KOELECTRA를 활용하여 스팀 게임 리뷰의 긍부정 예측을 하고자 한다.

## 1.1 문제 정의
[스팀](https://store.steampowered.com/)은 밸브 코퍼레이션에서 개발한 디지털 관리 멀티플레이어 플랫폼이다. 많은 게임 개발사와 출판사가 스팀에서 출시하고 판매하고 있으며 서비스는 2003년 9월 12일 시작되어 현재에도 서비스 되고 있다. 2억 명의 사람들이 가입하여 동시접속자는 약 3000만으로, 다양한 게임을 디지털로 관리하며 배급하는 아주 큰 규모의 게임 플랫폼으로 알려져있다. </br>
<div> <img src = "https://github.com/yeon0306/steam_game/assets/112537146/18cd9783-93d4-400e-9e41-b45f7a6f0823" width="800"></div>

게임 리뷰는 소비자들에게 게임의 품질과 가치를 알려주는 역할을 한다.[[1]](https://www.kci.go.kr/kciportal/ci/sereArticleSearch/ciSereArtiView.kci?sereArticleSearchBean.artiId=ART002231755)좋은 리뷰를 받은 게임은 높은 판매량과 수익을 기대할 수 있으며 리뷰 점수가 낮은 게임은 판매에 영향을 미칠 수 있다.[[2]](http://www.kodia.or.kr/upload/4/e69e99ca180c709fa8f29b190a140030.pdf) 또한 게임 리뷰를 통해 플레이어들은 게임 산업의 경향과 혁신을 파악할 수 있고 게임 개발사들은 경쟁적인 환경에서 품질을 향상시키고 새로운 아이디어를 도입할 동기를 얻을 수 있다.[[3]](https://s-space.snu.ac.kr/handle/10371/166332) </br>
이 프로젝트에서는 많은 수의 게임 소비자들의 리뷰 데이터를 활용하여 소비자가 게임 리뷰를 유용하다고 판단하는데 영향을 미치는 요인을 파악하고 게임 장르, 평점 등 다양한 특징에 따라 긍정 또는 부정을 예측하는 인공지능 모델을 개발하고자 한다.

## 1.2 데이터 및 모델 개요

데이터는 스팀게임 리뷰 데이터셋을 활용하여 총 10만 건의 데이터에 대해서 사전 학습 언어 모델의 재학습(fine-tuning)을 수행한다.

| 입력 | 모델 |출력|
|----------|---|---|
| 스팀 게임 리뷰 문장 |[KOELECTRA small](https://github.com/monologg/KoELECTRA)|부정(0), 긍정(1)|

스팀 게임 리뷰 문장은 전처리를 통해서 가공한다. 대표적인 전처리로는 결측치와 영어, 중복값을 제거하고, 띄어쓰기로 구분이 되지 않는 리뷰 역시 제거한다.

## 2. 데이터

## 2.1 데이터 소스

[스팀게임 리뷰 데이터셋](https://github.com/bab2min/corpus/tree/master/sentiment)

## 2.2 탐색적 데이터 분석

| | review |label|
|-|----------|---|
|0|돌겠네 진짜. 황숙아, 어크 공장 그만 돌려라. 죽는다.|0|
|1|막노동 체험판 막노동 하는사람인데 장비를 내가 사야돼 뭐지|1|
|2|차악!차악!!차악!!! 정말 이래서 왕국을 되찾을 수 있는거야??|1|
|3|시간 때우기에 좋음.. 도전과제는 50시간이면 다 깰 수 있어요|1|
|..|...|...|...|
|99999|역시 재미있네요 전작에서 할수 없었던 자유로운 덱 빌딩도 좋네요^^	|1|
|100000|은근 쉽지만 은근 어려운 게임|1|
|100001|베ㅈ스다 이 개^ㅐ끼들아. 시작할 때 체스판 돌아가는거 5분동안 3번 봤더나 ㅈㄴ 빡치네 진짜 무한로딩 버그 안쳐고치냐 겜하지말라는거냐|0|

100000개의 리뷰가 있으며 부정은 0, 긍정은 1로 구성되어있다. 


## 2.3 데이터 전처리
- 입력 데이터의 전처리 과정
- 학습에 활용할 데이터의 양
- 학습과 검증 데이터셋 분리
- 학습 데이터의 구성

# 3. 재학습 결과
## 3.1 개발 환경
- pycharm, python, torch, pandas, ...
-
## 3.2 KOELECTRA fine-tuning
## 3.3 학습 결과 그래프

# 4. 배운점

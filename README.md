![header](https://capsule-render.vercel.app/api?type=soft&color=auto&height=300&section=header&text=Steam%20Review&fontSize=90)<br/>

# KOELECTRA를 활용한 스팀 게임 리뷰 감성 분석
한국어 자연어 처리 모델인 KOELECTRA를 활용하여 스팀 게임 리뷰의 긍부정 예측

## 1. 개요

### 1.1 문제 정의
[스팀](https://store.steampowered.com/)은 밸브 코퍼레이션에서 개발한 디지털 관리 멀티플레이어 플랫폼이다. 많은 게임 개발사와 출판사가 스팀에서 출시하고 판매하고 있으며 서비스는 2003년 9월 12일 시작되어 현재에도 서비스 되고 있다. 2억 명의 사람들이 가입하여 동시접속자는 약 3000만으로, 다양한 게임을 디지털로 관리하며 배급하는 아주 큰 규모의 게임 플랫폼으로 알려져있다. </br>

<div> <img src = "https://github.com/yeon0306/steam_game/assets/112537146/18cd9783-93d4-400e-9e41-b45f7a6f0823" width="800"></div>

게임 리뷰는 소비자들에게 게임의 품질과 가치를 알려주는 역할을 한다.[[1]](https://www.kci.go.kr/kciportal/ci/sereArticleSearch/ciSereArtiView.kci?sereArticleSearchBean.artiId=ART002231755)좋은 리뷰를 받은 게임은 높은 판매량과 수익을 기대할 수 있으며 리뷰 점수가 낮은 게임은 판매에 영향을 미칠 수 있다.[[2]](http://www.kodia.or.kr/upload/4/e69e99ca180c709fa8f29b190a140030.pdf) 또한 게임 리뷰를 통해 플레이어들은 게임 산업의 경향과 혁신을 파악할 수 있고 게임 개발사들은 경쟁적인 환경에서 품질을 향상시키고 새로운 아이디어를 도입할 동기를 얻을 수 있다.[[3]](https://s-space.snu.ac.kr/handle/10371/166332)
이 프로젝트에서는 많은 수의 게임 소비자들의 리뷰 데이터를 활용하여 소비자가 게임이 유용하다고 판단하는데 영향을 미치는 요인을 파악하고 게임 장르, 평점 등 다양한 특징에 따라 긍정 또는 부정을 예측하는 인공지능 모델을 개발하고자 한다.

### 1.2 데이터 및 모델 개요

데이터는 bab2min의 Github - corpus 에서 제공하는 [스팀게임 리뷰 데이터셋](https://github.com/bab2min/corpus/tree/master/sentiment)을 활용하여 총 10만 건의 데이터에 대해서 사전 학습 언어 모델의 재학습(fine-tuning)을 수행한다.


| 입력 | 모델 |출력|
|----------|---|---|
| 스팀 게임 리뷰 문장 | KoELECTRA-Small-v3 <sup>[[4]](https://huggingface.co/monologg/koelectra-small-discriminator)</sup> |부정(0), 긍정(1)|

스팀게임 리뷰 데이터셋의 수집 기간은 2020년 5월부터 2020년 06월까지 수집되었으며 Steam의 각종 게임에 달린 한국어 리뷰를 수집되었다.데이터는 탭으로 분리 되어있으며 긍정과 부정의 비율이 1:1에 가깝도록 수집되어있다는 것이 특징이다.
KoELECTRA는 대규모 한국어 텍스트 데이터 말뭉치를 훈련하여 텍스트 분류, 명명 개체 인식, 감성 분석 등 다양한 자연어 처리 작업에서 최고 수준의 성능을 달성한 한국어 사전 학습 언어 모델이다. 이 프로젝트에서는 KoELECTRA-Small-v3 Discriminator의 모델을 사용했다. 

## 2. 데이터

## 2.1 탐색적 데이터 분석

| | review |label|
|-|----------|---|
|0|노래가 너무 적음|0|
|1|돌겠네 진짜. 황숙아, 어크 공장 그만 돌려라. 죽는다.|0|
|2|막노동 체험판 막노동 하는사람인데 장비를 내가 사야돼 뭐지|1|
|3|차악!차악!!차악!!! 정말 이래서 왕국을 되찾을 수 있는거야??|1|
|..|...|...|...|
|99997|노잼이네요... 30분하고 지웠어요...|0|
|99998|야생을 사랑하는 사람들을 위한 짧지만 여운이 남는 이야기. 영어는 그리 어렵지 않습니다.|1|
|99999|한국의 메탈레이지를 떠오르게한다 진짜 손맛으로 하는게임|1|

100000개의 리뷰가 있으며 부정은 0, 긍정은 1로 구성되어있다.

|-|건수|
|-|----|
|긍정|49,996|
|부정|50,004|
|계|100,000|

긍정과 부정의 비율이 1:1에 가깝도록 구성되어있으며, 부정적인 데이터가 조금 더 많다. 

## 2.2 데이터 전처리

데이터의 결측치는 확인해본 결과, 없는 것으로 나타났다.

![결측치](https://github.com/yeon0306/steam_game/assets/112537146/e306de7c-3cb0-4de4-aada-41a0547b1b67)

- 리뷰 문장 25자 이상 길이의 분포도

![25자 이상분포표](https://github.com/yeon0306/steam_game/assets/112537146/f7dfccbc-a867-47cc-b399-bd489f8f71d3)

약 30자 이상 ~ 40자 이하의 리뷰 데이터가 가장 많은 것을 알 수 있다. 
리뷰 데이터가 25자 이하의 짧은 리뷰는 매우 제한적이며 리뷰의 정보가 부족하다고 판단하여 25자 이하의 데이터 36317건을 삭제하였다.


- 최종 데이터셋

| | review |label|
|-|----------|---|
|0|돌겠네 진짜. 황숙아, 어크 공장 그만 돌려라. 죽는다.|0|
|1|막노동 체험판 막노동 하는사람인데 장비를 내가 사야돼 뭐지|0|
|2|차악!차악!!차악!!! 정말 이래서 왕국을 되찾을 수 있는거야??|1|
|3|시간 때우기에 좋음.. 도전과제는 50시간이면 다 깰 수 있어요|1|
|..|...|...|...|
|63680|엔딩도 이해가고 림보라는 제목을 정말 잘 지었어 근데 왜 도대체 이런 게임을 인디 ...|0|
|63681|야생을 사랑하는 사람들을 위한 짧지만 여운이 남는 이야기. 영어는 그리 어렵지 않습니다.|1|
|63682|한국의 메탈레이지를 떠오르게한다 진짜 손맛으로 하는게임|1|

100,000 건이었던 데이터에서 전처리를 마친 후 최종 데이터셋은 63,683건이 되었다.


- 긍정(1)/부정(0) 파이차트 

<div><img src="https://github.com/yeon0306/steam_game/assets/112537146/a59e1266-cb99-4fa8-90d7-682a5d568faf" width="500"></div>

데이터 전처리 후에도 긍/부정 데이터가 1:1 비율로 나누어져있으며 부정적인 데이터가 32037건, 긍정적인 데이터가 31646건으로 부정적인 데이터가 조금 더 많은 것을 알 수 있다.

- 학습과 검증 데이터셋 분리

![데이터구조](https://github.com/yeon0306/steam_game/assets/112537146/2fdefda9-b41d-4a51-bfaa-dd0dbe26600a)

긍정적인 리뷰 데이터 2000개와 부정적인 리뷰 데이터 2000개를 랜덤으로 추출하여 총 4000개로 만들어졌다.

![학습데이터수](https://github.com/yeon0306/steam_game/assets/112537146/d063f8b6-1a2f-4b5b-ba76-f1874abd9fa0)

학습 데이터 3200개, 검증 데이터 800개로 분리하였다. 
  
- 랜덤으로 추출한 학습 데이터의 구성

| | review |label|
|-|----------|---|
|0|방치형 게임인데 최상의 생산구조를 구축하면됩니다 잊고살다가 몇달뒤에 다시 들어오시면 됩니다|1|
|1|(+) 아름다운 배경 지루하지 않은 시스템 (-) 스토리 많이 빈약함|1|
|2|적당한 플레이타임 나름 괜찮다. 세일할때 사자|1|
|..|...|...|...|
|3999|이 게임 개꿀잼이였는데 제작진 내부갈등으로 공중분해되고 출시 일주일만에 망함|0|
|4000|프레임 드랍이 너무 심해서 도저히 게임을 진행할 수가 없습니다. 그리고 역시나 우려했던 서버 렉도...|0|
|4001|언제나 최고의 돈독오른 게임. 섬란 카구라가 세일해서 2만원인대인데 그넘의...|0|

랜덤으로 추출한 학습 데이터의 결과는 optimizer를 여러번 수정하여 돌려보았지만 정확도가 **0.4 에서 0.74** 로 매우 낮은 정확도로 나타났다. 
학습 데이터의 정확도가 낮은 이유를 찾아본 결과 **리뷰 데이터의 오류** 일 가능성이 높다고 판단하였다.  

- 데이터의 오류 예시 

| | review |label|
|-|----------|---|
|0|주의하실 점. 블랙 플래그에서 DLC로 프리덤 크라이를 사셧다면, 굳이 이 게임을 구매하실 필요는 없습니다.|1|
|1|HOMM시리즈와 차이가 있긴 한데 그게 유의미하게 느껴지진 않았다.|1|
|2|한글 자막이나 관련된 자료 같은게 있을까요...?|1|
|..|...|...|...|
|3999|처음 평점 남깁니다. 아름답고 슬프고 소름돋는 최고의 게임. 어떤 미사여구가 더 필요할까요?|0|
|4000|7000시간밖에 안해서 잘 모르겠지만 정말 갓겜이네요^^|0|
|4001|한국어가 나왔으면 좋겠다. 이거 나름 한국 팬도 있는데 말이다.ㅠㅠ|0|

랜덤으로 추출한 학습 데이터의 애매한 리뷰 데이터들이다. 긍정인 리뷰는 부정으로 보이기도 하며 부정인 리뷰는 긍정으로 보이기도 한다.
또한 게임 리뷰와 관련없는 내용들이 있는 데이터들이 매우 많아 모델이 제대로 학습할 수 없는 것을 알 수 있다. 

- 임의로 추출한 학습 데이터의 구성 

| | review |label|
|-|----------|---|
|0|시간 때우기에 좋음.. 도전과제는 50시간이면 다 깰 수 있어요 |1|
|1|역시 재미있네요 전작에서 할수 없었던 자유로운 덱 빌딩도 좋네요^^|1|
|2|포켓볼 1도 몰랐는데, 이걸로 배워 갑니다. 심심할때 하면 좋아요. 컴퓨터 상대하는거 제대로 이겨보고 싶은데 잘 안되네요.|1|
|..|...|...|
|1000|림월드 생각하고 구입할생각이라면 하지마시길... 물론 난 환불~|0|
|1001|사람들의 좋은 평에 이끌려서 샀으나 결과는? 좆노잼 미친노잼|0|
|1001|어떤 미친 러시아인이 지랄해서 죽이는데 관리자네 좆같은게임 왜하냐|0|

리뷰를 읽었을 때 긍정인지 부정인지 판단할 수 없는 리뷰들은 모두 제외하고 사람이 읽었을 때 긍부정인지 판단할 수 있는 정도의 리뷰 데이터만
긍정적인 리뷰 500개, 부정적인 리뷰 500개으로 총 1000개의 데이터셋을 만들었다. 학습 데이터는 800개, 검증 데이터는 200개로 분리하였다. 

<table>
      <tr><th align="center" colspan='6'>데이터셋 1000 </th></tr>
      <tr><th>긍정</th><td>500</td><th>학습</th><td>800</td></tr>
      <tr><th>부정</th><td>500</td><th>검증</th><td>200</td></tr>
</table>

# 3. 재학습 결과
## 3.1 개발 환경
<img src="https://img.shields.io/badge/pycharm-000000?style=flat-square&logo=pycharm&logoColor=white"/> <img src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=Python&logoColor=white"/> <img src="https://img.shields.io/badge/torch-EE4C2C?style=flat-square&logo=pytorch&logoColor=white"/> <img src="https://img.shields.io/badge/pandas-150458?style=flat-square&logo=pandas&logoColor=white"/> <img src="https://img.shields.io/badge/numpy-013243?style=flat-square&logo=numpy&logoColor=white"/> <img src="https://img.shields.io/badge/transformers-81c147?style=flat-square&logo=transformers&logoColor=white"/> <img src="https://img.shields.io/badge/scikit-learn-F7931E?style=flat-square&logo=scikit-learn&logoColor=white"/>
## 3.2 KOELECTRA fine-tuning
## 3.3 학습 결과 그래프


<div><img src="https://github.com/yeon0306/steam_game/assets/112537146/bc55087b-e460-472f-a834-86ee7afdac7f" width="500"><img src="https://github.com/yeon0306/steam_game/assets/112537146/66851e62-ec7d-4f09-b7c8-730789279208" width="500"></div>

<table>
  <tr align="center"><th></th><th></th><th>Epoch 1</th><th>Epoch 2</th><th>Epoch 3</th><th>Epoch 4</th></tr>
  <tr align="center"><th rowspan="2">학습데이터</th><td>평균 학습 오차</td><td>0.58</td><td>0.37</td><td>0.24</td><td>0.15</td></tr>
  <tr align="center"><td>검증 정확도</td><td>0.74</td><td>0.86</td><td>0.87</td><td>0.89</td></tr>



# 4. 배운점

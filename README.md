# 오프닝에 따른 체스 승률 예측

- 김민결 데이터사이언스학부 ki30ms19@gmail.com
- 이병규 데이터사이언스학부 lbg0217@hanyang.ac.kr
- 김리나 경영학부 linakim07@hanyang.ac.kr 

I. 주제
- 오프닝에 따른 체스 승률 예측
- 체스는 훌륭한 데이터셋을 가지고 있어 분석하여 알아보기로 하였다.
- 오프닝에 따라 체스 게임의 승률이 달라질 것이다.

II. 데이터셋
- kaggle에서 제공하는 chess game dataset lichess을 활용한다. 이 데이터셋은 온라인 체스 플랫폼 lichess에서 진행된 약 20000건의 체스 경기 데이터를 포함하고 있다. 주요 변수로는 매치 승패 결과, 백과 흑 플레이어의 레이팅, 오프닝 이름, 경기 턴 수 등이 있다.

III. 분석 및 진행 방법
- scikit learn 라이브러리를 활용하여 두 가지 모델을 비교 분석한다. 첫 번째는 직관적인 해석이 가능한 랜덤 포레스트이며, 두 번째는 복잡한 비선형 관계를 학습할 수 있는 다층 퍼셉트론 기반의 간단한 딥러닝 모델이다. 
- explaining features: 승패에 가장 큰 영향을 미칠 것으로 예상되는 백과 흑의 레이팅 차이라는 새로운 파생 변수를 생성한다. 또한 범주형 변수인 오프닝 전술은 빈도가 높은 상위 20개로 그룹화한 뒤 원 핫 인코딩을 적용하여 모델이 학습할 수 있도록 처리한다.

IV. 평가 및 분석
- 탐색적 데이터 분석: '레이팅 차이에 따른 승률 변화'를 산점도와 추세선으로 시각화하여 레이팅이 승패에 미치는 영향을 확인한다. 또한 가장 승률이 높은 오프닝 순위를 막대그래프로 시각화한다.  
- 성능 평가: 랜덤 포레스트와 MLP 모델의 예측 성능을 정확도와 F1 Score 표로 비교한다. 혼동 행렬을 그려 모델이 무승부를 얼마나 잘 예측하는지 분석한다.  
![체스승률예측1](https://github.com/linakim07-design/predictingchesswinrates.github.io/blob/main/%EC%B2%B4%EC%8A%A4%EC%8A%B9%EB%A5%A0%EC%98%88%EC%B8%A11.png?raw=true)
![체스승률예측2](https://github.com/linakim07-design/predictingchesswinrates.github.io/blob/main/%EC%B2%B4%EC%8A%A4%EC%8A%B9%EB%A5%A0%EC%98%88%EC%B8%A12.png?raw=true)
성능 평가 결과, 다층 퍼셉트론(MLP) 모델이 정확도 75%, F1-Score 0.512로 랜덤 포레스트 모델(정확도 71%, F1-Score 0.493)보다 전반적으로 더 우수한 예측 성능을 보였다. 이는 체스 경기 데이터 내의 복잡한 비선형적 관계를 MLP의 은닉층이 효과적으로 학습했음을 시사한다.

[최종 모델 성능 비교 결과 표]

| Model | Accuracy | F1-Score |
| :--- | :---: | :---: |
| **Random Forest** | 0.71 | 0.49395 |
| **MLP (Deep Learning)** | **0.75** | **0.51265** |

 체스 오프닝별 승률 탑 10과 승률 데이터 시각화

---

데이터 시각화 결과

<img width="1305" height="547" alt="다운로드" src="https://github.com/user-attachments/assets/8c0a5e0f-e14e-41b1-9062-c97ad088a41d" />

프로그램 코드

<img width="1315" height="586" alt="스크린샷 2026-06-09 오후 12 26 32" src="https://github.com/user-attachments/assets/edd99882-1437-4a38-a945-eefb28e00bd7" />
<img width="1225" height="466" alt="스크린샷 2026-06-09 오후 12 27 00" src="https://github.com/user-attachments/assets/339b0b59-b3b2-4d6d-bfb0-9fdb6999cb9e" />


* **초록색 (white):** 백(선공) 승리
* **주황색 (black):** 흑(후공) 승리
* **보라색 (draw):** 무승부

---

 주요 데이터 인사이트 분석

그래프를 면밀히 분석해 보면 체스 게임의 흥미로운 통계적 특성을 몇 가지 발견할 수 있습니다.

1. 흑(Black)이 유독 유리한 오프닝
* **Van't Kruijs Opening**: 전체 상위 10개 오프닝 중 가장 압도적인 경기 수를 기록했습니다. 하지만 놀랍게도 **흑(black)의 승리 빈도가 백(white)보다 훨씬 높게** 나타납니다. 백이 선공의 이점을 살리지 못하는 대표적인 오프닝임을 데이터가 증명합니다.
* **Sicilian Defense (시실리안 디펜스)**: 기본 'Sicilian Defense'와 더불어 'Sicilian Defense: Bowdler Attack' 모두 **흑의 승률이 백을 상회**하거나 대등한 강력한 모습을 보여줍니다. 현대 체스에서 왜 시실리안 디펜스가 흑의 가장 강력한 방어 무기로 꼽히는지 통계적으로 확인할 수 있습니다.

2. 백(White)이 확실한 우세를 가져가는 오프닝
* **Scandinavian Defense: Mieses-Kotroc Variation**: 이 오프닝에서는 **백(white)의 승률이 흑(black)을 압도**합니다. 흑이 초반 대응을 잘못할 경우 백에게 주도권을 완전히 내어줄 확률이 높음을 시사합니다.
* **Scotch Game (스코치 게임)**: 전통적으로 백이 중앙을 강하게 압박하는 오프닝답게, 백의 승리 횟수가 흑보다 유의미하게 높게 관찰되었습니다.

3. 무승부(Draw) 비율의 특성
* 전체적으로 **무승부(draw)의 비율은 승패에 비해 현저히 낮게** 나타납니다. 이는 아마추어 및 온라인 레이팅 데이터의 특성상 서로 비기기보다는 끝까지 승패를 겨루는 공격적인 성향이 반영된 결과로 해석할 수 있습니다.

---

결론 및 머신러닝/딥러닝 모델과의 연결성

이번 시각화 분석을 통해 "어떤 오프닝을 선택하느냐에 따라 초반 승률의 기댓값이 판이하게 달라진다"는 가설을 증명했습니다. 

우리가 구축한 승률 예측 모델은 단순히 선수의 실력(레이팅)뿐만 아니라, 이 그래프에서 나타난 **오프닝별 승률의 통계적 치우침을 학습**하여 미래의 경기 결과를 예측하게 됩니다. 다음 포스팅에서는 이 데이터를 바탕으로 학습시킨 인공지능 모델의 예측 정확도(Accuracy)를 다뤄보겠습니다.

V. 한계점 발견: 데이터 불균형(Data Imbalance) 문제

1. MLP 모델은 75%의 정확도를 보였으나, 혼동 행렬을 깊이 분석한 결과 치명적인 한계가 있었습니다. 약 2만 건의 체스 게임 데이터 중 무승부 비율이 극히 적어, 모델이 실제 무승부 경기를 단 한 번도 맞추지 못하고 모두 패배나 승리로 오분류하는 현상이 발생했습니다.

2. 기술적 해결 방안: SMOTE 오버샘플링 도입
소수 클래스가 무시되는 현상을 해결하기 위해 SMOTE 기법을 도입했습니다.

단순 복제가 아닌, K-최근접 이웃 알고리즘을 기반으로 무승부 데이터의 특성을 분석해 새로운 가상 데이터를 합성하는 방식을 사용했습니다.

이때 모델의 평가 신뢰도를 유지하기 위해, 검증&테스트 데이터는 원본 그대로 두고 오직 훈련 데이터(Train Set)에만 SMOTE를 적용하여 데이터 누수를 방지했습니다.

<img width="842" height="665" alt="스크린샷 2026-06-09 122657" src="https://github.com/user-attachments/assets/1312c56d-f32f-4baf-ab30-51bf2db0d7ce" />

다음은 코드입니다.
<img width="551" height="106" alt="스크린샷 2026-06-09 122933" src="https://github.com/user-attachments/assets/6a5a7425-6b1a-459a-b43d-a78410edda3e" />
<img width="980" height="656" alt="스크린샷 2026-06-09 122925" src="https://github.com/user-attachments/assets/ee2cdb59-5a77-45f9-8eb6-a08c7c90d5cb" />
<img width="932" height="627" alt="스크린샷 2026-06-09 122903" src="https://github.com/user-attachments/assets/70259ac3-d8a6-4750-8e50-b5f3e10ce482" />
<img width="676" height="676" alt="스크린샷 2026-06-09 122847" src="https://github.com/user-attachments/assets/18450e58-6f20-4004-b606-4795cf5dc832" />





V. 관련 출처
- Kaggle 체스 데이터셋 (https://www.kaggle.com/datasets/datasnaek/chess)

- Scikit-learn 홈페이지 (https://scikit-learn.org/stable/)

VI. 결론 
- 프로젝트 결과, 체스 승률 예측에서 가장 중요한 피처는 예상대로 플레이어 간의 레이팅 차이이다. 하지만 특정 오프닝을 사용했을 때 레이팅의 불리함을 일부 극복하는 경향성을 데이터로 확인할 수 있었다.
-모델이 '무승부' 클래스의 패턴을 성공적으로 학습하여, 기존에 0건이었던 무승부 정답 횟수가 유의미하게 증가했다.



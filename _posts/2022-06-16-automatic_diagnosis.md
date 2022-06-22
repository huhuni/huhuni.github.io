---
title: Automatic diagnosis of the 12-lead ECG using a deep neural network
categories: [Papers, ecg]
tags: [ecg, paper review, ]     # TAG names should always be lowercase
---

# Automatic diagnosis of the 12-lead ECG using a deep neural network review
링크 : [paper](https://www.nature.com/articles/s41467-020-15432-4?ref=https://githubhelp.com)


# *Abstract*

해당 논문은 Nature Communications에 출판된 논문으로 12 LEAD ECG를 이용하여 1dAVb, RBBB, LBBB, SB, AF, ST에 대해 예측하는 모델을 제안하였다. 모델의 구조는 ResBlock 기반의 CNN 모델이다.

여기서 target으로 보는 질병은 다음과 같다.

- 1dAVb : 방실 1도 차단 (AV Block 1st Degree) - 전도 장애
    - p-qrs-t 리듬이 보이나, 중간중간 p와 qrs간의 간격이 늘어지는 구간이 발생
    - qrs-t가 실종되는 구간은 없음
- RBBB, LBBB : 우/좌각 차단 (Right/Left Bundle Branch Block) - 전도 장애
    - 양각은 방실간 전류 이동을 빠르게 하는 역할을 하며, 방실결절이 제 기능을 하지 못할 대 최후의 페이슾메이커로 기능
    - 우각 차단의 경우 비교적 흔한 부정맥으로써 기저 심질환이 없는 경우 별다른 증상 없음
    - 좌각 차단의 경우 심장에 이상이 발생했을 때 동반되는 경우가 많음
- SB : 동성 서맥 (Sinus Bradycardia) - 리듬 장애
    - p-qrs-t가 정상적이고 규칙적이지만 맥박이 지나치게 느림
    - 동성 리듬이기는 하나 결코 안전하지 않음
- AF : 심방 세동 (Atrial Fibrillation) - 리듬 장애
    - 동발결절에서 시작되어야할 신호가 여기저기서 튀어나와 심방의 리듬이 세동
    - 혼잡한 신호가 발생함에도 방실결절은 멀쩡한 상태이기 때문에 심실은 불규칙하나 제대로 움직임
    - 상대적으로 QRS파형이 선명하게 나타남
- ST : 동성 빈맥 (Sinus Tachycardia) - 리듬 장애
    - p-qrs-t가 정상적이고 규칙적이나 맥박이 평소보다 빠른 상태
    - T와 S의 간격이 매우 짧아져 T와 S가 반쯤 합쳐진 상태가 되기도 함
    

---

# *Contributions*

- CODE - Clinical Outcomes in Digital Electrocardiology study라고 부르는 large-scale labeled ECG dataset을 구축함
- DNN을 이용하여 6가지 유형의 ECG abnormalities를 분류하였다 (CNN 기반이지만 DNN으로 표기한 것으로 보임)

---

# *Results*

본 논문에서는 실험을 위해 TNMG(the Telehealth Network of Minas Gerais)를 통해 1,676,384명의 서로 다른 환자(Minas Gerais/Brazil)로 부터 2,322,513개의 ECG records를 수집

![Untitled](/assets/img/automatic_diagnosis/Untitled1.png)


> 생각보다 Abnormality ECG가 적은것을 알 수 있으며 특히 Test Set은 더욱더 적은 데이터만을 포함하고 있음 → 정확한 분석을 위해 심전도 경험이 있는 3명의 심장학자가 주석을 단 개별 환자 827개의 추적 데이터 셋이라고 함
> 

Cardiologist-level arrhythmia detection and classification in ambulatory electrocardiograms using a deep neural network에서 제안된 모델보다 1/4 정도의 계층과 매개 변수를 가진 아키텍쳐에서 최고의 검증 결과를 얻을 수 있었음

## *Train*

신경망은 무작위로 초기화 되며 초기화에 따라 다른 결과가 도출됨 → 안전성을 보장하기 위해 동일한 하이퍼 파라미터 세트와 다른 초기화를 가진 10개의 신경망을 훈련

결과를 report하기 위해 maximum and minimum precision 사이의 중위수 값 바로 위의 micro average precision을 가지는 모델을 선택함 (mAP = 0.951)

> micro average precision?
전체 평균이라고 생각하면 됨
> 

## *Performance*

![Untitled](/assets/img/automatic_diagnosis/Untitled2.png)

전반적으로 모든 질병에 대해서 좋은 결과를 보여주고 있음

특히 LBBB는 매우 좋은 모습을 보여줌

- DNN : 논문에서 제안하는 모델
- cardio : 4년차 cardiology(심장) 레지던트 2명
- emerg : 3년차 응급 레지던트 2명
- stud : 5년차 의대생

훈련된 심장 전문의는 DNN의 실수 및 의료인, 학생들의 실수를 검토 →  아래와 같이 이유를 설명함

![Untitled](/assets/img/automatic_diagnosis/Untitled3.png)

- meas : measure(측정) 오류 - ECG 간격 측정이 교과서 정의에 의해 주어진 진단을 배제하는 경우
- noise : 노이즈로 인한 오류 - ECG의 품질이 일반적인 신호 품질보다 낮다고 판단된 경우
- unexplain : 설명되지 않는 다른 유형의 오류
- concep : conceptual(개념) 오류 - 질병에 대한 저의를 이해하지 못했다고 제안한 경우
- atte : attention(관심) 오류 - 검토자가 좀 더 주의를 기울였다면 피할 수 있는 오류

---

# *Discussion*

본 논문은 End-to-End 자동 S12L-ECG 분류 효과를 입증하고 있다. 기존의 ECG 자동 분석 방법인 전통적인 신호 처리 기술 보다 우수한 성능을 보여준다.

오류 분석 : 대부분의 DNN이 발생한 실수는 심전도 간격 측정과 관련이 있음 → 대부분은 측정값이 sharp cutoff point이상일 때만 확인할 수 있는 consensus definitions였다. 즉, DNN은 이러한 매우 sharp한 임계값을 인코딩하지 못한다고 볼 수 있음

ex) DNN은 심박수가 50bpm을 약간 초과하는 SB 또는 심박수가 100bpm보다 약간 낮은 ST를 잘 감지하지 못함

논문의 저자는 DNN이 높은 F1 socre를 얻었지만 McNemar 통계 테스트와 부트스트랩은 DNN이 실제로 통계적 중요성을 가진 의료인 및 학생보다 좋다고 주장할만큼 충분한 자신감은 없다고 한다. 이는 상대적으로 질환이 있는 심전도 클래스가 매우 드물기 때문이라고 생각하고 있다. (imbalance data이기 때문)

---

# *Methods*

## *Dataset acquisition*

브라질 미나스제라이스주 853개 자치단체 중 811개를 지원하는 공공 원격의료 시스템인 미나스제라이스 원격으로 네트워크(TNMG)를 통해 데이터를 획득하였다.

심전도 기록의 지속시간은 300~600Hz 범위의 주파수로 7~10초의 샘플링을 가지고 있다.

## *Labeling training data from text report*

train 및 validation set의 심장병 전문의 보고서는 검사의 이상 징후를 텍스트로 설명하는 경우에만 사용할 수 있다. 3단계 절차를 사용하여 텍스트 보고서에서 레이블을 추출하였다.

1. 의료 보고서에서 중단어를 제거하고 n-gram을 생성하여 텍스트를 사전 처리
2. 실제 진단 텍스트 보고서에서 생성된 2,800개 샘플 사전에 대해 훈련된 LAC(Lazy Associative Classifier)가 n-gram에 적용됨
3. 텍스트 레이블은 클래스 명확화를 위한 규칙 기반 분류기에서 LAC 결과를 사용하여 얻음

무료 텍스트와 함께 제공된 공인 심장 전문의에 의해 수동으로 라벨링된 4,557개의 의료 보고서에서 테스트 되었으며 사전 지정된 클래스 중 선택하였다. 분류 단계는 좋은 결과와 함께 실제 의료 라벨을 복구하였다.

macro F1 score achieved were: 0.729 for 1dAVb; 0.849 for RBBB; 0.838 for LBBB; 0.991 for SB; 0.993 for AF; 0.974 for ST.

> 해당 부분은 무슨 내용인지 정확하게 이해가 되지 않아 좀 더 공부해봐야할 것으로 보임
> 

## *Training and validation set annotation*

훈련 및 검증 데이터 셋에 주석을 달기 위해 3가지를 사용

1.  Uni-G 자동 분석으로 얻은 Uni-G statements 미네소타 코드
2. Uni-G 소프트웨어가 제공하는 자동 측정
3. 준지도 방법론(의료 진단)을 사용하여 전문가 텍스트 보고서에서 추출한 텍스트 레이블이 사용됨

자동 진단과 의료 진단 모두 오류가 발생할 수 있으며 자동 분류는 정확도가 제한적이고 텍스트 라벨은 실무 전문가 심장병 전문의와 라벨링 방법론 모두의 오류가 발생할 수 있음

따라서, 데이터 셋의 품질을 향상시키기 위해 전문가 주석을 자동 분석과 결합

1. 
    1. 전문가와 Uni-G statement 또는 자동 분석에 의해 제공된 Minnesota 코드가 동일한 이상을 나타내는 경우 이상 발생으로 간주
    2. 자동 분류기가 의사 및 다른 자동 분류기와 일치하지 않는 경우 이상 없음으로 간주
    
    이러한 Initial 단계 이후, 진단을 승인하거나 거부할 필요가 있는 두 가지 시나리오가 존재
    
    - 두 분류기 모두 이상을 나타내지만 전문가는 그렇게 생각하지 않음
    - 오직 전문가만이 이상을 나타내지만, 어떤 분류기들도 이상 없음을 나타냄
2. 다음 규칙을 사용하여 남은 진단 중 일부를 제거
    1. 심박수가 100 미만인 ST 진단(의료진단 8,376건, 자동진단 2건)
    2. 심박수가 50 이상인 SB 진단(의료진단 7,361건, 자동진단 16,427건)
    3. QRS간격이 115ms 미만인 LBBB/RBBB 진단(의료 진단 RBBB:9,313 / LBBB:8,260)
    4. PR 간격이 190ms 미만인 1dAVb 진단(자동진단 3,987)
3. 이후, 이상이 있는 데이터 당 100개의 수동 검토 검사의 민감도 분석을 사용하여 남은 진단 중 일부를 수용하기 위한 규칙 마련
    1. RBBB, 1dAVb, SB 및 ST의 경우 모든 의료 진단 수용. (26,033 / 13,645 / 12,200 / 14,604)
    2. AF의 경우 의사들에 의해 검사가 사실로 분류될 뿐 아니라 NN 간격의 표준 편차가 646보다 높아야한다고 요구(14,604)
    
    민감도 분석에 따르면, 이 절차로 도입될 false positive 횟수가 전체 검사 횟수의 3%보다 적었다고 한다.
    
4. 이러한 과정 이후에도, 34,512개의 예제가 남았으며 받아들일 수도, 거절될 수도 없어 심전도 해석 경험이 있는 공인 심장 전문의의 감독 하에 원격 건강 심전도 진단 시스템을 사용하여 의대생들에게 의해 수동으로 검토됨 → 수 개월 걸림(의대생이 녹는 소리가 여기까지 들린다..)
    
    논문의 저자는 이전 의료 보고서와 자동 측정의 정보는 훈련 및 검증 세트에 대한 근거 자료 획득에만 사용되었으며 DNN 훈련의 후속 단계에서는 사용되지 않았음을 강조하고 있다.
    

## *Test set annotation*

DNN 테스트에 사용된 데이터 셋도 TNMG의 ECG 시스템에서 획득하였다. 심전도에 경험이 있는 두 명의 공인 심장학자가 독립적으로 주석을 달았다. 

## *Neural network architecture and training*

![Untitled](/assets/img/automatic_diagnosis/Untitled4.png)

본 논문에서는 Residual Network와 유사한 1D CNN을 사용함 → 이는 skip connection을 통해 효율적으로 훈련될 수 있다.

모든 심전도 기록은 400Hz 샘플링으로 resampling / 7초와 10초 사이의 모든 심전도 기록은 zero padding을 적용하여 각 리드에 대해 4,096개의 샘플이 포함된 신호를 생성 → 신경망 입력으로 사용

네트워크는 Conv에 이어 블록당 2개의 Conv 레이어가 있는 4개의 Resblock으로 구성된다ㅏ. 마지막 블록의 출력은 sigmoid를 가진 FCNN로 공급되며 이는 클래스가 서로 배타적이지 않기 때문에 이렇게 진행하였다고 한다. (동일한 ECG 예제에서 두 개 이상의 클래스가 발생할 수 있음)

각 Conv레이어의 출력은 배치정규화를 사용하여 rescailing되며 ReLU로 공급된다. Dropout은 비선형성 이후에 적용된다

Conv레이어는 필터 길이=16을 가지며, 첫 번째 레이어와 Resblock을 위한 4,096개의 샘플과 64개의 필터로 시작하여 필터 수를 두 번째 Resblock마다 64개씩 증가시키고 나머지 블록마다 4의 배수로 subsampling을 진행한다.

Max Pooling과 필터 길이 1(1x1 Conv)의 컨볼루션 레이어가 skip connection에 포함되어 main branch의 신호로부터 차원을 일치시킨다. 

## *Hyperparameter tuning*

최종 아키텍처와 구성은 약 30회의 반복 후 얻어졌다고 한다.

1. 훈련 셋에서 신경망 가중치를 찾음
2. 검증 셋에서 성능 확인
3. 새로운 하이퍼 파라미터와 아키텍처를 수동으로 선택

이전 반복에서 얻은 insight를 사용한다.

하이퍼 파라미터의 매개변수는 

- {2, 4, 8, 16} 개의 resblock
- {8, 16, 32} 크기를 가지는 커널
- {16, 32, 64}의 배치 사이즈
- {0.01, 0.001, 0.0001} 초기 learning rate
- {SGD, ADAM} 옵티마이저
- {ReLU, ELU} activation function
- {0, 0.5, 0.8} drop out
- {5~10} plateaus

이 외에도 입력의 차원성을 줄이기 위해 벡터 심전도 선형 변환, 컨볼루션 레이어 앞에 LSTM 레이어 포함, 사전 활성화 아키텍처가 없는 Residual Network, VGG 사용, 활성화 함수와 배치 정규화의 순서 전환 등을 고려하였다.

## *Statistical and empirical anlysis of test results*

각 클래스의 모델 평가를 위한 precision-recall curve를 계산하였다. 

부트스트랩, F1 socre등을 사용하였으며 다양한 방법을 사용하였다고 한다.

## *Data availability*

본 논문에서 사용된 테스트 데이터 셋은 공개적으로 사용 가능하며 (https://[doi.org/10.5281/zenodo.3625006](http://doi.org/10.5281/zenodo.3625006))에서 다운로드 할 수 있다. 

또한 본 논문에서 개발한 모든 심층 신경망의 모델 가중치는 ([https://doi.org/10.5281/zenodo.3625017](https://doi.org/10.5281/zenodo.3625017))에서 확인할 수 있다.

학습 셋의 경우 제한적이며 접근하기 위해서는 미나스 제라이스의 원격 건강 네트워크에 의해 개별적으로 고려될 것이라고 한다. 모든 데이터 사용은 비상업적 연구 목적으로 제한되며, 데이터는 적절한 데이터 사용 계약을 이행할 때만 사용할 수 있다.

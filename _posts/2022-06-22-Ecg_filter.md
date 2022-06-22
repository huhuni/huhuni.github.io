---
title: ECG Filter
categories: [Medical, ecg]
tags: [ecg, filter]     # TAG names should always be lowercase
---

# ECG Filter

작성 날짜: 2022년 6월 22일 오후 10:10
태그: 메모

## ECG Filter

---

## *Reference*

[심전도 필터 — MEDTEQ](https://www.medteq.net/article/2017/4/1/ecg-filters)

[ECG filter settings and detection of pacemaker spikes – All About Cardiovascular System and Disorders (johnsonfrancis.org)](https://johnsonfrancis.org/professional/ecg-filter-settings-and-detection-of-pacemaker-spikes/#:~:text=In%20the%20illustration%20below%2C%200.08%20Hz%20is%20the,that%20all%20frequencies%20below%20that%20can%20pass%20through.)

---

## 필터란?

심전도는 환자들에게서 얻어진 심장의 전기신호로 일반적으로 원치 않는 노이즈를 제거하기 위해 Filter를 적용한다. 특히 ECG의 신호 레벨은 매우 작기 때문에(1mV), 필터링을 사용하여 광범위한 잡음을 제거한다. 얻어지는 잡음으로는 전극/바디 인터페이스로부터의 불안정한 DC offset, Muscle noise, mains hum(50/60Hz)  환경 및 ECG 장비 자체 내의 장비(내부 dc/dc converter)로 부터의 전기적 잡음으로 인해 발생할 수 있다.

> mains hum(50/60Hz) **← 해당부분은 찾아보아야 할 것으로 보임**,
> 

필터는 노이즈가 발생하는 주파수를 제거하거나 줄이면서 신호 주파수가 통과하도록 허용하여 작동한다. 이 작업은 하드웨어 또는 소프트웨어에서 수행할 수 있다. 최신 시스템에서 하드웨어 필터링의 주요 목적은 연산 포화 및 ADC 범위와 같은 아날로그 시스템의 한계를 초과하지 않도록 하는 것이다. 

## ECG 필터링의 전형적인 유형

이상적으로 필터는 관심있는 신호에 영향을 미치지 않고 노이즈를 제거해야하나 현실적으로 불가능하다. 

- 신호와 잡음이 동일한 주파수를 공유할 수 있기 때문
    - 주전원 잡음(50/60Hz), 근육 잡음 및 환자 움직임으로 인한 DC offset에서의 드리프트는 모두 일반적인 ECG와 동일한 주파수 범위에 속함
- 실용적인 필터가 일반적으로 ”Pass” band 와 “Cut” band 사이에 날카로운 edge가 없다는 것
    - 일반적으로 필터 응답에 느린 전환이 있으므로 원하는 신호와 원치 않는 신호가 가까우면 원하는 신호 중 일부를 제거하지 않으면 노이즈를 제거하지 못할 수 있음

그 결과 필터는 필연적으로 신호 주파수를 왜곡한다.

![Untitled](/assets/img/ecg_filter/Untitled1.png)

위 그림은 0.67Hz ~ 40Hz의 일반적인 “monitor”필터와 함께 IEC60601-2-25의 ANE20002 파형의 왜곡을 보여준다. 노이즈를 제거하는 것과 원래 신호를 보존하는 것 사이에서 균형을 찾아야한다. 다양한 목적(모니터링, 집중 치료, 진단, 외래, ST 세그먼트 모니터링 등)을 위해 균형이 바뀌므로 최상의 균형을 얻기 위해 조정된 다양한 필터를 사용한다. 

ECG 필터의 몇 가지 일반적인 예

- Diagnostic(진단): 0.05Hz ~ 150Hz
    - 가장 넓으며 움직이지 않는 저잡음 환경을 가정
- Ambulatory, patient monitoring(외래, 환자 모니터링): 0.67Hz ~ 40Hz
    - 시끄러운 환경을 위한 경미한 필터링, 주로 심박수 감지
- ST segment: 0.05Hz ~
    - ST segment 모니터링을 위한 특수 확장 저주파 응답(low frequency response)
- Muscle, ESU noise: ~ 15Hz
    - 근육 잡음 및 ESU와 같은 다른 간섭을 제거하기 위해 더 높은 주파수 응답 감소
    

## 저역 통과 필터 (Low pass filtering)

high frequency components를 줄임으로써 작동한다. 가장 일반적인 형태는 직렬 저항기 / 캐퍼시터

ECG에서는 근육 전위 간섭(EMG 인공물)을 피하는 용도로 사용

## 고역 통과 필터 (High pass filtering)

저역 통과 필터의 반대.

전극/젤/바디 인터페이스로 인해 크게 발생하는 DC offset을 제거한다. 장기간의 주기적인 파형에서의 경우 ECG에서 “기준선(Baseline)”으로 알려진 중심선 주위로 파형을 이동하거나 유지한다. 

**주의** : ST segment는 고역 통과 필터에 따라 문제가 생길 수 있음. ST segment를 보존하는 것이 매우 중요한 질병의 경우 고역 통과 필터를 0.05Hz로 줄여 ST segment가 크게 왜곡되지 않도록 해야함

![Untitled](/assets/img/ecg_filter/Untitled2.png)

![Untitled](/assets/img/ecg_filter/Untitled3.png)

위 이미지는 IEC 60601-2-25(+0.2mV 상승된 ST segment)의 CAL20160 파형에서 0.67Hz의 ECG 고역 통과 필터를 정상 모니터링하여 ST segment를 제거하고 있다. 이처럼 고역 통과 필터를 잘못 사용하는 경우 ST segment의 정보를 잃어버릴 수 있으므로 조심해야 한다.

ECG에서는 호흡 및 기타 저주파 신호로 인한 기준선 변동을 필터링하기 위한 용도로 사용

## 노치 필터 (Notch filter, mains hum filters, AC filter, 50/60Hz)

노치 필터는 고역 통과 필터와 저역 통과 필터를 결합하여 제거할 주파수의 작은 영역을 생성하는 방법이다.

ECG의 경우 주요 목표는 50Hz, 혹은 60Hz 잡음을 제거한다. 이러한 노치 필터는 일반적으로 선택사항이며 ECG 장비 내에서 주전원 잡음을 없애는 기능이 포함되어 있거나 AC필터가 필요하지 않을 수 있다.

ECG 기계에서는 대부분 50Hz에서 노치 필터가 있어 라인 전압 간섭을 필터링한다.
---
layout: post
title: "[논문 리뷰] Benchmarking deep learning models on large healthcare datasets"
date:   2021-06-15
image: '/assets/img/covers/benchmarking-deep-learning-on-mimic.jpg'
tags: [Journal]
use_math: True
---
`MIMIC3`를 이용한 딥러닝 모델들의 벤치마킹 연구 논문을 리뷰하려 한다.

<!--more-->

# Abstract

딥러닝 모델은 다양한 분야에서 많이 사용되고 있다. 그러나 오픈소스 의료데이터를 이용한 머신러닝과 예후 scoring system SOTA 수준의 딥러닝 모델은 거의 없다. 본 논문에서는 딥러닝 모델 또는 머신러닝 모델의 앙상블, SAPS II와 SOFA 점수를 이용한 몇개의 의료 분야에서의 예측(사망, 입원기간, ICD-9 code 그룹) 벤치마킹 결과를 제시하려한다. 해당 연구에서는 ICU 데이터셋인 `MIMIC III`를 사용하였다. 본 논문의 결과는 딥러닝 모델은 raw clinical time series data를 input feature로 사용하는 모델들에게 지속적인 성능이 나온다는 결과를 보여준다.

---

# Introduction

`환자의 사망과 입원기간`은 ICU 입원에서 가장 중요한 임상 결과이다. 그리고 질병의 중증 측면에서 도움을 줄 수 있고 새로운 치료, 중재 및 건강 관리 정책의 가치를 결정할 수 있다. SAPS-II, SOFA, APACHE와 같은 중증도 점수는 ICU에 들어온 뒤 24시간 내에 얻은 측정값으로 정의되었으며 사망률을 예측하기 위한 목적으로 개발되었다.
최근 머신러닝과 딥러닝의 발전과 성공으로 많은 연구자들이 ICU 입원의 임상 예측 작업에 머신러닝과 딥러닝 모델을 체택하였다. 본 논문에서 소개하는 연구들은 다음과 같다.

- 머신러닝 앙상블 모델을 이용한 사망률 예측
- gradient boosting과 logistic regression 모델을 이용한 사망률 예측
- 딥러닝 모델을 이용한 4개의 임상 예측

본 연구의 주 기여점은 다음과 같다.

- 딥러닝 모델을 이용한 사망률 예측, 입원기간 예측, ICD-9 code 그룹 예측(이 실험들은 머신러닝 모델과 severity scoring system들보다 더 좋은 성능을 나타냈다.
- 딥러닝 모델이 clinical feature들의 룰 기반 전처리가 딥러닝 모델에 필요하지 않다는 것을 가르키는 raw feature들의 더 나은 결과를 얻는다는 것을 보여준다.

---

# MIMIC-III dataset preprocessing
## Cohort selection
15세보다 많은 나이의 환자들을 어른이라 정의하였고 어른의 환자들만 활용하였다. 그 환자들의 첫번째 입원기록만을 사용하였고 그 이후에 입원한 기록은 사용하지 않았다. 이는 분석에 있어서 정보누출 가능성을 방지하고 related work와 유사한 실험 설정을 하기 위해서다.

## Data extraction
본 논문에서는 아래의 테이블을 사용하였다.

- inputevents(inputevents_cv, inputevents_mv): philips CareVue 시스템과 iMDSoft MetaVision 시스템을 사용하여 모니러링한 환자의 input 정보
- ouputevents: ICU에 있는 동안 환자의 output 정보
- chartevents: 환자에 대해 차트로 작성된 모든 정보
- labevents: lab 정보
- prescriptions: 환자에 처방된 정보, 처방받았으나 반드시 투여되는 것은 아님
  본 연구에서 사용하는 데이터셋은 MIMIC-III와 MIMIC-III  (CareVue)이다.

## Data cleaning
- 특정 변수의 unit들 불일치: 이를 해결하기 위해 변수별로 단위들의 비율을 구하였고 하나의 단위만 가지고 있을 경우는 그대로 두고 90% 이상이 차지하고 있는 unit을 major unit이라 정의하였는데 이 unit은 그대로 두고 나머지 unit들은 삭제하였다. 이외의 major unit(90% 이상을 차지하는 unit)가 없는 경우 [문헌](https://www.drugs.com/dosage/)을 참고하여 하나의 unit으로 변환하였다. 변환된 unit은 논문의 Appendix A.6에 확인할 수 있다. 이 위의 것들에 해당되지 않는 feature의 경우 삭제하였다.
  
- 일부 변수에는 동시에 기록된 값들이 존재함: numerical feature들의 경우, average를 구했고 categorical feature의 경우, 첫번째로 나타난 값을 유지하였다.

- 일부 변수의 경우 범위로 기록되어있음: 범위의 median 값을 구하였다.

## Feature selection and extraction

본 연구에서는 예측에 사용하기 위해 데이터셋에서 feature들을 추출하였고 철저한 비교를 위해 3가지의 features set을 선택하였다.

- feature set A는 SAPS-II의 계산에 사용되는 feature들로 구성된다. 이 feature들의 경우 의학 지식에 따라 outlier를 제거하고 관련 feature들을 병합하였다. 병합한 feature들은 논문에서 확인할 수 있다. 만성질병, 입원형태, 나이와 같은 feature들은 비시계열로 처리되었고 이외의 나머지는 시계열 feature들로 처리되었다.

- feature set B는 SAPS-II에 사용되는 17개의 feature들과 관련있는 20개의 feature들로 구성되어있다. Feature set A를 얻기 위해 처리된 17개의 feature들 대신 원 상태의 값(raw value)을 고려한다. 이상치를 제거하지 않고 0미만의 값만 삭제하였다. 이 feature set은 `raw value`의 feature들의 성능을 연구하기 위해 구축되었다.

- feature set C는 136개의 raw value들로 구성되며 feature set B의 feature들도 포함된다.  136개의 feature들은 2,000개의 feature들 중에 낮은 누락비율을 기반으로 선택되었다. Feature set B와 유사하게 전처리를 하지 않았으며 원 상태의 값을 사용했다. 이 feature set은 예측 모델이 대용량의 임상 시계열 데이터를 `feature representation`을 자동으로 학습할 수 있는지 동시에 더 `좋은 성능`을 나타낼 수 있는지` 연구하기 위해 선택되었다.

본 연구에서는 입원하고 첫 24시간과 첫 48시간 안에 수집된 feature들을 추출했으며 시계열 feature들의 경우 1시간마다 샘플링하였다. 샘플링 과정 중에 몇개의 feature들은 같은 시간안에 여러개의 판독값을 가진 경우, 평균 또는 합계를 구하였다. 일반적으로 수액 또는 약물에 대한 기록은 합계를 구하였고 이외의 다른 feature들은 평균을 구하였다. 결측값이 발생한 경우, 앞뒤의 값을 평균내서 imputation 하였다.

---

# Benchmarking experiments
## Benchmark prediction tasks

- Mortality prediction(binary classification)
  더 자세한 label 정의는 appendix A.2를 참고하세요.

  - In-hospital mortality prediction: ICU에 입원 후 환자의 사망여부를 예측
  - Short-term mortality prediction: 2, 3일 안에 환자의 사망여부를 예측하며 첫 24시간 데이터의 경우 2, 3일 mortality를 예측할 수 있지만 48시간 데이터의 경우 3일 mortality를 예측할 수 있다.
  - Long-term mortality prediction: 첫 24시간, 48시간 데이터를 이용하여 30일, 1년 안에 환자의 사망여부를 예측	

- ICD-9 code group prediction(multi-task prediction)

  ICD-9 diagnosis code를 예측하는 것이며, ICD 코드들은 다양한 증상, 징후,  부상이나 질병의 원인 등을 분류하는데 사용됩니다. 20개의 그룹으로 나뉘어져있다.
  ICD-9 코드 그룹 760-779는 신생아에게 분류되는 코드이기 때문에 어른을 대상으로 하는 벤치마킹 연구에서 제외하였다.

- Length of stay prediction(regression problem)
  각 admission에 대한 입원 기간을 예측하였다.

  

## Prediction algorithms

### Scoring methods

- SAPS-II : 이 점수는 ICU에 입원한 환자들 질병의 중증도 측정하기 위해 개발되었다. 12개의 feature들을 이용해 계산할 수 있다.  SAPS-II를 이용한 mortality 예측은 다음과 같다.

    $$
    log{\frac{p_{death}}{1-P_{death}}} = −7.7631 + 0.0737·S + 0.9971·log(1 + S)
    $$

- SOFA: ICU에 입원한 환자들의 장기부전 및 예후 평가를 위한 점수이다. SOFA를 기반으로 한 mortality 예측은 로지스틱 회귀 모델을 사용하여 얻을 수 있다.

- New SAPS-II: SAPS-II의 수정된 버전이며 SAPS-II에 사용된 feature들을 사용하여 로지스틱 회귀 모델을 사용하여 얻을 수 있다.

### Super Learner models

super learner는 예측 알고리즘의 최적의 조합을 찾기위해 개발된 supervised learning algorithm이며 cross-validation 이론을 기반으로 한다. 본 연구에서는 Super Learner I: 범주화된 변수들과의 super learner와 Super Learner II: 변환되지 않은 변수들의 super learner 두가지를 고려하였다. Super Learner-I는 feature set A만 적용되고 Super Learner-II는 feature set 3가지 모두 사용이 가능하다. 

### Deep Learning models

딥러닝 모델은 end-to-end training을 위해 복잡한 데이터 representation을 자동으로 추출하는데 성공적인 접근 방식이다. 본 연구에서는 Feedforward neural networks(FFN)과 Recurrent Neural Network(RNN)의 ensemble인 `Multimodal Deep Learning Model(MMDL)`을 제안한다. 이 모델은 공유된 representation layer를 사용하여 modality들의 유사성을 학습하는 것이다. EHR과 같이 각 테이블 별로 별도의 modality를 가지는 데이터에 유용하다.



<img src="/assets/img/post-img/benchmarking-deep-learning-on-mimic/deeplearning-model-structure.png" width="600px">



MMDL은 비시계열 데이터와 시계열 데이터를 개별적으로 다루는데 비시계열 데이터는 FFN에 시계열 데이터는 GRU에 input으로 들어가고 나온 ouput을 공유된 latent representaton layer에 combine한다.



## Implemetation details

본 연구에서는 Theano와 Keras로 시행하였고 5-fold cross validation을 실시하였고 5개의 testing fold의 성능 점수의 평균과 표준 에러를 구하였다. 평가 metric으로classfication 문제의 경우 AUROC와 AUPRC를 사용하였고, regression 문제의 경우 MSE를 사용하였다. parameter 튜닝의 경우, 성능의 큰차이를 보이지 않아 각 알고리즘의 기본 hyper parameter를 세팅하였다.

딥러닝 모델에서는 learning rate는 classificaton 문제는  0.001, regression 문제는 0.005로 두었고 RMSProp optimizer로 train하였다. activation function은 ReLU를 사용하였고 dropout rate은 0.1, batch sizesms 100, max epoch number는 250으로 세팅하였고 early stopping을 사용해서 최적의 weight를 구하고 batch normalization을 사용하였다. 위에서 언급했듯이 비시계열 데이터는 FFN, 시계열 데이터는 GRU로 다뤘다.



## Results

대부분의 예측 결과 MMDL의 성능이 더 좋았다는 것을 얘기하고 있고 feature들 중에서는 전처리를 하지 않고 다른 feature set들보다 많은 수의 feature들을 가진 feature set C가 더 좋은 점수를 얻었다. 따라서 본 연구에서는 원본의 임상 시계열 데이터가  예측 모델의 input feature로 사용되는 경우, 딥러닝 모델이 머신러닝 앙상블, SAPS II와 SOFA 점수를 이용한 예측보다 성능이 우수하다는 것을 보여준다.

----

# 이 논문을 읽고

주로 lab event 데이터를 사용하였고 결론으론 머신러닝이나 SAPS-II, SOFA와 같은 method보다 딥러닝 모델의 성능이 좋았다는 것을 말하고 있다. 이 논문을 읽고 나서 다른 머신러닝 모델보다 성능이 뛰어나다는 것은 이해했지만 이 모델이 실제로 활용 가능한지는 의문이 들었다. regression task의 경우 MSE는 어느 기준까지가 좋은 성능을 나타내는지 개인적으로 궁금하다. 

논문에서는 fine-tuning이 성능을 크게 좌지우지 하지 않는 것을 확인하고 기본값의 hyper parameter를 사용하는데 이 부분이 이 데이터셋에서만 그러는 건지 다른 데이터셋에도 그러는 건지 궁금해졌다.

---

$Reference.$
- [Benchmarking deep learning models on large healthcare datasets](https://www.sciencedirect.com/science/article/pii/S1532046418300716)
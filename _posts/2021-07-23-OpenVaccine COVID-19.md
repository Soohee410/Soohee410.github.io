---
layout: post
title: "[캐글 필사] OpenVaccine: COVID-19 mRNA Vaccine Degradation Prediction"
date:   2021-07-23
image: '/assets/img/covers/covid-19.png'
tags: [Kaggle]
use_math: True
---
캐글 필사를 하기로 한 [OpenVaccine: COVID-19 mRNA Vaccine Degradation Prediction](https://www.kaggle.com/c/stanford-covid-vaccine) 대회는 바로 시작하기엔 너무 버거웠다😂 기초지식이 없잖아?ㅠㅠ 그래서 필사하기 전에 기초지식을 쌓아보았다. 대부분의 내용은 [tootouch님의 깃헙 블로그](https://tootouch.github.io/research/ai_against_covid19/)를 참고하였다. 목차는 다음과 같다.
>1. 코로나 바이러스란?
2. RNA란?
3. RNA가 DNA보다 불안정한 이유
4. RNA 바이러스가 돌연변이를 자주 일으키는 이유
5. mRNA가 백신으로 유력한 이유
6. OpenVaccine: COVID-19 mRNA Vaccine Degradation Prediction<br>
    6-1. 개요<br>
    6-2. 목적<br>
    6-3. 데이터<br>
    6-4. 평가 방법<br>

---

## 1. 코로나 바이러스란?

코로나 바이러스는 **사람과 동물의 호흡기와 소화기계 감염을 유발하는 바이러스**로서 유전자 크기 27~32kb의 `RNA 바이러스`이다. 코로나 바이러스엔 7가지 유형이 있다. 이 중 인간 코로나 바이러스는 오래 전부터 사람에게 감염되어서 사람과 긴 시간에 걸쳐 함께 진화해 왔기 때문에 사람들이 **충분한 면역 능력**을 가지고 있다. 그래서 감기 등의 경미한 질환만을 일으키며 별다른 치료 없이 저절로 호전된다. 그러나 심한 면역저하환자에게는 중증 폐렴이 발생하기도 한다.

일반 인간 코로나 바이러스

1. 229E (alpha coronavirus)
2. NL63 (alpha coronavirus)
3. OC43 (beta coronavirus)
4. HKU1 (beta coronavirus)

그외 인간 바이러스

1. MERS-CoV (the beta coronavirus that causes Middle East Respiratory Syndrome, or MERS)
2. SARS-CoV (the beta coronavirus that causes severe acute respiratory syndrome, or SARS)
3. **SARS-CoV-2 (the novel coronavirus that causes coronavirus disease 2019, or COVID-19)**

인간 코로나 바이러스는 진화를 거듭하며 변화되기도 한다(5, 6, 7번). 이중 7번 유형이 2019년 새롭게 발견된 코로나 바이러스로 COVID-19이다. COVID-19는 사람 세포막에서 만들어지는 인지질 이중막 껍데기를 가지고 있다. 인지질막 때문에 바이러스의 외부 단백질의 `변이가 발생할 가능성이 높아지고` 그 결과 변이가 발생한 바이러스가 사람이 원래 가지고 있던 면역을 회피할 수 있는 확률이 증가한다. 코로나 바이러스는 인지질 껍질에 호흡기 점액질에 친화력을 가진 스파이크 단백질이 촘촘하게 박혀있다. 이 스파이크 단백질에 의해 호흡기 점막 친화성을 가지게 된다. 즉, 코로나 바이러스가 인후부의 상부호흡기와 기관지 이하의 하부호흡기 점막에 잘 부착할 수 있고 세포 안으로 잘 침투하여 쉽게 증식할 수 있다.

## 2. RNA란?

`DNA는 개개인마다 다른 고유의 유전 정보를 담고 있다.` 유전 정보는 어떠한 세포들을 만들어야 하는지 몸속 장기부터 전체적인 모습까지 어떻게 구성할지에 대한 정보라 할 수 있다. 결국 우리 몸에 필요한 단백질을 적재적소에 만들기 위한 정보이다. 이를 위해서는 누군가가 DNA가 지령한 정보를 **해석**한 뒤 단백질을 만드는 곳에 **전달**해야한다. 이 역할을 RNA가 한다. 

<img src="/assets/img/post-img/covid-19/covid-19-1.png" width="700px">

## 3. RNA가 DNA보다 불안정한 이유

RNA는 리보오스라고 부르는 당 분자에 인산과 염기가 붙은 구조인 것에 반해 DNA는 다른 부분은 다 동일한데 리보오스가 아닌 디옥시리보오스를 갖고 있다. RNA의 리보오스에 수산기(-OH)가 붙어있는 위치에 DNA의 디옥시리보오스에는 `산소가 빠진` 수소(-H)가 붙어있다. RNA가 가지고 있는 수산기(-OH)는 화학반응에 적극적이라는 특성이 있고 그 결과 남들과 반응을 잘하는 불안정하다는 뜻이다.

<img src="/assets/img/post-img/covid-19/covid-19-2.png" width="700px">

## 4. RNA 바이러스가 돌연변이를 자주 일으키는 이유

DNA 바이러스는 자체적으로 오타를 감지하고 수정하는 기능을 갖고 있지만 `RNA 바이러스는 자신의 유전자를 복제하는 동안 돌연변이가 발생해도 고치지 못한다.` 즉, 유전정보를 베껴쓰는 과정에서 오타가 나와도 수정을 못하는 것과 같다. 이 때문에 RNA 바이러스에서 돌연변이가 발생할 확률은 DNA 바이러스의 10만~100만배 이상이다. RNA 바이러스를 막는 백신을 만들어도 변이가 되면서 백신이 무용지물이 될 수 있다. **그래도 코로나 바이러스에는 돌연변이 수정 기능이 일부 있어 다른 RNA 바이러스에 비해 변이될 가능성이 낮다.**

## 5. mRNA가 백신으로 유력한 이유

mRNA는 DNA 원형으로부터 전사되고 유전정보에 암호(코돈)를 부여하여 리보솜(단백질 합성이 일어나는 곳)으로 운반하는 역할을 한다. mRNA 백신은 항원의 정보를 가지는 mRNA를 체내에 주입해 `인체의 면역계가 항체를 형성하도록 유도하는 방식`이다. COVID-19 바이러스는 바이러스의 표면에 뾰족뾰족 돌기처럼 솟아있는 스파이크 단백질(S-protein)이 인간 세포 표면의 수용체인 ACE2와 결합해 문제를 일으키는데 COVID-19 mRNA 백신은 이 바이러스 돌기인 **스파이크 성분을 체내에 미리 만들라는 지시를 내린다.** 지시를 받은 리보솜은 돌기를 만들어내고 항체는 이 반응을 메모리에 저장하여 `바이러스에 대한 면역력을 생성`하게 하는 원리이다.

<img src="/assets/img/post-img/covid-19/covid-19-3.png" width="700px">

mRNA 백신이 COVID-19의 유망한 백신 후보로 추앙받고 있는 이유는 백신 개발에 필요한 비용과 활용가치 때문이다. mRNA는 `신속하게 생산해 낼 수 있고 저비용으로 제조`할 수 있다는 장점을 가지고 있다. 다만, mRNA가 가진 장점에도 불구하고 단점이 있기 마련이다. mRNA가 가진 문제점을 해결하고자 스탠포드 대학에서 캐글에 본인들의 데이터를 오픈하였다.

## 6. OpenVaccine: COVID-19 mRNA Vaccine Degradation Prediction

### 6-1. 개요

가장 큰 과제는 mRNA를 `안정성있는 분자`로 설계하는 방법이다. 기존 백신은 일회용 주사기에 포장되어 전 세계적으로 배송되지만 mRNA 백신은 불가능하다. RNA 분자는 `자발적으로 분해되는 경향`이 있어 **한 번의 절단으로 mRNA 백신이 무용지물**이 될 수 있다. RNA의 백본에서 **어떤 위치가 영향을 받기 쉬운지**는 아직 밝혀지지 않았다. 이런 정보가 없으면 COVID-19에 대한 mRNA 백신은 `강력한 냉장 상태에서 준비 및 배송`되어야 하며 안정화되지 않는 한 지구상의 **극히 일부의 지역에는 도달할 가능성이 없다.**

<img src="/assets/img/post-img/covid-19/covid-19-4.png" width="700px">

### 6-2. 목적

이 대회의 목적은 `각 RNA 분자의 각 염기에서 가능한 분해 속도와 각 위치에서의 분해 속도를 예측하는 모델`을 만드는 것이고 데이터는 Eterna에서 제공하는 **3,000개의 RNA 분자데이터**를 사용한다. 그 다음 Eterna가 COVID-19 mRNA 백신을 위해 고안한 **2세대 RNA 서열을 바탕으로 모델의 점수를 매길 것**이다. 최종 평가는 스탠포드 대학에서 실험되고 네이처(Nature)에서 모델에 점수를 매긴다고 한다.

### 6-3. 데이터

train 데이터에는 여러 개의 정답값이 제공되는데 제출 형식은 5가지 모두를 예측해야하지만 **reactivity**, **deg_Mg_pH10**, **deg_Mg_50C**만 채점하게 된다. 파일은 train.json, test.json, sample_submission.csv가 주어진다. 컬럼의 설명은 아래와 같다.

| Column              | Description                                                                          |
|--------------------------|--------------------------------------------------------------------------------------|
| id                  | 각 샘플에 대한 임의의 식별자                                                         |
| seq_scored          | 예측 값으로 점수를 매기는 데 사용되는 위치의 수를 나타내는 정수값                    |
| seq_length          | 서열 길이를 나타내는 정수값                                                          |
| sequence            | `A`, `G`, `U`, `C`를 조합한 RNA 서열                                                 |
| structure           | 염기가 짝을 이루거나 이루지 못한 것 표기                                             |
| reactivity          | RNA 샘플의 가능한 이차 구조를 결정                                                   |
| deg_pH10            | 높은 pH(pH 10)에서 마그네슘 없이 배양한 후 염기/결합에서 분해 가능성을 결정          |
| deg_Mg_pH10         | pH(pH 10)에서 마그네슘과 함께 인큐베이션한 후 염기/결합에서 분해 가능성을 결정       |
| deg_50C             | 고온(섭씨 50도)에서 마그네슘 없이 배양한 후 염기/결합에서 분해 가능성을 결정         |
| deg_Mg_50C          | 고온(섭씨 50도)에서 마그네슘과 함께 인큐베이션한 후 염기/결합에서 분해 가능성을 결정 |
| *_error_*           | reactivity 또는 deg_*열에서 얻은 실험 값의 계산된 오류                               |
| predicted_loop_type | Vienna RNAfold2 구조의 bpRNA에서 지정된 루프 유형                                    |

### 6-4. 평가 방법

평가에 사용되는 metric은 **MCRMSE**(mean column wise root mean squared error)이다. 각 기호는 $N_t$:  맞춰야하는 컬럼들의 갯수, $y$: 실제 값, $\hat{y}$: 예측 값이다. 제출 형식은 5가지를 예측해야 하지만 **reactivity, degMgpH10, degMg50C**만 채점된다. (3가지만 채점된다는 의미로 봤을 때 $N_t$는 3이지 않을까하고 예상한다🤔)

$\textrm{MCRMSE} = \frac{1}{N_{t}}\sum_{j=1}^{N_{t}}\sqrt{\frac{1}{n} \sum_{i=1}^{n} (y_{ij} - \hat{y}_{ij})^2}$

제출 파일의 예시는 다음과 같다. test 데이터의 각 샘플 id 별로 서열 포지션(seqpos)을 예측해야 한다. 만약 어떤 id의 서열 갯수가 107이라면 107번을 예측해야 한다. 샘플의 **seq_scored** 값보다 큰 위치는 점수가 매겨지지 않지만 제출할 때 필요하다.

```json
id_seqpos,reactivity,deg_Mg_pH10,deg_pH10,deg_Mg_50C,deg_50C    
id_00073f8be_0,0.1,0.3,0.2,0.5,0.4
id_00073f8be_1,0.3,0.2,0.5,0.4,0.2
id_00073f8be_2,0.5,0.4,0.2,0.1,0.2
etc.
```

 ---

$Reference.$
- [https://tootouch.github.io/research/ai_against_covid19/](https://tootouch.github.io/research/ai_against_covid19/)
- [http://ncov.mohw.go.kr/baroView.do](http://ncov.mohw.go.kr/baroView.do)
- [https://www.ksid.or.kr/rang_board/list.html?num=3414&code=ncov_faq](https://www.ksid.or.kr/rang_board/list.html?num=3414&code=ncov_faq)
- [https://m.dongascience.com/news.php?idx=7212](https://m.dongascience.com/news.php?idx=7212)
- [https://www.seoulin.co.kr/intro/covid-19-차세대-백신-플랫폼-mrna-백신/](https://www.seoulin.co.kr/intro/covid-19-%EC%B0%A8%EC%84%B8%EB%8C%80-%EB%B0%B1%EC%8B%A0-%ED%94%8C%EB%9E%AB%ED%8F%BC-mrna-%EB%B0%B1%EC%8B%A0/)
- [https://www.snohd.org/ImageRepository/Document?documentId=6076](https://www.snohd.org/ImageRepository/Document?documentId=6076)
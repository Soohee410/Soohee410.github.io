---
layout: post
title: "[Object Detection] Object detection in 2021"
date:   2021-12-05
image: '/assets/img/covers/linear-algebra.jpg'
tags: [Object Detection]
use_math: True
---
# Object Detection이란?

object detection은 사진이나 영상과 같은 디지털 이미지에서 물체를 찾아내고 그 물체가 어떤 것(예를 들면, 인간, 동물, 차 등)인지 판별하는 작업이다. 최종 목적은 "어떤 물체가 어디에 있는지"이다. 

# Object Detection 주요 알고리즘

딥러닝 기반의 object detection 알고리즘은 2-stage detector와 1-stage detector 두가지로 나뉜다. 

Two-stage detector는 deep feature를 이용하여 1. 물체의 영역을 찾아내고 2. 분류한다. 이 방법은 높은 detection 정확도를 가지지만 이미지 별로 추론 단계를 거치기 때문에 일반적으로 속도가 느리다. 

One-stage detector는 영역을 제안하지 않고 이미지 위의 bounding box를 예측한다. 이 알고리즘은 two-stage detector보다 속도가 빠르고 구조적으로 단순하다는 장점이 있지만 불규칙한 모양의 물체나 작은 물체들을 인식하는 task에서는 정확도가 떨어진다.

![스크린샷 2021-11-30 오후 1.25.07.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d237b0a0-e41f-44ba-b821-d976b75874ba/스크린샷_2021-11-30_오후_1.25.07.png)

- Two-stage object detection algorithms
    1. RCNN and SPPNet (2014)
    2. Fast RCNN and Faster RCNN (2015)
    3. Mask R-CNN (2017)
    4. Pyramid Networks/FPN (2017)
    5. G-RCNN (2021)
- One-stage object detection alogorithms
    1. YOLO (2016)
    2. SSD (2016)
    3. RetinaNet (2017)
    4. YOLOv3 (2018)
    5. YOLOv4 (2020)
    6. YOLOR (2021)

Object detection은 2가지 문제를 해결해야 한다. 

1. 임의의 물체 갯수를 찾아내기
2. 각 물체들이 어떤 건지 분류하고 bounding box 사이즈를 평가하기

# RCNN
1. 이미지를 input으로 집어 넣음
2. selective search 알고리즘을 통해 ROI(약 2000개)를 추출하고 잘라냄(*selectvie search: 객체와 주변간의 색감, 질감차이, 다른 물체에 애워쌓여있는지 여부 등을 파악해서 다양한 전략으로 물체의 위치를 파악하는 알고리즘)
3. 227x227 사이즈로 warping함 
4. warped image를 각각 CNN 모델에 집어 넣고 feature vector를 추출함
5. 각각 binary SVM classification, bounding box regression을 진행하여 결과를 도출함

# Fast RCNN

RCNN의 단점들을 보완한 모델.

- 227x227 사이즈로 warping 시키면서 성능 손실
- selective search를 통해 2000개의 이미지 proposal 후보를 모두 CNN 모델에 집어 넣으면서 시간이 매우 오래 걸림
- selective search나 SVM이 GPU를 사용하기엔 적합한 구조가 아님
- 뒷 부분에서 수행한 computation을 share하지 않음
1. selective search를 통해 region proposal을 뽑아냄
2. crop regrion을 그대로 가지고 있는채로 전체 이미지를 CNN 모델에 집어 넣음
3. ROI를 feature map 크기에 맞춰서 projection함
4. ROI pooling함(FC에 넣을 수 있게 같은 resolution의 feature map으로 바꿔줌)
    ROI pooling: 각각의 영역 크기가 비슷하게 section을 나눈 후 max pooling함
5. fixed length feature vector를 FCs에 집어 넣은 후 두 ouput layer로 나눔
6. Softmax를 이용한 Classfication과 Bounding box regression을 진행함

## Loss function

classfication은 log loss, bouding box regression은 smooth L1 loss를 사용함

# Faster RCNN

region proposal을 GPU를 통해 학습. Fast RCNN에서 selective search가 했던 일을 Faster RCNN에서 RPN이 대신 하고 나머지는 Fast RCNN과 동일함.
1. VGG net을 이용하여 feature map 추출
2. RPN 네트워크로 물체가 있을 법한 위치를 찾음
3. k개의 anchor box 사용(다양한 크기의 bounding box)
4. sliding window를 거쳐 각 위치에 대해 regression(물체가 있는지 없는지)과 classification을 수행함

 ---

$Reference.$
- [https://viso.ai/deep-learning/object-detection/](https://viso.ai/deep-learning/object-detection/)
- [https://nuggy875.tistory.com/21](https://nuggy875.tistory.com/21)
- [https://medium.com/zylapp/review-of-deep-learning-algorithms-for-object-detection-c1f3d437b852](https://medium.com/zylapp/review-of-deep-learning-algorithms-for-object-detection-c1f3d437b852)
- [https://arxiv.org/pdf/1908.03673.pdf](https://arxiv.org/pdf/1908.03673.pdf)
- [https://89douner.tistory.com/88](https://89douner.tistory.com/88)
- [https://deepsense.ai/region-of-interest-pooling-explained/](https://deepsense.ai/region-of-interest-pooling-explained/)
- [https://ganghee-lee.tistory.com/33](https://ganghee-lee.tistory.com/33)
---
layout: post
title: "[Object Detection] Object detection RCNN, Fast RCNN, Faster RCNN, YOLO"
date:   2021-12-05
image: '/assets/img/covers/object-detection.jpg'
tags: [Object Detection]
use_math: True
---
이번 포스팅은 object detection의 기본 개념과 RCNN, Fast RCNN, Faster RCNN, YOLO에 대해서 알아보고자 한다.

---

# Object Detection이란?

object detection은 사진이나 영상과 같은 디지털 이미지에서 `물체를 찾아내고` 그 `물체가 어떤 것`(예를 들면, 인간, 동물, 차 등)인지 판별하는 작업이다. 최종 목적은 **"어떤 물체가 어디에 있는지"**이다. 즉 2가지의 task를 해결해야 한다.

---

# Object Detection 주요 알고리즘

딥러닝 기반의 object detection 알고리즘은 2-stage detector와 1-stage detector 두가지로 나뉜다. 

Two-stage detector는 deep feature를 이용하여 `1. 물체의 영역을 찾아내고` `2. 분류합니다.` 이 방법은 높은 detection 정확도를 가지지만 이미지 별로 추론 단계를 거치기 때문에 일반적으로 `속도가 느리다.` 

One-stage detector는 영역을 제안하지 않고 이미지 위의 bounding box를 예측한다. 이 알고리즘은 two-stage detector보다 속도가 빠르고 구조적으로 단순하다는 장점이 있지만 불규칙한 모양의 물체나 작은 물체들을 인식하는 task에서는 `정확도가 떨어진다.`

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

**다시 정리하자면 Object detection은 아래의 2가지 문제를 해결해야 한다.**

1. 임의의 물체 갯수를 찾아내기
2. 각 물체들이 어떤 건지 분류하고 bounding box 사이즈를 평가하기

---

# RCNN

<img src="/assets/img/post-img/object-detection/RCNN.png" width="700px">

1. 이미지를 input으로 집어 넣음
2. selective search 알고리즘을 통해 ROI(약 2000개)를 추출하고 잘라냄(*selectvie search: 객체와 주변간의 색감, 질감차이, 다른 물체에 애워쌓여 있는지 여부 등을 파악해서 다양한 전략으로 물체의 위치를 파악하는 알고리즘)
3. 227x227 사이즈로 warping함 
4. warped image를 각각 CNN 모델에 집어 넣고 feature vector를 추출함
5. 각각 binary SVM classification, bounding box regression을 진행하여 결과를 도출함

---

# Fast RCNN

<img src="/assets/img/post-img/object-detection/FastRCNN.png" width="700px">

아래의 RCNN 단점들을 보완한 모델이며 이름에서 알 수 있듯이 `속도가 빨라진` 모델
- 227x227 사이즈로 warping 시키면서 성능 손실이 발생함
- selective search를 통해 2000개의 이미지 proposal 후보를 모두 CNN 모델에 집어 넣으면서 시간이 매우 오래 걸림
- selective search나 SVM이 GPU를 사용하기엔 적합한 구조가 아님
- 뒷 부분에서 수행한 computation을 share하지 않음

1. selective search를 통해 region proposal을 뽑아냄
2. crop regrion을 그대로 가지고 있는채로 전체 이미지를 CNN 모델에 집어 넣음
3. ROI를 feature map 크기에 맞춰서 projection함
4. ROI pooling함(FC에 넣을 수 있게 같은 resolution의 feature map으로 바꿔줌)
    ROI pooling: 각각의 영역 크기가 비슷하게 section을 나눈 후 max pooling함

<img src="/assets/img/post-img/object-detection/ROIpooling.png" width="300px">

5. fixed length feature vector를 FCs에 집어 넣은 후 두 ouput layer로 나눔
6. Softmax를 이용한 Classfication과 Bounding box regression을 진행함

## Fast RCNN의 Loss function

<img src="/assets/img/post-img/object-detection/FastRCNN-loss.png" width="500px">

classfication은 log loss, bouding box regression은 smooth L1 loss를 사용한다.

---

# Faster RCNN

<img src="/assets/img/post-img/object-detection/FasterRCNN.png" width="700px">

region proposal을 GPU를 통해 학습한다. Fast RCNN에서 selective search가 했던 일을 Faster RCNN에서 RPN이 대신 하고 나머지는 Fast RCNN과 동일하다.
1. VGG net을 이용하여 feature map 추출
2. RPN 네트워크로 물체가 있을 법한 위치를 찾음(RPN 구조는 아래 그림 참고)
<img src="/assets/img/post-img/object-detection/The-structure-of-RPN.png" width="700px">
3. k개의 anchor box 사용(다양한 크기의 bounding box)
4. sliding window를 거쳐 각 위치에 대해 regression(물체가 있는지 없는지)과 classification을 수행함

---

# YOLO

<img src="/assets/img/post-img/object-detection/YOLO.png" width="700px">

YOLO는 위의 모델들과 다르게 One-stage object detection alogorithm이다. YOLO는 아래의 특징들을 가지고 있다.
- 객체 검출을 하나의 회귀 문제로 보기 때문에 매우 빠름
- 예측을 할 때 이미지 전체를 처리하기 때문에 background error가 작음
- 훈련 단계에서 보지 못한 새로운 이미지에 대해서도 검출 정확도가 높음
- SOTA 객체 검출 모델에 비해 정확도(mAP)가 다소 떨어짐

1. input image를 S x S 그리드로 나눔. 어떤 객체의 중심이 특정 그리드 셀 안에 위치한다면 그 그리드 셀이 해당 객체를 검출해야 함
2. 각각의 그리드 셀은 B개의 `bouding box`와 그 bounding box의 `confidence score`를 예측함
    - bounding box: 정규화된 x, y, w, h
    - confidence score: 객체 포함 확률 * bouding box 정확도
3. 각각의 그리드 셀은 conditional class probabilities=$Pr(Class_i\|Object)$ 예측
    - 객체가 있다는 조건 하에 어떤 클래스인지 조건부 확률
    - B의 개수와는 무관하게 하나의 그리드 셀에서는 클래스 하나만 예측
<img src="/assets/img/post-img/object-detection/YOLOdetail.png" width="700px">
4. 마지막 계층에는 linear activation function을 적용, 나머지 모든 계층에는 leaky ReLU 적용

## YOLO의 loss function
최적화를 쉽게 하기 위해 sum-squared error를 사용하였다.
- localization loss: bounding box 위치 예측
- classification loss: 클래스 예측
YOLO는 아래의 문제점을 보완하기 위해 loss를 개선하여 사용한다.
- 문제점1: 배경 영역이 객체가 없는 영역보다 더 크기 때문에 불균형을 초래함
    - 개선법: localization loss의 가중치를 증가시키고 그 중에서 객체가 없는 그리드 셀의 confidence loss보다 객체가 있는 confidence loss의 가중치를 증가시킴($λ_{coord}$와 $λ_{noobj}$로 조절)
- 문제점2: 큰 bounding box와 작은 bouding box에 대해 동일한 가중치로 loss를 계산함(작은 bouding box는 위치 변화에 더 민감함)
    - 개선법: width와 height에 square root를 취해줌

<img src="/assets/img/post-img/object-detection/YOLOloss.png" width="700px">

- $1_i^obj$: 그리드 셀 i 안에 객체가 존재하는지 여부
    - 객체가 존재하면 1, 존재하지 않으면 0
- $1_ij^obj$: 그리드 셀 i의 j번째 bounding box predictor가 사용되는지 여부

1. Object가 존재하는 그리드 셀 i의 bouding box predictor j에 대해, x와 y의 loss를 계산
2. Object가 존재하는 그리드 셀 i의 bounding box predictor j에 대해, w와 h의 loss를 계산
3. Object가 존재하는 그리드 셀 i의 bounding box predictor j에 대해, confidence score의 loss를 계산 ($C_i = 1$)
4. Object가 존재하지 않는 그리드 셀 i의 bounding box predictor j에 대해, confidence score의 loss를 계산. ($C_i = 0$)
5. Object가 존재하는 그리드 셀 i에 대해, conditional class probability의 loss를 계산. ($p_i(c)=1$ if class c is correct, otherwise: $p_i(c)=0$)

- $λ_{coord}$: coordinates(x, y, w, h)에 대한 loss와 다른 loss들과의 균형을 위한 balancing parameter.
- $λ_{noobj}$: 객체가 있는 box와 없는 box 간에 균형을 위한 balancing parameter. (일반적으로 image내에는 객체가 있는 그리드 셀보다는 없는 셀이 훨씬 많으므로)

## YOLO의 Inference

추론 단계에서 하나의 객체를 여러 그리드 셀이 동시에 검출하는 경우가 발생하기 떄문에 non-maximal suppression 방법을 적용한다. non-maximal suppression이란 가장 confidence가 높은 bounding box와 IOU가 일정 이상(보통 0.5)인 bounding box는 동일한 물체를 detect했다고 판단하여 지우는 작업(confidence score를 0으로 바꿈)이다. 코드를 보면 더 이해하기 쉽다.
    
```python
def nms(boxes, probs, threshold):
    """Non-Maximum supression.
    Args:
    boxes: array of [cx, cy, w, h] (center format)
    probs: array of probabilities
    threshold: two boxes are considered overlapping if their IOU is largher than
        this threshold
    form: 'center' or 'diagonal'
    Returns:
    keep: array of True or False.
    """
    
    order = probs.argsort()[::-1]
    keep = [True]*len(order)
    
    for i in range(len(order)-1):
    ovps = batch_iou(boxes[order[i+1:]], boxes[order[i]])
    for j, ov in enumerate(ovps):
        if ov > threshold:
        keep[order[j+i+1]] = False
    return keep

#출처: https://github.com/BichenWuUCB/squeezeDet/blob/master/src/utils/util.py
```

 ---

$Reference.$
- [viso.ai의 Object Detection in 2021](https://viso.ai/deep-learning/object-detection/)
- [nuggy875의 R-CNN: 딥러닝을 이용한 첫 2-stage Detector](https://nuggy875.tistory.com/21)
- [Review of Deep Learning Algorithms for Object Detection](https://medium.com/zylapp/review-of-deep-learning-algorithms-for-object-detection-c1f3d437b852)
- [Recent Advances in Deep Learning for Object Detection](https://arxiv.org/pdf/1908.03673.pdf)
- [89douner의 RCNN](https://89douner.tistory.com/88)
- [deepsense.ai의 Region of interest pooling explained](https://deepsense.ai/region-of-interest-pooling-explained/)
- [프라이데이의 컴퓨터비전에서의 기본 용어 및 개념 정리](https://ganghee-lee.tistory.com/33)
- [bkshin의 논문리뷰-YOLO(You Only Look Once) 톺아보기](https://bkshin.tistory.com/entry/%EB%85%BC%EB%AC%B8-%EB%A6%AC%EB%B7%B0-YOLOYou-Only-Look-Once)
- [A Survey of Modern Deep Learning based Object
Detection Models](https://arxiv.org/pdf/2104.11892.pdf)
- [Cucumber Fruits Detection in Greenhouses Based on Instance Segmentation](https://www.researchgate.net/publication/335895380_Cucumber_Fruits_Detection_in_Greenhouses_Based_on_Instance_Segmentation)
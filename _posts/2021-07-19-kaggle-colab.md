---
layout: post
title: "[Kaggle] 캐글 데이터셋 colab으로 다운받기"
date:   2021-07-19
image: '/assets/img/covers/kaggle-colab.jpg'
tags: [Kaggle]
use_math: True
---
새롭게 시작하는 캐글 스터디에서 캐글 필사를 하게 되었다. 내가 처음으로 필사하게 될 캐글 대회는 [OpenVaccine: COVID-19 mRNA Vaccine Degradation Prediction](https://www.kaggle.com/c/stanford-covid-vaccine/overview)를 골랐는데 이유는 끝난지 얼마되지 않아 최신 기법의 코드들을 사용했을 것이라 생각했고 코드 중에 관심있는 아키텍처들인 `GNN`과 `attention`이 있어서이다.<!--more--> 1등의 코드가 완벽하게 공개된 것은 아니지만 [솔루션](https://www.kaggle.com/c/stanford-covid-vaccine/discussion/189620#1042222)이 discussion에 공개되어있고 주 아이디어를 3개의 코드에서 가져왔다고 하여 3개의 코드를 필사하고 중간중간 이해 안가거나 모드른 부분들은 따로 공부해서 블로그에 올릴 예정이다. 우승자는 3개의 코드 중에서 [GRU 모델을 기반으로 한 코드](https://www.kaggle.com/xhlulu/openvaccine-simple-gru-model)가 기초가 되었고 [graph transformer with minimal fe]((https://www.kaggle.com/c/nfl-big-data-bowl-2020/discussion/119430))를 참고하여 <b>distance embedding</b>을 추가한 뒤 [[covid] AE pretrain + GNN + Attn + CNN](https://www.kaggle.com/mrkmakr/covid-ae-pretrain-gnn-attn-cnn)을 merge 하였다고 한다. 코드를 필사하기 전에 먼저 코랩 환경을 이용해서 캐글 데이터셋을 다운받았다. 방법은 엄청 간단하다! 캐글 데이터셋을 코랩에서 다운받은 법은 다음과 같다. 설명은 사진으로 보여주고 코드는 아래에 따로 추가해놓았다.

---

## API Token 만들기
먼저, 캐글 account 페이지에 들어가서 스크롤을 내리면 API가 나오는데 거기서 <b>Create New API Token</b> 버튼을 누른다. 만일 이미 API token을 만들었던 적이 있다면 옆에 있는 Expire API Token을 누른 뒤에 Create New API Token 버튼을 눌러야 한다.
<img src="/assets/img/post-img/kaggle-colab/kaggle-colab1.png" width="700px">


## API 파일 업로드하기
원하는 위치에 colabolatory를 새로 만들고 캐글 모듈을 설치한 뒤 위에서 다운받았던 <b>API 파일</b>을 코랩에 업로드해준다.
<img src="/assets/img/post-img/kaggle-colab/kaggle-colab2.png" width="700px">


## API 파일을 캐글 폴더에 복사
캐글 데이터셋을 저장할 디렉토리를 생성하고 위에서 코랩에 업로드한 API 파일을 캐글 디렉토리에 복사해주고 권한 변경 코드를 실행해준다.
<img src="/assets/img/post-img/kaggle-colab/kaggle-colab3.png" width="700px">

위의 코드들이 잘 실행되었는지 캐글 데이터셋 리스트를 확인해준다.
<img src="/assets/img/post-img/kaggle-colab/kaggle-colab4.png" width="700px">


## 캐글 데이터셋 다운로드
마지막으로 캐글 데이터셋을 다운로드해준다. 
<img src="/assets/img/post-img/kaggle-colab/kaggle-colab5.png" width="700px">

대회이름은 캐글 사이트 해당 대회의 Data에 들어가면 적혀있다.
<img src="/assets/img/post-img/kaggle-colab/kaggle-colab6.png" width="700px">

다운받은 파일 리스트 확인해보면 이렇게 zip파일로 되어있다.
<img src="/assets/img/post-img/kaggle-colab/kaggle-colab7.png" width="600px">

---

## 코드
```python
# 코랩으로 캐글 데이터 다운로드
from google.colab import files

!pip install -q kaggle #kaggle 설치
files.upload() #kaggle API file upload
!mkdir ~/.kaggle #kaggle 디렉토리 생성
!cp kaggle.json ~/.kaggle/ #kaggle.json 파일 kaggle 폴더에 복사
!chmod 600 ~/.kaggle/kaggle.json #권한변경
!kaggle datasets list #kaggle 데이터셋 리스트 체크
!kaggle competitions download -c stanford-covid-vaccine # stanford-covid-vaccine 데이터셋 다운로드
!ls # 다운받은 파일 리스트 확인

#구글 드라이브에 캐글 데이터셋 옮기기
import os
from google.colab import drive

os.environ['KAGGLE_CONFING_DIR'] = "/content/gdrive/MyDrive/data/kaggle/"
drive.mount('/content/gdrive')
!mv * /content/gdrive/MyDrive/data/kaggle

# 모든 파일 압축풀기
!cd /content/gdrive/MyDrive/data/kaggle && unzip \*.zip
```

---

$Reference.$
- [https://www.kaggle.com/general/74235](https://www.kaggle.com/general/74235)
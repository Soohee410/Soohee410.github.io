---
layout: post
title: "[Google Colab] 코랩에서 Kaggle dataset 검색 및 구글 드라이브에 다운로드"
tags: [삽질 기록]
use_math: true
---

저는 데이터셋을 캐글에서 얻는 경우가 정말 많은데요. 구글 코랩을 이용하는 저는 코랩에서 캐글 데이터를 바로 다운로드 받는 방법은 또 없을까 찾아보니까 역시나 있더라구요! [저번 포스트](https://soohee410.github.io/colab1)에서 코랩에서 데이터를 드라이브에 바로 저장하는 걸 알게 된 뒤로 제 삶의 질이 정말 많이 향상되었기 때문에(ㅠ_ㅠ), 이번에도 캐글 데이터를 코랩에서 바로 저장하는 것을 확실히 알아두고자 이렇게 포스팅하게 되었습니다~!
<br>

### 구글 코랩과 Kaggle 연동

먼저 캐글 사이트에 들어가서 자신의 account에 들어갑니다! API 카테고리에서 **Create New API Token** 을 클릭하면 kaggle.json 파일이 자동으로 저장됩니다.

<img src='/assets/colabkaggle6.PNG' width='720px'>

이제 구글 코랩으로 돌아와서 kaggle을 설치하고, kaggle.json 파일을 업로드합니다.

<img src='/assets/colabkaggle1.PNG' width='720px'>

다음과 같이 코드를 입력합니다. 이로써 Kaggle 연동 및 데이터셋을 다운로드하기 위한 세팅을 모두 마무리한 것입니다.

<img src='/assets/colabkaggle2.PNG' width='720px'>


### Kaggle Dataset 검색 및 다운로드

코랩에서 **-s** 태그를 이용해서 캐글 데이터를 검색할 수 있습니다. 예를 들면, 저는  캐글의 **english to french** 데이터를 코랩에서 다운로드 하고자 했는데요. 다음과 같이 코랩에서 검색하면, 데이터셋 제목에 english french가 들어간 데이터의 리스트가 출력됩니다. 저는 이 리스트에서 위에서 다섯번째 ``percevalw/englishfrench-translations`` 데이터를 다운받을 것입니다!

<img src='/assets/colabkaggle3.PNG' width='720px'>

이제 제가  원하는 데이터를 다운받기 위해, 해당 데이터셋 페이지로 들어가서 오른쪽 위에 있는 동그라미 3개를 클릭합니다. 그 곳에서 **Copy API command** 를 클릭하면 이 데이터셋의 다운로드 주소를 얻을 수 있습니다.
<img src='/assets/colabkaggle4.PNG' width='720px'>

이제 돌아와서 다음과 같이 데이터셋을 다운받고 압축도 해제합니다. ``!ls`` 명령어를 이용해서 데이터가 잘 다운되고 압축해제도 되었는지 확인합니다. 굳이 캐글 사이트에 들어가서 API 주소를 복사하지 않아도, ``!kaggle datasets download -d`` 명령어에 위 리스트에서 확인한 데이터셋의 ref를 입력해주면 되는 것 같습니다.

<img src='/assets/colabkaggle5.PNG' width='720px'>


### 캐글 데이터를 구글 드라이브에 저장

그런데 여기까지 하고 이제 끝인 줄 알았더니!! 다음날 들어가니까 다시 또 **kaggle.json** 파일을 업로드 하고 데이터 또 다운받고 그래야 하는 것 같더라구요..? 데이터가 커지면 여간 번거롭고 시간 낭비가 아닐 수 없습니다,, 그래서 저는 아예 캐글 데이터를 구글 드라이브에 저장해서 이 과정을 반복할 필요가 없도록 하기로 했습니다. 이를 위해 먼저 ``.kaggle`` 폴더를 구글 드라이브에 생성합니다. 그리고 그 폴더 안에 **kaggle.json** 파일을 업로드합니다. 저는 구글 드라이브에 data라는 폴더 안에 ``.kaggle`` 폴더를 생성했습니다!

<img src='/assets/colabkaggle7.PNG' width='720px'>

그 후에 저는 제가 참고한 [캐글 포럼](https://www.kaggle.com/general/51898)에서 이렇게 하라고 해서 하기는 했는데 이게 먹힌건지 아닌지는 잘 모르겠습니다.

<img src='/assets/colabkaggle8.PNG' width='720px'>

저는 일단 이 뒤로는 구글 드라이브 이용할 때 하는 것과 똑같이 했습니다! 먼저 구글 드라이브에 mount시키구요. 그 다음에 구글 드라이브 내에 ``.kaggle`` 폴더로 이동합니다.

<img src='/assets/colabkaggle9.PNG' width='720px'>


그 후에 위에서 했던 ``!kaggle~`` 명령어를 통해 캐글 데이터를 다운로드하고 압축해제하는 과정을 하면 됩니다! 그러면 구글 드라이브에 데이터가 다운로드 되고 매번 새로 kaggle을 연동할 필요 없이 저장한 데이터를 이용할 수 있습니다 :)

<img src='/assets/colabkaggle10.PNG' width='720px'>


<br>
따라서 전체 코드만 나열하면 다음과 같습니다!

- 코랩과 캐글 연동 및 다운로드
>!pip install kaggle  
from google.colab import files  
files.upload()  
!mkdir -p ~/.kaggle  
!cp kaggle.json ~/.kaggle/    
!chmod 600 ~/.kaggle/kaggle.json  
!kaggle datasets list -s english-french   
!kaggle datasets download -d percevalw/englishfrench-translations  
!unzip englishfrench-translations.zip  
!ls

- 구글 드라이브에 캐글 데이터 다운로드
> import os  
os.environ['KAGGLE_CONFIG_DIR'] = "/content/gdrive/My Drive/data/.kaggle/"   
from google.colab import drive  
drive.mount('/content/gdrive')  
cd /content/gdrive/My Drive/data/.kaggle  
!kaggle datasets download -d percevalw/englishfrench-translations  
!unzip englishfrench-translations.zip  
!ls  

<br>

---

$Reference$

- [stackoverflow: Using Kaggle Datasets in Google Colab
](https://stackoverflow.com/questions/49310470/using-kaggle-datasets-in-google-colab)
- [Easy way to use Kaggle datasets in Google Colab](https://www.kaggle.com/general/51898)

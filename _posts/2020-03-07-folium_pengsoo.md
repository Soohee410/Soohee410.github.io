---
layout: post
title: "[Visualization in Python] folium 마커 아이콘 펭수로 커스터마이징하기"
date:   2020-03-07
image: '/assets/img/pengsoo.gif'
tags: [Data Visualization]
use_math: false
---

안녕하세요! 오늘 포스팅은 재미로 시작했다가 저의 최애 시각화 결과물로 등극해버린 것을 소개하고자 합니다ㅋㅋㅋㅋ 이번에는 ``파이썬 지도 시각화 패키지 folium 제 2탄``입니다!! folium은 잘만 찾아보면 기능이 정말 많은데요. 저는 특히 여기 [folium 공식 예제 사이트](https://github.com/python-visualization/folium/tree/master/examples)에서 이것저것 잘 찾아봅니다. 그러다가 마커를 내 맘대로 커스터마이징할 수 있다는 것을 알게 되었는데요! folium을 사용해보신 분들은 알겠지만 기본 마커는 바로 아래 그림과 같이 저 파란색 대표적인 좌표 위치 모양입니다.

<img src='/assets/img/fo_icon1.PNG' width='600px'>

<br>그런데, 전형적인 좌표 아이콘 말고, 아래 그림처럼 정말 팬시하게 마커를 바꿔볼 수 있다는 것을 알게 되었습니다. 저 초록색, 노란색, 빨간색 잎가지가 마커입니다. 너무 예쁘지 않나요? 여기서 포인트는 저 잎가지들 밑에 생기는 바로 저 **그림자** 입니다! 그림자 덕분에 마커가 입체적으로 우뚝 서 있는 것처럼 보이고, 무엇보다 정확하게 어느 위치를 가리키고 있는지 알수 있게 됩니다. 이에 대한 [공식 예제 코드](https://github.com/python-visualization/folium/blob/master/examples/CustomIcon.ipynb)를 찾아보니 안그래도 저 **잎가지 이미지**와 **잎가지에 대한 그림자 이미지**, 이 두 가지 이미지가 필요하더라구요.

<img src='/assets/img/fo_icon2.PNG' width='600px'>


<br>그래서 저도 한번 해봤습니다! 바로 아래와 같이 펭수로 한번 귀엽게 마커를 찍어보았습니다. 아니 너무 귀엽지 않나요??ㅋㅋㅋㅋ popup 문구도 해놨기 때문에, 저 펭수를 꼭 클릭하셔야 합니다!! 위치는 경기도 일산 EBS 건물에 펭수 숙소가 지어졌다 길래 그곳 위치로 찍어보았습니다ㅎㅎ 이렇게 꼭 펭수 말고도, 자기가 좋아하는 어떠한 png 파일로 나만의 마커를 찍을 수 있습니다.






<br>

**코드**

```python
import folium
from folium.features import CustomIcon

m = folium.Map(location=[37.661863, 126.735475], zoom_start=12)


################ 첫번째 펭수
icon_image1 = 'data/펭수1.png'
shadow_image1 = 'data/펭수그림자1.png'

icon1 = CustomIcon(
    icon_image1,
    icon_size=(75, 95),
    icon_anchor=(10, 30),
    shadow_image=shadow_image1,
    shadow_size=(70, 65),
    shadow_anchor=(-10, -2),
    popup_anchor=(30, -30)
)

marker1 = folium.Marker(
    location=[37.661863, 126.755475],
    icon=icon1,
    popup=folium.Popup("여긴 펭수 숙소", max_width='100')
)
m.add_child(marker1)


############## 두번째 펭수
icon_image2 = 'data/펭수2.png'
shadow_image2 = 'data/펭수그림자2.png'

icon2 = CustomIcon(
    icon_image2,
    icon_size=(50, 95),
    icon_anchor=(0, 85),
    shadow_image=shadow_image2,
    shadow_size=(150, 60),
    shadow_anchor=(30, 50),
    popup_anchor=(30, -85)
)

marker2 = folium.Marker(
    location=[37.671053, 126.713676],
    icon=icon2,
    popup=folium.Popup("펭-하!", max_width='100')
)

m.add_child(marker2)
m
```
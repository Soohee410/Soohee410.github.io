---
layout: post
title: "[Python] 자잘한 코드들"
date:   2021-08-05
image: '/assets/img/covers/python-tips.jpg'
tags: [Tip]
use_math: false
---

python을 이용하여 코드를 짜면서 필요한 자잘한 코드들을 한곳에 모아두었다.<!--more--> 자주 사용하는데도 자주 까먹을 때가 많아서 적어두면 다시 보기에 좋고 공유하기도 좋을 것 같아서 하나씩 적으려 한다.

커다란 코드가 없을 수는 있지만 분명히 꼭 필요한 것들이 업데이트될 것이다!

## Dictionary에서 value를 중심으로 sorting하는 법

```python
d = {'a': 1, 'b': 3, 'c': 2}
sorted(d.items(), key=lambda x: x[1])

# 결과
[('a', 1), ('c', 2), ('b', 3)]
```

추가로 내림차순으로 정렬하고 싶을 땐 `reverse = True`를 추가해주면 된다.

```python
d = {'a': 1, 'b': 3, 'c': 2}
sorted(d.items(), key=lambda x: x[1], reverse = True)

# 결과
[('b', 3), ('c', 2), ('a', 1)]
```

## 아스키코드로 알파벳 리스트 생성하기
```python
list(map(chr, range(97, 123)))
```

## 알파벳 대문자, 소문자로 변환하기
```python
# test라는 단어가 있다면
'test'.upper() #대문자로 변환
'TEST'.lower() #소문자로 변환
```

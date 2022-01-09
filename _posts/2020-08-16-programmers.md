---
layout: post
title: "[Programmers] 2016년 | Python"
date:   2020-08-16
image: '/assets/img/covers/baekjoon.jpg'
tags: [Algorithm]
use_math: false
---

# 문제 
2016년 1월 1일은 금요일입니다. 2016년 a월 b일은 무슨 요일일까요? 두 수 a ,b를 입력받아 2016년 a월 b일이 무슨 요일인지 리턴하는 함수, solution을 완성하세요. 요일의 이름은 일요일부터 토요일까지 각각 ```SUN, MON, TUE, WED, THU, FRI, SAT```
입니다. 예를 들어 a=5, b=24라면 5월 24일은 화요일이므로 문자열 TUE를 반환하세요.

제한 조건
- 2016년은 윤년입니다.
- 2016년 a월 b일은 실제로 있는 날입니다. (13월 26일이나 2월 45일같은 날짜는 주어지지 않습니다)

입출력 예

|  a   |   b  |result|
| :--- | :--- | :--- |
|  5   |   24 |  TUE |

# 풀이
```python
days = [0,31,29,31,30,31,30,31,31,30,31,30,31]
week = ['FRI','SAT','SUN','MON','TUE','WED','THU']

def solution(a, b):
    total_days = 0
    for i in range(a):
        total_days += days[i]
    total_days += b
    answer = week[total_days%7-1]
    return answer
```

# 개선된 풀이
```python
import datetime

week = 'MON,TUE,WED,THU,FRI,SAT,SUN'.split(',')

def solution(a, b):
    answer = week[datetime.datetime(2016, a, b).weekday()]
    return answer
```

datetime을 이용해서 2016년의 월별 일자를 가져오고 weekday를 사용한다. weekday를 사용하면 월요일부터 순서대로 index로 변환된다. 
문제 설명에서 요일을 그대로 가져오고 일요일만 마지막 순서로 바꿔준 뒤 요일의 콤마(,)를 빼기 귀찮으니까 콤마(,)별로 split해준다.

## 잡담
---
풀이를 풀고 다른사람들의 풀이를 보면 함수를 잘 활용하는 것을 볼 수 있다. 나는 아직 머릿속에 잘 안 떠오르는데 많이 풀다보면 함수가 떠오르겠지??
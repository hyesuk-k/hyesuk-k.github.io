---
title:  "딥러닝 입문 2일차" 
excerpt: 최소한의 도구로 딥러닝을 시작합니다

categories:
  - 'dl'
tags:
  - AI
  - DL
  - python
  - book

toc: true
toc_sticky: true

date: 2021-05-29
last_modified_at: 2021-06-03

---

# 01. 구글 코랩 (Google Colab)

* 코랩이란?
  + 구글이 제공하는 주피터 노트북
  + colab: <https://colab.research.google.com/notebooks/intro.ipynb?hl=ko>
  + 상세 사용 방법은 해당 페이지 참고


# 02. Numpy

* Numpy
  + 파이썬에서 제공하는 머신너닝 / 딥러닝 패키지
  + colab에 기본 탑재되어 있음
 
```python
import numpy as np
print(np.__version__)
```

# 03. Numpy 예제

* Numpy를 이용하여 임의의 데이터를 생성
* 맽트플롯립으로 시각화 처리


```python
import numpy as np
from matplotlib import pyplot as plt

ys = 200 + np.random.randn(100)
x = [x for x in range(len(ys))]

plt.plot(x, ys, '-')
plt.fill_between(x, ys, 195, where=(ys > 195), facecolor='g', alpha=0.6)

plt.title("Sample Visualization")
plt.show()
```

* 선 그래프 그리기

```python
import numpy as np
from matplotlib import pyplot as plt

plt.plot([1, 2, 3, 4, 5], [1, 4, 9, 16, 25])
plt.show()
```

* 산점도 그리기

```python
import numpy as np
from matplotlib import pyplot as plt

plt.scatter([1, 2, 3, 4, 5], [1, 4, 9, 16, 25])
plt.show()
```

* 배열로 산점도 그리기

```python
import numpy as np
from matplotlib import pyplot as plt

x = np.random.randn(1000)
y = np.random.randn(1000)

plt.scatter(x, y)
plt.show()
``` 

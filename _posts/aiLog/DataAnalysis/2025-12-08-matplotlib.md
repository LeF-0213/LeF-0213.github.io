---
layout: post
related_posts:
  - /aiLog/da
title:  "Matplolib"
date:   2025-12-04
categories:
  - da
description: >
  
---
* toc
{:toc .large-only}

# [Matplotlib](https://matplotlib.org/)

Matplolib은 파이썬에서 데이터를 시각화하는 데 널리 사용되는 강력한 라이브러리이다. 다양한 그래프와 차트를 그릴 수 있으며, 선 그래프, 막대 그래프, 히스토그램, 산점도 등 기본적인 그래프부터 복잡한 3D 플롯까지 지원한다. 사용법이 비교적 간다하고 커스터마이징이 가능하여 데이터의 패턴과 트렌드를 효과적으로 표현할 수 있다.

또한, Numpy와 Pandas와 같은 데이터 분석 라이브러리와 잘 통합되어 데이터 과학, 머신러닝, 통계 등 다양한 분야에서 활용된다. Matplolib의 기본 모듈인 pyplot은 MATLAB과 유사한 인터페이스를 제공해 초보자도 쉽게 사용할 수 있도록 설계되었다.

```bash
pip install matplotlib
```

## 기본 구조

- Figure: 전체 그림(도화지)
- Axes: 하나의 서브 그래프(보통 x축, y축 포함)
- Axis: x축과 y축 자체

```python
import matplotlib.pyplot as plt
import numpy as np
```

### Axes 객체

### plot(), show()
* **plot()**: 선(line) 그래프나 점(Marker) 그래프를 형태로 그리는데 사용되는 가장 기본적인 함수이다. 일반적으로 2차원 데이터를 표현하는데 사용된다.
* **show()**: 생성된 Figure 객체와 그 안에 포함된 그래프(Axes)를 화면에 출력하고 표시하는 역할을 한다.

```python
plt.plot([1, 2, 3, 4]) # 하나일 때는 y축 기준
plt.plot([1, 2, 3, 4], [1, 2, 3, 4]) # 두 개일 때는 x축, y축 순서
plt.show()
```


### 스타일 옵션

```bash
# 한글 fonts-nanum 설치
!sudo apt-get install -y fonts-nanum
!sudo fc-cache -fv
!rm ~/.cache/matplotlib -rf
```

## [Seaborn](https://seaborn.pydata.org/)
Searborn은 Python의 데이터 시각화 라이브러리로, Matplotlib 위에 구축되어 데이터를 더 간단하고 쉽게 시각화할 수 있도록 설계되었다. 주로 통계적 데이터 시각화에 초점이 맞춰져 있으며, 다양한 차트와 스타일 옵션을 제공한다.

```python
import seaborn as sns
```

## [Folium](https://python-visualization.github.io/folium/latest/)

```
pip install folium
```
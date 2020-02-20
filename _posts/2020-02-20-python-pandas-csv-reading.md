---
title : python, pandas를 이용하여 csv 파일 열기
tags : python pandas csv
---

### pandas CSV 파일 읽기

```python
import pandas as pd

z_data = pd.read_csv('./summary_fft.csv', encoding = "utf-8")
```

CSV의 형태가 다음과 같을 때,
```
date,0,1,2,3,4
2020-02-20 9:05,3,10,10,9,4
2020-02-20 9:05,11,6,4,2,4
2020-02-20 9:05,15,8,2,2,3
```

``z_data['date'], z_data['0']`` 등의 형태로 각 Column에 접근 가능

이를 ``z_data['date'].values`` 이용하여 np.array로 변경 가능
단 np.array로 변경시 다른 타입으로 인식될 수 있음.
``np.array().asType(np.float)`` 등과 같은 방식으로 형식 통일 가능.

``z_data.loc[:,'0':'3550']`` 와 같은 방식으로 Header를 이용하여 DataFrame Slice 가능

``list(z_data)``로 각 Column의 헤더를 가져올 수 있음.

```python
# FFT가 된 csv를 읽어서 3D로 그려보기 위함.
import pandas as pd

import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D
import numpy as np

# Read data from a csv
z_data = pd.read_csv('./summary_fft.csv', encoding = "utf-8")

X = np.array(list(z_data)[1:]).astype(np.float)

Y = np.arange(len(z_data['date'].values)).astype(np.float)

Z = np.array(z_data.loc[:,'0':'3550'].values).astype(np.float)

fig = plt.figure()
ax = fig.gca(projection='3d')

X,Y = np.meshgrid(X,Y)

surf = ax.plot_surface(X,Y,Z,cmap='coolwarm',linewidth=0,antialiased=False)
wire = ax.plot_wireframe(X,Y,Z,color='r',linewidth=0.1)
fig.colorbar(surf,shrink=0.5,aspect=5)
fig.tight_layout()
plt.show()
```

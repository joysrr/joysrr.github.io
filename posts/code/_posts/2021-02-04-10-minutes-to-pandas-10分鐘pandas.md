# 官方文件翻譯版 [原文](https://pandas.pydata.org/docs/user_guide/10min.html)
## 還有一些自己的註解
##### 因為懶得看文件，只好藉由練習英文的藉口來試著翻譯看看並好好讀一下文件 ; )
***
此文章為新手面向的pandas套件簡短介紹，更詳細的說明可以查看此[文件(官方原文)](https://pandas.pydata.org/docs/user_guide/cookbook.html#cookbook)
> 詳細說明的文件不可能翻譯的 ><

一般來說起手式為

	In [1]: import numpy as np

	In [2]: import pandas as pd
	
## 物件的建立
詳細請查看[資料結構介紹(官方原文)](https://pandas.pydata.org/docs/user_guide/dsintro.html#dsintro)
首先讓我們先來產生一個[Series](https://pandas.pydata.org/docs/reference/api/pandas.Series.html#pandas.Series)
給他一個陣列，他會自行建立對應的數值索引

	In [3]: s = pd.Series([1, 3, 5, np.nan, 6, 8])
	
	In [4]: s
	Out[4]: 
	0    1.0
	1    3.0
	2    5.0
	3    NaN
	4    6.0
	5    8.0
	dtype: float64

再來產生一個[DataFrame](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.html#pandas.DataFrame)
給他一個NumPy的陣列

<!--stackedit_data:
eyJoaXN0b3J5IjpbOTk4MDA5OTQyLDE2MTg1NjcxODAsNzAwNT
U4NDgsLTMyMTM2OTUxMSwyMTIwNDcyNDEwXX0=
-->
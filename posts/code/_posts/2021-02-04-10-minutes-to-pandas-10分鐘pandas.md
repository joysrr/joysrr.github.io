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

	In [5]: dates = pd.date_range("20130101", periods=6)

	In [6]: dates
	Out[6]: 
	DatetimeIndex(['2013-01-01', '2013-01-02', '2013-01-03', '2013-01-04',
	 '2013-01-05', '2013-01-06'],
	 dtype='datetime64[ns]', freq='D')

再來指定他的索引為日期並且替欄位命名，就完成囉~

	In [7]: df = pd.DataFrame(np.random.randn(6, 4), index=dates, columns=list("ABCD"))

	In [8]: df
	Out[8]: 
	 A         B         C         D
	2013-01-01  0.469112 -0.282863 -1.509059 -1.135632
	2013-01-02  1.212112 -0.173215  0.119209 -1.044236
	2013-01-03 -0.861849 -2.104569 -0.494929  1.071804
	2013-01-04  0.721555 -0.706771 -1.039575  0.271860
	2013-01-05 -0.424972  0.567020  0.276232 -1.087401
	2013-01-06 -0.673690  0.113648 -1.478427  0.524988

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQzNTYwOTk1MiwxNjE4NTY3MTgwLDcwMD
U1ODQ4LC0zMjEzNjk1MTEsMjEyMDQ3MjQxMF19
-->
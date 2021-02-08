---
output:
  html_document:
    toc: true
    toc_depth: 2
---


### 官方文件翻譯版 [原文](https://pandas.pydata.org/docs/user_guide/10min.html)

> 還有一些自己的註解

>因為懶得看文件，只好藉由練習英文的藉口來試著翻譯看看並好好讀一下文件 ; )
***
此文章為**新手**面向的pandas簡短介紹，更詳細的說明可以查看此[文件(官方原文)](https://pandas.pydata.org/docs/user_guide/cookbook.html#cookbook)。
> 詳細說明的文件不可能翻譯的 ><

一般來說起手式為：

	In [1]: import numpy as np

	In [2]: import pandas as pd
	
## 物件的建立
詳細請查看[資料結構介紹(官方原文)](https://pandas.pydata.org/docs/user_guide/dsintro.html#dsintro)。
首先讓我們先來產生一個[`Series`](https://pandas.pydata.org/docs/reference/api/pandas.Series.html#pandas.Series)，給他一個陣列，他會自行建立對應的數值索引：

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

再來產生一個[`DataFrame`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.html#pandas.DataFrame)，給他一個`NumPy`的陣列：

	In [5]: dates = pd.date_range("20130101", periods=6)

	In [6]: dates
	Out[6]: 
	DatetimeIndex(['2013-01-01', '2013-01-02', '2013-01-03', '2013-01-04',
	 '2013-01-05', '2013-01-06'],
	 dtype='datetime64[ns]', freq='D')

再來指定他的索引為日期並且替欄位命名：

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

或者可以直接用`dict`直接建立：

	In [9]: df2 = pd.DataFrame(
	 ...:    {
	 ...:        "A": 1.0,
	 ...:        "B": pd.Timestamp("20130102"),
	 ...:        "C": pd.Series(1, index=list(range(4)), dtype="float32"),
	 ...:        "D": np.array([3] * 4, dtype="int32"),
	 ...:        "E": pd.Categorical(["test", "train", "test", "train"]),
	 ...:        "F": "foo",
	 ...:    }
	 ...: )
	 ...: 

	In [10]: df2
	Out[10]: 
	 A          B    C  D      E    F
	0  1.0 2013-01-02  1.0  3   test  foo
	1  1.0 2013-01-02  1.0  3  train  foo
	2  1.0 2013-01-02  1.0  3   test  foo
	3  1.0 2013-01-02  1.0  3  train  foo

欄位的型別可以不同：

	In [11]: df2.dtypes
	Out[11]: 
	A           float64
	B    datetime64[ns]
	C           float32
	D             int32
	E          category
	F            object
	dtype: object

如果你使用的是`IPython`，會把欄位自動加入Tab自動完成的屬性中，如下：

	In [12]: df2.<TAB>  # noqa: E225, E999
	df2.A                  df2.bool
	df2.abs                df2.boxplot
	df2.add                df2.C
	df2.add_prefix         df2.clip
	df2.add_suffix         df2.columns
	df2.align              df2.copy
	df2.all                df2.count
	df2.any                df2.combine
	df2.append             df2.D
	df2.apply              df2.describe
	df2.applymap           df2.diff
	df2.B                  df2.duplicated

依照上方結果顯示，欄位 `A`,  `B`,  `C`, `D`  已被設定到Tab自動完成的屬性中，而  `E`  and  `F`  同樣也是，但為了版面整潔，我們還是省略了吧!

## 資料呈現
詳細資料請查看[基礎介紹](https://pandas.pydata.org/docs/user_guide/basics.html#basics)。

以下示範如何顯示頭尾資料，

前五列資料：

	In [13]: df.head()
	Out[13]: 
	 A         B         C         D
	2013-01-01  0.469112 -0.282863 -1.509059 -1.135632
	2013-01-02  1.212112 -0.173215  0.119209 -1.044236
	2013-01-03 -0.861849 -2.104569 -0.494929  1.071804
	2013-01-04  0.721555 -0.706771 -1.039575  0.271860
	2013-01-05 -0.424972  0.567020  0.276232 -1.087401

最後五列資料：

	In [14]: df.tail(3)
	Out[14]: 
	 A         B         C         D
	2013-01-04  0.721555 -0.706771 -1.039575  0.271860
	2013-01-05 -0.424972  0.567020  0.276232 -1.087401
	2013-01-06 -0.673690  0.113648 -1.478427  0.524988

顯示索引及欄位：

	In [15]: df.index
	Out[15]: 
	DatetimeIndex(['2013-01-01', '2013-01-02', '2013-01-03', '2013-01-04',
	 '2013-01-05', '2013-01-06'],
	 dtype='datetime64[ns]', freq='D')

	In [16]: df.columns
	Out[16]: Index(['A', 'B', 'C', 'D'], dtype='object')

[`DataFrame.to_numpy()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_numpy.html#pandas.DataFrame.to_numpy "pandas.DataFrame.to_numpy") 可以轉換成`NumPy`格式的基本資料。
但要注意的是，如果你 [`DataFrame`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.html#pandas.DataFrame "pandas.DataFrame") 的欄位有多種型別，對於效能會有很大的影響，主要是因為`NumPy`和`pandas`有根本上的不同 ，而且 **NumPy 整個陣列只有一個型別, pandas的DataFrames則是每個欄位可以有一個型別**。
所以當你呼叫 [`DataFrame.to_numpy()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_numpy.html#pandas.DataFrame.to_numpy "pandas.DataFrame.to_numpy")，pandas 會找一個可以容下所有DataFrame中欄位型別的 NumPy dtype。
最終可能會導致型別會指向Python物件中的`object`型別。

當欄位的型別全部為浮點數時，[`DataFrame.to_numpy()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_numpy.html#pandas.DataFrame.to_numpy "pandas.DataFrame.to_numpy") 執行速度會很快而且也不需要透過資料複製產生：

	In [17]: df.to_numpy()
	Out[17]: 
	array([[ 0.4691, -0.2829, -1.5091, -1.1356],
	 [ 1.2121, -0.1732,  0.1192, -1.0442],
	 [-0.8618, -2.1046, -0.4949,  1.0718],
	 [ 0.7216, -0.7068, -1.0396,  0.2719],
	 [-0.425 ,  0.567 ,  0.2762, -1.0874],
	 [-0.6737,  0.1136, -1.4784,  0.525 ]])

而欄位包含多型別時， 相對來說[`DataFrame.to_numpy()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_numpy.html#pandas.DataFrame.to_numpy "pandas.DataFrame.to_numpy") 會較消耗資源：

	In [18]: df2.to_numpy()
	Out[18]: 
	array([[1.0, Timestamp('2013-01-02 00:00:00'), 1.0, 3, 'test', 'foo'],
	 [1.0, Timestamp('2013-01-02 00:00:00'), 1.0, 3, 'train', 'foo'],
	 [1.0, Timestamp('2013-01-02 00:00:00'), 1.0, 3, 'test', 'foo'],
	 [1.0, Timestamp('2013-01-02 00:00:00'), 1.0, 3, 'train', 'foo']],
	 dtype=object)


>[`DataFrame.to_numpy()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_numpy.html#pandas.DataFrame.to_numpy "pandas.DataFrame.to_numpy")  輸出中 **不包含** 索引以及欄位的標籤

[`describe()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.describe.html#pandas.DataFrame.describe "pandas.DataFrame.describe")  顯示簡單的統計資料：

	In [19]: df.describe()
	Out[19]: 
	 A         B         C         D
	count  6.000000  6.000000  6.000000  6.000000
	mean   0.073711 -0.431125 -0.687758 -0.233103
	std    0.843157  0.922818  0.779887  0.973118
	min   -0.861849 -2.104569 -1.509059 -1.135632
	25%   -0.611510 -0.600794 -1.368714 -1.076610
	50%    0.022070 -0.228039 -0.767252 -0.386188
	75%    0.658444  0.041933 -0.034326  0.461706
	max    1.212112  0.567020  0.276232  1.071804

Transposing your data:

	In [20]: df.T
	Out[20]: 
	 2013-01-01  2013-01-02  2013-01-03  2013-01-04  2013-01-05  2013-01-06
	A    0.469112    1.212112   -0.861849    0.721555   -0.424972   -0.673690
	B   -0.282863   -0.173215   -2.104569   -0.706771    0.567020    0.113648
	C   -1.509059    0.119209   -0.494929   -1.039575    0.276232   -1.478427
	D   -1.135632   -1.044236    1.071804    0.271860   -1.087401    0.524988

Sorting by an axis:

	In [21]: df.sort_index(axis=1, ascending=False)
	Out[21]: 
	 D         C         B         A
	2013-01-01 -1.135632 -1.509059 -0.282863  0.469112
	2013-01-02 -1.044236  0.119209 -0.173215  1.212112
	2013-01-03  1.071804 -0.494929 -2.104569 -0.861849
	2013-01-04  0.271860 -1.039575 -0.706771  0.721555
	2013-01-05 -1.087401  0.276232  0.567020 -0.424972
	2013-01-06  0.524988 -1.478427  0.113648 -0.673690

Sorting by values:

	In [22]: df.sort_values(by="B")
	Out[22]: 
	 A         B         C         D
	2013-01-03 -0.861849 -2.104569 -0.494929  1.071804
	2013-01-04  0.721555 -0.706771 -1.039575  0.271860
	2013-01-01  0.469112 -0.282863 -1.509059 -1.135632
	2013-01-02  1.212112 -0.173215  0.119209 -1.044236
	2013-01-06 -0.673690  0.113648 -1.478427  0.524988
	2013-01-05 -0.424972  0.567020  0.276232 -1.087401

## 未完待續...
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIzMzQyMTQ4NiwtNTcyNDc1MTEyLC0xNj
Y2MDg0MzA2LDc0NzY2MzYyMiwtNDE0MTgxMzkzXX0=
-->
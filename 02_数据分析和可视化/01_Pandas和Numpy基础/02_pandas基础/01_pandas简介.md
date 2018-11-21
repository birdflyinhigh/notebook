## Pandas基础

### 前言

numpy的局限性：

+ 无法使用列名作为索引
+ 每个ndarray只支持一种数据类型，无法实现同时数字和字符串的处理
+ 包含太多低级的方法。

<p style="color: orange"> Pandas完美的解决了上诉问题！numpy的语法在pandas同样适合！</p> 

本文将学习：

+ pandas的两种类型： dataframes series
+ 如何使用行或列标签选择数据
+ 一系列pandas探索数据方法
+ pandas如何赋值
+ 适合使用布尔索引进行选择和赋值

数据源csv地址： [f500.csv](https://dsserver-prod-resources-1.s3.amazonaws.com/291/f500.csv?versionId=N5YxSNt3PP16vi5ziFaVyNlrDzR_Vj2r)

### 1. 理解pandas和numpy

```python
import pandas as pd
f500 = pd.read_csv("f500.csv", index_col=0)
f500.index.name = None
f500_type = type(f500)
f500_shape = f500.shape
```

### 2. 理解dataframe(2D数组)

![anatomy of a dataframe](https://s3.amazonaws.com/dq-content/291/df_anatomy.svg)

+ 图片说明：
   + dataframe包含二维数组，两个周，**index**和**colume** 
   + 数据类型可以包含**整型**， **浮点型**，**字符串型**
   + 轴 index和colume包含字符串类型的**标签**

```python
df.dtypes  # 查看元素的类型，包括int64, float64, object等
```

+ 当使用pandas导入数据的时候，pandas会自动判断数据的类型，准确率也很好
+ 常见的pandas操作方法

```python
# 获取df的前5行
df.head(5)
# 获取df的后5行
df.tail(5)
# 获取df的整体信息
df.info()
```

### 3. DF 选择列

+ df 选择数据的主要方式是 df.loc[] 方法

```python
df.loc[row, column]
# 对应的row和column可以是以下任意一个类型
# 1. 单个标签
# 2. 列表或者标签列表
# 3. 标签切片
# 4. 布尔索引
```

![f500_selection dataframe](https://s3.amazonaws.com/dq-content/291/loc_original.svg)

![loc single column](https://s3.amazonaws.com/dq-content/291/loc_single.svg)

![loc list of columns](https://s3.amazonaws.com/dq-content/291/loc_list.svg)

![loc slice of columns](https://s3.amazonaws.com/dq-content/291/loc_slice.svg)

### 4. 列选择的简洁方式

+ 直接使用方括号【】

  ```python
  # df.loc[:, 'col1']   ==> df['col1']  # 可以为单个'col1', 也可以为列名列表list 
  ```

+ 使用.列名

  ```python
  # df.loc[:, 'col1']   ==> df.col1   # 只能针对单个col1 
  ```

+ df 简洁方式选择

  ```python
  # 使用列名选择 数据
  # 选择country列
  countries = df['country']
  
  # 选择revenues和years_on_global_500_list
  columns = ['revenues', 'years_on_global_500_list']
  revenues_years = df[columns]
  
  # 选择ceo--> sector的所有列
  ceo_to_sector = df.loc[:, 'ceo': 'sector']
  ```

### 5. series选择行

+ 一维数组在pandas中叫做**series**. 二维数组在pandas中叫做**dataframe** 

+ series选择行，可以使用loc或者【】

  ```python
  from pandas import Series
  s = Series([2,3,5,7], index=['a', 'b', 'c', 'd'])
  s['a']
  # 结果： 2
  s['a', 'c', 'd']
  # 结果：2， 5， 7
  s['c'：'d']
  # 结果： 5， 7
  ```

+ series选择行实例练习：

  ```python
  # df选择实例，选出ceo列，然后选出对应公司的ceo 
  # 列选择，生成一个ceos的series 1D数据
  ceos = df['ceo']  
  # 选出index为Walmart的ceo
  walmart = ceos['Walmart']
  # 选出index从apple到samsung的ceos
  apple_to_samsung = ceos['Apple': 'Samsung Electronics']
  # 选出 Exxon Mobil, BP, Chevron公司的ceos
  oil_companies_index = ['Exxon Mobil', 'BP', 'Chevron']
  oil_companies = ceos[oil_companies_index]
  ```

### 6. DF选择行

+ 选择单行

  ```python
  # 选择单行, 返回一个1D对象
  single_row = f500_selection.loc["Sinopec Group"]
  # 多行1
  slice_rows = f500_selection["State Grid":"Toyota Motor"]
  # 多行2
  list_rows = f500_selection.loc[["Toyota Motor", "Walmart"]]
  ```

### 总结： 使用通用公司： df.loc[row, column], row和column可以是单个标签，列表，标签切片，布尔索引

### 7、 Series和DF的Describe方法

```python
print(f500["country"].describe())
----------------------------------------------------------
count     500
unique     34
top       USA
freq      132
Name: country, dtype: object
-------------------------------------------------------------
print(f500.describe(include=['O']))
```

```python
profits_desc = f500["profits"].describe()
revenue_and_employees_desc = f500[["revenues", "employees"]].describe()
all_desc = f500.describe(include="all")
```

### 8. 其他方法

+ pandas在方法的使用上和numpy类似，比如

  ```python
  my_series = my_series + 10
  print(my_series)
  # 将在每个元素添加10，返回一个新的series 
  ```

  - [`Series.max()`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.max.html) and [`DataFrame.max()`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.max.html)
  - [`Series.min()`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.min.html) and [`DataFrame.min()`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.min.html)
  - [`Series.mean()`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.mean.html) and [`DataFrame.mean()`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.mean.html)
  - [`Series.median()`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.median.html) and [`DataFrame.median()`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.median.html)
  - [`Series.mode()`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.mode.html) and [`DataFrame.mode()`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.mode.html)
  - [`Series.sum()`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.sum.html) and [`DataFrame.sum()`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.sum.html)

+ series不需要指定轴，但是dataframe需要指定轴

  ```python
  # 计算列的值
  df.max(axis=0) # 或者
  df.max(axis='index')
  # 计算行的值
  df.max(axis=1) # 或者
  df.max(axis='column')
  ```

+ value_counts()方法--

```python
top5_countries = f500['country'].value_counts().head()
top5_previous_rank = f500['previous_rank'].value_counts().head()
max_f500 = f500.max(numeric_only=True)
```

### 9. Pandas赋值

+ pandas可以直接通过选中元素赋值，如下

  ```python
  top5_rank_revenue["revenues"] = 0
  top5_rank_revenue.loc["Sinopec Group", "revenues"] = 999
  ```

  ![boolean series](https://s3.amazonaws.com/dq-content/291/bool_series.svg)

![boolean indexing dataframe](https://s3.amazonaws.com/dq-content/291/boolean_indexing_df.svg)

![boolean indexing series](https://s3.amazonaws.com/dq-content/291/boolean_indexing_s.svg)

### 10. 使用布尔索引赋值

```python
f500.loc[f500["sector"] == "Motor Vehicles & Parts","sector"] = "Motor Vehicles and Parts"
prev_rank_before = f500["previous_rank"].value_counts(dropna=False).head()
```


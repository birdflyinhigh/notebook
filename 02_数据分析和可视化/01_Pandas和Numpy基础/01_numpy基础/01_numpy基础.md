## Numpy基础

### 1.理解矢量化操作

+ python之所以流行，是因为写程序确实方便。执行python程序的时候，程序交给python解释器执行。开发人员并不需要处理内存分配和清理等工作。
+ python属于高级语言，书写快，但是执行慢；C语言属于低级语言，书写慢，但是执行快。
+ Pandas和numpy能实现程序书写快，执行也快。主要的原因就是使用矢量化操作
+ 矢量化利用了一个被称为单指令多数据(SIMD)的处理器特性来更快地处理数据

### 2. 纽约机场出租数据

+ pickup_year: 行程年份

+ pickup_month: 行程月份

+ pickup_day： 行程日

+ pickup_location_code: 接驳地点

+ dropoff_location_code : 到达地点

+ trip_distance: 行程距离

+ trip_length: 行程花费时间

+ fare_amount: 行程费用

+ total_amount: 总费用

  [nyc_taxis.csv下载地址](https://dsserver-prod-resources-1.s3.amazonaws.com/289/nyc_taxis.csv?versionId=UBV8FKlpJm1QF3BJGrQEBlmAk7AQfX23)

  ```python
  import csv
  import numpy as np
  
  # import nyc_taxi.csv as a list of lists
  f = open('nyc_taxis.csv', 'r')
  
  # 去除首行
  taxi_list = list(csv.reader(f))[1:]
  
  # 将里面的数值转化为浮点型
  converted_taxi_list = []
  for row in taxi_list:
      converted_row = []
      for each in row:
          converted_row.append(float(each))
      converted_taxi_list.append(converted_row)
      
  # 生成ndarray对象  
  taxi = np.array(converted_taxi_list)
  
  ```

### 3. 理解Ndarray对象

+ ndarray对应“N-dimensional array' --N维数组

+ 主要处理1维数组和2维数组。

+ Ndarray里面的每个元素不一定是相同的类型

+ <b> ndarray属性</b>

  ```python
  # 定义一个元祖，（X， Y) 
  # 其中X表示数组有多少行，Y表示数组有多少列
  taxi.shape()
  ```

+ <b>行切片</b>

  ```python
  # 行切片
  taxi[1:]
  ```

  ```python
  taxi_ten = taxi[:10]
  print(taxi_ten)
  ```

### 4. Ndarray元素选择和切片

+ 通用公式

  ```python
  # 通用公式
  ['orange', 'blue', 'black', 'blue', 'purple']
  # row 和column可以是单个数字，也可以是一个切片, 也可以是一个列表比如【1， 3， 7】
  # 比如row-->3:5, column-->4,6, 表示选择3-5行，4-6列的数据
  ```

  ```python
  # 选择第一行
  row_0 = taxi[0]
  # 选择391到500行（包括500行）
  rows_391_to_500 = taxi[391:501]
  # 选择21行5列的元素
  row_21_column_5 = taxi[21,5]
  # 示例
  taxi[1:3, :3]
  # 示例
  taxi[0:3, 2:7]
  ```


### 5. 矢量计算

+ numpy比python list 快30倍。

+ 矢量计算：

  ```python
  import numpy as np
  # 现有列表
  my_numbers = [
                [6, 5],
                [1, 3],
                [5, 6],
                [1, 4],
                [3, 7],
                [5, 8],
                [3, 5],
                [8, 4]
               ]
  # 计算求和
  # 1 先将list of list转化为numpy ndarray 
  
  data = np.array(my_numbers)
  # 2 获取第一列和第二列的为1D数组
  column_01 = data[:, 0]
  column_02 = data[:, 1]
  # 第一列和第二列求和
  sum = column_01 + column_02 
  ```

+ 矢量计算可以使用加减乘除 

### 6. Numpy算术运算

[numpy常用算术方法链接](https://docs.scipy.org/doc/numpy-1.14.0/reference/routines.math.html#arithmetic-operations)

+ 1D数组常见计算

  ```python
  # 最大值
  trip.max()
  # 最小值
  trip.min()
  # 平均值
  trip.mean()
  # 求和
  trip.sum()
  ```

+ 2D数组常见计算

  ```python
  # 假如需要计算所有的数据
  ndarray.max()  # --->> 返回一个数，所有数据中的最大值
  # 假如需要计算每一行的最大值
  ndarray.max(axis=1)   # --->>返回一个1D数组
  # 计算每一列的最大值
  ndarray.max(axis=0)   # --->> 返回一个1D数组
  ```

### 7. 添加行或者列到数组

+ 两个数组要相加，shape必须一致

  ```python
  # 可以使用expand_dim()函数更改shape
  zeros_2d = np.expand_dims(zeros, axis=0)
  # 使用np.concatenate()函数实现合并
  combined = np.concatenate([ones, zeors_2d], axis=0)
  ```

### 8. 数组排序

+ 使用np.argsort()函数， np.argsort()返回的是一个是该数组排序的一个索引。比如：

  ```python
  # 对一下数组对象进行排序
  fruit = np.ndarray(['orange', 'banana', 'apple', 'grape', 'cherry'])
  # 排序，返回一个索引
  sorted_order = np.argsort(fruit)
  # 获取排序后的数组
  sorted_fruit = fruit[sorted_order]
  ```



  	




















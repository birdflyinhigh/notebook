## Numpy 布尔索引

### 1. Numpy读取csv文件

```python
# 使用Numpy读取csv文件
import numpy as np 
taxi = np.genfromtxt('nyc_taxi.csv', delimiter=',', skip_header=True)
# 获取元素的类型
taxi.dtype
```

### 2. 1D数组 布尔选择

+ 选择数组的方式有5种：

  + 数字选择
  + 切片选择
  + “：”选择
  + 列表选择
  + 布尔选择

+ 其中最有效的选择方式是布尔选择。

  ```python
  # 使用+进行计算
  np.array([2, 4, 6, 8]) + 10 
  # 结果： np.array([12,14, 16, 18])
  # 使用大于 >10 
  np.array([12,23,14,3]) > 10 
  # 结果： array([ True,  True, TRUE, False], dtype=bool)
  ```

+ 使用布尔索引进行筛选数据

  ```python
  # 创建数组
  data = np.array([80, 70, 60, 50])
  # 获取布尔索引
  data_bool = data > 50 
  # 筛选数据
  select_data = data[data_bool]
  # 结果： np.array([80, 70, 60])
  ```

### 3. 2D数组 布尔索引

+ 布尔索引<strong style="color:orange">可以和先前的任何一种选择数组的方式组合</strong>

  ```python
  arr[:, bool]
  ```

+ 如何获取平均速度大于50的行

  ```python
  # 1.选取包含平均速度的行
  avg_speed = taxi[:, 12]
  # 2. 获取boolean index 
  avg_speed_bool = avg_speed > 50 
  # 3. 获取大于50的行
  Avg_Speed_Filtered = taxi[avg_speed_bool, :]		
  ```

### 4. Numpy赋值

+ 先前主要学习了如何获取数据，增加行或者列

  ```python
  #赋值方法
  ndarray[location_of_values] = new_value
  # 比如
  a = np.array(['red','blue','black','blue','purple'])
  a[0] = 'orange'
  print(a)
  # 输出： ['orange', 'blue', 'black', 'blue', 'purple']
  taxi.copy()  #  可以获取一个numpy备份
  ```

+ Boolean index在赋值的时候非常厉害

  ```python
  taxi[bool] = 10
  ```

### 5. 实战-I

+ 实战-I： 找出最受欢迎的机场

```python
# 目标，找出最受欢迎的机场
# 获取数据
import numpy as np
taxi = np.genfromtxt('nyc_taxis.csv', delimiter=',', skip_header=True)
# 1. JFK 
dropoff_code = taxi[:, 6]
jfk = taxi[dropoff_code == 2]
jfk_count = jfk.shape[0]
# 2. laguardia
laguardia = taxi[dropoff_code == 3]
laguardia_count = laguardia.shape[0]
# 3. lnewark
newark = taxi[dropoff_code == 5]
newark_count = newark.shape[0]
# 4. 找出最大值
max_count = max([jfk_count, laguardia_count, newark_count])
```

+ 实战-II: 删除坏数据，然后做计算

```python
# 获取数据
import numpy as np

taxi = np.genfromtxt('taxi.csv', delimiter=',', skip_header=True)
# 计算平均时速
trip_mph = taxi[:, 7] / (taxi[:, 8] / 3600)
# 筛选小于100的行
cleaned_taxi = taxi[trip_mph < 100]
# 计算mean值
mean_distance = cleaned_taxi[:, 7].mean()
mean_length = cleaned_taxi[:, 8].mean()
mean_total_amount = cleaned_taxi[:, 13].mean()
# 将trip_mph加入到列
trip_mph = cleaned_taxi[:, 7] / (cleaned_taxi[:, 8] / 3600)
mean_mph = trip_mph.mean()
print(mean_distance, mean_length, mean_total_amount, mean_mph)

```

### 6. Numpy学习要点

+ 如何使用numpy.genfromtxt()读取数据
+ 如何处理NaN数据
+ 如何使用布尔索引
+ 如何在1D和2D中使用布尔索引筛选值
+ 如何给予索引赋值 
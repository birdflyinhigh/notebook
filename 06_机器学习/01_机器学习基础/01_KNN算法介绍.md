## K-Nearest Neighbors介绍

### 1. 问题定义(Problem Definition)

+ AirBnB是一家短租平台，允许用户将自己的公寓或者整栋房屋出租，由于出租是短期的，AirBnb已经成为酒店的替代品。从2008年成立至今，已经估值超过300亿美元，超过任何一家连锁酒店
+ 房东在发布房源的时候，面临一个问题： 如何确定最佳的房屋价格。因为AirBnB是一个租赁市场，其出租价格与市场动态紧密关联。以下是AirBnb的搜索页面：

![Imgur](https://s3.amazonaws.com/dq-content/airbnb.png)

+ 作为房东，房价不能定得太高或者太低，否则租户就选择其他房源了。
+ 为了决定最合适的价格，我们的策略是：
  + 找到和我们相似的房源列表
  + 算出和我们相似房源的评价价格
  + 将我们的房租价格设置为该平均价格
+ 机器学习的定义： <b>通过现有数据，找出其中的模式，然后做出预测</b>

### 2. 数据介绍

+ 数据 [dc_airbnb.csv](http://data.insideairbnb.com/united-states/dc/washington-dc/2015-10-03/data/listings.csv.gz)

### 3. KNN算法

+ KNN算法需要找出两个关键参数
  - similarity metric 
  - 如何选择K值

### 4. 欧几里何度量

+ 基本公式：

  ![](https://s3.amazonaws.com/dq-content/euclidean_distance_five_features.png)

+ 通过计算可住人数与3 的欧几里何度量。

  ```python
  import pandas as pd
  
  data = pd.read_csv('data.csv')
  
  data['distance'] = abs(data['accomodates'] - 3)
  print(data.distance.value_counts().sort_index())
  
  ```
















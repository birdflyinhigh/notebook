

## 使用Pandas进行数据探索

### 1. 前言

当我们使用numpy的时候，我们使用数字作为索引取数；我们使用pandas的时候，我们使用label（标签）取数。 大多数时候，pandas使用标签取数非常的方便和快捷。但是有时候也十分的繁杂，比如你想选择第十行和第十一行的数据时，你需要找到第10行对应的index，这样就不user-friendly了。

幸运的是，pandas由于底层使用的是numpy存储数据，pandas同样可以使用数字作为索引取数。

### 2. pandas常见的取数方式

+ pandas常见的取数方式

  + 方式一： df.loc(row, column), 其中的row 和colum是
    + 1.单个标签
    + 2.标签列表
    + 3.标签切片
    + 4.布尔索引
  + 方式二： df.iloc(row, coloumn)， 其中的row和column是
    + 1.单个数字
    + 2.数字列表
    + 3.数字切片
    + 4.布尔索引
  + loc和iloc的区别图示

  ![loc和iloc的区别](http://p2.pstatp.com/large/pgc-image/1539245181811e2c91139ee)

  + 选择整行或整列示例

```python
# 选择第一行
first_row = df.iloc[0, :]
# 选择第一列
first_column = df.iloc[:, 0]
```

 + 选择单行的简便方式

```python
third_row = df.iloc[2]
```

|          数字索引          |     iloc语法     |  简化语法  |
| :------------------------: | :--------------: | :--------: |
|         DF选择单列         |   df.iloc[:,3]   |            |
|     DF选择多列（列表）     | iloc[:,[3,5,6]]  |            |
|     DF选择多列（切片）     | df.iloc[:,3:7]]  |            |
|         DF选择单行         |   df.iloc[20]    |            |
|     DF选择多行（列表）     | df.iloc[[0,3,8]] |            |
|     DF选择多行（切片）     |   df.iloc[3:5]   |  df[3:5]   |
|       series单个元素       |    s.iloc[8]     |    s[8]    |
| series选择多个元素（列表） | s.iloc[[2,8,1]]  | s[[2,8,1]] |
| series选择多个元素（切片） |   s.iloc[5:10]   |  s[5:10]   |

### 3. Pandas读取csv文件

+ pandas读取csv文件常见的姿势

  ```python 
  import pandas as pd 
  data = pd.read_csv('data.csv', index_col=0)
  f500.index.name = None
  ```

+ 说明1： index_col=0的意思是，将第一列作为行标签，作为后面选择行标签的索引， 0表示第一列

+ 说明2：我们使用`DataFrame.index`访问索引轴属性，然后我们`index.name`用来访问索引轴的名称；将index.name设置为None的目的是删除名称。

  ***

  ```python
  import pandas as pd 
  f500 = pd.read_csv('f500.csv', index_col=0)
  f500.index.name = None
  f500.iloc[:5, :3]
  ```

  ![1539324967139](C:\Users\ADMINI~1\AppData\Local\Temp\1539324967139.png)

  ### 4. loc和iloc, 数字标签

  + 区别 iloc[1] 会永远选择第二行，loc[1]则会选择标签为1的行

  ![loc vs iloc for rows in different order](https://s3.amazonaws.com/dq-content/292/integer_labels_2.svg)

+ 需要记住的是，iloc永远选择的的是位置，loc选择的是index标签

+ 排序

  ```python
  sorted_emp = f500.sort_values("employees", ascending=False)
  top5_emp   = sorted_emp.iloc[:5]
  ```

  ### 5. 其他布尔筛选方法

  + series.str.contrains() 方法

    ```python
    # 选择 地点包含 CA的数据
    has_CA = f500['hq_location'].str.contains('CA')
    # 布尔索引
    ca = f500[has_CA ]
    ```

  + series.str.endswith()方法

    ```python
    # 选择以CA 结尾的数据
    is_ca = f500['hq_location'].str.endswith('CA')
    # 布尔索引
    ca = f500[is_ca]
    ```

  + series.isnull() 和series.notnull()方法

    ```python
    previously_ranked_notnull = f500[f500["previous_rank"].notnull()]
    previously_ranked_null = f500[f500["previous_rank"].isnull()]
    ```

  ### 6. 布尔运算符

  + 主要的运算符 “and" , "or" 和”not"

    | pandas语法 | python语法 | 意义 |
    | :--------: | ---------- | ---- |
    |   a & b    | a  and b   | 交集 |
    |   a \| b   | a or b     | 并集 |
    |     ~a     | not a      | 取反 |

  + 布尔运算符实战， 需求： 找出中国公司中利润大于265000的公司和利润

    ```python
    # 利润大于265000的行
    over_265 = f500['revenues'] > 265000
    # 在中国的行
    china = f500['country'] == 'China'
    # 公司在中国，且利润大于265000的公司和利润数据
    china_over_265 = f500[china & over_265， ['company', 'revenues']]
    ```

  ### 7. 索引赋值

  + 场景：现有dataframe food和series colors, food和colors都有相同的索引，如下图，将color series加到food dataframe的方法：

    ![align on index 1](https://s3.amazonaws.com/dq-content/292/align_index_1.svg)

  ```python
  food['color'] = color
  ```

  + 结果：

    ![align on index 2](https://s3.amazonaws.com/dq-content/292/align_index_2.svg)

  + 可以看到pandas已经将又脏又累的活帮我们干了，color会自动添加到相同index的行上。感受一下pandas的强大

  + 假如![align on index 4](https://s3.amazonaws.com/dq-content/292/align_index_4.svg)

    ```python
    food["alt_name"] = alt_name
    ```

  + 结果

    ![align on index 5](https://s3.amazonaws.com/dq-content/292/align_index_5.svg)

  + 可以看出，含有相同index的数据才会添加进去，索引不同或者没有的行，会用NAN补充。
# 开发|pandas模块
*整了一篇关于pandas模块的使用文章，方便检查自己的学习质量。自从使用了pandas之后，真的是被它的功能所震撼~~~*

## 前言
    目前所有的文章思想格式都是:知识+情感。
    知识:对于所有的知识点的描述。力求不含任何的自我感情色彩。
    情感:用我自己的方式，解读知识点。力求通俗易懂，完美透析知识。

## 正文
pandas是一个强大的Python数据分析的工具包，**是基于NumPy构建的**。**Python Data Analysis Library **( pandas )是为了解决数据分析任务而创建的。

所以，在使用pandas的时候，需要适当的回顾一下关于numpy的使用，多回顾是好事，防止遗忘。

### pandas 安装
一般使用Python的工具都是使用pip进行安装，只是因为使用的是Python3 ，所以使用的是pip3 进行安装，其实使用pip也不会报错~~
**安装方式: pip3 install pandas**
**引入方式: import pandas as pd**
**注意**: 如果安装了anaconda就不需要了pip了~

### Series
**1.Series是什么？**
Series是一种类似于一位数组的对象，由**一组数据和一组与之相关的数据标签（索引）组成**。

**2、Series 怎么创建？**
**注意**:前面一列表示的是索引(可以自己进行指定)，后面一列是自己的数据值。
```python
In [1]: import numpy as np

In [2]: import pandas as pd

In [3]: pd.Series([1, 2, 3, 4])
Out[3]:
0    1
1    2
2    3
3    4
dtype: int64

In [4]: pd.Series([11, 12, 13, 14], index=['a', 'b', 'c', 'd'])
Out[4]:
a    11
b    12
c    13
d    14
dtype: int64

In [5]: pd.Series(np.arange(10))
Out[5]:
0    0
1    1
2    2
3    3
4    4
5    5
6    6
7    7
8    8
9    9
dtype: int32

```

3.获取值数组和索引数组：**values属性和index属性**
**注意:**所谓的指定下标，只不过是显示的，底层还是基于原来的数字索引下标。
对于数组的所以，依然可以使用列表的方式。
```python
In [7]: s = pd.Series([11, 12, 13, 14], index=['a', 'b', 'c', 'd'])

In [8]: s
Out[8]:
a    11
b    12
c    13
d    14
dtype: int64

In [9]: s[0]
Out[9]: 11

In [10]: s[-1]
Out[10]: 14
```

**4.Series的使用特性**
1)Series比较像列表（数组）和字典的结合体。
2)具有如下特性:

    Series支持array的特性（下标）：
        从ndarray创建Series：Series(arr)
        与标量运算：sr*2
        两个Series运算：sr1+sr2
        索引：sr[0], sr[[1,2,4]]
        切片：sr[0:2]
        通用函数：np.abs(sr)
        布尔值过滤：sr[sr>0]
    Series支持字典的特性（标签）：
        从字典创建Series：Series(dic), 
        in运算：’a’ in sr
        键索引：sr['a'], sr[['a', 'b', ‘d']]
    
3)示例代码如下:
```python
In [11]: a
Out[11]:
a    4
b    5
c    6
d    7
dtype: int64

In [12]: a[a>5]
Out[12]:
c    6
d    7
dtype: int64
```

**5.Series 的索引选择问题？**
**注意**:由于自己可以指定数据的索引标签，所以在进行取出数据的时候，使用自己指定的索引标签呢？还是使用原生的0,1,2...索引？？
**解决**:如果索引是整数类型，则根进据整数行下标获取值时总是面向标签的。
**解决方法**：loc属性（将索引解释为标签）和iloc属性（将索引解释为下标）

**代码实现:**
```python
In [20]: s = pd.Series([11, 12, 13, 14], index=['a', 'b', 'c', 'd'])

In [21]: s
Out[21]:
a    11
b    12
c    13
d    14
dtype: int64

In [22]: s.loc['a']
Out[22]: 11

In [23]: s.iloc[0]
Out[23]: 11
```

**6.Series的数据根据索引匹配**
1)**注意**:pandas在进行两个Series对象的运算时，会**按索引标签进行对齐**，然后计算。
如果两个Series对象的索引不完全相同，则结果的索引是两个操作数索引的**并集**。如果只有一个对象在某索引下有值，则结果中该索引的值为**NaN（缺失值）**。

```python
In [25]: sr1 = pd.Series([12,23,34], index=['c','a','d'])
    ...: sr2 = pd.Series([11,20,10], index=['d','c','a'])
    ...: sr1+sr2
Out[25]:
a    33
c    32
d    45
dtype: int64

In [26]: sr1 = pd.Series([12,23,34], index=['c','a','d'])
    ...: sr2 = pd.Series([11,20,10], index=['b','c','a'])
    ...: sr1+sr2
Out[26]:
a    33.0
b     NaN
c    32.0
d     NaN
dtype: float64
```
2)当不希望因为没有对应的标签出现NaN的时候，可以使用add()方法，fill_valus属性进行操作
```python
In [28]: sr1 = pd.Series([12,23,34], index=['c','a','d'])
    ...: sr2 = pd.Series([11,20,10], index=['b','c','a'])

In [29]: sr1.add(sr2, fill_value=0)
    ...:
Out[29]:
a    33.0
b    11.0
c    32.0
d    34.0
dtype: float64
```

**7.缺失数据处理**
缺失数据：**使用NaN（Not a Number）来表示缺失数据**。其值等于np.nan。内置的None值也会被当做NaN处理。

**处理缺失数据的相关方法：**

        dropna()	过滤掉值为NaN的行
        fillna()		填充缺失数据
        isnull()		返回布尔数组，缺失值对应为True
        notnull()	返回布尔数组，缺失值对应为False

过滤缺失数据：sr.dropna() 或 sr[data.notnull()]

填充缺失数据：fillna(0)


### DatFrame
**1.DataFrame是什么？**
DataFrame是一个表格型的数据结构，含有一组有序的列。DataFrame可以被看做是由Series组成的字典，并且共用一个索引。并且可以看成**是二维数据类型。**

**2.创建二维数组**
注意:可以使用类似于字典的方式进行二维数组的创建。
```python
In [30]: animals = pd.DataFrame({'kind': ['cat', 'dog', 'cat', 'dog'],
    ...:  'height': [9.1, 6.0, 9.5, 34.0],
    ...: 'weight': [7.9, 7.5, 9.9, 198.0]})

In [31]: animals
Out[31]:
  kind  height  weight
0  cat     9.1     7.9
1  dog     6.0     7.5
2  cat     9.5     9.9
3  dog    34.0   198.0
```
注意:创建的过程中，标签索引不存在的时候，可以使用NaN进行代替，并且二维数组是以列为一个以为数组，所以每一列的数据类型会保持一致。回忆numpy中的保证数据类型一致的情景。
NaN被认为是浮点数形式。
```python
In [32]: pd.DataFrame({'one':pd.Series([1,2,3],index=['a','b','c']),
    ...: 'two':pd.Series([1,2,3,4],index=['b','a','c','d'])})
Out[32]:
   one  two
a  1.0    2
b  2.0    1
c  3.0    3
d  NaN    4

```

**3.csv文件的读写**
**注意: csv文件，类似于数组，行区分是逗号，列区分是换行。**
**读**出数据:
df.read_csv('filename.csv')
**写**入数据:
df.to_csv()

**4.DataFrame-常用属性**
**注意**：Series里面是将一列看成一起的，所以数据类型也会是一样的。

    index	获取索引
    T		转置
    columns		获取列索引
    values		获取值数组
    describe()	获取快速统计

**5.DataFrame-索引和切片**
1)DataFrame是一个二维数据类型，所以**有行索引和列索引**。
2)DataFrame同样可以**通过标签和位置**两种方法进行索引和切片
** ........   loc属性和iloc属性**
使用方法：逗号隔开，前面是行索引，后面是列索引
行/列索引部分可以是常规索引、切片、布尔值索引、花式索引任意搭配

**6.DataFrame-数据对齐与缺失数据**
1)DataFrame对象在运算时，同样会进行数据对齐，其行索引和列索引分别对齐。如果指定标签就按照标签进行对其，默认按照数字对其。

2)DataFrame处理缺失数据的相关方法：

    dropna(axis=0,where='any',…)
    fillna()
    isnull()
    notnull()
    


### pandas常用方法
注意:NumPy的通用函数同样适用于pandas

    mean(axis=0,skipna=False)				对列（行）求平均值
    sum(axis=1)				 	对列（行）求和
    sort_index(axis, …, ascending)		对列（行）索引排序
    sort_values(by, axis, ascending)		按某一列（行）的值排序，可以按列，升序与降序（NaN表示缺失值，不进行排序）
    apply(func, axis=0)	将自定义函数应用在各行或者各列上，func可返回标量或者Series
    applymap(func)			将函数应用在DataFrame各个元素上
    map(func)					将函数应用在Series各个元素上
    axis表示在二维数组中表示行与列，当等于1的时候就表示等于行，0表示等于列
    describe（）,快速看到每一列的信息，平均值，标准差，方差，最大值，最小值….
        
例子:
```python
In [37]: s = pd.DataFrame({'a': np.arange(10), 'b': np.arange(10, 20)})

In [38]: s
Out[38]:
   a   b
0  0  10
1  1  11
2  2  12
3  3  13
4  4  14
5  5  15
6  6  16
7  7  17
8  8  18
9  9  19

In [39]: s.describe()
Out[39]:
              a         b
count  10.00000  10.00000
mean    4.50000  14.50000
std     3.02765   3.02765
min     0.00000  10.00000
25%     2.25000  12.25000
50%     4.50000  14.50000
75%     6.75000  16.75000
max     9.00000  19.00000
```


### pandas时间对象
1.**时间序列类型：**

    时间戳：特定时刻
    固定时期：如2017年7月
    时间间隔：起始时间-结束时间

2.Python标准库**处理时间对象：datetime**

    灵活处理时间对象：dateutil
        dateutil.parser.parse()
    成组， 批量转换时间对象：pandas
        pd.to_datetime()
    
3.**产生时间对象数组：date_range**

    start			开始时间
    end			结束时间
    periods		时间长度
    freq			时间频率，默认为'D'，可选H(our),W(eek),B(usiness),S(emi-)M(onth),(min)T(es), S(econd), A(year),…
    
```python
In [40]: pd.date_range?
Signature:
pd.date_range(
    start=None,
    end=None,
    periods=None,
    freq=None,
    tz=None,
    normalize=False,
    name=None,
    closed=None,
    **kwargs,
)
```

**4.pandas 的时间序列**
1)时间序列就是**以时间对象为索引的Series或DataFrame。**
datetime对象作为索引时是存储在DatetimeIndex对象中的。

2)时间序列特殊功能：

    传入“年”或“年月”作为切片方式
    传入日期范围作为切片方式
    丰富的函数支持：resample(), strftime(), ……
    

### pandas文件处理
1)数据文件**常用格式：csv**（以某间隔符分割数据）

2)pandas读取文件：从文件名、URL、文件对象中加载数据
read_csv		默认分隔符为逗号
read_table	默认分隔符为制表符
科普:**excel文件是xml文件打包的文件**，可以使用重命名为zip之后进行解压查看相关数据。

**3.文件处理参数读参数**

    read_csv、read_table函数主要参数：
        sep				指定分隔符，可用正则表达式如'\s+'
        header=None		指定文件无列名
        name				指定列名
        index_col			指定某列作为索引
        skip_row			指定跳过某些行
        na_values			指定某些字符串表示缺失值
        parse_dates		指定某些列是否被解析为日期，类型为布尔值或列表

**4.文件处理写参数**

    写入到csv文件：to_csv函数
    写入文件函数的主要参数：
    sep			指定文件分隔符
    na_rep			指定缺失值转换的字符串，默认为空字符串
    header=False	不输出列名一行
    index=False	不输出行索引一列
    cols			指定输出的列，传入列表
    
**5.支持数据格式**
pandas支持的其他文件类型：
    json, XML, HTML, 数据库，pickle，excel...



## 结束语
**在使用pandas处理数据的时候，对比list(列表)的最大的改变是在内存数据的存储上进行了更改。开始着重凸显出以标签与索引为向导的数据，让处理数据更加快捷，可视化效果更好~~**
*最后补充一下，多看源码，pandas库的函数特别多，在处理相应问题的时候，从源码入手，里面会有很多例子~~*

*参考资料1:https://pandas.pydata.org/pandas-docs/stable/whatsnew/v0.25.0.html*
*参考资料2:https://pandas.pydata.org/*







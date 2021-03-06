# 开发|浅谈Python函数
*函数在实际使用中有很多不一样的小九九，我将从最基础的函数内容，延伸出函数的高级用法。此文非科普片~~*

## 前言
    目前所有的文章思想格式都是:知识+情感。
    知识:对于所有的知识点的描述。力求不含任何的自我感情色彩。
    情感:用我自己的方式，解读知识点。力求通俗易懂，完美透析知识。

## 正文
**首先介绍函数是什么，接着走进函数，并且发现函数的高级使用方法，最后列出常用的Python的内置函数。**

### 函数是什么？
1.函数，在代码执行的是不执行，只有在调用函数的时候才会执行。

2.函数使用关键字: def进行定义，看下面:
```python
In [1]: def demo():
   ...:     print('I am a demo ...')
   ...:

In [2]: demo()
I am a demo ...
```

3.书写函数的时候，**默认返回值是:None**。并且需要书写**函数的说明文档**。函数内部在必要的位置进行**写注释，对应的执行内容说明**。


### 函数基本参数
1.函数传递数据的**参数**
1)在函数括号中书写的是**形参**
2)调用函数传递的数据，叫做**实参**

2.函数**形参的分类**
1)**位置参数**，通过书写数据的先后位置进行数据的传输。看代码~传递参数的顺序

2)**默认参数**，直接在函数中进行默认指定数据，函数调用的时候，可以传递也可以不传数据。看下面，sex属于默认参数。

3)**关键字参数**，在调用函数的时候，直接指定形参的名字进行传输数据。看下面，age属于关键字传参。
```python
In [3]: def func(name, age, sex='male'):
   ...:     print('My name is %s,I am %s years old, I am a %s.' %(name, age, sex))
   ...:

In [4]: func('rongming', age=18)
My name is rongming,I am 18 years old, I am a male.
```

4)**非固定参数**
使用 *args  ， * * kwargs 表示
其中，*args 可以传递任意非键值对数据，并且存储为**元组形式**。
* * kwargs 保存传递的键值对数据，使用**字典的形式**进行存储，非常方便，时间使用次数非常多~~

3.**函数作用域**
1)**规则**:只要是进行了代码的缩进(一个tab键或者四个空格，大多数都是有作用域的产生。)
2)函数内部与函数外部，所包含的变量不在同一个作用域，变量的查找范围是**LEGB**。
表示在函数内部调用变量，优先去自己当前区域(locals)找，接着去函数嵌套空间中(enclosing function)找，再接着去全局空间(globals)找，最后去内置模块空间(builtins)找。
3)**查看**局部变量与全局变量方法:
局部变量 locals()
全局变量 globals()
4)函数内部使用外部的变量
使用， global，例如: global name  # 在函数内部声明全局变量name，不推荐
**可以使用nonlocal，** 例如:nonlocal name,同样也可以，推荐

4.函数中的**坑**
**注意:**函数或者说Python中更改数据产生的坑都可以是不可变数据类型，所以需要从底层认清楚不可变数据类型的存储方式，一定要记住**不可变数据类型只是相对于不可变数据类型中元素的不可变。**
注意:如果在函数内部使用函数外部的字典或者列表 这些可变数据类型，在函数中可以修改函数外的内容，没想到吧~~~

        

### 高阶函数
#### 高阶函数是什么？
**定义**:接收函数为参数，或者把函数作为结果返回的函数是高阶函数。

#### 嵌套函数
嵌套函数就是函数里面还写函数的样子~
看一下示例:(func函数里面写inner函数)
```python
def func():
    a = 10

    def inner():
        print(a)
    return inner

```

#### 匿名函数
匿名函数使用**关键字: lambda。**看下面的例子:
```python
addfunc = lambda x: x*3
```
 注意:由于匿名函数可以将函数简写为一行，并且大家看着逼格高，所以在某些时候，应用还是比较多的。
例子: map() 与匿名函数应用场景场景
```python
s = map(lambda x: x+10, [i for i in range(10)])
for i in s:
    print(i)
    
10
11
12
13
14
15
16
17
18
19
```


####  递归函数
1.递归函数： 递归表示在函数执行的时候，内部调用自身执行。
**注意**: **函数退出是否需要执行代码**，也就是函数在调用自身的之后还有没有代码执行，超过递归默认次数，会产生栈溢出。

2.知识点：
**pycharm里栈溢出报错是：**
    Process finished with exit code -1073741571 (0xC00000FD)

** linux栈溢出报错是：**
    Segmentation fault (core dumped)


**3.递归栈容量：**

    OS          default             sys.setrecursionlimit(100000)
    win             996                2512
    linux           998               16361
    linux默认递归栈容量是8M，可以通过ulimit -s扩充。
    

#### 闭包
**注意:一定要注意闭包的定义是什么!!!**
1.闭包的产生条件，在嵌套函数中才会出现，闭包表示匿名函数，闭包的本质属于函数作用域的延伸(不局限于函数体内的作用域)，只要**在嵌套函数中内部函数可以访问本身之外的全局变量就称为闭包。**
闭包中，内部函数访问的外部变量叫做**自由变量。**
实现闭包的例子:
```python
In [4]: def func():
   ...:     """闭包"""
   ...:     numbers = []
   ...:
   ...:     def inner(num):
   ...:         numbers.append(num)
   ...:         return numbers
   ...:
   ...:     return inner
   ...:

In [5]: num = func()

In [6]: num(10)
Out[6]: [10]

In [7]: num(11)
Out[7]: [10, 11]

In [8]: num(12)
Out[8]: [10, 11, 12]
```
**2.闭包的底层**
闭包可以实现对于自由变量的调用，关键点在于闭包函数存在一个Python的__ code __属性(对代码编译后的函数的定义体)中，存在一个函数的 __ closure __的属性，将自由变量进行绑定。
看下面的示例:
```python
In [15]: num.__closure__
Out[15]: (<cell at 0x000001A723D50348: list object at 0x000001A7234789C8>,)

In [16]: num.__closure__[1]
---------------------------------------------------------------------------
IndexError                                Traceback (most recent call last)
<ipython-input-16-56cdbd3f3ffc> in <module>
----> 1 num.__closure__[1]

IndexError: tuple index out of range

In [17]: num.__closure__[0].cell_contents  # 取到的num.__closure__[0]属于cell对象，存在cell_contents属性
Out[17]: [10, 11, 12]
In [18]: num.__code__.co_freevars   # 函数体中的，绑定的自由变量
Out[18]: ('numbers',)

In [19]: num.__code__.co_varnames  # 函数体内部的本地变量
Out[19]: ('num',)
```



#### 装饰器(语法糖)
**注意:装饰器不要懵逼~~给我一起透析它**
1.使用@加上函数名字，并且放在另一个函数上面的样式就是说，搞了个装饰器，别蒙蔽，现在就已经认识装饰器了~

2.装饰器使用的是闭包的思想，既然是闭包的思想，那么是不是会在被装饰的函数的函数，返回一个对应函数内部的函数名(地址)。
**重点**:此时被装饰的函数就不再是大家肉眼看的函数了，而是被返回装饰的函数内部函数，一旦调用，就先执行了装饰器里面的内容，在执行函数的执行内容。(还是看代码吧~)

3.时间装饰器
```python
In [26]: import time
    ...: import functools
    ...:
    ...:
    ...: def timer(func):
    ...:     @functools.wraps(func)
    ...:     def inner(*args, **kwargs):
    ...:         start = time.time()
    ...:         res = func(*args, **kwargs)
    ...:         end = time.time()
    ...:         print('This code used %s sec.' % (end - start))
    ...:         return res
    ...:     return inner
    ...:
    ...:
    ...:
    ...: @timer
    ...: def func(num1, num2):
    ...:     time.sleep(3)
    ...:     return num1 + num2
    ...:

In [27]: func(10, 33)
This code used 3.008526563644409 sec.
Out[27]: 43
```



####  生成器
1.生成器，**所有的生成器都是迭代器**，generator，只需要将列表生成式变为()即可，看下面
```python
In [28]: s = [i for i in  range(10)]

In [29]: s
Out[29]: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

In [30]: b = (i for i in range(10))

In [31]: b
Out[31]: <generator object <genexpr> at 0x000001A7223B0228>
```

2.生成器， 防止大数据生成，省空间，在做大数据生成的时候，可以保证，用多少我产生多少，占用多大的空间。产生的用完了就会报错StopIteration。
```python
In [41]:  b = (i for i in range(10))

In [42]: next(b)
Out[42]: 0

In [43]: next(b)
Out[43]: 1

In [44]: for i in b:
    ...:     print(i)
    ...:
2
3
4
5
6
7
8
9

In [45]: next(b)
---------------------------------------------------------------------------
StopIteration                             Traceback (most recent call last)
<ipython-input-45-adb3e17b0219> in <module>
----> 1 next(b)

StopIteration:
```

       
#### 函数生成器
1.函数生成器，保证了函数可以，在执行一半的时候，干点其他的事，并且可以将干的事返回到函数中，这也是协程的思想。**所有的生成器都是迭代器。**

2.实现函数生成器，只需要在函数内部添加yield即可。并且知识函数外部传递参数到函数内部，在初始化生成器的时候，需要使用__next__ 调用生成器，此时发送None到yield启动生成器，就可以使用next()进行调用了!其中，send() 表示调用生成器，并发送数据。

3看代码:
```python
In [46]: def gen(a):
    ...:     while a < 10:
    ...:         yield a
    ...:         a += 1
    ...:

In [47]: gen(5)
Out[47]: <generator object gen at 0x000001A723309228>

In [48]: b = gen(5)

In [49]: next(b)
Out[49]: 5

In [50]: next(b)
Out[50]: 6

In [51]: for i in b:
    ...:     print(i)
    ...:
7
8
9
```


        

####  迭代器
1.迭代器的构建使用:iter() ，可以将可迭代对象变为迭代器。

2.可以使用isinstance()判断是不是iterator，可以使用next()调用，for循环调用。

3.例子:
```python
In [52]: s = iter(i for i in range(10))

In [53]: s
Out[53]: <generator object <genexpr> at 0x000001A7226116D8>

In [54]: next(s)
Out[54]: 0

In [55]: next(s)
Out[55]: 1

In [56]: for i in s:
    ...:     print(i)
    ...:
2
3
4
5
6
7
8
9

In [57]: next(s)
---------------------------------------------------------------------------
StopIteration                             Traceback (most recent call last)
<ipython-input-57-61c30b5fe1d5> in <module>
----> 1 next(s)

StopIteration:
```
        


### python内置函数
Python 的内置函数很多，但是还是可以排列一个顺序的，所以下面我将我自己常用的函数进行一个顺序排列:

    print()
    id()
    enumerate()
    str() 字符的集合
    list() 元素的集合
    bytes()
    map()
    sum()
    isinstance()
    next()
    abs()
    max()
    min()
    bool()
    all() iterable is bool()
    any() iterable is bool()
    bytearray()
    complex()
    divmod()
    eval() 字符串数据类型转换成为Python的数据类型
    exec() 执行字符串
    exit()
    filter() 符合条件的进行过滤
    zip()



## 结束语
**Python的函数设计很多内容，实现功能要么是函数，要么是对象，所以有一些底层的内容，需要多加 挖掘，比如闭包的 __ code __，还有个自由变量等等。**
*感慨:路还很长，自己最需要的是时间与经历，加上一个不错的理解能力~~*















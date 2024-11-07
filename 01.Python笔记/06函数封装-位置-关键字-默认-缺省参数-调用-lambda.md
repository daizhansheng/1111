[TOC]

# 1.函数的定义及调用

==函数概念==：是将一段实现功能的完整代码，使用函数名称进行封装，通过函数名进行调用，以此达到一次编写，多次调用的目的。

==函数介绍==：在Python中有内置函数比如print()/input()/list()/tuple()/dict()/set()等，这些是不需要用户自定义的函数称之为内置函数。如果用户想要封装实现特定功能的函数，那些这种函数就称之为自定义函数。

==自定义函数==的格式如下：

```python
def 函数名称(参数列表):
    函数体
    return 返回值列表
```

==函数的调用==格式如下：

```python
函数名(参数列表)
```

==函数定义==实例：

```python
# 封装求n个数和的代码
def getsum(num):
    assert isinstance(num,int),"类型不正确，请重试"
    s=0
    for i in range(1,num+1):
        s+=i
    return s


print(getsum(100))
print(getsum(1000))
try:
    print(getsum(1.0))
except AssertionError as e:
    print(e)


# 定义列表成员显示函数
def show_list(lst):
    for index, item in enumerate(lst):
        print(f"list[{index}] = {item}")

# 定义传参的列表
lst = {item for item in range(100, 111)}
# 调用函数显示列表
show_list(lst)

```

# 2.函数的参数传递

函数在进行参数传递的时候，分为位置参数，关键字参数，默认参数

## 2.1位置参数

==位置参数==：它是指调用的时候参数的个数和顺序必须与定义的参数个数和顺序相同

```python
def func(name, age, sex):
    print("姓名：" + name, "age:" + str(age), "sex:" + str(sex))


# 函数的调用
func("zhangsan", 20, "m")
# func("zhangsan",20) #错误，参数个数传递不对

```

## 2.2关键字参数

==关键字参数==：它是指在调用函数的时候，使用"形参名称=值"的方式进行传参，传递参数的顺序可以与定义时参数的顺序不同

```python
def func(name, age, sex):
    print("姓名：" + name, "age:" + str(age), "sex:" + str(sex))


# 函数的调用
func(age=20, sex='m', name="zhangsan")  # 在调用的时候根位置无关，但是键和值必须相关

```

## 2.3默认参数

==默认参数==：它是指在函数定义的时候，直接对形式参数进行赋值，在调用时如果该参数不传参，将使用默认值，如果该参数传值，则使用传递的值。

```python
#默认值传参,默认参数从又向左,否则错误
def show_person_info(name="zhangsan",age=22,score=66.78):
    print(f"name = {name},age = {age},score = {score:.2f}")

show_person_info()
show_person_info("lisi")
show_person_info(age=15)
show_person_info("田七",score=100)
```

## 2.4可变参数

==可变参数==：可变参数分为==个数可变的位置参数==和==个数可变的关键字参数==两种，其中各位可变的位置参数是在参数前加一颗星(*para)，para形式参数的名称，函数调用时可接收任意个数的实际参数，并放到一个元组中。个数可变的关键字参数是在参数前加两颗星(\*\*para),在函数调用时可接收任意多个"参数=值"形式的参数，并放到一个字典中。

### 2.4.1个数可变的位置参数（元组）

```python
def fun(*args):  # 参数传递过来后是一个元组
    for item in args:
        print(item, end="|")


fun(11, 22, 33, 44)
print()

fun(10)
print()

fun([11, 22, 33, 44])  # 他会把这个列表当成一个成员
print()

fun(*[11, 22, 33, 44, 55])  # 加1个*解包为多个成员
print()
```

### 2.4.2个数可变的关键字参数（字典）

```python
def fun(**args):  # 这里是字典
    for key, value in args.items():
        print(f"{key}={value}", end="|")


fun(name="lisi", age=50, score=88.6)
print()

d={"name":"lisi","age":50,"score":88}
fun(**d)  # 系列解包

```

# 3.函数的返回值

==函数的返回值==：

1.  如果函数的运行结果需要再其他函数中使用，那么这个函数就应该被定义为带返回值的函数
2.  函数的运行结果使用return关键之进行返回
3.  return可以出现在函数的任意位置用于结束函数
4.  返回值可以是一个值，或多个值，如果返回的值是多个结果是元组类型

```python
# 100-999区间中的水仙花数
def get_narcissus():
    lst = []
    for item in range(100, 1000):
        b = item // 100
        s = item % 100 // 10
        g = item % 10
        if b ** 3 + s ** 3 + g ** 3 == item:
            lst.append(item)
    return lst


narc = get_narcissus()
print(narc, type(narc))


# 输入一个数，求1-num中的数的和，及奇数和，偶数和
def get_sum(num=10):
    sum = 0  # 总和
    odd = 0  # 奇数和
    even = 0  # 偶数和
    for item in range(1, num + 1):
        if item % 2 == 0:
            even += item
        else:
            odd += item
        sum += item
    return sum, odd, even


my_sum = get_sum()
print(my_sum, type(my_sum))

my_sum = get_sum(20)
print(my_sum, type(my_sum))

a, b, c = get_sum(20)  # 系列解包赋值
print(a, b, c)

```

# 4.函数中变量的作用域

==变量的作用域==：是指变量起作用的范围，根据范围作用的大小可分为局部变量和全局变量。

1.  局部变量

    定义：在函数定义处的参数和函数内部定义的变量

    作用域：仅在函数内部，函数执行结束，局部变量的生命周期结束

2.  全局变量

    定义：在函数外定义的变量或函数内部使用global关键字修饰的变量

    作用域：整个程序，程序运行结束全局变量的生命周期才结束

### 4.1局部作用域

局部作用域指的是在函数内部定义的变量。这些变量只能在函数内部访问，函数结束后，局部变量会被销毁。

```python
def my_function():
    x = 10  # 局部变量
    print(x)

my_function()  # 输出：10
# print(x)  # NameError: name 'x' is not defined
```

### 4.2全局作用域

全局作用域指的是在函数外部定义的变量。这些变量在整个脚本中都可以访问。要在函数内部修改全局变量，需要使用 `global` 关键字。

```python
x = 20  # 全局变量

def my_function():
    global x
    x = 10
    print(x)

my_function()  # 输出：10
print(x)  # 输出：10
```

### 4.3嵌套作用域

嵌套作用域指的是在嵌套函数中使用的变量。内层函数可以访问外层函数的局部变量，但不能直接修改。如果需要修改外层函数的局部变量，可以使用 `nonlocal` 关键字。

```python
def outer_function():
    x = 10
    
    def inner_function():
        nonlocal x
        x += 5
        print(f"Inner function: x = {x}")
    
    inner_function()
    print(f"Outer function: x = {x}")

outer_function()
# 输出：
# Inner function: x = 15
# Outer function: x = 15
```

# 5.匿名函数lambda的使用

==匿名函数lambda==:是指没有名字的函数，这种函数只能使用一次，一般在函数的函数体只有一句代码且只有一个返回值时，可以使用匿名函数来简化。

语法结构：

`result=lambda 参数列表:表达式`

```python
# 匿名函数1
# 将lambda函数赋值给了f，f是函数类型
f = lambda x, y: x + y
print(type(f))  # <class 'function'>

print(f(100,200))
print((lambda x,y:x+y)(100,200))

# 匿名函数2
print('-' * 50)
result = lambda i: lst[i]  # 将匿名函数赋值给了result，它是函数哦
for i in range(len(lst)):
    print(result(i))

print('-' * 50)

# 匿名函数3

d = [
    {"name": "zhangsan", "score": 90},
    {"name": "lisi", "score": 92},
    {"name": "wangwu", "score": 100},
    {"name": "tianqi", "score": 60},
]

print(d)
d.sort(key=lambda x:x.get("score"),reverse=True)
print(d)

```



# 6.属性常用的内置函数

1. 内置函数![image-20231027135008483](https://hqyj-note-picture.oss-cn-beijing.aliyuncs.com/picture_bak/mypictureimage-20231027135008483.png) 

2. 数学函数 ![image-20231027135204912](https://hqyj-note-picture.oss-cn-beijing.aliyuncs.com/picture_bak/mypictureimage-20231027135204912.png) 

3. 迭代器操作函数

   ![image-20231027135504183](https://hqyj-note-picture.oss-cn-beijing.aliyuncs.com/picture_bak/mypictureimage-20231027135504183.png)

   > [!CAUTION]
   >
   > `map(function, iterable, ...)`
   >
   > `map` 函数将给定的函数应用到迭代器的每个元素上，返回一个 `map` 对象（可以通过 `list()` 转换为列表）。如果有多个可迭代对象，`map` 会将它们元素逐一对应地传给函数。
   >
   > ```python
   > # 将一个列表中的每个数值乘以2
   > nums = [1, 2, 3, 4]
   > result = map(lambda x: x * 2, nums)
   > print(list(result))  # 输出 [2, 4, 6, 8]
   > 
   > ```
   >
   > `filter(function, iterable)`
   >
   > `filter` 函数用于从迭代器中筛选出满足条件的元素，返回一个 `filter` 对象（可以通过 `list()` 转换为列表）。`function` 返回 `True` 或 `False`，`filter` 会保留返回 `True` 的元素。
   >
   > ```python
   > # 过滤出列表中的偶数
   > nums = [1, 2, 3, 4, 5, 6]
   > result = filter(lambda x: x % 2 == 0, nums)
   > print(list(result))  # 输出 [2, 4, 6]
   > ```
   >
   > 

   

4. 常用的其他 内置函数

   ![image-20231027140823622](https://hqyj-note-picture.oss-cn-beijing.aliyuncs.com/picture_bak/mypictureimage-20231027140823622.png)

   ```python
   # format()
   print(format(3.14, "20"))  # 数值默认右对齐，占20个各
   print(format(3.14, ".5f"))  # 显示小数点后五位
   
   print(format("hello", "20"))  # 字符串默认左对齐
   print(format("hello", "<20"))  # 字符串左对齐
   print(format("hello", ">20"))  # 字符串右对齐
   print(format("hello", "^20"))  # 字符串居中对齐
   print(format("hello", "*^20"))  # 字符串居中对齐,不足补充*
   
   print(format(10, "b"))  # 二进制
   print(format(10, "x"))  # 十六进制
   print(format(10, "o"))  # 八进制
   print(format(10, "d"))  # 十进制
   
   year=2023
   month=2
   day=23
   hour=7
   min=9
   sec=5
   print(f"{year}-{format(month,"02d")}-{format(day,"02d")} {format(hour,"02d")}:{format(min,"02d")}:{format(sec,"02d")}")
   ```

      


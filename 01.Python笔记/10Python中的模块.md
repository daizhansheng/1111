# 1.Python中的模块

## 1.1模块的简介

**模块：**

- 在Python中一个后缀为.py的文件就是一个模块
- 模块中可以定义函数、类等
- ==模块也可以比避免函数、类、变量等名称冲突的问题==
- 模块不仅提高了代码的可维护性，同时提高了代码的可重复使用性
- 在给模块命名的时候要求全部使用小写字母，多个单词之间使用下划线进行分割
- 如果自定义模块名称与系统内置模块名称相同，那么在导入是会优先导入自定义的模块

**模块的种类:**

- 系统内置模块：有开发人员编写好的模块，在安装Python解析器是一同安装到计算机中
- 自定义模块：一个以.py结尾的文件就是一个模块，新建Python文件，实际上就是在新建一个模块。

> [!CAUTION]
>
> 自定义模块的作用：
>
> - 一是规范代码，将功能相同的函数、类等封装在一个模块中，让代码更易阅读
> - 另外一个目的与系统内置模块相同，既可以被其他模块调用提高开发效率。



## 1.2自定义模块的创建

`my_module.py`

```python
module_var = "hello Python module"


def my_module():
    print("这是我自定义的模块")


class ModuleSelf:
    def module_show(self):
        print("this is ModuleSelf show func")

```

## 1.3模块的导入

**模块的导入：**

模块编写完成就可以被其他模块进行调用，并使用被调用模块中的功能。

1. import导入方式

   `import 模块名称 [as 别名]`

2. from...import导入方式

   `from 模块名称 import 变量/函数/类/*`

```python
import my_module

# 导入my_module模块
print(my_module.module_var)

my_module.my_module()

m = my_module.ModuleSelf()

m.module_show()

import my_module as m

# 导入my_module模块并取别名为m
print(m.module_var)

from my_module import module_var

# 从my_module模块中导入module_var变量
print(module_var)

from my_module import my_module

# 从my_module中导入my_module函数
my_module()

from my_module import ModuleSelf

# 从my_module模块中带入ModuleSelf类
t = ModuleSelf()
t.module_show()

from my_module import *
# 导入模块中所有的内容，这里的*是一个通配符
my_module()

import math,time,random,my_module
# 同时导入多个模块

```

> [!CAUTION]
>
> 导入多个模块，函数，变量同名问题？
>
> my_module.py
>
> ```python
> module_var = "hello Python module1"
> 
> 
> def module_func():
>     print("这是我自定义的模块1")
> 
> 
> class ModuleClass:
>     def module_show(self):
>         print("this is ModuleSelf show func1")
> 
> 
> ```
>
> module_my.py
>
> ```python
> module_var = "hello Python module2"
> 
> 
> def module_func():
>     print("这是我自定义的模块2")
> 
> 
> class ModuleClass:
>     def module_show(self):
>         print("this is ModuleSelf show func2")
> 
> 
> ```
>
> 调用模块.py
>
> ```python
> from module_my import *
> from my_module import *
> # 同时导入两个模块，但是模块中有相同的方法的或者变量，
> # 规则是后者覆盖前者
> 
> module_func()
> # 因为两个模块都存在module_func方法，调用这个函数的时候时候后者模块中的方法
> 
> 
> #如何解决上述的冲问题？
> #通过模块名.成员方式访问
> import my_module
> import module_my
> module_my.module_func()
> my_module.module_func()
> 
> ```



## 1.4Python中包的定义

**包：**

- 含有`__init__.py`文件的文件夹就是包
- 可以避免模块相冲突的问题

```python
import package.module as m

m.my_module()
# V1.0
# 代战胜
# this is module func

from package import module as m
m.my_module()
# this is module func

from package.module import *
my_module()
# this is module func

from package.module import my_module
my_module()
# this is module func
```



## 1.5Python中主程序运行

主程序运行结构：

```python
if __name__== '__main__':
    pass
```

主程序运行实例：

`moduleA.py`

```python
def showA():
    print("这是模块A的show函数")

if __name__ == '__main__':
    print("hello")
    showA()
    # 这里如果不加主程序运行，当moduleB模块导入moduleA
    # 的时候这里的print("hello")和showA()也会运行，
    # 但如果加入这个主程序运行，这有这个文件作为主程序的时候这两句话才执行
    # 当moduleB作为主程序的时候只能够调用这个模块中的函数
```

`moduleB.py`

```python
import moduleA

moduleA.showA()
```



## 1.6Python中常用的内置模块

在安装Python解释器时与解释器一起安装进来的模块被称为系统内置模块，也被称为标准模块或标准库

| 标准库名称   | 功能描述                                               |
| ------------ | ------------------------------------------------------ |
| os模块       | 与操作系统和文件相关操作的模块                         |
| re模块       | 用于Python的字符串执行正则表达式的模块                 |
| random模块   | 用于产生随机数的模块                                   |
| json模块     | 用于对高维数据进行编码解码的模块                       |
| time模块     | 与时间相关的模块                                       |
| datetime模块 | 与日期时间相关模块，可以方便的显示日期和对进入进行计算 |

**random模块：**

| 函数名称           | 功能描述                                   |
| ------------------ | ------------------------------------------ |
| `seed(x)`          | 初始化给定的随机数种子，默认为当前系统时间 |
| `random()`         | 产生一个[0.0,1.0)之间的随机小数            |
| `randint(a,b)`     | 生成一个[a,b]之间的整数                    |
| `randrange(m,n,k)` | 生成一个[m,n)之间步长为k的随机整数         |
| `uniform(a,b)`     | 生成一个[a,b]之间的随机小数                |
| `choice(seq)`      | 从序列中随机选择一个元素                   |
| `shuffle(seq)`     | 将序列seq中元素随机排列，返回打乱后的序列  |

**time模块：** 

| 函数名称         | 功能描述                                    |
| ---------------- | ------------------------------------------- |
| `time()`         | 获取当前时间戳(秒)                          |
| `localtime(sec)` | 获取指定时间戳对应的本地时间struct_time对象 |
| `ctime()`        | 获取当前时间戳对应的易读字符串              |
| `strftime()`     | 格式化时间，结果为字符串                    |
| `strptime()`     | 提取字符串的时间，结果为struct_time对象     |
| `sleep(sec)`     | 休眠sec秒                                   |

| 格式化字符串 | 日期/时间    | 取值范围         |
| ------------ | ------------ | ---------------- |
| `%Y`         | 年份         | 0001~9999        |
| `%m`         | 月份         | 01~12            |
| `%B`         | 月名         | January~December |
| `%d`         | 日期         | 01~31            |
| `%A`         | 星期         | Monday~Sunday    |
| `%H`         | 小时(24h制)  | 00~23            |
| `%I`         | 小时(12h制)  | 01~12            |
| `%M`         | 分钟         | 00~59            |
| `%S`         | 秒           | 00~59            |
| `%j`         | 一年中第几天 | 0~366            |

```python
import time

t = time.localtime()
print(type(t),t)
# <class 'time.struct_time'> time.struct_time(tm_year=2024, tm_mon=11, tm_mday=4, tm_hour=11, tm_min=57, tm_sec=37, tm_wday=0, tm_yday=309, tm_isdst=0)

print(time.strftime("%Y-%m-%d %H:%M:%S week=%A  month=%B",t))
# 2024-11-04 11:57:37 week=Monday  month=November

print(time.strptime("2024-11-04 11:56:18 week=Monday  month=November","%Y-%m-%d %H:%M:%S week=%A  month=%B"))
# time.struct_time(tm_year=2024, tm_mon=11, tm_mday=4, tm_hour=11, tm_min=56, tm_sec=18, tm_wday=0, tm_yday=309, tm_isdst=-1)
```

**datetime模块：**

| 类名                 | 功能描述         |
| -------------------- | ---------------- |
| `datetime.datetime`  | 表示日期时间的类 |
| `datetime.timedelta` | 表示时间间隔的类 |
| `datetime.date`      | 表示日期的类     |
| `datetime.time`      | 表示时间的类     |
| `datetime.tzinfo`    | 时区相关的类     |



**openpyxl模块：**

- openpyxl模块用于处理Microsoft Excel文件的第三方库
- 可用于对Excel文件中的数据进行写入和读取

| 函数/属性名                | 功能描述                                                     |
| -------------------------- | ------------------------------------------------------------ |
| `load_workbook(filename)`  | 打开已存在的表格，结果为工作簿对象                           |
| `workbook.sheetnames`      | 工作簿对象的sheetnames属性，用于获取所有工作表的名称，结果为列表类型 |
| `sheet.append(lst)`        | 向工作表中添加一行数据，新数据表在工作表已有数据的后面       |
| `workbook.save(excelname)` | 保存工作簿                                                   |
| `Workbook()`               | 创建新的工作簿对象                                           |

### openpyxl 常用方法概述

| 示例代码                                                     | 功能描述           |
| ------------------------------------------------------------ | ------------------ |
| `workbook = load_workbook('example.xlsx')`                   | 加载工作簿         |
| `sheet_names = workbook.sheetnames`                          | 获取所有工作表名称 |
| `sheet = workbook['Sheet1']`                                 | 选择工作表         |
| `new_sheet = workbook.create_sheet('NewSheet')`              | 创建新工作表       |
| `workbook.remove(sheet)`                                     | 删除工作表         |
| `value = sheet['A1'].value`                                  | 读取单元格的值     |
| `sheet['A1'] = 'Hello'`                                      | 写入单元格的值     |
| `workbook.save('new_file.xlsx')`                             | 保存工作簿         |
| `for row in sheet.iter_rows(values_only=True): print(row)`   | 读取整行           |
| `for col in sheet.iter_cols(values_only=True): print(col)`   | 读取整列           |
| `for row in sheet.rows: print([cell.value for cell in row])` | 遍历所有行并读取值 |
| `for col in sheet.columns: print([cell.value for cell in col])` | 遍历所有列并读取值 |
| `rows = sheet.max_row`, `cols = sheet.max_column`            | 获取最大行号和列号 |
| `sheet.merge_cells('A1:D1')`                                 | 合并单元格         |
| `sheet.unmerge_cells('A1:D1')`                               | 拆分单元格         |
| `from openpyxl.styles import Font; cell.font = Font(bold=True)` | 设置单元格字体     |
| `from openpyxl.styles import PatternFill; cell.fill = PatternFill('solid', fgColor='FFFF00')` | 设置单元格填充颜色 |
| `sheet.freeze_panes = 'B2'`                                  | 冻结窗格           |
| `sheet.column_dimensions['A'].width = 20`                    | 调整列宽           |
| `from openpyxl.chart import BarChart, Reference`             | 插入图表           |

### 读写excel表格

get_weather.py

```python
import requests
import re

def get_info_from_url():
    url = "https://www.weather.com.cn/forecast/"
    result = requests.get(url)
    result.encoding = "utf-8"
    return result.text

def get_weather(result):
    city = re.findall('<span class="name">([\u4e00-\u9fff]*)</span>', result)
    weather = re.findall('<span class="weather">([\u4e00-\u9fff]*)</span>', result)
    wd = re.findall('<span class="wd">(.*)</span>', result)
    zs = re.findall('<span class="zs">([\u4e00-\u9fff]*)</span>', result)
    lst = []
    for a, b, c, d in zip(city, weather, wd, zs):
        lst.append([a, b, c, d])
    return lst


```

```python
from openpyxl import *  # 导入 openpyxl 库的所有模块以处理 Excel 文件
from get_weather import *  # 导入 get_weather 模块中的所有函数

# 创建一个新的工作簿和工作表
wb = Workbook()
ws = wb.active  # 获取当前活动的工作表

# 从 URL 获取天气信息
result = get_info_from_url()
# 将获取的信息格式化为可用于 Excel 的列表
lst = get_weather(result)

# 将每个天气信息项添加到工作表中
for item in lst:
    ws.append(item)  # 在工作表中追加一行数据

# 保存工作簿到文件
wb.save("天气.xlsx")

# 重新加载刚刚保存的工作簿
wb = load_workbook("天气.xlsx")
# 选择工作簿中的默认工作表
ws = wb['Sheet']

# 初始化一个空列表以存储从 Excel 读取的数据
lst = []
# 遍历工作表的所有行
for row in ws.rows:
    sublst = []  # 创建一个子列表以存储每行的值
    for item in row:
        sublst.append(item.value)  # 将单元格的值添加到子列表中
    lst.append(sublst)  # 将子列表添加到主列表中

# 打印从 Excel 中读取的数据
print(lst)

```



## 1.5Python中的常用第三方模块

第三方模块有全球Python爱好者、程序员、各行各业的专家进行开发并进行维护。

1. 安装第三方模块

   `pip install 模块名`

   `pip install 模块名 -i http://pypi.tuna.tsinghua.edu.cn/simple --trusted-host pypi.tuna.tsinghua.edu.cn`

   这里是修改下载的源为国内清华

2. 卸载第三方模块

   `pip uninstall 模块名`

3. 升级pip命名的语法结构

   `python -m pip install --upgrade pip`

   
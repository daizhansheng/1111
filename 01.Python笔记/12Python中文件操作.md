# 1.文件操作

## 1.1open()函数

**功能**：打开文件并返回一个文件对象，用于进行文件操作。

```python
file = open(filename, mode, buffering, encoding, errors, newline, closefd, opener)
```

参数：

- `filename` (必需)：文件路径（相对路径或绝对路径）。

- `mode` (可选)：文件打开模式（如 `'r'`、`'w'`、`'a'` 等）。

  | 模式    | 描述                   | 文件存在 | 文件指针位置 | 文件内容   |
  | ------- | ---------------------- | -------- | ------------ | ---------- |
  | `'r'`   | 只读模式               | 必须存在 | 开头         | 不修改     |
  | `'w'`   | 写入模式（覆盖）       | 可不存在 | 开头         | 清空       |
  | `'a'`   | 追加模式               | 可不存在 | 末尾         | 保留原内容 |
  | `'x'`   | 排他性创建模式         | 不存在   | 开头         | 创建新文件 |
  | `'r+'`  | 读写模式               | 必须存在 | 开头         | 不清空     |
  | `'w+'`  | 读写模式（覆盖）       | 可不存在 | 开头         | 清空       |
  | `'a+'`  | 追加读写模式           | 可不存在 | 末尾（写）   | 保留原内容 |
  | `'rb'`  | 二进制读模式           | 必须存在 | 开头         | 不修改     |
  | `'wb'`  | 二进制写模式（覆盖）   | 可不存在 | 开头         | 清空       |
  | `'ab'`  | 二进制追加模式         | 可不存在 | 末尾         | 保留原内容 |
  | `'xb'`  | 二进制排他性创建模式   | 不存在   | 开头         | 创建新文件 |
  | `'rb+'` | 二进制读写模式         | 必须存在 | 开头         | 不清空     |
  | `'wb+'` | 二进制读写模式（覆盖） | 可不存在 | 开头         | 清空       |
  | `'ab+'` | 二进制追加读写模式     | 可不存在 | 末尾（写）   | 保留原内容 |

- `buffering` (可选)：控制文件读写时的缓冲策略。

  - **0**：无缓冲。所有读写操作直接作用于底层文件系统（仅限二进制模式）。
  - **1**：行缓冲。如果文件以文本模式打开，每次遇到换行符时会刷新缓冲区。
  - **大于1 的整数**：设置缓冲区的大小（以字节为单位）。
  - **默认值**：根据系统和文件模式自动选择合理的缓冲策略。4096 或 8192 字节。

- `encoding` (可选)：指定文件的编码方式（例如 `utf-8`,`gbk`），默认为 `None`（平台默认）。

返回值：

- 返回一个文件对象，通过该对象可以进行读写操作。

## 1.2read()函数

`read()` 方法用于从文件中读取内容。可以根据需要一次性读取整个文件或读取指定数量的字节/字符。它在处理文本和二进制数据时均可使用。

```python
file.read(size=-1)
```

参数：

**`size`**（可选）：指定要读取的字节或字符数。

- **默认值 `-1`**：读取整个文件内容，直到文件末尾（EOF）。
- **`size > 0`**：读取指定数量的字符或字节。
- **`size = 0`**：返回空字符串（`''`）。
- 在文本模式下，`size` 表示字符数；在二进制模式下，表示字节数。

返回值:

- 返回一个包含从文件中读取内容的**字符串**（文本模式）或**字节对象**（二进制模式）。如果读取到文件末尾，则返回**空字符串** `''`。

## 1.3write()函数

`write()` 方法用于将内容写入文件。它将指定的字符串或字节数据写入到文件中。在文本模式下，`write()` 需要字符串类型的数据，在二进制模式下，它需要字节对象。

```python
file.write(string)
```

参数： 

- `string`

  （必需）：要写入文件的内容。

  - 在**文本模式**下，它应该是一个字符串。
  - 在**二进制模式**下，它应该是一个字节对象（`bytes`）。
  - **注意**：如果 `string` 中包含非 ASCII 字符，而文件的编码方式不同，可能会引发编码错误。确保文件编码与数据一致。

返回值：

- 返回写入的字符数（成功写入的字节数或字符数），如果文件已打开为文本模式，返回的是写入的字符数；如果是二进制模式，返回的是字节数。如果操作失败，可能会抛出异常。

## 1.4close函()数

`close()` 方法用于关闭文件对象，释放与文件相关的资源。关闭文件是非常重要的操作，它确保文件内容被正确保存，并且释放操作系统资源，以便其他程序或进程能够访问该文件。

```python
file.close()
```

参数：

- `close()` 方法没有参数。

返回值：

- `close()` 方法没有返回值，它返回 `None`。

## 1.5文件操作练习实例

```python
def write_file(filename):
     file = open(filename,"w+",encoding="utf-8")
     file.write("祝贺神州19号法神成功")
     file.close()

def read_file(filename):
     file = open(filename,"r",encoding="utf-8")
     s = file.read()
     file.close()
     return s

if __name__ == '__main__':
     write_file("hello.txt")
     print(read_file("hello.txt"))
```

## 1.6常见文件操作的方法

| **方法和参数**                                     | **功能描述**                                                 |
| -------------------------------------------------- | ------------------------------------------------------------ |
| `open(file, mode, encoding, errors)`               | 打开一个文件，返回文件对象。<br>- `file`：文件路径<br>- `mode`：打开模式（如 `'r'`、`'w'`、`'a'` 等）<br>- `encoding`：编码方式（如 `'utf-8'`）<br>- `errors`：错误处理方式（如 `'ignore'`）。这个函数可以用来打开文件进行读取、写入或追加操作，可以指定编码格式，常用于文本文件操作。 |
| `read(size)`                                       | 读取整个文件内容或指定大小的内容。<br>- `size`（可选）：读取的字节数，默认读取整个文件。如果省略或为负值，则读取整个文件的内容。这个函数适用于读取较小的文件内容，因为一次性读取大的文件可能会消耗大量内存。 |
| `readline(size)`                                   | 逐行读取文件内容，每次读取一行。<br>- `size`（可选）：读取的字符数，默认读取一行。如果指定大小参数，则读取该行的指定字符数，否则读取整行。非常适用于按行处理大文件。 |
| `readlines()`                                      | 读取所有行，返回一个列表，每个元素是一行。这个函数将文件中的每一行作为列表中的一个元素返回，适用于将整个文件的内容读取到内存中进行批量处理。 |
| `write(string)`                                    | 将字符串写入文件。<br>- `string`：要写入的字符串。这个函数用于将指定的字符串写入到文件中，如果文件不存在则创建文件，适用于写入文本数据。 |
| `writelines(list)`                                 | 将一个字符串列表写入文件。<br>- `list`：要写入的字符串列表。这个函数用于将多个字符串写入文件，每个字符串作为文件中的一行，适用于批量写入文本数据。 |
| `seek(offset, whence)`                             | 移动文件指针到指定位置。<br>- `offset`：偏移量<br>- `whence`（可选）：起始位置，0 表示文件开头，1 表示当前位置，2 表示文件末尾。这个函数用于在文件中移动读取或写入的位置，非常适用于在文件中随机访问特定位置。 |
| `tell()`                                           | 返回文件指针的当前位置。这个函数返回当前文件指针的位置，即从文件开头到当前指针位置的字节数，常用于记录当前处理文件的位置。 |
| `close()`                                          | 关闭文件。这个函数用于关闭打开的文件，释放文件相关的资源。如果不调用此函数，可能会导致文件资源未被释放，影响系统性能。 |
| `with open(file, mode, encoding, errors) as file:` | 上下文管理器，用于自动管理文件的打开和关闭。<br>- `file`：文件路径<br>- `mode`：打开模式（如 `'r'`、`'w'`、`'a'` 等）<br>- `encoding`：编码方式（如 `'utf-8'`）<br>- `errors`：错误处理方式（如 `'ignore'`）。上下文管理器确保文件在使用完毕后自动关闭，简化了文件操作代码并减少错误。 |
| `os.path.exists(path)`                             | 检查文件是否存在。<br>- `path`：文件路径。这个函数用于检查指定路径的文件是否存在，返回布尔值，常用于在进行文件操作前进行检查。 |
| `os.path.join(path1, path2, ...)`                  | 拼接文件路径。<br>- `path1, path2, ...`：多个路径组成的字符串。这个函数用于将多个路径拼接成一个路径，确保路径分隔符的正确性，适用于跨平台文件路径操作。 |

### 1.6.1写文件实例

```python

def write_file_line(fn,s):
    file = open(fn,"a+",encoding="utf-8")
    file.write(s)
    file.write('\n')
    file.close()

def write_file_lines(fn,lst):
    file = open(fn,"a+",encoding="utf-8")
    file.writelines(lst)
    file.close()

# 这行代码检查当前脚本是否是主程序（不是被其他脚本导入）
if __name__ == '__main__':
    write_file_line("file_line.txt","hello world")
    write_file_line("file_line.txt","hello Python")
    lst = ["city\t","weather\n","北京\t","晴天\n","上海\t","多云\n"]
    write_file_lines("file_lines.txt",lst)

```

### 1.6.2读文件实例

```python
def read_file_line(fn):
    file = open(fn, "r")
    s = file.readline()
    return s


def read_file_lines(fn):
    file = open(fn, "r")
    s = file.readlines()
    return s


if __name__ == "__main__":
    s = read_file_line("file_line.txt")
    s = s[:len(s)-1]  # 去除读取到的'\n'
    print(s)
    lst = read_file_lines("file_lines.txt")
    print(lst)

```

### 1.6.3文件光标操作

```python
"""
city	weather
北京	晴天
上海	多云
city	weather
北京	晴天
上海	多云
"""

# 0（os.SEEK_SET）：从文件开头开始计数。
# 1（os.SEEK_CUR）：从当前位置开始计数。
# 2（os.SEEK_END）：从文件结尾开始计数。

file = open("file_lines.txt", "r+")
print(file.read(1))
file.seek(30, 0)
# file.seek(10,1) # 当前位置偏移，错误，因为文本文件编码格式问题
print(file.read(1))
print(file.tell())
file.close()

```

## 1.7with上下文管理

**with语句**：又称上下文挂力气，在处理文件实，无论是否产生异常，都能保证with语句执行完毕后关闭已经打开的文件，这个过程是自动的，无需收到操作。

**语法结构**：

```python
with open(...) as file:
    pass
```

```python
def copy(src,dest):
    with open(src,"r") as file1:
        with open(dest,'w') as file2:
            file2.write(file1.read())

if __name__ == '__main__':
    copy("file_lines.txt","hello.txt")
```

## 1.8json格式存储及读取

![image-20241106171726054](https://hqyj-note-picture.oss-cn-beijing.aliyuncs.com/picture_bak/mypicture202411061717450.png)  

```python
# 导入 json 模块，用于序列化和反序列化数据
import json

# 定义一个列表，包含三个字典，每个字典有三个键值对：name、age 和 score
lst = [
    {"name": "zhangsan", "age": 20, "score": 88.6},
    {"name": "lisi", "age": 24, "score": 67.0},
    {"name": "wangwu", "age": 18, "score": 100},
]

# 使用 json.dumps() 函数将列表序列化为一个 JSON 字符串
# indent 参数指定了缩进级别，4 表示每层缩进 4 个空格
s = json.dumps(lst, indent=4)

# 打印序列化后的 JSON 字符串的类型
print(type(s))

# 打印序列化后的 JSON 字符串
print(s)

# 使用 json.loads() 函数将 JSON 字符串反序列化为一个列表
lst1 = json.loads(s)

# 打印反序列化后的列表
print(lst1)

# 使用 with 语句打开一个文件，文件名为 "json.txt"，模式为 "w+"，编码为 "utf-8"
with open("json.txt", "w+", encoding="utf-8") as file:
    # 使用 json.dump() 函数将列表序列化为一个 JSON 文件
    # indent 参数指定了缩进级别，4 表示每层缩进 4 个空格
    json.dump(lst1, file, indent=4)

# 使用 with 语句打开一个文件，文件名为 "json.txt"，模式为 "r"
with open("json.txt", "r") as file:
    # 使用 json.load() 函数将 JSON 文件反序列化为一个列表
    lst3 = json.load(file)

    # 打印反序列化后的列表
    print(lst3)
```

# 2.目录相关操作

## 2.1OS模块

Python内置的与操作系统文件相关的模块，该模块中语句的执行结果通常与操作系统相关，及有些函数执行的相关在windwos操作系统上和MacOS系统上不一样。

![image-20241106173501337](https://hqyj-note-picture.oss-cn-beijing.aliyuncs.com/picture_bak/mypicture202411061735605.png) 

![image-20241106173538483](https://hqyj-note-picture.oss-cn-beijing.aliyuncs.com/picture_bak/mypicture202411061735739.png) 

```python
import os
import time
from idlelib.run import flush_stdout

print(os.getcwd())
# /Users/dzs/Desktop/Py_work/52目录操作

print(os.listdir("../"))
# 结果存放到了列表中，效果和ls相同
try:
    os.mkdir("./testdir")
except FileExistsError as e:
    print("文件存在，创建失败", e)
# 在当前目录下创建testdir,类似于mkdir命令
try:
    os.makedirs("./testdir/aaa/111")
except FileExistsError as e:
    print(e)
# 创建具备层级关系的目录，类似于mkdir -p

os.rmdir("./testdir/aaa/111")
# 只能用来删除空目录，类似于rmdir命令

os.removedirs("./testdir/aaa")

os.chdir("../")
print(os.getcwd())

ll = os.walk("./")
print(type(ll))
for item in ll:
    print(item)
# 上述的功能类似于tree

os.chdir("./52目录操作")
print(os.getcwd())
with open("hello.txt", "w", buffering=1) as file:
    file.write("hello")
    file.close()

time.sleep(1)

os.rename("hello.txt", "new_hello.txt")

time.sleep(1)

os.remove("./new_hello.txt")

st = os.stat("./01目录操作实例1.py")
print(type(st))
print(st.st_ino)

# os.startfile("./startfile.py")
# os.startfile() 是 Windows 独有的功能，在 macOS 和 Linux 等非 Windows 系统上不可用。

```

## 2.2os.path模块

![image-20241106181503438](https://hqyj-note-picture.oss-cn-beijing.aliyuncs.com/picture_bak/mypicture202411061815834.png) 

![image-20241106181525877](https://hqyj-note-picture.oss-cn-beijing.aliyuncs.com/picture_bak/mypicture202411061815124.png) 

```python
import os.path as op

print(op.abspath("./"))
# 获取当前的绝对路径

print("hello.txt 是否存在？", op.exists("./hello.txt"))
print("02目录文件操作.py 是否存在？", op.exists("./02目录文件操作.py"))

s = op.join(op.abspath("./"), "02目录文件操作.py")
print(s)
# 将文件名和目录拼接在一起

print(op.splitext(s))
# 拆分文件名和后缀

print(op.basename(s))
# 从路径中提取文件名

print(op.dirname(s))
# 从文件中提取路径

print("s 是否是目录 ？ ",op.isdir(s))
print("s 是否是文件 ？ ",op.isfile(s))
```


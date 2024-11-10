

# 1.串行并发并行的区别

## 1.1串行执行
- **定义**：串行执行是指任务一个接一个地按顺序执行。每个任务完成后，才会开始下一个任务。
- **执行方式**：只有一个任务在执行，任务间没有重叠。
- **特点**：
  - **简单**：串行执行是最简单的执行方式，每个任务按顺序完成。
  - **效率低**：任务不能重叠执行，因此执行时间较长，尤其是当有多个任务时。
  - **无资源竞争**：因为任务是依次执行的，所以不会发生资源竞争问题。
- **示例**：假设有三个任务 \(A\)、\(B\)、\(C\)，它们必须按顺序执行：
  - 执行 \(A\) → 完成后执行 \(B\) → 完成后执行 \(C\)。

## 1.2并发执行
- **定义**：并发执行是指多个任务在同一时间段内进行，但是并不是同时执行。任务可以交替执行，每个任务可能在不同的时间片中运行。
- **执行方式**：在一个核心或处理器上，操作系统会在任务之间切换，给用户一种“同时执行”的感觉。多个任务并发执行是因为操作系统不断切换任务的执行。
- **特点**：
  - **任务交替执行**：任务并不是完全同时进行，而是操作系统或程序在多个任务之间切换，单核系统也可以实现并发。
  - **提高资源利用**：并发使得程序可以在等待某个任务（比如 I/O 操作）完成时，执行其他任务。
  - **I/O 密集型任务**：并发通常用于 I/O 密集型任务（如网络请求、磁盘读写等），在任务等待 I/O 时，程序可以执行其他任务，从而提高效率。
- **示例**：假设有任务 \(A\)、\(B\)、\(C\)，它们会被操作系统交替执行。例如：
  - 执行 \(A\) 的一部分 → 切换到 \(B\) → 切换到 \(C\) → 再次切换回 \(A\)，如此反复。

## 1.3并行执行
- **定义**：并行执行是指多个任务**真正同时执行**，并且每个任务在不同的处理器或多个核心上独立运行。
- **执行方式**：每个任务分配到不同的 CPU 核心上，同时进行计算，任务间没有等待。
- **特点**：
  - **多核或多处理器支持**：并行执行要求硬件支持，比如多个 CPU 核心或多个处理器。每个任务可以在独立的处理器上同时执行。
  - **适合 CPU 密集型任务**：并行非常适合需要大量计算的任务，可以显著提高计算效率。
  - **更高的执行效率**：与串行执行相比，并行执行可以显著减少任务的总体完成时间。
- **示例**：假设有任务 \(A\)、\(B\)、\(C\)，它们会在不同的处理器核心上同时执行：
  - \(A\) 在核心1上执行，\(B\) 在核心2上执行，\(C\) 在核心3上执行，它们同时进行，不需要等待。

## 1.4对比总结

| 执行方式 | 任务执行方式                                             | 资源使用        | 适用场景                                       |
| -------- | -------------------------------------------------------- | --------------- | ---------------------------------------------- |
| **串行** | 任务按顺序一个接一个执行                                 | 1个核心/处理器  | 任务不重叠，适用于较简单的、没有并行需求的任务 |
| **并发** | 任务交替执行，单核系统通过时间切换执行任务               | 1个核心/处理器  | 适合 I/O 密集型任务（如网络请求、文件读写）    |
| **并行** | 多个任务真正同时执行，每个任务在不同的处理器或核心上执行 | 多个核心/处理器 | 适合 CPU 密集型任务（如数据处理、大规模计算）  |

# 2.CPU密集型和IO密集型

## 2.1CPU 密集型任务

- **定义**：指那些主要依赖 CPU 进行大量计算的任务。CPU 密集型任务通常需要执行大量的数学运算、复杂的数据处理、加密解密、图像处理等。
- **特点**：CPU 密集型任务的性能主要取决于 CPU 的处理能力和核心数量，任务的执行过程基本上占用的是 CPU 资源。
- **示例**：科学计算、矩阵运算、图像处理、大量数据的排序或统计运算等。
- **适用方案**：多进程。因为 CPU 密集型任务需要多个 CPU 核心的处理能力，使用多进程可以让每个进程分配到不同的核心，充分利用多核 CPU。

## 2.2I/O 密集型任务
- **定义**：指那些主要受限于 I/O 操作（输入/输出）的任务，例如网络请求、文件读写、数据库操作等。这类任务的执行时间大部分花费在等待 I/O 设备响应上，而非 CPU 运算。
- **特点**：I/O 密集型任务通常会因为等待外部设备（如硬盘、网络）的响应而导致 CPU 闲置，因此 CPU 利用率低。
- **示例**：网络爬虫、读取和写入文件、大量网络数据传输等。
- **适用方案**：多线程或异步编程。由于 I/O 密集型任务常常在等待 I/O 操作完成，使用多线程或 `asyncio` 可以在等待期间执行其他任务，从而更有效地利用 CPU。

## 2.3对比总结
- **CPU 密集型任务**：大量计算，消耗 CPU 资源，适合多进程。
- **I/O 密集型任务**：大量 I/O 操作，等待时间长，适合多线程或异步。

根据任务的类型，可以选择合适的并发模式来提升性能。例如，Python 中的 `multiprocessing` 模块适合 CPU 密集型任务，而 `threading` 模块或 `asyncio` 则更适合 I/O 密集型任务。



# 3.多进程

## 3.1多进程的概念

在 Python 中，多进程（multiprocessing）是指利用多个进程同时执行任务，来实现并行计算的概念。Python 提供了 `multiprocessing` 模块，可以方便地创建和管理多个进程，尤其适合 CPU 密集型任务。多进程是一种绕过 Python 中的全局解释器锁（GIL）的方式，可以让 Python 程序利用多核 CPU 的全部计算能力。



Python 有一个全局解释器锁（GIL），限制了多线程的并行执行。特别是对于 CPU 密集型任务，使用多线程无法真正地并行运行，而多进程则不受 GIL 限制，每个进程都有独立的内存空间和 GIL。多进程适合需要大量计算的任务（如图像处理、大规模数据计算、科学计算等），可以显著提高程序的执行效率。

## 3.2多进程的创建

在 Python 中，多进程的创建和管理通常通过 `multiprocessing` 模块进行，特别是 `Process` 类。

```python
class multiprocessing.Process(group=None, target=None, name=None, args=(), kwargs={}, daemon=None)
```

==功能：==

`Process` 类用于创建和管理一个新的进程。进程是操作系统资源的单独实例，它包含程序的代码和运行时状态。在多进程模型中，每个进程独立运行，并且有自己的内存空间。

==参数：==

- **group**：目前必须为 `None`，是为了将来扩展预留的。
- **target**：一个可调用对象，当进程启动时会调用这个对象。如果 `None`，则不调用任何对象。
- **name**：进程的名称。默认情况下，这将是 `Process-N`，其中 `N` 是一个自动递增的整数。
- **args**：传递给 `target` 函数的元组。
- **kwargs**：传递给 `target` 函数的字典。
- **daemon**：如果是 `True`，表示该进程是守护进程。守护进程会在主进程退出时自动终止。

==返回值：==

构造函数本身没有返回值，但是它创建并返回一个 `Process` 对象实例。

==方法:==

`start()`

- **功能**：启动进程，将进程从创建状态切换到运行状态。调用此方法时，进程开始执行其 `run()` 方法，通常是调用 `target` 函数。
- **返回值**：无。

`run()`

- **功能**：定义进程的活动。默认情况下，此方法会调用传递给构造函数的 `target` 函数。如果未指定 `target`，则该方法不执行任何操作。通常不需要直接调用此方法，而是通过 `start()` 方法间接调用。
- **返回值**：无。

`join(timeout=None)`

- **功能**：阻塞当前线程，直到调用该方法的进程终止或超时（如果指定了 `timeout`）。如果 `timeout` 参数为 `None`，该方法将无限期等待。
- **参数**：
  - `timeout`（可选）：等待的最长时间，以秒为单位。如果为 `None`，则无限期等待。
- **返回值**：无。

`terminate()`

- **功能**：强制终止进程。调用此方法会立即停止进程的执行，不能保证进程会安全地清理资源。
- **返回值**：无。

`is_alive()`

- **功能**：检查进程是否仍在运行。返回一个布尔值，指示进程是否还在活动。
- **返回值**：布尔值 `True` 或 `False`。

`close()` 

- **功能**：用于防止更多的数据被添加到进程池或队列中。这是终止池的一部分过程。调用 `close()` 后，将不再接受新的任务提交。
- **返回值**：无

`kill()` 

- **功能**：用于强制终止进程。这是一个立即停止进程执行的方法，与 `terminate()` 类似，但通常更激进，可能不会执行任何清理操作。
- **返回值**：无

==属性:==

- **name**：进程名称。
- **pid**：进程 ID。
- **daemon**：是否是守护进程。
- **exitcode**：进程退出代码。正常退出为 `None`，意外终止时为负数（例如被信号终止）。
- **authkey**：进程的认证密钥。
- **sentinel**：用于进程的可等待哨兵。

## 3.3多进程创建实例

### 3.3.1创建不执行任务的进程

```python
from multiprocessing import *

if __name__ == '__main__':
    # 创建多进程
    p = Process()
    # 运行多进程
    p.start()
    p.join()

```

### 3.3.2创建执行任务的进程

```python
import os
import time
from multiprocessing import *


def process1():
    n = 0
    print(f"子进程 process1 pid = {os.getpid()},ppid = {os.getppid()}")
    while True:
        print("我是子进程")
        time.sleep(1)
        n += 1
        if n == 5:
            exit(0)


if __name__ == "__main__":
    print(f"父进程的pid = {os.getpid()}")
    # 默认情况下，子进程是非守护进程，这意味着主进程会等待所有非守护子进程完成后再退出。
    # 即使父进程没有调用join()方法，只要子进程仍在运行，主进程就不会退出。
    p = Process(target=process1, name="child_process")
    p.start()
    # 阻塞等待子进程结束
    p.join()
    print("子进程退出了")

```

### 3.3.3向进程传递参数

```python
from multiprocessing import Process


def process(who,work):
    print(f"who = {who},work = {work}")
    exit(0)
if __name__ == '__main__':
    p = Process(target=process,args=("zhangsan", "执行进程"))
    p.start()
    p.join()
if __name__ == '__main__':
    p = Process(target=process,kwargs={"who": "zhangsan", "work": "执行进程"})
    p.start()
    p.join()
```

# 4.进程间通信

在 Python 中，`multiprocessing` 模块提供了多种进程间通信（IPC）方式，方便多个进程之间的数据交换和同步。这些通信方式在多进程编程中非常重要，尤其是在不同进程之间需要共享数据或协调操作的情况下。以下是 Python 中常用的几种进程间通信方式：

## 4.1Pipe（管道）

在 Python 中，`multiprocessing` 模块提供了 **Pipe()** 函数来实现进程间的通信。管道（Pipe）是双向的通信通道，可以连接两个进程，通过管道发送和接收数据。

`Pipe()` 返回一个管道连接对象，它有以下操作方法：

### 4.1.1创建管道

通过 `Pipe()` 创建管道时，返回一个包含两个连接对象（端点）的元组：

```python
parent_conn, child_conn = Pipe(duplex=True)
```

- **`parent_conn`** 和 **`child_conn`**：这两个连接对象表示管道的两端。通过这些对象，进程之间可以进行数据交换。
- **`duplex`**：可选参数，默认为 `True`，表示双向通信。如果设置为 `False`，管道是单向的，即只能在一个方向上传输数据。

### 4.1.2管道操作方法

管道的每个端点（`parent_conn` 和 `child_conn`）都有以下几个常用方法：

1. `send(obj)`

   将数据发送到管道的另一端。这个方法用于将 Python 对象传输到管道中。可以传输任意 Python 可序列化的数据（如字符串、字典、列表等）。

   ```python
   parent_conn.send("Hello from sender")
   ```

2. `recv()`

   从管道中接收数据。接收方法会阻塞，直到管道另一端发送了数据。

   ```python
   data = child_conn.recv()
   print(data)  # 打印接收到的数据
   ```

3. `close()`

   关闭管道的一端，关闭后无法再使用该端点发送或接收数据。推荐在不再需要该端点时关闭它，防止资源浪费。

   ```python
   parent_conn.close()
   ```

4. `poll()`

   `poll()` 方法用于检查管道是否有数据可接收。如果有数据返回 `True`，如果没有数据返回 `False`。这是一个非阻塞的方法。

   ```python
   if parent_conn.poll():
       data = parent_conn.recv()
   ```

5. `fileno()`

   `fileno()` 返回管道的文件描述符，主要用于底层的文件I/O操作。一般来说，普通的进程间通信不需要直接使用这个方法。

   ```python
   fd = parent_conn.fileno()
   ```



### 4.1.3管道通信实例

父子进程双向通信实例：

```python
import os
import sys
import time
from multiprocessing import *
def process_handle1(child,pip1):
    while True:
        # Python子进程无法调用input，因为Python的终端只和主进程相关
        # 默认子进程的输入是关闭的，但是可以使用系统的函数代替input输入
        # s = input("input > ")
        s = child.recv()
        pip1.send(s)
        if s == "quit":
            exit(0)

def process_handle2(pip2):
    while True:
        s = pip2.recv()
        if s == "quit":
            exit(0)
        print(s,flush=True)

if __name__ == '__main__':
    parent, child = Pipe()
    pip1,pip2 = Pipe()
    p1 = Process(target=process_handle1,args=(child,pip1))
    p2 = Process(target=process_handle2,args=(pip2,))
    p1.start()
    p2.start()

    while True:
        x = input()
        parent.send(x)
        if x=='quit':
            break

    p1.join()
    p2.join()


```

管道大小验证实例：

```python
from multiprocessing import *


def send(pipe):
    n=0
    while True:
        pipe.send('a')
        print(n)
        n+=1

if __name__ == '__main__':
    p,c = Pipe()
    process = Process(target=send,args=(c,))
    process.start()
    while True:
        x = input()
        p.recv()
        if x == "quit":
            process.terminate()
            break
    process.join()
```

### 4.1.4管道的特性

1. 双向通信：

   支持双向数据传输，两个进程可以互相发送和接收数据。

2. 阻塞与非阻塞模式：

   支持阻塞和非阻塞模式，读写操作可以设置为阻塞或非阻塞。

3. 支持序列化数据：

   可以传输任意可序列化的数据类型，如字符串、整数、对象等。

4. 性能：

   通常比文件或网络通信更高效，适用于小规模数据传输。

## 4.2Queue（队列）

在 Python 的 `multiprocessing` 模块中，`Queue` 是一种进程安全的先进先出（FIFO）数据结构，用于在不同进程之间交换数据。它适用于单向通信，`Queue` 非常适合实现生产者-消费者模式，多个进程可以安全地使用它来传递信息。

以下是关于 `Queue` 的接口、操作方法和实例。

### 4.2.1创建 Queue

在 `multiprocessing` 中创建一个 `Queue` 非常简单：

```python
from multiprocessing import Queue

q = Queue(maxsize=0)
```

- **`maxsize`**：指定队列的最大容量。默认为 `0`，表示队列没有大小限制。设置 `maxsize` 可以防止队列数据过多导致内存占用增加。

### 4.2.2Queue 操作方法

1. put(item)

   将数据放入队列中，如果队列已满且 `maxsize` 被限制，`put()` 会阻塞直到有空余位置。

   ```python
   q.put("data")
   ```

2. get()

   从队列中获取数据。若队列为空，`get()` 会阻塞等待数据直到队列中有内容。通常在消费者进程中使用。

   ```python
   data = q.get()
   print("Received:", data)
   ```

3. put_nowait(item)

   非阻塞地放入数据。如果队列已满，`put_nowait()` 会立即抛出 `queue.Full` 异常，而不会等待。

   ```python
   try:
       q.put_nowait("data")
   except queue.Full:
       print("Queue is full")
   ```

4. get_nowait()

   非阻塞地获取数据。如果队列为空，`get_nowait()` 会立即抛出 `queue.Empty` 异常，而不会等待。

   ```python
   try:
       data = q.get_nowait()
   except queue.Empty:
       print("Queue is empty")
   ```

5. qsize()

   返回当前队列中元素的数量。在某些系统上可能不可用。

   ```python
   print("Queue size:", q.qsize())
   ```

6. empty()和full()

   检查队列是否为空或已满。注意，这两个方法并不是绝对安全的，因为在检查后，队列状态可能会被其他进程改变。

   ```python
   if q.empty():
       print("Queue is empty")
   
   if q.full():
       print("Queue is full")
   ```

   

### 4.2.3Queue 使用实例

**生产者-消费者模型**

下面是一个使用 `Queue` 实现的生产者-消费者示例，展示了如何使用队列在进程之间传递数据。

```python
import time
from multiprocessing import *

def producer(queue):
    for i in range(5):
        queue.put(f"我是生产者，我发送的消息id = {i}")
        time.sleep(1)

def consumer(queue):
    while True:
        msg = queue.get()
        print(msg)
        i = msg.index("id")
        if msg[i+5:] == "4":
            break


if __name__ == '__main__':
    q = Queue()

    p1 = Process(target=producer,args=(q,))
    p2 = Process(target=consumer,args=(q,))

    p1.start()
    p2.start()

    p1.join()
    p2.join()
```

### 4.2.4Queue 特性与注意事项

- **进程安全**：`Queue` 在实现时内置了锁机制，因此可以保证多进程访问时的数据安全性。
- **阻塞与非阻塞操作**：`Queue` 支持阻塞与非阻塞模式，可以通过 `put_nowait()` 和 `get_nowait()` 实现非阻塞操作。
- **容量限制**：可以通过 `maxsize` 限制队列的最大容量，这样可以有效控制数据量，避免内存消耗过多。

## 4. 3共享内存（Value 和 Array）

`Value` 和 `Array` 是 Python `multiprocessing` 模块中提供的共享内存对象，用于在多个进程间共享简单的数据结构，而不需要序列化或数据复制。
- **`Value`** 用于共享单个值，比如整数或浮点数。
- **`Array`** 用于共享一维数组，适合需要在进程间共享一组相同类型的元素。

它们内部使用共享内存来存储数据，并提供锁机制来管理并发访问。

### 4.3.1创建共享内存（Value 和 Array）

`Value` 和 `Array` 使用 `typecode` 指定数据类型。`typecode` 是 `ctypes` 标准类型的一种标识符，表示共享内存中存储的数据类型。以下是常用的 `typecode` 对照表：

| 类型代码 (typecode) | 数据类型                | Python 解释               |
| ------------------- | ----------------------- | ------------------------- |
| `'b'`               | 有符号字符型 (int8)     | -128 到 127               |
| `'B'`               | 无符号字符型 (uint8)    | 0 到 255                  |
| `'u'`               | Unicode 字符            | 单个 Unicode 字符         |
| `'h'`               | 有符号短整型 (int16)    | -32,768 到 32,767         |
| `'H'`               | 无符号短整型 (uint16)   | 0 到 65,535               |
| `'i'`               | 有符号整型 (int)        | 一般为 32 位整数          |
| `'I'`               | 无符号整型 (uint)       | 一般为 32 位整数          |
| `'l'`               | 有符号长整型 (int32)    | -2147483648 到 2147483647 |
| `'L'`               | 无符号长整型 (uint32)   | 0 到 4294967295           |
| `'q'`               | 有符号长长整型 (int64)  | 64 位整数                 |
| `'Q'`               | 无符号长长整型 (uint64) | 64 位无符号整数           |
| `'f'`               | 浮点型 (float)          | 单精度浮点数              |
| `'d'`               | 双精度浮点型 (double)   | 双精度浮点数              |
| `'c'`               | 字符 (char)             | 单个字节字符              |

#### 4.3.1.1Value的创建

`Value` 的函数原型及参数解释：

```python
multiprocessing.Value(typecode, initial_value=None, lock=True)
```

**参数说明:**

- **`typecode`**：字符类型代码，用于定义共享变量的数据类型。常见的 `typecode` 有：
  - `'i'`：表示有符号整数
  - `'d'`：表示双精度浮点数
  - `'c'`：表示单个字符
  - 更多类型请参考上表。
- **`initial_value`** (可选)：用于设置共享变量的初始值。如果不指定，则默认为类型的初始值，如整数默认为 `0`。
- **`lock`** (可选)：默认为 `True`，表示在访问共享变量时使用一个锁来保证线程安全。设为 `False` 可以禁用锁，适用于无需并发控制的场景。

**返回值:**

- `Value` 返回一个支持进程间共享的 `Synchronized` 对象，该对象有一个 `.value` 属性，==可以直接读取和修改共享数据==。

#### 4.3.1.2 Array的创建

`Array` 的函数原型及参数解释：

```python
multiprocessing.Array(typecode, size_or_initializer, lock=True)
```

**参数说明:**

- **`typecode`**：字符类型代码，用于定义数组元素的数据类型。常见的 `typecode` 有：
  - `'i'`：表示有符号整数
  - `'d'`：表示双精度浮点数
  - 更多类型请参考上表。
- **`size_or_initializer`**：可以是一个整数或一个可迭代对象。如果是整数，则表示数组的大小，所有元素会初始化为类型默认值（如 `0`）；如果是可迭代对象，则数组会按此对象的内容进行初始化。
- **`lock`** (可选)：默认为 `True`，表示在访问共享数组时使用一个锁以保证线程安全。设为 `False` 可以禁用锁，适用于无需并发控制的场景。

**返回值:**

- `Array` 返回一个支持进程间共享的 `SynchronizedArray` 对象，==可以直接通过索引读取和修改共享数组的元素。==



### 4.3.2在进程中使用共享内存

共享内存可以在多进程中使用。在子进程中可以直接访问主进程创建的 `Value` 和 `Array` 对象，从而实现数据共享。

#### 4.3.2.1多进程中使用共享 Value

```python
from multiprocessing import Process, Value

def increment(shared_val):
    with shared_val.get_lock():  # 获取锁，确保安全访问
        shared_val.value += 1

if __name__ == "__main__":
    shared_value = Value('i', 0)
    
    processes = [Process(target=increment, args=(shared_value,)) for _ in range(5)]
    for p in processes:
        p.start()
    for p in processes:
        p.join()
    
    print("Final Value:", shared_value.value)  # 输出：Final Value: 5
```

共享内存同步：

```python
import time
from multiprocessing import Process,Value,Semaphore

def task(shm,sem):
    while True:
            sem[1].acquire()
            shm.value+=1
            time.sleep(1)
            sem[0].release()

if __name__ == '__main__':
    # 创建共享内存
    shm = Value('i',0)
    sem = [Semaphore(i) for i in range(2)]
    p = [Process(target=task,args=(shm,sem)) for _ in range(1)]

    for i in p:
        i.start()
    while True:
        sem[0].acquire()
        print(shm.value)
        sem[1].release()

```

#### 4.3.2.2多进程中使用共享 Array

```python
import time
from multiprocessing import Process,Array

def task(shm,i):
    with shm.get_lock():
        shm[i] = shm[i] ** 2


if __name__ == '__main__':
    # 创建共享内存
    shm = Array('i',[1,2,3,4,5])

    p = [Process(target=task,args=(shm,i)) for i in range(5)]

    for i in p:
        i.start()

    for i in p:
        i.join()

    print(shm[:])
```

### 4.3.3共享内存的特点及注意事项

特点：

- **简单数据类型共享**：`Value` 和 `Array` 适用于共享简单数据结构（例如，单个值或一维数组）。
- **线程安全**：默认情况下，`Value` 和 `Array` 提供了锁（`Lock`），确保并发访问的安全性。
- **便捷性**：它们是跨平台的，并且容易实现，不需要复杂的序列化或反序列化过程。

注意事项：

- **并发控制**：`Value` 和 `Array` 默认带有锁来确保数据安全。如果不需要锁定机制（例如，只有一个进程访问时），可以通过 `lock=False` 禁用锁。
- **限制于简单类型**：`Value` 和 `Array` 适用于简单的数据结构，如标量或一维数组。如果需要共享更复杂的数据结构，推荐使用 `Manager` 或 `shared_memory` 模块。
- **数据类型限制**：`Value` 和 `Array` 使用 `ctypes` 类型，因此需要使用对应的 `typecode` 指定数据类型。



## 4.4shared_memory共享内存

在 Python 的 `multiprocessing` 模块中，`shared_memory` 提供了一种高效的共享内存方式，允许不同进程间共享数据，而不必复制数据。共享内存适合于需要多进程频繁访问相同数据的大型数据集场景。

### 4.4.1 创建 SharedMemory

在 `multiprocessing.shared_memory` 中可以轻松创建一个共享内存块，并使用该块在进程之间传递数据。

```python
from multiprocessing import shared_memory

# 创建一个共享内存块，大小为 1024 字节
shm = shared_memory.SharedMemory(create=True, size=1024)
```

- **`create`**：指定是否创建新的共享内存块，`True` 表示新建；若为 `False`，则表示连接到已有的共享内存。
- **`size`**：共享内存块的大小（字节数）。仅在创建新共享内存时设置。

### 4.4.2 SharedMemory操作方法

以下是 `SharedMemory` 提供的主要操作方法：

| 方法           | 描述                                                         |
| -------------- | ------------------------------------------------------------ |
| **`buf`**      | 共享内存的缓冲区，允许直接读取或写入数据。例如，可以通过 `shm.buf[:]` 进行数组形式的数据操作。 |
| **`close()`**  | 关闭共享内存对象，但不释放内存块。通常在进程结束时调用。     |
| **`unlink()`** | 删除共享内存块。在所有进程不再使用共享内存后调用，以释放内存资源。 |
| **`name`**     | 共享内存块的唯一名称，其他进程可以通过此名称连接到该内存块。 |

### 4.3.3 SharedMemory使用实例

**MyClass.py**

```python
import struct

class MyClass:
    def __init__(self,name,age):
        self.__name = name
        self.__age = age

    def name_show(self):
        print("name = ",self.__name)
    def age_show(self):
        print("age = ",self.__age)

    def to_bytes(self):
        # 将 name 转换为 10 字节的 UTF-8 字节数组，不足补空格，age 转换为 4 字节整数
        name_bytes = self.__name.encode('utf-8').ljust(10, b' ')  # 固定 10 字节
        age_bytes = struct.pack('i', self.__age)  # 4 字节整数
        return name_bytes + age_bytes

    @classmethod
    def from_bytes(cls, byte_data):
        # 从 10 字节的 name 和 4 字节的 age 解析
        name =  bytes(byte_data[:10]).decode('utf-8').strip()  # 去除补充的空格
        age = struct.unpack('i', byte_data[10:14])[0]
        return cls(name, age)
```

**sender.py**

```python
import os
import time
from  MyClass  import *
from multiprocessing import shared_memory
if __name__ == '__main__':
    print(os.getpid())
    p = MyClass("zhangsan",20)
    shm = shared_memory.SharedMemory(name="shm",create=True,size=14)
    shm.buf[:14] = p.to_bytes()
    time.sleep(10)
    try:
        shm.close()
        shm.unlink()
    except BufferError as e:
        print(e)

```

**reader.py**

```python
import os

from MyClass import *
from multiprocessing import shared_memory

if __name__ == "__main__":
    print(os.getpid())
    shm = shared_memory.SharedMemory(name="shm",)
    ba = shm.buf[:14]
    p = MyClass.from_bytes(ba)
    p.name_show()
    p.age_show()
    try:
        shm.close()
        shm.unlink()
    except BufferError as e:
        print(e)

```



### 4.4.4 SharedMemory特性与注意事项

- **高效性**：`SharedMemory` 允许在不同进程间共享数据而不复制数据，适用于处理大量数据的情景。
- **需要手动管理内存**：在所有进程不再使用共享内存后，必须调用 `unlink()` 来释放资源，否则可能会造成内存泄漏。
- **跨进程访问**：共享内存通过 `name` 在不同进程间共享，确保每个进程都使用 `close()` 和 `unlink()` 正确清理内存资源。



# 5.进程的同步互斥机制

## 5.1进程同步互斥的概念

**进程互斥**是指在某一时刻，只有一个进程能够访问共享资源或临界区（即对共享资源进行操作的代码段）。互斥机制防止多个进程同时操作共享资源，以避免资源竞争和数据不一致的问题。

**进程同步**指的是控制多个进程的执行顺序，以确保它们按特定的逻辑顺序或条件执行。同步机制确保某个进程在执行前等待另一个进程的信号或条件。

Python中实现同步互斥机制如下：

| 机制          | 类型      | 说明                                         |
| ------------- | --------- | -------------------------------------------- |
| **Lock**      | 互斥      | 控制一次只有一个进程访问共享资源             |
| **RLock**     | 互斥      | 支持嵌套的互斥锁，避免同一进程内死锁         |
| **Semaphore** | 互斥/同步 | 控制最大并发数，限制并发访问的数量           |
| **Event**     | 同步      | 等待或触发一个事件，用于进程间通信和信号传递 |
| **Condition** | 同步      | 等待和通知特定条件，用于协调进程的执行顺序   |

## 5.2锁

### 5.2.1锁的概念

`Lock` 是一种用于互斥的工具，它确保同一时刻只有一个线程或进程能够访问共享资源。`Lock` 的状态有两种：

- **锁定（Locked）**：在这个状态下，任何其他线程或进程尝试获取该锁都会被阻塞。
- **未锁定（Unlocked）**：在这个状态下，任何线程或进程都可以获取该锁并进入临界区。

使用锁可以避免多个进程或线程同时修改共享资源，从而保证数据的一致性。

### 5.2.2锁的基本操作

Python 提供了 `threading.Lock` 和 `multiprocessing.Lock`，分别用于线程和进程的同步：

- **`threading.Lock`**：用于线程之间的互斥，适合在多线程编程中控制对共享资源的访问。
- **`multiprocessing.Lock`**：用于进程之间的互斥，适合在多进程编程中控制对共享资源的访问。



锁的主要操作有两个：

- **`acquire(block=True, timeout=None)`**：尝试获取锁。如果锁已被其他线程或进程获取，则调用该方法的线程或进程会阻塞，直到锁被释放。
- **`release()`**：释放锁，使其他等待的线程或进程可以获取锁。


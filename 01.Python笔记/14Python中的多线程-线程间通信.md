[TOC]

# 1.Python中的多线程

## 1.1Python中线程的概念

线程（Thread）是计算机科学中一个执行流的基本单位，也称为“轻量级进程”。每个线程代表一个任务或一段程序的执行路径，能够独立运行并且与其他线程共享同一进程的资源（如内存、文件句柄等）。线程允许程序在同一时间处理多个任务，提升程序的响应速度和性能。

在 Python 中，尤其是 CPython 解释器中，线程被称为“伪线程”主要是因为全局解释器锁（GIL，Global Interpreter Lock）的存在。GIL 限制了多线程在 Python 中的真正并行性，使得多个线程在大多数情况下无法真正同时执行。因此，Python 的线程通常被称为“伪线程”。具体原因如下：



**1. GIL 的限制**

GIL 是 CPython 实现中用来保证线程安全的一个全局锁。GIL 限制了同一时刻只能有一个线程执行 Python 字节码，即使在多核 CPU 上也是如此。这意味着多个线程在 CPU 密集型任务中无法真正实现并行运行。线程虽然能在多个任务之间切换，但切换的时间片由 GIL 控制，这使得 Python 中的多线程更像是单线程的切片执行。

**2. 影响线程的并行性**

由于 GIL 的存在，Python 的多线程表现出以下行为：

- **无法利用多核 CPU**：Python 的多线程不能有效地利用多核 CPU 处理器，因为同一时刻只能有一个线程在执行。这对 CPU 密集型任务的性能造成了很大影响，导致 Python 的多线程在 CPU 密集型任务中效率低下。

- **I/O 密集型任务有一定效果**：在 I/O 密集型任务中（如网络请求、文件操作等），由于线程在等待 I/O 时会释放 GIL，其他线程可以趁机运行，因此多线程能够提升一定的效率。但这并不是真正的并行，只是在 I/O 等待时，GIL 切换到了其他线程。

**3. 真正的多线程实现与Python的对比**

在没有 GIL 的语言中（如 Java、C++ 等），线程可以在多核处理器上真正并行地执行，充分利用多核 CPU 的性能优势。然而在 Python 中，由于 GIL 的限制，Python 线程的并行性大打折扣，所以被称为“伪线程”。

**4. 伪线程的替代方案**

为了应对 GIL 的限制，在需要多核并行的情况下，Python 提供了一些替代方案：

- **多进程**：可以使用 multiprocessing 模块来创建多个进程，每个进程有独立的 GIL 和内存空间，因此可以真正实现并行。但多进程的内存占用比多线程高。

- **异步编程**：对于 I/O 密集型任务，可以使用 asyncio 等异步编程方式，通过事件循环处理并发任务，绕过 GIL 对性能的限制。

> [!CAUTION]
>
> Python 的全局解释器锁（Global Interpreter Lock，简称 GIL）是一个机制，主要存在于 Python 的 CPython 实现中。GIL 锁的设计原因如下：
>
> **简化内存管理**：Python 的内存管理和垃圾回收机制主要依赖于引用计数。引用计数在多线程中存在竞争问题，例如两个线程同时修改某个对象的引用计数，可能导致数据损坏。GIL 的存在避免了这种问题，使得引用计数操作变得线程安全，而不需要为每个对象的引用计数添加额外的锁，从而简化了内存管理。
>
> **提升执行效率**：Python 代码在执行过程中需要频繁调用 C 扩展模块。对于多线程程序来说，如果每次都需要添加锁机制来保证线程安全，会导致性能下降。GIL 在一定程度上可以避免频繁加锁、解锁带来的开销，使得单线程的 Python 程序在执行 I/O 密集型任务时性能有所提升。
>
> **历史原因**：GIL 的设计始于 Python 的早期。当时多核处理器还不普及，Python 的设计目标主要是单核性能。为了简化解释器的实现、提升执行效率，GIL 成为了一个折中的方案。随着 Python 社区对多线程性能的需求增加，GIL 的限制开始显现，但移除它需要对 Python 的底层内存管理机制做大规模的改动，因此在 CPython 中依然保留了 GIL。

## 1.2Python中线程的创建

threading.Thread 是 Python 中用于创建和管理线程的类，它提供了一系列的参数和方法，用于控制线程的行为。以下是一些关键的类和函数：

1. `threading.Thread` **类**：用于创建和管理线程。
2. `threading.Lock` **类**：用于创建线程间互斥锁。
3. `threading.RLock` **类**：可重入锁，用于递归锁定的场景。
4. `threading.Event` **类**：用于线程间通信。
5. `threading.Condition` **类**：提供复杂的线程间同步机制。
6. `threading.Semaphore` **类**：用于控制资源的访问。

**threading.Thread 类**

```python
class threading.Thread(group=None, target=None, name=None, args=(), kwargs={}, *, daemon=None)
```

**参数**

- **group**: 保参数，默认为 `None`。
- **target**: 线程函数，当线程启动时调用。如果未指定，线程对象不会调用任何函数。
- **name**: 线程名称，默认是 "Thread-N" 格式的字符串。
- **args**: 传递给 `target` 函数的位置参数，默认是空元组 `()`。
- **kwargs**: 传递给 `target` 函数的关键字参数，默认是空字典 `{}`。
- **daemon**: 如果为 `True`，线程将在主线程退出时自动终止。

**返回值**

- 返回一个 `Thread` 对象。

**常用方法**

- start()：启动线程，并调用线程的 run() 方法。

- run()：表示线程的活动。通常不直接调用此方法，而是通过 start() 启动线程，由 start() 间接调用 run() 方法。

- join(timeout=None)：阻塞主线程，直到调用此方法的线程终止或达到指定的超时时间。

- is_alive()：返回线程是否仍在运行。

- isDaemon()：返回线程是否为守护线程。

- setDaemon(daemonic)：设置线程为守护线程或非守护线程，daemonic 为布尔值，True 表示守护线程，False 表示非守护线程。

- name：线程名称的属性，可以直接读取或设置。

- ident：线程的唯一标识符属性，在线程运行期间返回线程的 ID，线程终止后为 None。

- daemon：守护线程属性，可以直接读取或设置，True 表示为守护线程，False 表示为非守护线程。

## 1.3Python中的多线程使用实例

### 1.3.1多线程的创建

```python
import sys
import threading
import time
from threading import Thread


def thread1(num):
    print(f"num = {num},thread-name:{threading.current_thread().name}")
    sys.stdout.flush()  # 刷新缓冲区
    time.sleep(1)

#	•	每个 Python 模块都有一个内置变量 __name__。
#	•	如果模块是直接运行的（比如执行 python script.py），__name__ 的值会被设置为 "__main__"。
#	•	如果模块是被其他模块导入的（比如 import module），__name__ 的值会被设置为模块的名称（如 "module"）。

if __name__ == "__main__":
    thread = Thread(target=thread1, name="www.baidu.com", args=(11,))

    thread.start()

    print(thread.is_alive())  # 线程是否在运行
    print(thread.name)  # 线程的名字
    print(thread.ident)  # 线程号

    thread.join()

```

## 1.4Timer

### 1.4.1Timer定时器接口详解

Timer 是 threading.Thread 的子类，其主要功能是在经过指定的时间间隔后执行一个函数。

基本语法：

```python
threading.Timer(interval, function, args=None, kwargs=None)
```

- interval：延迟的时间（以秒为单位）。
- function：定时器到点后要执行的目标函数。
- args：传递给目标函数的位置参数（可选）。
- kwargs：传递给目标函数的关键字参数（可选）。

### 1.4.2Timer定时器实例

```py
import threading


def timer_func():
    print(f"定时时间到了")


if __name__ == "__main__":
    t = threading.Timer(5, timer_func)
    t.start()
    t.join()

```

### 1.4.3循环定时器的实例

```python
import threading
import inspect


def timer_func():
    f = inspect.currentframe()
    print(f"文件名:{f.f_code.co_filename},函数名:{f.f_code.co_name},行号：{f.f_lineno}")
    t = threading.Timer(2, timer_func)
    t.start()


if __name__ == "__main__":
    t = threading.Timer(2, timer_func)
    t.start()

```

# 2.线程间通信

## 2.1使用全局变量通信

```python
import threading
import time

money = 1000
# 在函数内可以直接读取全局变量
# 在函数内如果要修改全局变量的值必须加上global的修饰
# 这个实例中因为线程执行需要获取GIL锁，所以thread1和
# thread2并没有同步，但是可以验证两线程通信


def thread1():
    global money
    while money > 0:
        money -= 100
        print(f"{threading.current_thread().name} 修改money={money}")
        time.sleep(1)


def thread2():
    while money > 0:
        print(f"{threading.current_thread().name} 获取money={money}")
        time.sleep(2)


if __name__ == "__main__":
    t1 = threading.Thread(target=thread1, name="thread1")
    t2 = threading.Thread(target=thread2, name="thread2")
    t1.start()
    t2.start()

    t1.join()
    t2.join()

```

## 2.2线程间通信之Queue

### 2.2.1queue.Queue概述

- queue.Queue 是线程安全的 FIFO（先进先出）队列，适用于线程间通信。

- 内部通过锁机制保证线程安全，支持多个线程同时放入或取出数据而不会引发数据竞争。

### 2.2.2初始化队列

```python
Queue(maxsize=0)
```

- **功能**：创建一个队列实例。

- **参数** maxsize：

  - 限制队列的最大容量。

  - 如果为 0 或负值，表示队列的容量是无限的。

- **返回值**：队列实例。

  

### 2.2.3基本操作方法

| 方法                                  | 功能                                                         | 返回值         |
| ------------------------------------- | ------------------------------------------------------------ | -------------- |
| `put(item, block=True, timeout=None)` | 向队列尾部插入一个元素。如果队列已满，行为取决于 `block` 和 `timeout`。 | 无返回值       |
| `get(block=True, timeout=None)`       | 从队列头部获取一个元素。如果队列为空，行为取决于 `block` 和 `timeout`。 | 返回获取的元素 |
| `put_nowait(item)`                    | 非阻塞地向队列尾部插入一个元素。如果队列已满，抛出 `queue.Full`异常。 | 无返回值       |
| `get_nowait()`                        | 非阻塞地从队列头部获取一个元素。如果队列为空，抛出 `queue.Empty`异常。 | 返回获取的元素 |
| `task_done()`                         | 表示队列中的一个任务完成。如果使用了 `get`，但未调用 `task_done`，会导致队列阻塞。 | 无返回值       |
| `join()`                              | 阻塞主线程，直到队列中所有任务都被处理完，即 `task_done` 的调用次数与 `get` 的调用次数一致。 | 无返回值       |

### 2.2.4查询状态的方法

| 方法      | 功能                                          | 返回值                                                       |
| --------- | --------------------------------------------- | ------------------------------------------------------------ |
| `qsize()` | 返回当前队列中的元素数量。                    | 队列中元素的数量（整数）。注意，这个值可能不准确，尤其在多线程环境中。 |
| `empty()` | 如果队列为空，返回 `True`；否则返回 `False`。 | 布尔值                                                       |
| `full()`  | 如果队列已满，返回 `True`；否则返回 `False`。 | 布尔值                                                       |

### 2.2.5生产者消费者模型实例

```python
import threading
import queue
import time


def producer(q):
    n = 1
    while True:
        if q.empty():
            q.put(f"item{n}")
            if n == 15:
                break
            n += 1
            time.sleep(1)


def consumer(q):
    while True:
        item = q.get()
        q.task_done()
        print(item)
        if item == "item15":
            break


if __name__ == "__main__":
    q = queue.Queue(maxsize=5)

    t1 = threading.Thread(target=producer, args=(q,))
    t2 = threading.Thread(target=consumer, args=(q,))
    t1.start()
    t2.start()

    t1.join()
    t2.join()
    q.join()  # 如果队列数据取出后没有标记为task_done,这里会永久阻塞

```

## 2.3线程间通信之Event

在Python中，`threading.Event`是一个非常有用的同步原语，用于在线程之间通信和协调。`Event`对象管理一个内部标志，可以被设置为`True`或`False`，线程可以等待这个标志被设置为`True`，以便继续执行。

### 2.3.1Event创建

```python
import threading
event = threading.Event()
```

功能

`threading.Event` 对象用于线程间通信和同步。它管理一个内部的布尔标志，通过设置和清除这个标志，来通知等待的线程某个事件已经发生或尚未发生。主要用于协调线程活动，让线程等待某个条件或事件的发生。

参数

`threading.Event` 的构造函数不接受任何参数。调用时直接创建一个新的 Event 对象。

返回值

返回一个新的 `Event` 对象。该对象提供了一组方法，用于控制和检查内部标志的状态。

### 2.3.2主要方法

1. set()

- 功能：将内部标志设置为 `True`，并唤醒所有等待该事件的线程。
- 参数：无
- 返回值：无

2. clear()

- 功能：将内部标志设置为 `False`。之后调用 `wait()` 的线程将被阻塞，直到再次调用 `set()`。
- 参数：无
- 返回值：无

3. wait(timeout=None)

- 功能：阻塞当前线程，直到内部标志为 `True`。可选参数 `timeout` 指定最大等待时间（秒）。如果超过这个时间仍未设置标志，则返回 `False`，否则返回 `True`。
- 参数：
  - `timeout`（可选）：等待的最大时间（秒）。
- 返回值：
  - `True` 如果标志被设置为 `True`。
  - `False` 如果 `timeout` 结束标志仍未设置。

4. is_set()

- 功能：检查内部标志是否为 `True`。
- 参数：无
- 返回值：`True` 如果内部标志为 `True`，否则为 `False`。

### 2.3.3Event使用实例

```python
import threading
import time


def thread1(e):
    n = 5
    while n:
        time.sleep(2)
        e.set()
        n -= 1


def thread2(e):
    n = 5
    while n:
        e.wait()  # 阻塞等待就绪
        e.clear()  # 清除就绪标志
        print("111111111111111")
        n -= 1


if __name__ == "__main__":
    ev = threading.Event()
    t1 = threading.Thread(target=thread1, args=(ev,))
    t2 = threading.Thread(target=thread2, args=(ev,))
    t1.start()
    t2.start()

    t1.join()
    t2.join()

```

## 2.4线程间通信之Condition

多线程条件变量（Condition）是Python中线程同步的一种机制，主要用于实现线程间的协调和通信。它允许一个或多个线程等待某个条件满足后再继续执行。下面是对Python多线程条件变量的详细讲解：

基本概念

- **Condition对象**：Condition对象是一个线程同步原语，包含了一个Lock对象和一个等待队列。只有拥有条件的线程才能改变或检查共享资源。
- **等待（wait）**：线程可以调用Condition对象的wait()方法进入等待状态，直到被其他线程通知（唤醒）。
- **通知（notify/notify_all）**：Condition对象提供了notify()方法和notify_all()方法，前者唤醒一个等待的线程，后者唤醒所有等待的线程。

### 2.4.1条件变量的创建

条件变量（Condition）的接口定义在`threading`模块中。其基本结构如下：

```python
class threading.Condition(lock=None)
```

参数

- **lock** (可选)：一个Lock或RLock对象。如果没有提供，Condition对象会自动创建一个RLock对象。

### 2.4.2条件变量常用的操作方法

1. acquire(*args)

   获取Condition对象关联的锁。调用的是底层锁的`acquire`方法。

   - **参数**：可变参数，传递给底层锁的`acquire`方法。
   - **返回值**：布尔值，表示是否成功获取锁。

2. release()

   释放Condition对象关联的锁。调用的是底层锁的`release`方法。

   - **参数**：无。
   - **返回值**：无。

3. wait(timeout=None)

   等待条件变量通知或超时。

   - **参数**：
     - **timeout** (可选)：浮点数，指定等待的超时时间（秒）。如果省略，默认无限期等待。
   - **返回值**：无。如果调用时条件已满足，则返回。

4. notify(n=1)

   通知一个（默认）或多个等待的线程，可以继续执行。

   - **参数**：
     - **n**：整数，指定通知的线程数，默认是1。
   - **返回值**：无。

5. notify_all()

   通知所有等待的线程。

   - **参数**：无。
   - **返回值**：无。

### 2.4.3条件变量使用实例

```python
import queue
import random
import threading


def producer(con, q):
    while True:
        with con:
            while q.qsize() >= 5:
                print(f"队列成员已满5个，生产者线程阻塞")
                con.wait()
            x = random.randint(1, 101)
            q.put(x)
            print(f"生产者线程添加数据：{x}")
            con.notify()


def consumer(con, q, i):
    while True:
        with con:
            while q.empty():
                print(f"队列中没有数据，消费着线程阻塞")
                con.wait()
            print(f"消费者线程{i}获取数据：{q.get()}")
            q.task_done()
            con.notify()


if __name__ == "__main__":
    q = queue.Queue(5)
    con = threading.Condition()
    t1 = threading.Thread(target=producer, args=(con, q))
    tc = [threading.Thread(target=consumer, args=(con, q, i)) for i in range(5)]
    t1.start()
    for t in tc:
        t.start()

    t1.join()
    for t in tc:
        t.join()

```

# 3.线程池

# Python Advanced Core Interview Questions and Answers

### 1. What is iterator in Python?

An iterator is an object in Python that allows you to traverse through all the elements of a collection, such as a list or tuple, **one element at a time**. Iterators implement two methods: `__iter__()` and `__next__()`. You can use the `next()` function to retrieve elements sequentially until it raises `StopIteration` when no items are left.

### 2. What is iterable?

An iterable is any Python object capable of returning its elements one by one, **allowing it to be iterated over in a loop**. Examples include lists, tuples, dictionaries, and sets. An object is iterable if it implements the `__iter__()` method.

### 3. Difference between iterable and iterator

An **iterable** is an object you can loop over, but it does not itself keep track of the iteration state. An **iterator** is an object that keeps the state of where it is during iteration and provides elements one at a time using `__next__()`.

### 4. What is `__iter__()` and `__next__()`?

The `__iter__()` method returns the iterator object itself, and `__next__()` returns the next element in the sequence. If there are no elements left, `__next__()` raises a `StopIteration` exception, signaling the end of iteration.

### 5. What is generator?

A generator is a special type of iterator in Python that **produces items lazily**, meaning it generates each value on the fly rather than storing the entire sequence in memory. Generators use the `yield` keyword to produce a value and pause execution until the next value is requested.

### 6. Difference between generator and normal function

A normal function returns all its results at once and exits, while a generator **yields results one at a time** and can resume execution, maintaining its state between calls. This makes generators memory-efficient for large datasets.

### 7. What is `yield` keyword?

The `yield` keyword is used inside a function to make it a generator. When `yield` is called, it **produces a value and pauses the function’s execution**, resuming later from the same point on the next call.

### 8. What is generator expression?

A generator expression is a concise way to create a generator, similar to a list comprehension but with parentheses instead of square brackets. It generates values **lazily**, saving memory.

```python
squares = (x*x for x in range(5))
```

### 9. What is lazy evaluation?

Lazy evaluation is the process of **delaying computation until the result is actually needed**. Generators and iterators in Python use lazy evaluation, which improves efficiency when working with large datasets.

### 10. What is decorator?

A decorator is a **function that takes another function and extends its behavior without modifying its code**. It allows you to wrap functionality around existing functions in a clean and readable way.

### 11. Why are decorators used?

Decorators are used for code **reuse, logging, access control, memoization, and other cross-cutting concerns**. They let you add functionality dynamically without changing the original function.

### 12. How do decorators work internally?

A decorator is a function that returns a **wrapper function**. The wrapper function takes the same arguments as the original function, calls it, and can add logic before or after it. Using `@decorator` is just syntactic sugar for `func = decorator(func)`.

### 13. What is `@staticmethod`?

A `@staticmethod` is a method that **does not require access to the instance or class**. It behaves like a normal function but belongs to the class’s namespace for organizational purposes.

### 14. What is `@classmethod`?

A `@classmethod` is a method that receives the **class itself as the first argument** (`cls`) instead of an instance (`self`). It can modify class-level variables or create instances using class methods.

### 15. Difference between staticmethod and classmethod

`staticmethod` cannot access the class or instance data, whereas `classmethod` can access class attributes and methods through the `cls` parameter. `classmethod` is commonly used as alternative constructors.

### 16. What is monkey patching?

Monkey patching is the **dynamic modification of a class or module at runtime**, such as adding or changing methods. While powerful, it should be used carefully as it can lead to unpredictable behavior.

### 17. What is deep copy and shallow copy?

A **shallow copy** copies the object structure but references the same nested objects. A **deep copy** duplicates the object and all nested objects, creating an entirely independent clone.

### 18. Difference between `copy()` and `deepcopy()`

`copy()` creates a shallow copy, while `deepcopy()` from the `copy` module creates a recursive deep copy, ensuring nested objects are also duplicated.

### 19. What is garbage collection in Python?

Garbage collection is the **automatic process of reclaiming memory** by deleting objects that are no longer referenced. Python uses reference counting and a cyclic garbage collector to manage memory efficiently.

### 20. What is reference counting?

Reference counting tracks the number of references to an object. When the reference count drops to zero, the object becomes unreachable and is automatically removed by the garbage collector.

### 21. What is memory leak?

A memory leak occurs when objects are **no longer needed but still referenced**, preventing Python’s garbage collector from freeing memory, which can eventually slow down or crash programs.

### 22. What is GIL (Global Interpreter Lock)?

GIL is a mutex in CPython that allows only **one thread to execute Python bytecode at a time**, even on multi-core systems. It simplifies memory management but limits multi-threaded CPU-bound performance.

### 23. Why does Python have GIL?

Python has GIL to **protect access to Python objects and memory management**, making implementation of CPython simpler and thread-safe. However, it can restrict CPU-bound multi-threading performance.

### 24. What is multithreading?

Multithreading is the **execution of multiple threads within a single process**. Threads share memory and resources, which makes them lightweight compared to multiple processes. It is suitable for I/O-bound tasks.

### 25. Difference between multithreading and multiprocessing

Multithreading shares memory space and is **limited by GIL for CPU-bound tasks**. Multiprocessing creates separate processes with independent memory, bypassing GIL and utilizing multiple CPU cores effectively.

### 26. What is serialization?

Serialization is the process of **converting an object into a byte stream** so it can be stored or transmitted. It is essential for saving state or sending objects over networks.

### 27. What is pickling and unpickling?

Pickling is **serializing a Python object to a byte stream**, while unpickling is **deserializing the byte stream back to a Python object** using the `pickle` module.

### 28. What is `__slots__`?

`__slots__` is used to **explicitly declare allowed attributes** in a class, preventing the creation of `__dict__` for each object. It reduces memory usage for large numbers of objects.

### 29. What is name mangling?

Name mangling is Python’s technique for **making private attributes harder to access** by prefixing the attribute name with `_ClassName`. It is a convention for enforcing limited access.

### 30. What is introspection in Python?

Introspection is the ability of Python to **examine objects at runtime**, such as checking attributes, methods, or types. It allows dynamic and flexible programming.

### 31. What is `dir()` and `help()`?

`dir()` lists all attributes and methods of an object, while `help()` shows the **documentation and usage** of an object, function, or module.

### 32. What is `eval()`?

`eval()` evaluates a **string as a Python expression** and returns the result. For example, `eval('2 + 3')` returns `5`. It is limited to single expressions.

### 33. What is `exec()`?

`exec()` executes **dynamic Python code** from a string, which can include multiple statements. Unlike `eval()`, it does not return a value.

### 34. Difference between `eval()` and `exec()`

`eval()` is used for **expressions and returns a value**, while `exec()` can execute **statements or code blocks** but returns `None`. `exec()` is more flexible but potentially riskier.

### 35. What is Python memory model?

Python’s memory model manages **object allocation, reference counting, and garbage collection**. Immutable objects like strings and numbers may be shared to optimize memory, while mutable objects are allocated per instance. Memory is handled by the Python interpreter, with cyclic garbage collection for unreachable objects.


# Python Multithreading and Multiprocessing Interview Questions and Answers

### 1. What is multithreading in Python?

Multithreading is the process of **running multiple threads concurrently within a single process**. Each thread can execute a task independently while sharing the same memory space. Multithreading is especially useful for I/O-bound tasks like file operations, network requests, or database queries, as it allows the program to perform other tasks while waiting for I/O operations to complete.

### 2. What is multiprocessing in Python?

Multiprocessing is the use of **multiple processes running in parallel**. Each process has its own memory space and Python interpreter, allowing true parallel execution on multi-core CPUs. Multiprocessing is ideal for **CPU-bound tasks** where tasks require significant computation, bypassing Python’s Global Interpreter Lock (GIL) limitation.

### 3. Difference between multithreading and multiprocessing

Multithreading shares memory and resources among threads, but due to the GIL, only one thread executes Python bytecode at a time. Multiprocessing uses separate memory spaces and processes, achieving **true parallelism** and efficiently using multiple CPU cores. Threads are lighter and faster to create, whereas processes are heavier but suitable for CPU-intensive tasks.

### 4. What is Global Interpreter Lock (GIL)?

The Global Interpreter Lock (GIL) is a mutex in CPython that **allows only one thread to execute Python bytecode at a time**, even on multi-core systems. While it simplifies memory management and prevents race conditions, it restricts CPU-bound multi-threading performance.

### 5. Why does Python have GIL?

Python uses GIL to **protect access to Python objects and memory management**, making the interpreter thread-safe. It ensures reference counting for garbage collection is consistent but limits true parallel execution for CPU-bound tasks.

### 6. How does multithreading handle CPU-bound vs I/O-bound tasks?

For **I/O-bound tasks**, multithreading is very effective because threads can wait for I/O operations without blocking other threads. For **CPU-bound tasks**, multithreading is limited due to the GIL. In CPU-intensive scenarios, multiprocessing is preferred to utilize multiple cores effectively.

### 7. How to create a thread in Python?

Threads can be created by **subclassing `threading.Thread`** or by passing a target function to the `Thread` class.

```python
import threading

def task():
    print("Thread running")

# Using Thread class with target function
thread1 = threading.Thread(target=task)
thread1.start()
thread1.join()
```

### 8. How to create a process in Python?

Processes can be created using the `multiprocessing` module by creating `Process` objects and starting them.

```python
from multiprocessing import Process

def task():
    print("Process running")

process1 = Process(target=task)
process1.start()
process1.join()
```

### 9. What is the difference between thread and process?

A **thread** is a lightweight unit of execution within a process, sharing the same memory space. A **process** has its own memory space and interpreter, making it heavier but capable of running in parallel without GIL limitations.

### 10. What is thread safety?

Thread safety means that **shared data can be safely accessed by multiple threads** without causing data corruption or race conditions. Techniques like locks, semaphores, and synchronization mechanisms are used to ensure thread safety.

### 11. What is a Lock in Python threading?

A Lock is a synchronization primitive in Python used to **ensure that only one thread accesses a shared resource at a time**. It prevents race conditions in multithreaded programs.

```python
import threading
lock = threading.Lock()

lock.acquire()
# critical section
lock.release()
```

### 12. What is a Queue in multithreading?

A `Queue` is a thread-safe FIFO data structure provided by Python’s `queue` module. It allows threads to **communicate and exchange data safely** without explicit locks.

### 13. What is a Pool in multiprocessing?

A Pool is a collection of worker processes in the `multiprocessing` module. It allows you to **map tasks across multiple processes** efficiently and simplifies process management.

```python
from multiprocessing import Pool

def square(x):
    return x*x

with Pool(4) as p:
    result = p.map(square, [1,2,3,4])
```

### 14. How to share data between threads?

Threads share the same memory space, so **shared variables or data structures** can be accessed by multiple threads. However, synchronization mechanisms like `Lock` are needed to avoid race conditions.

### 15. How to share data between processes?

Since processes have separate memory, data sharing requires **IPC mechanisms** such as `multiprocessing.Queue`, `Pipe`, or shared memory (`Value` and `Array`).

### 16. What is deadlock?

A deadlock occurs when **two or more threads or processes wait indefinitely** for each other to release resources. Proper locking strategies, timeout mechanisms, and careful resource management are needed to prevent deadlocks.

### 17. What is a daemon thread?

A daemon thread runs in the **background and does not prevent the program from exiting**. When all non-daemon threads finish, the program terminates, killing all daemon threads automatically.

### 18. What is the difference between synchronous and asynchronous execution?

Synchronous execution runs tasks **one after another**, blocking until each task finishes. Asynchronous execution allows tasks to **run concurrently**, improving responsiveness, especially for I/O-bound operations.

### 19. How to handle exceptions in threads?

Exceptions in threads do not propagate to the main thread automatically. You can **catch exceptions inside the thread function** or subclass `Thread` and override `run()` to handle exceptions.

### 20. When to use multithreading vs multiprocessing?

Use **multithreading** for I/O-bound tasks where waiting for input/output occurs frequently, like network requests or file operations. Use **multiprocessing** for CPU-bound tasks that require intensi


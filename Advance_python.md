# Python Advanced Core Interview Questions & Answers

### 1. What is iterator in Python?
An iterator is an object that contains a countable number of values and can be iterated upon (traversed). It implements the **Iterator Protocol**, consisting of `__iter__()` and `__next__()`. Unlike a list, an iterator doesn't store all elements in memory; it calculates the next value only when requested, making it memory efficient.
* **Example:**
    ```python
    my_list = [10, 20]
    my_iter = iter(my_list) # Get iterator
    print(next(my_iter))    # 10
    print(next(my_iter))    # 20
    # next(my_iter) -> Raises StopIteration
    ```

### 2. What is iterable?
An iterable is any object that can return an iterator. If an object has the `__iter__()` method defined (or a `__getitem__()` method for sequence types), it is an iterable. Iterables can be used in `for` loops, but they do not keep track of their "current position" like an iterator does.
* **Example:** Lists, Strings, Tuples, and Dictionaries are iterables.
    ```python
    numbers = [1, 2, 3] # This is an iterable
    for n in numbers:    # The loop internally calls iter(numbers)
        print(n)
    ```

### 3. Difference between iterable and iterator
The main difference is "state." An **iterable** is the container (like a box of tools), while the **iterator** is the mechanism that hands you the tools one by one and knows which one is next. An iterable doesn't "exhaust," but an iterator does—once you reach the end of an iterator, it remains empty.
* **Example:**
    ```python
    nums = [1, 2]
    it = iter(nums)
    print(next(it)) # 1
    print(next(it)) # 2
    # The list 'nums' can still be used, but 'it' is now exhausted.
    ```

### 4. What is `__iter__()` and `__next__()`?
`__iter__()` is called when you initialize an iterator; it must return the iterator object itself. `__next__()` is called to fetch the subsequent value. When no more items are available, `__next__()` must raise the `StopIteration` exception to signal the end of the loop.
* **Example:**
    ```python
    class MyCounter:
        def __init__(self, low, high):
            self.current = low
            self.high = high
        def __iter__(self):
            return self
        def __next__(self):
            if self.current > self.high:
                raise StopIteration
            self.current += 1
            return self.current - 1
    ```

### 5. What is generator?
A generator is a function that returns an iterator. It looks like a normal function but contains the `yield` statement. Generators are "lazy," meaning they produce values on demand, which is excellent for processing infinite streams or large files without loading them into RAM entirely.
* **Example:**
    ```python
    def count_up_to(n):
        count = 1
        while count <= n:
            yield count
            count += 1
    ```

### 6. Difference between generator and normal function
A normal function uses `return`, which sends a value back and destroys the function's local state. A generator uses `yield`, which "pauses" the function, saving all variables, and returns a value. When called again, the generator resumes exactly where it left off.
* **Example:** A `return` function can only give you a list of 1 million items (heavy RAM). A `yield` generator gives you 1 item at a time (light RAM).

### 7. What is `yield` keyword?
`yield` is the keyword that turns a function into a generator. It acts as a temporary return. When the Python interpreter hits a `yield`, it returns the value to the caller but stays inside the function, waiting for the next request to continue to the next line of code.

### 8. What is generator expression?
A generator expression is a one-line shorthand for creating a generator. It uses the same syntax as list comprehension but with parentheses `()` instead of square brackets `[]`. It is much more memory-efficient than list comprehension for large datasets.
* **Example:**
    ```python
    # List comprehension (Immediate memory usage)
    squares_list = [x**2 for x in range(1000000)]
    # Generator expression (Memory usage is near zero)
    squares_gen = (x**2 for x in range(1000000))
    ```

### 9. What is lazy evaluation?
Lazy evaluation is a strategy where an expression is not evaluated until its value is needed. This prevents unnecessary calculations and reduces memory footprint. In Python, generators and iterators are the primary tools for implementing lazy evaluation.
* **Example:** `range(1000000000)` in Python 3 is lazy. It doesn't create a billion numbers; it just "knows" how to generate the next one when you ask.

### 10. What is decorator?
A decorator is a design pattern that allows you to add new functionality to an existing object (usually a function) without modifying its structure. It is a "wrapper" that executes code before and after the original function runs.
* **Example:**
    ```python
    def my_decorator(func):
        def wrapper():
            print("Before call")
            func()
            print("After call")
        return wrapper

    @my_decorator
    def say_hi():
        print("Hi!")
    ```

### 11. Why are decorators used?
Decorators are used to clean up code by separating "business logic" from "utility logic." Common use cases include logging, enforcing authorization (checking if a user is logged in), timing functions (benchmarking), and rate-limiting API calls.

### 12. How do decorators work internally?
In Python, functions are first-class objects. This means you can pass a function as an argument to another function. When you use the `@decorator` syntax, Python effectively does: `original_func = decorator(original_func)`. The original function is replaced by the "wrapper" returned by the decorator.

### 13. What is `@staticmethod`?
A static method is a method that belongs to a class but does not have access to the class instance (`self`) or the class itself (`cls`). It behaves like a plain function but is nested inside the class for better organization and namespace management.
* **Example:**
    ```python
    class Math:
        @staticmethod
        def add(a, b):
            return a + b
    ```

### 14. What is `@classmethod`?
A class method is a method that is bound to the class and not the object of the class. It receives the class itself as the first argument, typically named `cls`. It is often used for "factory methods" that create class instances in different ways.
* **Example:**
    ```python
    class Person:
        @classmethod
        def from_birth_year(cls, year):
            return cls(2024 - year) # Creates a Person instance
    ```

### 15. Difference between staticmethod and classmethod
`@classmethod` can access and modify the class state (attributes of the class), while `@staticmethod` cannot. Use `@classmethod` when you need to access other class methods or class variables. Use `@staticmethod` when the function logic is totally independent of the class data.

### 16. What is monkey patching?
Monkey patching is the practice of replacing or extending code at runtime. You can change the behavior of a function or a class after it has been defined. While useful for testing or fixing bugs in 3rd party libraries, it can make code very difficult to debug.
* **Example:**
    ```python
    import math
    math.sqrt = lambda x: "I am a monkey!"
    print(math.sqrt(4)) # Outputs: I am a monkey!
    ```

### 17. What is deep copy and shallow copy?
A **shallow copy** creates a new collection object but populates it with references to the original nested objects. A **deep copy** creates a new collection and then recursively creates new copies of all nested objects.


### 18. Difference between `copy()` and `deepcopy()`
If you change a nested object in a `copy()`, the change will appear in the original because they share the same reference. In `deepcopy()`, the original and the copy are 100% independent.
* **Example:**
    ```python
    import copy
    list_a = [[1], [2]]
    list_b = copy.copy(list_a)
    list_b[0][0] = 99 # This changes list_a too!
    ```

### 19. What is garbage collection in Python?
Python uses an automated memory management system. The garbage collector (GC) tracks objects in memory and deallocates them when they are no longer reachable. It mainly uses "Reference Counting" but also has a "Generational Garbage Collector" to find and fix reference cycles.

### 20. What is reference counting?
Every object in Python has a counter that tracks how many variables or objects are pointing to it. When you delete a variable or it goes out of scope, the count decreases. When the count hits **zero**, Python immediately frees that memory.

### 21. What is memory leak?
In Python, a memory leak usually happens when objects are technically "reachable" but no longer used. This often occurs due to "reference cycles" (Object A points to B, and B points back to A) or by keeping large global variables/lists that never get cleared.

### 22. What is GIL (Global Interpreter Lock)?
The GIL is a physical lock in the CPython interpreter that ensures only one thread executes Python bytecode at a time. This means that even on a 16-core CPU, a single Python process will only use 1 core for executing Python code, which can be a bottleneck for CPU-heavy tasks.

### 23. Why does Python have GIL?
The GIL was implemented to make memory management (Reference Counting) simple and thread-safe. Without the GIL, every single change to a reference count would need its own lock, which would make single-threaded programs significantly slower and more complex.

### 24. What is multithreading?
Multithreading is running multiple threads (tasks) concurrently within one process. Because threads share memory, it's very fast to switch between them. However, due to the GIL, it is best used for **I/O-bound** tasks where the CPU spends time waiting for data (like downloading files).

### 25. Difference between multithreading and multiprocessing
Multithreading runs on 1 core and shares memory (good for web scraping). Multiprocessing creates separate memory spaces and separate Python instances for each core (good for heavy math). Multiprocessing bypasses the GIL because each process has its own GIL.

### 26. What is serialization?
Serialization is the process of converting complex data structures (like a Dictionary or a Class instance) into a format that can be stored (like a file) or transmitted (over a network). Common formats are JSON, XML, and Pickle.

### 27. What is pickling and unpickling?
**Pickling** is the Python-specific way of serializing objects into a binary format. **Unpickling** is the reverse process. Unlike JSON, Pickle can store almost any Python object, but it is not secure—never unpickle data from an untrusted source.
* **Example:**
    ```python
    import pickle
    data = {'id': 1}
    with open('file.pkl', 'wb') as f:
        pickle.dump(data, f)
    ```

### 28. What is `__slots__`?
By default, Python objects store their attributes in a dictionary (`__dict__`). This consumes a lot of RAM. `__slots__` allows you to tell Python not to use a dictionary and only allocate space for a fixed set of attributes, significantly reducing memory usage for millions of objects.
* **Example:**
    ```python
    class Point:
        __slots__ = ('x', 'y')
    ```

### 29. What is name mangling?
If you name a class attribute with two underscores (e.g., `__value`), Python changes the name to `_ClassName__value`. This is called "name mangling." It is used to ensure that subclasses don't accidentally override "private" variables of a parent class.

### 30. What is introspection in Python?
Introspection is the ability of a program to examine the type or properties of an object at runtime. You can ask an object "What are your methods?" or "What is your type?" This makes Python highly dynamic and flexible.

### 31. What is `dir()` and `help()`?
* `dir(object)`: Returns a list of all attributes and methods available for that object.
* `help(object)`: Opens the interactive documentation for that object, showing its docstrings and how to use it.

### 32. What is `eval()`?
`eval()` takes a string and evaluates it as a Python expression. For example, `eval("1 + 2")` returns the integer `3`. It is considered dangerous if used with user-provided strings because it can execute malicious code.

### 33. What is `exec()`?
`exec()` is similar to `eval()`, but it can execute entire blocks of Python code, including loops, class definitions, and imports. It does not return a value.
* **Example:** `exec("for i in range(3): print(i)")`

### 34. Difference between `eval()` and `exec()`
`eval()` is used for **expressions** that return a value (e.g., math or variable access). `exec()` is used for **statements** (e.g., `if`, `for`, `import`) and always returns `None`.

### 35. What is Python memory model?
The Python memory model involves a "Private Heap" where all objects and data structures are stored. The Python Memory Manager handles the allocation of this heap. It uses "Arenas," "Pools," and "Blocks" to manage small objects efficiently and relies on the Garbage Collector to clean up when objects are no longer needed.



# Python Multithreading and Multiprocessing Interview Questions

### 1. What is multithreading in Python?
Multithreading is a technique where a single process spawns multiple "threads" to perform tasks concurrently. In Python, these threads share the same memory space. While it doesn't provide true parallel execution for CPU tasks due to the GIL, it is incredibly efficient for I/O-bound tasks where the program would otherwise sit idle.
* **Example:**
    ```python
    import threading
    import time

    def print_numbers():
        for i in range(5):
            time.sleep(1) # Simulating I/O (waiting)
            print(f"Number: {i}")

    t1 = threading.Thread(target=print_numbers)
    t1.start()
    t1.join()
    ```

### 2. What is multiprocessing in Python?
Multiprocessing allows the programmer to run multiple independent processes, each with its own Python interpreter and memory space. This bypasses the Global Interpreter Lock (GIL) because each process has its own GIL. It allows you to fully utilize multiple CPU cores for heavy mathematical calculations or data processing.
* **Example:**
    ```python
    from multiprocessing import Process

    def compute_heavy_task():
        print("Computing on a separate CPU core...")

    if __name__ == "__main__":
        p1 = Process(target=compute_heavy_task)
        p1.start()
        p1.join()
    ```

### 3. Difference between multithreading and multiprocessing
The core difference is memory and CPU usage. **Threads** share memory, making them lightweight but limited by the GIL; they are best for tasks like web scraping. **Processes** have separate memory, making them heavier but capable of true parallelism; they are best for tasks like image processing or matrix multiplication.



### 4. What is Global Interpreter Lock (GIL)?
The GIL is a mutex (or a lock) that allows only one thread to hold control of the Python interpreter at a time. This means that even if you have a multi-core processor, only one thread executes Python bytecode at any given moment. It exists mainly to make memory management (like reference counting) simpler and safer.

### 5. Why does Python have GIL?
Python uses reference counting for memory management. Without the GIL, if two threads increased the reference count of an object simultaneously, it could lead to memory leaks or objects being deleted while still in use. The GIL was the simplest solution to prevent these race conditions when Python was first developed.

### 6. How does multithreading handle CPU-bound vs I/O-bound tasks?
For **I/O-bound tasks** (like requesting data from a URL), multithreading is great because while one thread waits for the internet, another can start. However, for **CPU-bound tasks** (like calculating prime numbers), multithreading actually becomes slower than a single thread because of the overhead of switching between threads that are fighting for the GIL.

### 7. How to create a thread in Python?
You can create a thread by using the `threading.Thread` class. You pass the function you want to run to the `target` argument and then call `.start()`. It is important to call `.join()` if you want the main program to wait for the thread to finish before exiting.
* **Example:**
    ```python
    import threading

    def task(name):
        print(f"Task {name} is running")

    thread = threading.Thread(target=task, args=("Alpha",))
    thread.start()
    ```

### 8. How to create a process in Python?
Similar to threading, you use the `multiprocessing.Process` class. Because each process is a totally new instance of Python, this code must be protected by the `if __name__ == "__main__":` block to avoid infinite loops of process creation on certain operating systems like Windows.
* **Example:**
    ```python
    from multiprocessing import Process

    def task():
        print("Child Process")

    if __name__ == "__main__":
        p = Process(target=task)
        p.start()
    ```

### 9. What is the difference between thread and process?
A **thread** is an entity within a process that can be scheduled for execution. Multiple threads within one process share the same data space. A **process** is an independent execution unit that contains its own memory, file handles, and a copy of the interpreter. If a process crashes, others continue; if a thread crashes, it might crash the whole process.

### 10. What is thread safety?
Thread safety is a concept in multithreaded programming where shared data is accessed in a way that guarantees it won't be corrupted. If multiple threads try to modify the same variable at the exact same time, you get a "race condition." We use "Locks" to ensure only one thread touches the data at a time.

### 11. What is a Lock in Python threading?
A Lock (or Mutex) is a synchronization primitive. When one thread "acquires" a lock, any other thread that tries to acquire it will be blocked (paused) until the first thread "releases" it. This ensures that a "critical section" of code is only run by one thread at a time.
* **Example:**
    ```python
    import threading

    counter = 0
    lock = threading.Lock()

    def increment():
        global counter
        with lock: # Automatically acquires and releases
            counter += 1
    ```

### 12. What is a Queue in multithreading?
The `queue.Queue` class is a thread-safe way to exchange data between threads. It handles all the locking logic internally, so you don't have to worry about race conditions. It is perfect for the "Producer-Consumer" pattern where one thread generates data and another processes it.

### 13. What is a Pool in multiprocessing?
A `Pool` allows you to manage a fixed number of worker processes simultaneously. You can submit many tasks to the pool (e.g., a list of 1000 items), and the pool will distribute them among the available CPU cores automatically, returning the results once finished.
* **Example:**
    ```python
    from multiprocessing import Pool

    def square(n):
        return n * n

    if __name__ == "__main__":
        with Pool(4) as p:
            results = p.map(square, [1, 2, 3])
        print(results) # [1, 4, 9]
    ```

### 14. How to share data between threads?
Because threads live in the same memory space, you can share data using **Global Variables** or by passing a **Mutable Object** (like a list or dictionary) as an argument. However, you must always use a `Lock` if multiple threads are writing to that shared data.

### 15. How to share data between processes?
Since processes have separate memory, you cannot use global variables. You must use Inter-Process Communication (IPC). Python provides `multiprocessing.Queue` (for passing messages) or `multiprocessing.Value` and `multiprocessing.Array` (for shared memory buffers).

### 16. What is deadlock?
A deadlock is a situation where two or more threads are blocked forever, waiting for each other to release a lock. For example, Thread A holds Lock 1 and waits for Lock 2, while Thread B holds Lock 2 and waits for Lock 1. Neither can move forward.

### 17. What is a daemon thread?
A daemon thread is a thread that runs in the background. The main difference is that the Python program will exit even if daemon threads are still running. Non-daemon threads (the default) will keep the program alive until they finish. It is useful for background tasks like "heartbeat" signals or garbage collection.

### 18. What is the difference between synchronous and asynchronous execution?
**Synchronous** means tasks are performed one after another; the program waits for Task A to finish before starting Task B. **Asynchronous** (using `asyncio`) allows a single thread to switch tasks while waiting for I/O. It doesn't use multiple threads but "pauses" and "resumes" functions to maximize efficiency.

### 19. How to handle exceptions in threads?
If a thread raises an exception, it will print a traceback but will not stop the main program or other threads. To handle it, you should wrap the code inside the thread's target function in a `try...except` block, or use the `threading.excepthook` (available in Python 3.8+) to catch it globally.

### 20. When to use multithreading vs multiprocessing?
* Use **Multithreading** for: Network requests, API calls, File I/O, or user interface responsiveness.
* Use **Multiprocessing** for: Image/Video processing, heavy calculations (Prime numbers, Matrix math), or training Machine Learning models locally.

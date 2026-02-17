# Python, Django, REST API, MySQL Interview Questions (4+ Years Experience)

---

## Python Core

# 1. What does "everything is an object" mean in Python?

In Python, "everything is an object" means that all data types, including primitives like integers and booleans, are implemented as classes rather than raw memory values. This architecture ensures a uniform interface where every entity has its own identity, type, and value, allowing you to treat functions and classes as "first-class citizens." Because even basic types are objects, you can dynamically inspect their attributes or pass them as arguments just as you would with a complex data structure.

    # Demonstrating that even a basic integer is a class instance
    number = 42
    print(number.__class__)   # <class 'int'>
    
    # Demonstrating that a function is an object that can be passed around
    def my_function():
        pass
    
    print(isinstance(my_function, object)) # True

# 2. Explain mutable vs immutable objects with a real bug example

Mutable objects (like lists or dictionaries) can be changed after creation, while immutable objects (like strings or tuples) cannot. In Python, when you modify a mutable object, you change it in place at its existing memory address; however, "changing" an immutable object actually creates a brand-new object in memory. A common "real bug" occurs when using mutable default arguments in functions, where a list is shared across all function calls instead of resetting, leading to unexpected data accumulation.

    # THE BUG: Using a mutable list as a default argument
    def add_to_item(item, list_items=[]):
        list_items.append(item)
        return list_items
    
    print(add_to_item("Apple"))  # Output: ['Apple']
    print(add_to_item("Banana")) # Output: ['Apple', 'Banana'] (Expected ['Banana'])
    
    # THE FIX: Use None as a placeholder
    def add_to_item_fixed(item, list_items=None):
        if list_items is None:
            list_items = []
        list_items.append(item)
        return list_items
   
# 4. What is the internal difference between list and tuple?

The simplest way to think about the difference is that Lists are dynamic and Tuples are static. Internally, a list is a variable-length array that reserves extra memory so it can grow when you add items, which makes it slightly heavier and slower. A tuple is a fixed-size array that allocates the exact amount of memory needed the moment it is created, making it faster to access and "hashable" (usable as a dictionary key). Because tuples are constant, Python doesn't have to worry about providing methods like append() or pop(), which reduces their internal overhead.
    
    # Lists are for data that changes
    users = ["Alice", "Bob"]
    users.append("Charlie") # Works fine
    
    # Tuples are for data that stays constant (like coordinates)
    location = (40.7128, 74.0060)
    # location.append(10.123) # AttributeError: 'tuple' has no append

# 5. Where does shallow copy cause issues in real projects?

A shallow copy creates a new collection object, but it only copies the references to the nested items, not the items themselves. This becomes a major issue in real projects when dealing with nested data structures, like a list of dictionaries or a configuration object. If you modify a nested element in the copied version, the change "leaks" back into the original data because both objects are still pointing to the same memory address for their internal contents. This can lead to silent data corruption in multi-user applications or state management systems.

    import copy
    
    # Real Project Scenario: Updating a user's settings
    original_data = [{'user': 'Alice', 'admin': False}]
    shallow_copied_data = list(original_data)
    
    # THE BUG: Modifying the nested dictionary in the copy
    shallow_copied_data[0]['admin'] = True
    
    # The change affects the original data unexpectedly!
    print(original_data[0]['admin'])  # Output: True (Security Risk)
    
    # THE FIX: Use deepcopy to clone everything
    deep_copied_data = copy.deepcopy(original_data)

# 6. What is the cost of deep copy?

The cost of a deep copy is significantly higher than a shallow copy because it involves recursive memory allocation and object creation. For every nested object found within the original structure, Python must find a new memory address, copy the data, and maintain a lookup table to handle circular references (objects that point to themselves). In real-world projects with massive datasets or complex tree structures, this results in increased CPU cycles for the traversal and a much larger memory footprint since you are essentially duplicating the entire data tree in RAM.

    import copy
    import sys
    import time
    
    # Large nested structure: List containing 10,000 lists
    large_data = [[i] for i in range(10000)]
    
    # MEASURING COST:
    start_time = time.time()
    deep_cloned = copy.deepcopy(large_data)
    end_time = time.time()
    
    print(f"Time taken: {end_time - start_time:.4f} seconds")
    print(f"Memory used: {sys.getsizeof(deep_cloned) + sys.getsizeof(large_data)} bytes")

# 7. How does Python manage memory internally?

Python manages memory through a combination of Reference Counting and a Generative Garbage Collector. Every object in Python has a counter that tracks how many references point to it; as soon as this count hits zero, the memory is immediately deallocated. To handle complex cases like "circular references" (where two objects point to each other and would never hit zero), Python runs a background cycle-detecting collector. Additionally, Python uses a specialized memory manager called PyMalloc to handle small objects efficiently, reducing the overhead of frequent system-level memory requests.

    import sys
    
    # Reference Counting Example
    a = [1, 2, 3]
    b = a
    print(sys.getrefcount(a))  # Count increases because 'b' also points to 'a'
    
    # Circular Reference Example (Handled by Garbage Collector)
    list1 = []
    list2 = [list1]
    list1.append(list2)
    
    del list1
    del list2
    # The objects still exist in memory until the GC runs its cycle



# 8. What is reference counting?

Reference counting is Python’s primary memory management strategy where every object maintains a counter of how many variables or structures are currently pointing to it. When you assign an object to a new variable or pass it to a function, the count increases; when a variable goes out of scope or is explicitly deleted, the count decreases. As soon as this counter hits zero, Python immediately reclaims the memory used by that object. This provides a "real-time" approach to memory cleanup, ensuring that unused data doesn't sit in RAM longer than necessary.

    import sys
    
    # Initial object creation
    data = [1, 2, 3]
    print(sys.getrefcount(data))  # Count is 2 (the variable + the function argument)
    
    # Adding a second reference
    alias = data
    print(sys.getrefcount(data))  # Count increases to 3
    
    # Removing a reference
    del alias
    print(sys.getrefcount(data))  # Count drops back to 2

# 12. What is duck typing?

Duck typing is a programming concept where an object's suitability is determined by the presence of certain methods and properties, rather than its actual type or class. In Python, the interpreter doesn't care if an object "is" a specific type (like a File or a List); it only cares if the object can "do" what is required (like having a .read() or .append() method). The name comes from the phrase: "If it walks like a duck and quacks like a duck, then it must be a duck." This allows for high flexibility and polymorphism without the need for complex inheritance hierarchies or interface declarations.

    class Duck:
        def quack(self): print("Quack!")
    
    class Person:
        def quack(self): print("I'm pretending to be a duck!")
    
    def make_it_quack(obj):
        # We don't check 'type(obj)', we just call the method
        obj.quack()
    
    make_it_quack(Duck())   # Works
    make_it_quack(Person()) # Also works, because it has the 'quack' method

# 13. Is Python pass by value or pass by reference?
Python uses pass-by-object-reference. When you pass an argument to a function, you are passing a reference to the object in memory, not a copy of the object itself. If the object is mutable (like a list), changes made inside the function will affect the original object outside. However, if you reassign the variable inside the function, you are simply pointing that local name to a new object, which does not change the original variable’s reference.

    # Demonstrating that the object is shared, but the label is local
    def update_data(my_list):
        my_list.append(4)      # Modifies the shared object in memory
        my_list = [100, 200]   # Local name now points to a NEW object
    
    original = [1, 2, 3]
    update_data(original)
    
    print(original)  # Output: [1, 2, 3, 4] (Modified, but not replaced)

# 14. Explain GIL with a practical example.
Global Interpreter Lock (GIL) is that it acts as a "single-lane bridge" for Python threads. Even if you have a powerful 8-core processor, the GIL only allows one thread to execute Python code at any given moment to prevent memory management conflicts. This means that for CPU-intensive tasks like heavy math or image processing, adding more threads won't make the code faster—it might actually make it slower due to the overhead of switching between threads. However, for I/O tasks (like downloading files), the GIL "steps aside" while waiting for the network, allowing other threads to work in the meantime.

    import threading
    
    # THE LIMITATION: Both threads cannot run at the same time on different cores
    def cpu_heavy_task():
        x = 0
        for i in range(10**7):
            x += i
    
    thread1 = threading.Thread(target=cpu_heavy_task)
    thread2 = threading.Thread(target=cpu_heavy_task)
    
    # They will "take turns" rather than running truly in parallel
    thread1.start()
    thread2.start()
    thread1.join()
    thread2.join()
    
    # THE SOLUTION: Use 'multiprocessing' to bypass the GIL by creating 
    # entirely separate memory spaces (separate interpreters).

    
# 15. Why is multithreading slow for CPU-bound tasks in Python?
Multithreading is slow for CPU-bound tasks in Python because of the Global Interpreter Lock (GIL), which prevents multiple threads from executing Python bytecode simultaneously. Even on a multi-core processor, the GIL forces threads to "take turns" on a single CPU core, adding significant overhead due to constant context switching (the CPU jumping between threads). Instead of gaining speed from parallel execution, the program spends extra time managing the lock and swapping thread states, which often makes it slower than a simple sequential script. To achieve true parallelism for math or data processing, you must use the multiprocessing module to create separate memory spaces and interpreters.

    import threading
    import time
    
    #CPU-bound task: simple calculation
    def heavy_math():
        count = 0
        for i in range(10**7):
            count += i
    
    #Sequential Execution
    start = time.time()
    heavy_math()
    heavy_math()
    print(f"Sequential time: {time.time() - start:.2f}s")
    
    #Multithreaded Execution (Often slower or same speed due to GIL)
    t1 = threading.Thread(target=heavy_math)
    t2 = threading.Thread(target=heavy_math)
    
    start = time.time()
    t1.start(); t2.start()
    t1.join(); t2.join()
    print(f"Multithreaded time: {time.time() - start:.2f}s")

# 16. Generator vs list: memory comparison.
A List stores all its elements in memory (RAM) simultaneously, making it an "eager" data structure that allows for fast indexing and multiple traversals. In contrast, a Generator is "lazy," meaning it yields one item at a time only when requested and does not store the entire sequence in memory. This makes generators incredibly efficient for processing massive datasets or infinite streams, as their memory footprint remains constant regardless of the number of items being processed.

    import sys
    
    # List: Stores all 1 million integers in RAM
    my_list = [i for i in range(1000000)]
    print(f"List Memory: {sys.getsizeof(my_list)} bytes") 
    
    # Generator: Only stores the 'formula' to create the next number
    my_gen = (i for i in range(1000000))
    print(f"Generator Memory: {sys.getsizeof(my_gen)} bytes")
    
    # Real Project Use-Case: 
    # Use a List if you need to access items by index (e.g., my_list[500])
    # Use a Generator if you are reading a 10GB log file line by line.

# 17. How does yield work internally?
When you use the yield keyword, Python transforms a regular function into a Generator object. Internally, instead of executing the entire function and returning a value, the function's execution state—including its local variables, instruction pointer, and stack—is "frozen" and saved in memory. When the generator's __next__() method is called (e.g., in a for loop), Python resumes the function exactly where it left off until it hits the next yield. This allows for lazy evaluation, where data is produced on-demand rather than being stored all at once in RAM

    def count_up_to(n):
        count = 1
        while count <= n:
            yield count  # The function "pauses" here and returns 'count'
            count += 1   # Resumes here on the next call
    
    # Usage
    counter = count_up_to(3)
    
    print(next(counter))  # 1 (State is saved at line 4)
    print(next(counter))  # 2 (State resumes at line 5, then pauses at line 4)
    print(next(counter))  # 3
    # next(counter)       # Would raise StopIteration (Execution finished)


# 19. How does a decorator work internally?
A decorator is internally a Higher-Order Function that takes another function as an argument and returns a new "wrapper" function. Because functions in Python are objects, the decorator captures the original function in a Closure, allowing it to execute code before and after the original function runs without modifying its source code. When you use the @decorator syntax, Python is actually performing an assignment: func = decorator(func). This replaces the original function name with the wrapper, effectively intercepting every future call to that function.

    # The internal logic of a decorator
    def my_decorator(func):
        def wrapper():
            print("Something is happening before the function is called.")
            func()  # This is the original function being called
            print("Something is happening after the function is called.")
        return wrapper
    
    @my_decorator
    def say_hello():
        print("Hello!")
    
    # Internally, Python did: say_hello = my_decorator(say_hello)
    say_hello()

# 22. Difference between *args and **kwargs.
<b> 1. *args (Non-Keyword Arguments) </b>
Use *args when you want to pass a variable number of positional arguments to a function. Internally, Python packs these arguments into a tuple. This is useful when you don't know beforehand how many inputs the user will provide
    def sum_numbers(*args):
        # args is a tuple: (1, 2, 3)
        return sum(args)
    
    print(sum_numbers(1, 2, 3)) # Output: 6

<b>**kwargs (Keyword Arguments)</b>

Use **kwargs when you want to pass a variable number of named (keyword) arguments. Internally, Python packs these into a dictionary, where the argument name is the key and the value is the dictionary value. This is ideal for handling optional settings or configurations.

    def user_profile(**kwargs):
        # kwargs is a dict: {'name': 'Alice', 'age': 30}
        for key, value in kwargs.items():
            print(f"{key}: {value}")
    
    user_profile(name="Alice", age=30, job="Engineer")

# 23. Difference between is and ==.
<b>The == Operator (Equality)</b>
The == operator checks if the values of two objects are the same. It calls the __eq__() magic method under the hood. Use this 99% of the time when you want to know if two variables "hold the same data."

<b>The is Operator (Identity)</b>
The is operator checks if two variables point to the exact same object in memory. It compares their memory addresses (which you can see using id()). Use this only when comparing against singletons like None, True, or False.


---

## Advanced Python / Design

# 2. Why do we use context managers?
We use context managers primarily to handle resource management safely and cleanly. Their main job is to ensure that resources—like file handles, database connections, or network sockets—are properly opened and, more importantly, closed, even if your code encounters an error or crashes halfway through.

<b>1. The "Safety Net" (Exception Handling)</b>

Without a context manager, if an error occurs after opening a file but before closing it, the file remains open in memory. This can lead to memory leaks or file corruption. Context managers use a try...finally logic internally to guarantee the "cleanup" code runs no matter what.


# 5. What is a metaclass?
A Metaclass is the "class of a class." While a standard class defines how instances (objects) are created, a metaclass defines how classes themselves are constructed. In Python, classes are not just static code blocks; they are first-class objects created at runtime, and the metaclass is the "factory" that builds them.
        
        # The Metaclass
        class SecretSauce(type):
            def __new__(cls, name, bases, dct):
                # We can modify the class before it is created
                dct['is_awesome'] = True
                return super().__new__(cls, name, bases, dct)
        
        # Using the Metaclass
        class MyClass(metaclass=SecretSauce):
            pass
        
        # The class now has an attribute it never defined!
        print(MyClass.is_awesome)  # Output: True

# 11. Difference between Lock and RLock.
In multi-threaded programming, both Lock and RLock (Re-entrant Lock) are used to prevent race conditions by ensuring only one thread accesses a resource at a time. The primary difference lies in how they handle a thread that tries to acquire the same lock multiple times.

<b>The Standard Lock</b>
A standard Lock is "owned" by no one. If a thread acquires it and then tries to acquire it again (without releasing it first), the thread will deadlock. It will sit forever waiting for itself to release the lock it is currently holding.
        import threading
        
        lock = threading.Lock()
        
        lock.acquire()
        print("First acquire")
        
        #This will hang forever (Deadlock)!
        lock.acquire() 
        print("Second acquire")
        lock.release()
<b>The RLock (Re-entrant Lock)</b>
An RLock keeps track of the owner thread and a recursion level. If the thread that already owns the lock tries to acquire it again, the "recursion level" simply increments by one, and the code continues. To fully unlock the resource, the thread must call release() the exact same number of times it called acquire()

        import threading
        
        rlock = threading.RLock()
        
        rlock.acquire()
        print("First acquire")
        
        # This works fine with RLock!
        rlock.acquire() 
        print("Second acquire")
        
        rlock.release()
        rlock.release() # Must release twice to unlock for other threads

# 12. What is a deadlock and how does it happen?
a Deadlock is a "traffic jam" in a computer program. It occurs when two or more threads (or processes) are frozen forever because each is waiting for the other to release a resource.

The "Four Pillars" of Deadlock (Coffman Conditions)

For a deadlock to occur, four conditions must be met simultaneously. If you can break just one of these, you can prevent deadlocks entirely:

Mutual Exclusion: Only one thread can use a resource at a time (e.g., a specific database row or a file).

Hold and Wait: A thread is already holding at least one resource and is waiting to acquire additional resources held by others.

No Preemption: Resources cannot be forcibly taken away from a thread; they must be released voluntarily.

Circular Wait: A chain of threads exists where Thread A waits for Thread B, Thread B waits for Thread C, and Thread C waits for Thread A.


---



# 2. Difference between MVT and MVC.

While MVC (Model-View-Controller) is the classic architectural pattern for software, Django uses a slight variation called MVT (Model-View-Template). The core difference isn't necessarily a change in logic, but rather a shift in who handles the "control" part of the application.
1. The MVC Pattern (The Classic)

MVC is widely used in frameworks like Ruby on Rails or Spring.

Model: Manages data and business logic (Database).

View: The user interface (what the user sees).

Controller: The "Brain." It accepts input from the user, processes it using the Model, and decides which View to show.

2. The MVT Pattern (The Django Way)

In Django, the framework itself acts as the "Controller."

Model: Still manages the data and database schema (the models.py file).

View: This is the main point of confusion. In MVT, the View is the "Brain" (the views.py file). It executes business logic and interacts with the Model.

Template: This is the presentation layer (the .html files). It handles how data is displayed to the user.

# 3. Explain Django middleware execution order.
n Django, middleware acts like a chain of processing layers that sit between the web server and the view. When a request comes to a Django application, it does not directly go to the view. Instead, it first passes through each middleware class defined in the MIDDLEWARE setting of the project. The important concept to understand is that Django follows a top-to-bottom order for processing requests and a bottom-to-top order for processing responses.

When an HTTP request enters a Django application, it moves through the middleware classes in the exact order they are defined in the MIDDLEWARE list inside the settings file. Each middleware gets a chance to inspect or modify the request before passing it to the next middleware. After all middleware layers have processed the request, it finally reaches the view. The view performs the main business logic and returns an HTTP response.

Once the response is generated by the view, the flow reverses. The response travels back through the same middleware classes but in the opposite order. This means the middleware that executed last during the request phase will execute first during the response phase. Each middleware can again inspect or modify the response before sending it back to the client.

This execution pattern is often described as an “onion model.” The request goes inward through each layer until it reaches the core (the view), and the response comes outward through those same layers in reverse order. This design allows middleware to handle cross-cutting concerns such as authentication, session management, security checks, logging, and CSRF protection in a structured and predictable way.

# 4. Why did you write a custom middleware?
I created custom middleware when I needed functionality that should apply to every request instead of writing the same code in multiple views. For example, I used it to log request details, measure response time, and perform additional security validations before the request reached the view. Since middleware runs before and after the view execution, it is the right place to handle cross-cutting concerns like authentication checks, logging, IP filtering, or modifying responses.

Writing custom middleware helped me maintain separation of concerns, keep the views clean, and avoid code duplication. It also made the system more scalable and easier to maintain because any global change could be handled in one place instead of updating many files.

# 5. What changes are required in settings.py for production?
When moving a Django project from development to production, the settings.py file must be updated to make the application secure, stable, and optimized. The first important change is setting DEBUG = False. In development, DEBUG is usually True to show detailed error pages, but in production, it must be False to prevent exposing sensitive information such as stack traces or environment details.

Another critical setting is ALLOWED_HOSTS. In production, this should contain the actual domain name or server IP address where the application is hosted. This prevents HTTP Host header attacks and ensures the app only responds to trusted domains.

The SECRET_KEY should never be hardcoded in production. Instead, it should be stored securely using environment variables. This protects the application from security risks if the code is exposed.

Database configuration also changes in production. Instead of using SQLite (which is common in development), production typically uses a more robust database like PostgreSQL or MySQL. The database credentials should also be stored securely using environment variables.

Static files handling must be configured properly. In production, you need to define STATIC_ROOT and run collectstatic so that all static files are collected into a single directory. Usually, a web server like Nginx serves these static files. You may also configure MEDIA_ROOT and MEDIA_URL if users upload files.

Security-related settings should also be enabled in production. These include settings such as SECURE_SSL_REDIRECT = True to force HTTPS, SESSION_COOKIE_SECURE = True, and CSRF_COOKIE_SECURE = True to ensure cookies are only sent over secure connections.

Additionally, proper logging configuration should be added so that errors are logged to files or monitoring systems instead of only printing in the console.

# 6. What is WSGI?
it is a standard way for a web server to talk to a Python web application like Django.  When a user opens a website, the request first goes to a web server such as Nginx or Apache. The web server cannot directly run Python code. So it passes the request to a WSGI server like Gunicorn or uWSGI. The WSGI server then sends that request to the Django application. After Django processes the request and prepares a response, the response goes back through the WSGI server to the web server, and finally to the user’s browser.  So basically, WSGI works like a translator or middle layer between the web server and the Python application.  In a Django project, there is a file called wsgi.py. This file contains the application object that the WSGI server uses to communicate with Django.

# 7. When should ASGI be used?
ASGI stands for Asynchronous Server Gateway Interface. It is an improved version of WSGI that supports asynchronous communication. While WSGI can handle only traditional request-response cycles, ASGI can handle both synchronous and asynchronous operations.  You should use ASGI when your application requires real-time features such as chat applications, live notifications, online gaming updates, or streaming data. ASGI supports WebSockets, which allow two-way communication between the client and the server. This means the server can send data to the client without the client requesting it again.  ASGI is also useful when your application performs many I/O operations like calling external APIs, handling long-running tasks, or managing multiple concurrent users. Because ASGI supports asynchronous processing, it can handle many requests efficiently without blocking other operations.  In Django, ASGI is supported through Django Channels or by running Django with an ASGI server like Uvicorn or Daphne. Modern Django versions also include an asgi.py file by default.

# 8. Django signals: pros and cons.
Django signals are a way to allow certain pieces of code to get notified when something happens in the application. For example, when a model is saved, deleted, or a user logs in, Django can automatically trigger a signal. This allows you to execute additional logic without directly modifying the main code where the action happens.  One major advantage of signals is loose coupling. The sender (for example, a model) does not need to know who is listening. This makes the system flexible and modular. For instance, when a new user is created, you can automatically create a profile object using a post_save signal. The user model does not need to know anything about the profile creation logic. This keeps responsibilities separated and improves code organization.  Signals are also useful when you want to add functionality to third-party apps without modifying their source code. You can simply listen to their signals and extend behavior cleanly.  However, signals also have disadvantages. The biggest problem is that they can make the codebase harder to understand. Since signals run automatically in the background, it is not always obvious where certain logic is being executed. This can make debugging difficult because the flow of execution is not clearly visible in the main code. New developers may struggle to understand what is happening behind the scenes.  Another issue is overuse. If too much business logic is placed inside signals, the application becomes difficult to maintain and test. Signals should not replace proper service layers or explicit function calls.  For example, suppose you want to create a profile whenever a user is created. You can use a post_save signal on the User model to automatically create the profile after the user is saved. This is a good use case because it is simple and related to the lifecycle of the model. But if you start placing complex payment processing or heavy business workflows inside signals, it becomes messy and hard to track.


# 10. Best practices for Django app structure.
In Django, it is best practice to keep each app focused on a single responsibility, such as users, orders, or payments, instead of mixing everything in one app. Business logic should not be written inside views; instead, it should be separated into services or utility modules. Models, serializers, views, and URLs should be organized clearly to maintain readability. It is also recommended to use environment-based settings (development and production) and follow a consistent folder structure so the project remains scalable and easy to manage.

# 11. Difference between manage.py and django-admin.
django-admin is a global command-line tool that comes with Django and is used to perform administrative tasks like creating a new project. It does not automatically know about your specific project settings.

manage.py is created automatically when you start a Django project. It is a wrapper around django-admin but is configured specifically for that project. It sets the correct settings file and project environment automatically.

# 12. What security features does Django provide?
Django includes many security features by default. It protects against Cross-Site Scripting (XSS) by automatically escaping template variables. It prevents Cross-Site Request Forgery (CSRF) attacks using CSRF tokens in forms. Django also protects against SQL injection because it uses an ORM that safely handles database queries. It provides secure password hashing mechanisms instead of storing plain text passwords. Additionally, Django supports protection against clickjacking, session security, and allows easy configuration of HTTPS-related settings for secure communication.

# 16. How do Django migrations work internally?
Internally, when you modify a model, Django compares the current models with the last applied migration using the migration files stored in the app’s migrations folder. When you run makemigrations, Django creates a new migration file that contains operations like CreateModel, AddField, or AlterField. These migration files are Python scripts that describe what changes should be applied to the database.

When you run migrate, Django reads those migration files and executes the corresponding SQL commands on the database. It also records which migrations have been applied in a special table called django_migrations, so it does not run the same migration again. This system ensures database schema changes are version-controlled, consistent, and reversible across different environments.

# 17. How do you resolve migration conflicts?
Migration conflicts usually occur when multiple developers create migrations separately and then merge their code. Django detects this when there are multiple migration files with the same parent migration.

To resolve it, first run python manage.py makemigrations --merge. Django will attempt to create a merge migration that combines both changes. If automatic merging is not possible, you may need to manually review both migration files, understand the changes, and create a new merged migration that correctly includes both sets of operations.

After resolving the conflict, run python manage.py migrate to apply the updated migration chain. In interviews, you can say that migration conflicts are resolved by merging migration files properly and ensuring the migration history remains consistent without losing any schema changes.


# 20. How do you scale a Django application?
To scale a Django application, first I make sure it runs in production with a proper setup using a WSGI or ASGI server behind a web server like Nginx. Then I enable horizontal scaling by running multiple application instances behind a load balancer so traffic is distributed across servers.

I also optimize the database by using indexing, query optimization, and sometimes read replicas to reduce load. Caching with tools like Redis or Memcached is very important to reduce repeated database queries. For background tasks such as emails or heavy processing, I use task queues like Celery so they do not block user requests.

# 21. Why use multiple settings files?
In a real project, development and production environments require different configurations. For example, in development we keep DEBUG = True, use a local database, and allow all hosts. But in production, DEBUG must be False, security settings must be strict, and a production database like PostgreSQL is used.

If everything is kept in a single settings file, it becomes risky and harder to manage. By separating settings into files like base.py, development.py, and production.py, we can keep common settings in one place and environment-specific configurations separately.

# 22. Why are environment variables important?
Environment variables are important because they allow you to configure applications without hardcoding sensitive or environment-specific data, like API keys, database URLs, or debug settings. This makes your code more secure, portable, and easier to manage across development, staging, and production environments. For example, in Python you can access a database URL with os.environ['DATABASE_URL'] instead of writing it directly in your code. It also helps in following the Twelve-Factor App methodology for clean deployment practices.

# 23. What usually breaks during Django upgrades?
During Django upgrades, the things that usually break are:

Deprecated features – Methods, settings, or middleware that were marked deprecated in earlier versions may stop working. For example, MIDDLEWARE_CLASSES was replaced by MIDDLEWARE in Django 1.10+.

Third-party packages – Some packages may not yet support the new Django version, leading to import errors or incompatible APIs.

Template and ORM changes – Changes in template tags, filters, or ORM behavior can break queries or renderings.

Settings and default behaviors – Defaults for things like password hashing, timezone handling, or CSRF may change.

# 24. Django monolith vs microservices.
A Django monolith is a single, unified application where all features, models, and views are part of one project. It’s simpler to develop, deploy, and manage initially, and everything is tightly integrated. For example, a small e-commerce site can work efficiently as a monolith.

<b>Microservices </b>, on the other hand, break the application into smaller, independent services (like user service, payment service) that communicate over APIs. This makes scaling specific parts easier and allows teams to work independently, but adds complexity with inter-service communication, deployment, and data consistency.

# 25. What are Django limitations?
Monolithic structure – By default, Django encourages a monolithic design, which can be harder to scale horizontally compared to microservices.

Performance overhead – For very high-performance needs, Django’s abstraction layers and ORM can introduce latency compared to lightweight frameworks like FastAPI or Flask.

Real-time applications – Django wasn’t originally built for real-time features like WebSockets, though Django Channels can help.

Steep learning curve for advanced features – Features like signals, middleware, and class-based views can be confusing for beginners.

---

## Django ORM / Database

# 1. What is ORM and why avoid raw SQL?

ORM (Object-Relational Mapping) is a technique that lets you interact with a database using programming language objects instead of writing SQL queries directly. In Django, the ORM allows you to create, read, update, and delete records using Python classes and methods, like Book.objects.filter(author="John").
Using ORM instead of raw SQL is preferred because it:
Prevents SQL injection by safely handling user inputs.
Improves readability and maintainability, since queries are written in Python.
Provides database portability, making it easier to switch between PostgreSQL, MySQL, or SQLite.
Raw SQL should be avoided unless you need complex queries that the ORM can’t efficiently handle.

# 2. select_related vs prefetch_related.
select_related vs prefetch_related in Django:

select_related performs a SQL JOIN and fetches related objects in the same query. It’s efficient for one-to-one or foreign key relationships. For example: Book.objects.select_related('author') fetches books and their authors in one query.
prefetch_related performs a separate query for related objects and joins them in Python. It’s ideal for many-to-many or reverse foreign key relationships. For example: Author.objects.prefetch_related('books') fetches authors and their books in two queries, avoiding repeated database hits.
Both help prevent the N+1 query problem and improve performance in Django applications.

# 3. Explain the N+1 query problem.
The N+1 query problem happens when your code runs one query to fetch main objects (the “1”) and then runs an additional query for each related object (the “N”), causing many unnecessary database hits.
For example, if you fetch 10 books and then access each book’s author without optimization

        books = Book.objects.all()
        for book in books:
            print(book.author.name)  # triggers a query for each book
            
Using select_related or prefetch_related solves this by fetching related objects in bulk, reducing queries and improving performance.


# 4. Difference between annotate and aggregate.
<b>aggregate :</b> computes a summary across the entire queryset and returns a single value.
        from django.db.models import Avg
        Book.objects.aggregate(Avg('price'))  # returns {'price__avg': 250}

<b>annotate :</b>computes a value for each object in the queryset and adds it as an extra field.
        from django.db.models import Count
        Author.objects.annotate(num_books=Count('books'))


# 5. values vs values_list.
<b> Values </b> returns a list of dictionaries, where each dict maps field names to values.
        Book.objects.values('title', 'author')
        # [{'title': 'Book1', 'author': 1}, {'title': 'Book2', 'author': 2}]
        
<b>values_list :</b> returns a list of tuples (or flat values if flat=True) for the fields you specify.

        Book.objects.values_list('title', 'author')
        # [('Book1', 1), ('Book2', 2)]
        
        Book.objects.values_list('title', flat=True)
        # ['Book1', 'Book2']



# 7. What is lazy evaluation in QuerySets?
Lazy evaluation in Django means that querysets are not executed immediately when they are created. Django waits until the data is actually needed—like when you iterate, slice, or print the queryset—to hit the database.

        books = Book.objects.filter(author='John')  # No query yet
        for book in books:                           # Query runs here
            print(book.title)
            
This improves performance because multiple filters or operations can be chained without hitting the database repeatedly, and only the final, necessary query is executed. It’s also why you need to be careful with repeated iteration over large querysets.


# 8. How does Django handle transactions?

Django handles transactions using its database transaction management system, ensuring that a group of database operations either all succeed or all fail. By default, each HTTP request is wrapped in an atomic transaction, meaning if an error occurs, changes are rolled back automatically.
You can also manually control transactions using transaction.atomic() to wrap a block of code:

        from django.db import transaction
        with transaction.atomic():
            book.save()
            author.save()


# 9. How does transaction.atomic work internally?

transaction.atomic() in Django works by creating a transaction context that ensures all database operations inside it are executed as a single unit. Internally, it uses the database’s BEGIN / COMMIT / ROLLBACK mechanism:

When entering the atomic block, Django issues a BEGIN to start a transaction.

All database operations inside the block are tracked but not permanently committed until the block exits successfully.

If an exception occurs, Django issues a ROLLBACK, undoing all operations in that block.

If everything succeeds, Django issues a COMMIT, saving all changes.

It also supports nested atomic blocks using savepoints, so only the innermost failing block is rolled back without affecting outer transactions.

        from django.db import transaction
        
        with transaction.atomic():
            book.save()       # part of transaction
            author.save()     # part of transaction
        # Commit happens here if no exceptions

# 10. Why are indexes important?
Indexes are important in databases because they speed up data retrieval by allowing the database to locate rows without scanning the entire table. Without indexes, queries like WHERE filters or ORDER BY can become very slow as the table grows.

        class User(models.Model):
            email = models.EmailField(db_index=True)

Now, User.objects.get(email="abc@example.com") is much faster.
Indexes improve read performance, but they slightly slow down writes and updates, so they should be added strategically on frequently queried fields.

# 11. When should composite indexes be used?
Composite indexes should be used when your queries filter or order by multiple columns together frequently. They help the database quickly locate rows based on the combination of these fields instead of scanning individually.

        Book.objects.filter(author='John', published_year=2020)

Creating a composite index on (author, published_year) speeds up this query.

Key points:
Order matters: the index helps queries that filter starting from the first field.
They are most useful for complex filters or sorting, not every combination of fields.
Overusing them can increase write overhead, so use them only where query performance benefits are clear.

# 12. Explain on_delete options in ForeignKey.
In Django, the on_delete option in a ForeignKey defines what happens to related objects when the referenced object is deleted. Common options are:
<b>CASCADE</b> – Deletes dependent objects automatically.

        author = models.ForeignKey(Author, on_delete=models.CASCADE)
        
If an author is deleted, all their books are also deleted.
<b>PROTECT :</b> Prevents deletion by raising an error.

        author = models.ForeignKey(Author, on_delete=models.PROTECT)
you cannot delete an author if books reference them.

SET_NULL – Sets the foreign key to NULL (requires null=True).

SET_DEFAULT – Sets the foreign key to a default value.

DO_NOTHING – Does nothing, which may cause integrity errors if the database enforces constraints.



# 13. What are the risks of CASCADE?
The main risks of using CASCADE in Django are:

Accidental data loss – Deleting a single parent object can unintentionally delete a large number of related records. For example, deleting an Author with CASCADE will delete all their Book records.

Hard-to-trace deletions – In complex models with multiple cascading relationships, a single delete can trigger a chain reaction, removing many objects silently.

Performance impact – Deleting many related objects at once can be slow and may lock tables in large databases.

Best practice: Use CASCADE only when you are certain that deleting a parent should remove all dependent objects, and consider alternatives like PROTECT or SET_NULL for safer data management

# 14. OneToOneField vs ForeignKey.
ForeignKey represents a many-to-one relationship. Many objects can point to the same related object.
Example: Many Book objects can have the same Author.

        author = models.ForeignKey(Author, on_delete=models.CASCADE)
        
OneToOneField represents a one-to-one relationship. Each object is linked to exactly one related object.
Example: A User can have only one Profile.
        user = models.OneToOneField(User, on_delete=models.CASCADE)

# 15. How is ManyToMany stored internally?
In Django, a ManyToManyField is stored internally using an intermediate (junction) table that holds the relationships between the two models. This table contains at least two columns: the primary keys of the related models.

        class Book(models.Model):
            title = models.CharField(max_length=100)
        
        class Author(models.Model):
            name = models.CharField(max_length=50)
            books = models.ManyToManyField(Book)

Django automatically creates a table like author_books with columns: author_id and book_id. Each row represents a link between an author and a book.
This allows:
Multiple authors per book
Multiple books per author

# 18. Why can ORM be slow?
Django’s ORM can be slow because it adds abstraction layers on top of SQL, which sometimes generate inefficient queries. Common reasons include:
Multiple queries (N+1 problem) – Accessing related objects in loops without select_related or prefetch_related.
Inefficient joins or filters – Complex relationships may produce large joins that could be written more efficiently in raw SQL.
Large data retrieval – Fetching full model instances when only a few fields are needed; using values() or values_list() is faster.
Database hits in loops – Repeated queries for each object instead of batch operations.
Tip: The ORM is great for readability and portability, but for high-performance, complex queries, sometimes raw SQL or database-specific optimizations are necessary.



---

## Django REST Framework (DRF)

# 1. What is REST and why use DRF?
REST is an architectural style used to build scalable web APIs. It works on standard HTTP methods like GET, POST, PUT, DELETE and treats data as resources, for example /api/users/. REST is stateless, meaning each request contains all required information, usually exchanged in JSON format.

Django REST Framework (DRF) is a powerful library built on top of Django to create RESTful APIs quickly. It provides built-in features like serializers for data conversion, authentication, permissions, pagination, and viewsets, which reduce boilerplate code.

# 2. Serializer vs ModelSerializer.
3. In Django REST Framework, both Serializer and ModelSerializer are used to convert complex data like model instances into JSON and vice versa, but they differ in automation.

Serializer is fully manual. You must define all fields and implement create() and update() methods yourself. It gives more control and is useful for non-model data or custom validation logic.

ModelSerializer automatically generates fields based on a Django model and provides default create() and update() methods. It reduces boilerplate code and is commonly used for standard CRUD APIs.

# 6. APIView vs ViewSet.
 In Django REST Framework, both APIView and ViewSet are used to build APIs, but they differ in structure and automation.

APIView gives full control over HTTP methods like get(), post(), put(), and delete(). You manually define each method, so it’s more flexible but requires more code. It’s useful when you need custom logic for each request.

ViewSet groups related actions (like list, create, retrieve, update, delete) into a single class. When combined with a router, URLs are generated automatically, reducing boilerplate code.

# 8. When to use generic views?

In Django REST Framework, generic views are used when you want to build common CRUD APIs quickly with minimal code. They provide built-in functionality for operations like list, create, retrieve, update, and delete.
For example, ListCreateAPIView handles both listing and creating objects without writing separate get() and post() methods. You only need to define queryset and serializer_class.
Use generic views when your API follows standard CRUD behavior and doesn’t require highly customized logic.

# 9. JWT vs session authentication.
In web applications, Session Authentication and JWT (JSON Web Token) Authentication are two common authentication methods.

Session Authentication stores user data on the server. After login, the server creates a session and sends a session ID in a cookie. Each request includes that cookie, and the server verifies it. It’s simple and works well for traditional web apps.

JWT Authentication is stateless. After login, the server generates a signed token containing user information. The client sends this token in the header (Authorization: Bearer <token>) with each request. No session is stored on the server.

# 10. Why are refresh tokens needed?
Refresh tokens are needed to maintain security while keeping users logged in for a longer time.

In JWT authentication, access tokens are short-lived (for example, 15–30 minutes) to reduce the risk if they are stolen. When the access token expires, the client uses a refresh token to request a new access token without forcing the user to log in again.

This improves security (short expiry for access tokens) and user experience (no frequent logins).

# 11. Authentication vs permission.
Authentication is the process of verifying who the user is. It checks the user’s identity using credentials like username/password, token, or session. For example, logging in with JWT or session authentication confirms the user’s identity.

Permission determines what the authenticated user is allowed to do. It controls access to specific resources or actions, such as read-only access or admin-only operations. In DRF, this is handled using permission classes like IsAuthenticated or IsAdminUser.


# 12. How do you write custom permissions?
In Django REST Framework, custom permissions are written by creating a class that inherits from BasePermission and overriding specific methods.

There are two main methods:

        has_permission(self, request, view) → Checks general access to the view.
        
        has_object_permission(self, request, view, obj) → Checks permission for a specific object.
        
        from rest_framework.permissions import BasePermission
        
        class IsOwner(BasePermission):
            def has_object_permission(self, request, view, obj):
                return obj.user == request.user
        permission_classes = [IsOwner]

# 13. Why is throttling required?
Throttling is required to control the number of API requests a user or client can make within a specific time period. It helps protect the application from abuse, brute-force attacks, and excessive traffic.
For example, you can limit login attempts to 5 requests per minute to prevent password guessing attacks. In DRF, throttling classes like UserRateThrottle or AnonRateThrottle can be configured easily.
Throttling also ensures fair usage, so one user does not consume all server resources.

# 14. Types of pagination in DRF.
n Django REST Framework, pagination is used to divide large datasets into smaller chunks to improve performance and response size. DRF provides three main types of pagination:
PageNumberPagination – Uses page numbers like ?page=2. It’s simple and commonly used in web applications.
LimitOffsetPagination – Uses limit and offset like ?limit=10&offset=20. It’s flexible and similar to SQL LIMIT/OFFSET.
CursorPagination – Uses an encoded cursor instead of page numbers. It is efficient for large datasets and prevents duplicate or missing records when data changes.

# 15. API versioning strategies.
API versioning is used to manage changes in APIs without breaking existing clients. It allows backward compatibility when updating features or response formats.

Common versioning strategies in DRF are:
URL Versioning – Version is included in the URL, like /api/v1/users/. It is simple and most commonly used.
Query Parameter Versioning – Version is passed as a query parameter, like ?version=1.0.
Header Versioning – Version is sent in request headers, keeping URLs clean.
Namespace Versioning – Different URL namespaces for different versions.

# 16. PUT vs PATCH.
PUT and PATCH are both used to update resources in REST APIs, but they behave differently.
PUT is used for full update. It replaces the entire resource with the new data. If a field is missing, it may be set to null or default.
PATCH is used for partial update. It updates only the fields provided in the request and keeps the rest unchanged.

# 17. What is an idempotent API?
An idempotent API is one where making the same request multiple times produces the same result as making it once. In other words, repeating the request does not create additional side effects.
For example, a PUT request to update a user’s email to "a@gmail.com" will always result in the same final state, even if sent multiple times. Similarly, a DELETE request remains idempotent because deleting an already deleted resource does not change the outcome.
However, POST is usually not idempotent because sending it multiple times may create multiple records.

# 18. Correct usage of HTTP status codes.
Correct usage of HTTP status codes is important because it clearly communicates the result of an API request to the client. It helps in proper error handling and debugging.

Common categories:

2xx (Success) – 200 OK (successful GET), 201 Created (new resource created), 204 No Content (successful but no response body).

4xx (Client Errors) – 400 Bad Request (invalid input), 401 Unauthorized (not authenticated), 403 Forbidden (no permission), 404 Not Found.

5xx (Server Errors) – 500 Internal Server Error (unexpected failure).


# 19. Error handling best practices in APIs.
Error handling in APIs should be consistent, secure, and informative without exposing sensitive details. The goal is to help the client understand what went wrong and how to fix it.
Best practices:
Use proper HTTP status codes – 400 for validation errors, 401 for authentication failure, 403 for permission denied, 500 for server errors.
Return structured error responses – For example:

        {
          "error": "ValidationError",
          "message": "Email field is required"
        }
Do not expose internal details – Avoid sending stack traces or database errors in production.

Handle exceptions globally – In DRF, use custom exception handlers for consistent responses.

# 21. Rate limiting design.
Rate limiting design is about controlling how many requests a client can make within a specific time window to protect system resources and prevent abuse.

Common approaches:

Fixed Window – Allow a set number of requests per minute/hour (simple but can cause burst traffic at window reset).

Sliding Window – More accurate; limits requests based on a moving time window.

Token Bucket / Leaky Bucket – Allows controlled bursts while maintaining a steady request rate.

Rate limiting can be applied per IP, user, or API key, often using tools like Redis for fast counters.


# 23. Performance issues with serializers.
Performance issues with serializers in DRF usually occur when handling large datasets or complex nested relationships.

Common problems:

N+1 queries – Nested serializers may trigger multiple database hits if select_related or prefetch_related is not used.

Large querysets – Serializing thousands of objects at once increases memory usage and response time.

Deep nesting – Multiple nested serializers add processing overhead.

Unnecessary fields – Serializing all model fields when only a few are needed.

Optimization tips: Use pagination, optimize queryset with select_related/prefetch_related, limit fields, and avoid heavy nested serializers when possible.


# 25. Common API security risks.
Common API security risks include:

Broken authentication – Weak token handling or improper session management can allow unauthorized access.

Broken authorization – Missing permission checks may let users access or modify other users’ data.

Injection attacks – SQL injection or command injection if inputs are not properly validated.

Excessive data exposure – Returning sensitive fields like passwords or internal IDs in API responses.

Lack of rate limiting – Can lead to brute-force or DDoS attacks.

Insecure transport – Not using HTTPS exposes tokens and credentials.

# 26. What is CORS?
CORS (Cross-Origin Resource Sharing) is a browser security mechanism that controls how resources are requested from a different domain than the one serving the web page.

By default, browsers block cross-origin requests for security reasons. CORS allows the server to specify which domains are permitted using headers like Access-Control-Allow-Origin.

For example, if your frontend runs on http://localhost:3000 and backend on http://localhost:8000, the backend must allow that origin through CORS configuration.


# 27. How do you test APIs?
APIs can be tested at multiple levels to ensure correctness, security, and performance.

Unit Testing – Test individual views or functions using Django’s TestCase or DRF’s APIClient.

        response = self.client.get('/api/books/')
        self.assertEqual(response.status_code, 200)
        
Integration Testing – Test complete request–response flow including database interaction.

Manual Testing – Use tools like Postman to validate endpoints and edge cases.

Authentication & Permission Testing – Verify access control for different user roles.

Performance Testing – Check response time and load handling.

# 28. Swagger / OpenAPI usage.
Swagger / OpenAPI is used to document and describe REST APIs in a standard, machine-readable format. It provides clear information about endpoints, request/response formats, authentication, and status codes.

In Django REST Framework, tools like drf-yasg or drf-spectacular automatically generate interactive API documentation. This allows developers to test APIs directly from the browser.

Benefits:

Improves API documentation and clarity

Helps frontend and backend teams collaborate

Enables automatic client SDK generation

# 29. How do you optimize API response time?

o optimize API response time, focus on database, serialization, and infrastructure improvements.

Optimize database queries – Use select_related, prefetch_related, proper indexing, and avoid the N+1 query problem.

Limit data size – Use pagination and return only required fields instead of full objects.

Caching – Cache frequent responses using Redis or Django cache framework.

Efficient serialization – Avoid deep nested serializers and large querysets.

Asynchronous tasks – Move heavy operations (emails, reports) to background jobs using Celery.

# 30. Alternatives to DRF signals.
In Django/DRF, signals are used to trigger actions automatically (like post_save), but they can make code harder to trace and debug. So, there are better structured alternatives.

Common alternatives:

Override save() method – Add logic directly inside the model for predictable behavior.

Service layer / business logic functions – Keep logic in a separate service file and call it explicitly from views. This improves clarity and testability.

Custom model managers – Encapsulate complex database logic inside a manager method.

Use Django signals carefully only for decoupled features like logging or notifications.


# 31. How do you implement API caching?
API caching is implemented to reduce database load and improve response time by storing frequently requested data temporarily.

Common ways to implement caching in Django/DRF:

Per-view caching – Use Django’s @cache_page decorator to cache entire API responses.
        
        from django.views.decorators.cache import cache_page
        
        @cache_page(60 * 5)  # cache for 5 minutes
        def get(self, request):
            ...
Low-level caching – Use Django cache framework with Redis or Memcached to store specific query results.

Database query caching – Cache expensive query results manually.

Use HTTP cache headers – Like ETag and Last-Modified for client-side caching.

# 32. Limitations of DRF.
Django REST Framework (DRF) is powerful, but it has some limitations:

Performance overhead – DRF adds abstraction layers (serializers, viewsets), which can be slower compared to lightweight frameworks like FastAPI for high-performance APIs.

Complex nested serializers – Deep relationships can become hard to manage and impact performance.

Not fully async by default – Although Django supports async, DRF is still largely synchronous in many use cases.

Over-engineering for simple APIs – For very small services, DRF can feel heavy.

Learning curve – Concepts like serializers, permissions, throttling, and viewsets require time to master.

# What isa Synchronous

Sync (Synchronous) means tasks are executed one after another, and each step waits for the previous one to complete before moving forward.

In a synchronous web request, the server processes the request and blocks until the response is ready. If a database query or external API call takes time, the server waits during that time.

        def get_books(request):
            books = Book.objects.all()  # waits until DB returns data
            return JsonResponse({"count": books.count()})
# what is Asynchronous

Asynchronous (Async) means tasks can run without blocking the execution flow, allowing other operations to continue while waiting for a response (like a database query or API call).

In async programming, when a task is waiting (for I/O operations such as network or database), the system can handle other requests instead of staying idle.

        async def get_data(request):
            data = await fetch_external_api()
            return JsonResponse(data)

# What is the architecture of DRF?
The architecture of Django REST Framework (DRF) is built on top of Django and follows a layered, modular design to handle API requests efficiently.

Request Layer – DRF wraps Django’s request into a Request object, adding features like authentication, parsing, and content negotiation.

View Layer – Uses APIView, Generic Views, or ViewSets to handle business logic.

Serializer Layer – Converts complex data (models/querysets) into JSON and validates incoming data.

Authentication & Permission Layer – Ensures security before processing the request.

Renderer Layer – Converts Python data into response formats like JSON.

# How does the DRF request lifecycle work?

The DRF request lifecycle describes how a request is processed from client to response.

Request enters Django – The URL router maps the request to the correct DRF view.

Request wrapping – DRF converts Django’s request into a Request object and performs authentication, throttling, and permission checks.

Parsing – The request body is parsed (e.g., JSONParser) into Python data.

View execution – The appropriate method (get, post, etc.) runs business logic.

Serialization – Data is validated (for input) or converted to JSON (for output).

Rendering response – The response is passed to a renderer (usually JSON) and sent back to the client.

# What are different authentication classes in DRF?
In Django REST Framework, authentication classes define how the system verifies the identity of a user. Common authentication classes are:
SessionAuthentication – Uses Django’s session framework and cookies. Mostly used for web applications.
BasicAuthentication – Sends username and password in the request header (Base64 encoded). Simple but not recommended for production without HTTPS.
TokenAuthentication – Uses a token stored in the database. The client sends it in the Authorization: Token <key> header.
JWT Authentication (via third-party packages like SimpleJWT) – Uses signed JSON Web Tokens, which are stateless and scalable.

# What is the difference between BasicAuth, TokenAuth, and JWT?

The main difference between BasicAuth, TokenAuth, and JWT is how they send and manage credentials.

Basic Authentication – Sends username and password with every request (Base64 encoded in header). It is simple but less secure unless used over HTTPS, and not ideal for APIs.

Token Authentication – After login, the server generates a token stored in the database. The client sends this token in the header (Authorization: Token <key>). It is more secure than BasicAuth and avoids sending credentials repeatedly.

JWT (JSON Web Token) – After login, the server issues a signed token containing user information. It is stateless, meaning no session or token storage is required on the server. It scales better in distributed systems.

# What are mixins in DRF and why are they used?
In DRF, mixins are small reusable classes that provide specific functionality like create, list, retrieve, update, and destroy. They are not complete views by themselves but are combined with GenericAPIView to build full CRUD behavior.

For example, ListModelMixin provides a list() method, and CreateModelMixin provides a create() method.
        
        from rest_framework import mixins, generics
        
        class BookView(mixins.ListModelMixin,
                       mixins.CreateModelMixin,
                       generics.GenericAPIView):
            queryset = Book.objects.all()
            serializer_class = BookSerializer

# What is the difference between GenericAPIView and APIView?
The main difference between APIView and GenericAPIView in DRF is the level of built-in functionality.

APIView is the base class. You manually define methods like get(), post(), and handle queryset and serializer logic yourself. It gives full control but requires more code.

GenericAPIView extends APIView and adds support for queryset, serializer_class, pagination, filtering, and lookup fields. It is usually combined with mixins to quickly build CRUD APIs.

# How do routers work in DRF?

In DRF, routers automatically generate URL patterns for ViewSets, reducing manual URL configuration.

When you register a ViewSet with a router, it maps standard actions like list, create, retrieve, update, and destroy to corresponding HTTP methods and URLs.
        
        from rest_framework.routers import DefaultRouter
        
        router = DefaultRouter()
        router.register(r'books', BookViewSet)
        urlpatterns = router.urls

This automatically creates routes like:

GET /books/ → list
POST /books/ → create
GET /books/{id}/ → retrieve

# What is DefaultRouter vs SimpleRouter?

Both SimpleRouter and DefaultRouter in DRF automatically generate URL routes for ViewSets, but they differ slightly in features.
SimpleRouter generates standard RESTful routes like list, create, retrieve, update, and delete. It does not include any extra endpoints.
DefaultRouter extends SimpleRouter and additionally provides a root API view, which lists all registered endpoints at the base URL. This makes API discovery easier.

# How do you customize API responses in DRF?

You can customize API responses in DRF at multiple levels depending on the requirement.

Inside the view – Modify the response structure manually using the Response object.

        return Response({
            "status": "success",
            "data": serializer.data
        })
Override serializer to_representation() – Customize how model data is converted to JSON.
Custom exception handler – Define a global exception handler to standardize error responses.

Custom renderer – Create a custom renderer class to control the final output format.


# How do you override serializer create() and update() methods?

In DRF, you override create() and update() methods in a serializer when you need custom save logic beyond the default behavior. This is commonly done in ModelSerializer.
Override create() – Used when creating a new object:

        class BookSerializer(serializers.ModelSerializer):
            class Meta:
                model = Book
                fields = '__all__'
        
            def create(self, validated_data):
                return Book.objects.create(**validated_data)

Override update() – Used when updating an existing object:

            def update(self, instance, validated_data):
                instance.title = validated_data.get('title', instance.title)
                instance.save()
                return instance
# How do you handle file uploads in DRF?

In DRF, file uploads are handled using FileField or ImageField in serializers and models. The request must use multipart/form-data content type.

        class Document(models.Model):
            file = models.FileField(upload_to='uploads/')

Serializer

        class DocumentSerializer(serializers.ModelSerializer):
            class Meta:
                model = Document
                fields = '__all__'

In the view, ensure proper parser classes:

        parser_classes = [MultiPartParser, FormParser]

# How do you implement filtering in DRF?
Filtering in DRF is implemented using filter backends, which allow clients to filter querysets using query parameters.
Basic filtering – Override get_queryset() manually:

        def get_queryset(self):
            queryset = Book.objects.all()
            author = self.request.query_params.get('author')
            if author:
                queryset = queryset.filter(author=author)
            return queryset

Using django-filter (recommended) –
Install django-filter and configure:

        filter_backends = [DjangoFilterBackend]
        filterset_fields = ['author', 'published_year']

Then use: /api/books/?author=John
Search and ordering filters –

        filter_backends = [SearchFilter, OrderingFilter]
        search_fields = ['title']
        ordering_fields = ['published_year']

# How do you implement role-based access control (RBAC) in DRF?

Role-Based Access Control (RBAC) in DRF is implemented by assigning roles to users and restricting access based on those roles using permissions.
Using Django Groups – Assign users to groups like Admin, Editor, Viewer.
Custom permission class – Check the user’s role inside has_permission().

        from rest_framework.permissions import BasePermission
        
        class IsAdminRole(BasePermission):
            def has_permission(self, request, view):
                return request.user.groups.filter(name='Admin').exists()

Apply in view:

        permission_classes = [IsAdminRole]

# How do you secure sensitive fields in serializers?

Sensitive fields in serializers must be protected to prevent data leaks or unauthorized modification.
Use write_only=True – For fields like passwords so they are accepted in input but not returned in response.

        password = serializers.CharField(write_only=True)

Use read_only=True – For fields that should not be modified by users (e.g., id, created_at).
Exclude fields – Avoid exposing sensitive model fields like is_superuser, last_login, etc.
Override to_representation() – Customize output to remove or mask sensitive data.
Hash passwords properly – Use set_password() instead of saving raw passwords.

# When should you not use DRF?

You should avoid using Django REST Framework (DRF) when building a full REST API is unnecessary or adds extra complexity.
1️⃣ When You Are Building a Simple Server-Rendered Website
If your project only returns HTML pages using Django templates (no separate frontend like React or Angular), then normal Django views are enough. DRF is mainly useful when building APIs, not traditional web pages.
2️⃣ When Performance Is Extremely Critical
DRF adds extra layers like serializers, authentication classes, and renderers. For very high-performance or lightweight microservices, plain Django views or frameworks like FastAPI may be faster.
3️⃣ When You Don’t Need an API
If your application does not expose data to mobile apps, frontend frameworks, or third-party clients, then using DRF may be overengineering.
4️⃣ When the Project Is Very Small
For a small internal tool with limited endpoints, plain Django JSON responses (JsonResponse) are often simpler and quicker to implement.



---

## MySQL / Database

1. Explain ACID properties.
2. How does an index work internally?
3. B-tree vs hash index.
4. Primary key vs unique key.
5. Pros and cons of foreign key constraints.
6. Normalization vs denormalization.
7. What is a deadlock?
8. Transaction isolation levels.
9. Repeatable read vs read committed.
10. How do you debug slow queries?
11. Explain EXPLAIN query output.
12. When does a full table scan occur?
13. Why is pagination costly in SQL?
14. Problems with OFFSET-based pagination.
15. Cursor-based pagination.
16. Data consistency vs performance trade-off.
17. Database connection leaks.
18. Backup strategies.
19. Risks in migration rollback.
20. MySQL vs PostgreSQL.

---

## Multithreading & Multiprocessing

1. Difference between thread and process.
2. Limitations of Python threading.
3. Explain GIL clearly.
4. I/O-bound vs CPU-bound examples.
5. When does threading help?
6. When is multiprocessing better?
7. When should Celery be used?
8. Worker vs thread.
9. Role of Queue in threading.
10. Lock usage with example.
11. Deadlock scenario.
12. What is a daemon thread?
13. What is thread-safe code?
14. Shared memory problems.
15. How do processes communicate?
16. What is a Pool in multiprocessing?
17. How do you handle exceptions in threads?
18. Async vs threading.
19. Django + Celery architecture.
20. Handling background job failures.

---

## Performance & Scaling

1. First steps when a Django app becomes slow.
2. How do you identify DB vs code bottlenecks?
3. Caching strategies.
4. When to use Redis?
5. Per-view vs low-level caching.
6. How do you calculate Gunicorn workers?
7. Horizontal vs vertical scaling.
8. Basics of load balancing.
9. Why should APIs be stateless?
10. CDN usage.
11. Logging overhead concerns.
12. Monitoring tools.
13. How do you detect memory leaks in production?
14. Handling API timeouts.
15. What is graceful shutdown?

---

## Testing & Quality (Additional)

1. Difference between unit test and integration test.
2. How do you test Django views?
3. How do you test DRF APIs?
4. What is mocking and why is it needed?
5. How do you mock external APIs?
6. Test database vs production database.
7. What is pytest vs unittest?
8. What is test coverage?
9. How much test coverage is enough?
10. Common testing mistakes in Django projects.
11. How do you test authentication and permissions?
12. How do you test background tasks (Celery)?
13. What should not be tested?
14. How do you write testable code?
15. CI/CD impact on testing.

---

## Deployment & DevOps Basics (Additional)

1. How do you deploy a Django application?
2. What is Gunicorn and why is it used?
3. Nginx role in Django deployment.
4. WSGI vs ASGI in deployment.
5. Environment-specific settings.
6. How do you manage secrets in production?
7. What is Docker and why use it?
8. Dockerfile vs docker-compose.
9. Zero-downtime deployment strategy.
10. Handling migrations during deployment.
11. Static files handling in production.
12. Media files handling in production.
13. Rollback strategy after failed deployment.
14. What is CI/CD pipeline?
15. Common deployment mistakes.

---

## System Design (Python/Django Focus)

1. Design a simple user authentication system.
2. Design a REST API for file upload.
3. Design a rate-limited API.
4. Design a background job processing system.
5. How would you design email sending service?
6. Design a notification system.
7. Design pagination for large datasets.
8. Design caching layer for APIs.
9. Design audit logging system.
10. Design role-based access control.
11. How do you scale read-heavy APIs?
12. How do you handle high write load?
13. Designing idempotent APIs.
14. Designing retry mechanism.
15. Designing fault-tolerant services.

---

## Production & Real-World Scenarios (Very Important)

1. A migration broke production. What do you do?
2. API response time suddenly doubled. How do you debug?
3. Database CPU is high. What are your steps?
4. Memory usage keeps increasing. How do you investigate?
5. Users report random logout issues.
6. Background jobs are failing silently.
7. Duplicate records are getting created.
8. Data inconsistency between services.
9. Handling partial failures in APIs.
10. How do you handle backward incompatible changes?
11. Handling traffic spikes.
12. Debugging issues without logs.
13. Hotfix vs proper fix decision.
14. Handling long-running requests.
15. Handling timezone-related bugs.

---

## Behavioral (Technical-Focused)

1. A senior disagrees with your implementation. What do you do?
2. You missed a production bug. How do you handle it?
3. How do you review someone else's code?
4. How do you handle pressure during production incidents?
5. How do you estimate task timelines?
6. How do you handle unclear requirements?
7. How do you prioritize bugs vs features?
8. How do you explain technical debt to non-technical people?
9. How do you mentor juniors?
10. Biggest technical mistake you made and learned from.

---

## API Design & Integration (Additional)

1. How do you design APIs for backward compatibility?
2. How do you handle partial success responses?
3. What is HATEOAS? Have you used it?
4. How do you design bulk APIs?
5. How do you handle idempotency keys?
6. API timeout vs long polling.
7. Webhooks vs polling.
8. How do you secure webhooks?
9. How do you design retry-safe APIs?
10. How do you version APIs without breaking clients?
11. Handling third-party API failures.
12. Circuit breaker pattern.
13. API gateway role.
14. Rate limiting strategies.
15. API contract testing.

---

## Data Modeling & Architecture (Additional)

1. How do you design database schema from requirements?
2. When do you choose NoSQL over SQL?
3. How do you handle schema evolution?
4. Soft delete vs hard delete.
5. Audit tables design.
6. Multi-tenant database design.
7. Sharding basics.
8. Read-heavy vs write-heavy schema design.
9. Handling large tables.
10. UUID vs auto-increment IDs.
11. Handling historical data.
12. Data migration strategy.
13. Referential integrity vs performance.
14. Designing for analytics vs transactions.
15. Handling data privacy requirements.

---

## Security (Advanced)

1. OWASP Top 10 relevance to Django.
2. How do you prevent SQL injection in Django?
3. How do you prevent XSS?
4. CSRF attack flow.
5. Token storage best practices.
6. Password hashing algorithms.
7. Secrets rotation strategy.
8. Role-based vs attribute-based access control.
9. API key management.
10. Preventing brute force attacks.
11. Security headers usage.
12. HTTPS enforcement.
13. Handling sensitive data in logs.
14. Security testing approaches.
15. Handling security incidents.

---

## Code Quality & Maintainability

1. What makes code readable?
2. How do you reduce cyclomatic complexity?
3. Refactoring strategy.
4. Handling legacy code.
5. Code review best practices.
6. Common code smells.
7. Dependency management.
8. Handling breaking changes.
9. Writing self-documenting code.
10. Documentation importance.
11. Managing large Django projects.
12. Modularization strategies.
13. Avoiding tight coupling.
14. Backward compatibility in code.
15. Technical debt management.

---

# Python, Django Interview Questions – Advanced / Part 2

---

## Distributed Systems (Advanced)

1. What is a distributed system?
2. CAP theorem and real-world trade-offs.
3. Consistency vs availability.
4. Eventual consistency examples.
5. Synchronous vs asynchronous communication.
6. Message queues vs REST APIs.
7. Kafka vs RabbitMQ basics.
8. Exactly-once vs at-least-once delivery.
9. Handling duplicate messages.
10. Designing idempotent consumers.
11. Distributed transactions problems.
12. Saga pattern basics.
13. Service discovery.
14. Circuit breaker pattern.
15. Backpressure handling.

---

## Caching Deep Dive

1. Cache-aside vs write-through.
2. Cache invalidation strategies.
3. Cache stampede problem.
4. TTL tuning.
5. Redis data structures.
6. Redis persistence options.
7. Redis eviction policies.
8. Hot key problem.
9. Distributed cache consistency.
10. When caching hurts performance.
11. API-level caching vs DB caching.
12. Per-user cache design.
13. Cache warming strategies.
14. Handling stale data.
15. Monitoring cache efficiency.

---

## Django Internals (Advanced)

1. How Django URL resolver works internally.
2. Django ORM query compilation.
3. How QuerySets are evaluated.
4. Django app loading process.
5. Signal dispatch mechanism.
6. Middleware execution chain.
7. Django template rendering flow.
8. How Django handles sessions.
9. Cookie vs session storage.
10. Django authentication backend flow.
11. Permission checks internally.
12. How Django handles file uploads.
13. Static file collection process.
14. Django startup time optimization.
15. Django internals limitations.

---

## Advanced REST Concepts

1. REST vs RPC.
2. GraphQL vs REST trade-offs.
3. gRPC basics.
4. API aggregation pattern.
5. Backend-for-Frontend (BFF).
6. API schema evolution.
7. OpenAPI contract-first approach.
8. Handling breaking API changes.
9. API documentation automation.
10. API mocking strategies.
11. HATEOAS practical issues.
12. Long-running operations APIs.
13. Streaming APIs basics.
14. Pagination consistency.
15. API governance.

---

## Background Processing & Async

1. When background jobs are mandatory.
2. Celery architecture deep dive.
3. Broker vs backend.
4. Task retries and backoff.
5. Idempotent Celery tasks.
6. Task time limits.
7. Monitoring Celery workers.
8. Handling stuck tasks.
9. Async views in Django.
10. When async Django does not help.
11. Async ORM limitations.
12. Thread pool vs process pool.
13. Scheduling periodic jobs.
14. Handling large batch jobs.
15. Graceful worker shutdown.

---

## Failure Handling & Resilience

1. Types of failures in production.
2. Partial failure handling.
3. Retry vs fail-fast.
4. Exponential backoff.
5. Timeout tuning.
6. Bulkhead pattern.
7. Graceful degradation.
8. Fallback strategies.
9. Chaos testing basics.
10. Designing for failure.
11. Preventing cascading failures.
12. Error budgets.
13. Incident severity levels.
14. Root cause analysis.
15. Learning from outages.

---

## Senior Engineer Decision Making

1. How do you choose between two architectures?
2. How do you evaluate technical risk?
3. Build vs buy decision.
4. When to refactor vs rewrite.
5. How to say no to bad ideas.
6. Balancing deadlines vs quality.
7. Handling unclear ownership.
8. Leading technical discussions.
9. Documenting design decisions.
10. Aligning tech with business goals.
11. Estimating unknown tasks.
12. Handling production pressure.
13. Avoiding over-engineering.
14. Choosing the right abstraction.
15. What makes a system "boring" and why it is good.

---

## Interview Trap & Scenario Questions

1. A fix works locally but not in production.
2. A bug appears only under load.
3. Data loss incident – first steps?
4. Rollback is not possible – what now?
5. Third-party API is down.
6. DB migration is slow on prod.
7. Cache outage impact handling.
8. Memory leak but no clear logs.
9. Sporadic authentication failures.
10. Timezone bugs across services.
11. Duplicate payments incident.
12. Sudden spike in error rates.
13. Background jobs stuck.
14. Deployment succeeded but app is broken.
15. CEO wants a risky hotfix.

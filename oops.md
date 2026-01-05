# Python OOP Interview Questions and Answers

### 1. What is Object-Oriented Programming?

Object-Oriented Programming, or OOP, is a programming paradigm that organizes software design around **objects** rather than functions or logic. In Python, everything is an object, and OOP allows us to model real-world entities with attributes (data) and behaviors (methods). This approach makes code more modular, reusable, and easier to maintain because each object is self-contained and can interact with other objects in a predictable way.

### 2. What are the main principles of OOP?

The main principles of OOP are **Encapsulation, Abstraction, Inheritance, and Polymorphism**. Encapsulation keeps data safe by restricting access. Abstraction hides unnecessary details and exposes only what’s needed. Inheritance allows a class to reuse code from another class. Polymorphism lets objects take multiple forms, enabling flexibility in code. Together, these principles make OOP a powerful way to structure complex programs.

### 3. What is a class?

A class is like a blueprint or template for creating objects. It defines the structure and behavior that the objects created from it will have. For example, if you have a `Car` class, it can define attributes like `color` and `model`, and methods like `start()` and `stop()`. The class itself is not an actual car; it’s the design from which car objects are made.

### 4. What is an object?

An object is a specific instance of a class. If the class is the blueprint, then the object is the real-world entity created using that blueprint. Continuing the car example, a red Tesla Model S would be an object of the `Car` class. Each object can have different attribute values, but they all share the same structure and behavior defined in the class.

### 5. Difference between class and object

A class is the **blueprint** or definition of what an object should be, while an object is an **actual instance** created from that blueprint. You can think of a class as a recipe and the object as the cake you bake from it. Classes define the potential, and objects bring that potential into existence with real data.

### 6. What is `__init__()` method?

The `__init__()` method in Python is a **special constructor method** that gets called automatically when a new object of a class is created. It’s used to initialize the object’s attributes. For example, when creating a `Car` object, `__init__()` can set its color and model so each object starts with its own specific values.

```python
class Car:
    def __init__(self, color, model):
        self.color = color
        self.model = model

my_car = Car("Red", "Tesla Model S")
```

### 7. What is `self` keyword?

The `self` keyword represents the **instance of the class**. It is used to access variables and methods associated with the current object. Whenever you define a method in a class, `self` must be the first parameter so Python knows which object you are referring to.

### 8. Can we create multiple objects of a class?

Yes, we can create **multiple objects** from a single class. Each object is independent, and it can have different values for its attributes. This is one of the core advantages of OOP, as the same class can produce many real-world entities without repeating code.

```python
car1 = Car("Red", "Tesla Model S")
car2 = Car("Blue", "BMW X5")
```

### 9. What are instance variables?

Instance variables are **attributes that belong to a specific object**. They are defined inside methods using `self`. Each object has its own copy of instance variables, so changes to one object do not affect another.

### 10. What are class variables?

Class variables are **shared across all instances** of a class. They are defined directly inside the class, outside any method. All objects of that class share the same value for class variables unless explicitly overridden.

```python
class Car:
    wheels = 4  # class variable
```

### 11. Difference between instance variable and class variable

Instance variables are **unique to each object**, whereas class variables are **shared among all objects** of the class. Instance variables define the state of an object individually, while class variables define a property common to all objects.

### 12. What is encapsulation?

Encapsulation is the concept of **restricting access to certain parts of an object** and bundling the data and methods together. It helps in protecting data from accidental modification and provides controlled access through methods.

### 13. How is encapsulation achieved in Python?

In Python, encapsulation is achieved using **access specifiers**. Prefixing variables with a single underscore `_` indicates a protected variable, and double underscores `__` make it private. Although Python doesn’t strictly enforce access control, these conventions guide developers to handle attributes safely.

### 14. What is abstraction?

Abstraction is the process of **hiding complex implementation details** and exposing only essential features. It allows users to interact with an object without worrying about internal complexity.

### 15. How do you implement abstraction in Python?

In Python, abstraction can be implemented using **abstract classes and methods** from the `abc` module. An abstract class defines methods that must be implemented in derived classes.

```python
from abc import ABC, abstractmethod

class Vehicle(ABC):
    @abstractmethod
    def start(self):
        pass

class Car(Vehicle):
    def start(self):
        print("Car started")
```

### 16. What is inheritance?

Inheritance allows a class to **derive properties and methods from another class**. The parent class provides base functionality, and the child class can extend or override it. This promotes code reuse and logical hierarchy.

### 17. Types of inheritance in Python

Python supports **single, multiple, multilevel, hierarchical, and hybrid inheritance**. Single inheritance is one parent, multiple inheritance has more than one parent, multilevel is a chain of inheritance, hierarchical is one parent with multiple children, and hybrid is a combination.

### 18. What is multiple inheritance?

Multiple inheritance occurs when a **child class inherits from more than one parent class**. Python handles method resolution order (MRO) to determine which parent method to execute if there’s a conflict.

### 19. What is method overriding?

Method overriding happens when a **child class provides a new implementation** for a method already defined in the parent class. The child’s method takes precedence, allowing customized behavior.

### 20. What is method overloading in Python?

Python does not support traditional method overloading by default. However, you can simulate it using **default parameters or `*args` and `**kwargs`** to handle multiple scenarios within the same method.

### 21. What is polymorphism?

Polymorphism allows objects of different classes to be **treated as objects of a common superclass**, while still executing class-specific behavior. It enables flexibility and reusable code.

### 22. What is dynamic binding?

Dynamic binding refers to **the runtime decision of which method or function to call** based on the object’s type. It’s closely related to polymorphism in Python.

### 23. What is `super()` function?

The `super()` function is used to **call a method from the parent class**. It’s commonly used in `__init__()` to initialize parent class attributes or to extend functionality without rewriting the parent’s code.

### 24. What is Method Resolution Order (MRO)?

MRO is the **order in which Python searches for methods** in a hierarchy of classes, especially in multiple inheritance. It determines which method gets called first. You can check it using `ClassName.__mro__`.

### 25. What is `isinstance()`?

`isinstance()` checks if an object is an **instance of a specific class or a subclass**. It returns `True` or `False`.

```python
isinstance(car1, Car)  # True
```

### 26. What is `issubclass()`?

`issubclass()` checks if a **class is derived from another class**. It also returns `True` or `False`.

```python
issubclass(Car, Vehicle)  # True if Car inherits Vehicle
```

### 27. What are magic methods?

Magic methods are special methods with **double underscores** (`__init__`, `__str__`, `__repr__`, etc.) that allow customizing object behavior for built-in operations like addition, printing, or comparison.

### 28. What is `__str__()` and `__repr__()`?

`__str__()` returns a **user-friendly string** representation of an object, whereas `__repr__()` returns a **developer-friendly, unambiguous string** meant for debugging. Ideally, `__repr__` should give enough info to recreate the object.

### 29. What is data hiding in Python?

Data hiding is a technique to **restrict direct access to certain attributes** of an object, typically by making them private. This ensures internal data integrity and prevents accidental modification.

### 30. What is private, protected, and public access?

* **Public:** Accessible from anywhere.
* **Protected (_var):** Accessible within class and subclasses, a convention rather than strict.
* **Private (__var):** Name-mangled to discourage direct access; intended for internal use only.

### 31. Can Python enforce data hiding strictly?

No, Python cannot enforce strict data hiding. Private variables can still be accessed using name mangling. Python relies on **developer discipline** to maintain encapsulation.

### 32. What is composition vs inheritance?

Inheritance models an **“is-a” relationship**, while composition models a **“has-a” relationship**. Composition involves creating objects within objects rather than inheriting behavior.

### 33. When should you use composition?

Use composition when you want **flexibility** and when the relationship is not strictly hierarchical. It’s often preferred for complex systems because it avoids deep inheritance chains and allows dynamic behavior.

### 34. What is an abstract class?

An abstract class is a class that **cannot be instantiated** directly and may contain **abstract methods** that must be implemented by subclasses. It’s used to define a common interface for related classes.

### 35. What is an interface in Python?

Python doesn’t have a separate interface keyword like Java. Interfaces can be created using **abstract base classes with abstract methods**, which enforce that derived classes implement certain methods.

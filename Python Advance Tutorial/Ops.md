# what is class

a pytho class is a group of attributes and methods

# what is attributes
 Attrubutes are represented by varuable that contains data
 
 # what is method
 method performs an action or task. It is similar to function
 
 
 <b>Self is a variable</b>
 
 
       class Classname(object):

          def __init__(self):
              self.variable_name = value
              self.variable_name = "value"

          def method_name(self):
              Body of method

1 ) class - class keyword is used to create a class
2 ) object - object represents the base class name from where all classes in python are derived
     this class is also derived from object class. This is optional.
     
3 ) __init__() - This method is used to initialize the variable. This is a special method. We do not call this method explicitly
4 ) self - self is a variable which refers to current class instance/object


# Rules

1 ) The class name can be any valid identifier.
2 ) It can't be python reserved word.
3 ) A valid class name starts 


 # Object
 
 Each time you create an object of a class a copy of each variable defined in the clas is created
 
 
 
    class Mobile:

        def __init__(self):
            self.model = "RealMe X"

        def show_model(self):
            print("Model",self.model)

    realme = Mobile()
    redmi = Mobile()
    geek = Mobile()

    redmi.show_model()
    

<b>1 Example </b>

      class Myclass:
          def show(self):
              print("I am a Method")

      x = Myclass()
      x.show()
 
 <b> 2 Example </b>
 
     class Mobile:
        def __init__(self):
            self.model = "RealMe X"

        def show_model(self):
            print("Modelo :",self.model)

    realme = Mobile()

    realme.show_model()
 
 
 <b>3 Example </b>
 
     class Mobile:
        def __init__(self):
            self.model = "RealMe X"

        def show_model(self):
            print("Modelo :",self.model)

    realme = Mobile()
    realme.model = "RealMe Pro2"

    print(realme.model)
    realme.show_model()
    
OUTPUT 

RealMe Pro2
Modelo : RealMe Pro2


_____________________________________________________

# Constructor

Python supports a special type of method called constructor for initializiing the instance variable of a class
A class constructor , if defined is called whenever a program creates an objec of that class

A construcotr is called only once at the time of creating an instance
if two instance are created for a class, the constructor will be called once for each instance




# Constructor without parameter

       class Mobile:

          def __init__(self , a):
              self.name = a

      data = Mobile("I-phone")

      print(data.name)


# Type of variable

1 ) Instance variables are the variables whose separate copy is created in every object

2 ) Instance variables are defined and initialized using a constructor with self parameter


        class Mobile:
            def __init__(self):
                self.name = "Iphone"     <<---- Instance Variable

        data = Mobile()

# Accessing Instance Variable


To access instance variable, we need instance methods with self as first parameter then we can access instance variable using self.variable_name


       class Mobile:
           def __init__(self):
               self.name = "I-Phone" <<---- Instance Variable

           def show(self):      <<------ Instance Method
               self.name <<----  Accessing instance variable

       mobile = Mobile()
       print(mobile.name)


# Accessing Instance Variable

<b>Outside Class</b>

We can access instance variable using <b>object_name.variable_name</b>


      class Mobile:
          def __init__(self):
              self.name = "I-Phone"

      data = Mobile()
      print(data.name)      <<----- Access instance variable
      
 

# Instance Variable

Instance variable are the variables whose separate copy is created in every object. if we modify the copy of instance variable in an instance, it will
not effect all the copies in the other instance



       class Mobile:
           def __init__(self):
               self.model = "RealMe X"

           def show_model(self):
               print(self.model)

       realme = Mobile()
       redmi = Mobile()
       geek = Mobile()




# class Variable / Static Variable


class variables are the variables whose single copy is available to all the instance of the class
if we modify the copy of class variable in an instance, it will effect all the copies in the other instance

class Mobile:
    fp = "yes"     <<<<---- class varaible
    
    def show(self):
        self.name = "sachin"
        print(self.fp)
    
data = Mobile()
data.show()

 ___________________________________________________________
 
# Inheritance

The mechanism of deriving a new class from an old one ( existing class ) such that the new clas inherit all the members ( variable and mehods ) of old class is called inheritance or derivation


 old class
    |
    |
 New CLass


# Super class and sub class

The old class is referred to as the super class and the new one is called the sub class

* Parent class - Base class  or super class
* Child class  - Derived class or sub class



<b>Real life example<b>
 
         Father
          1) Home
          2) Money
          3) Business

         Son
           1 ) BMW



# Inheritance
 
All classes in python are build from a single super class called 'object' so whenever we create a class inpython, object will become super class for 
 them internally.
 
         class Mobile(object):
         class Mobile:

* The main advantage of inheritance is code resuability.
 
 

 # Single Inheritance
 
 
 <b> Type of Inheritance </b>
 
 1 ) Single Inheritance
 2 ) Multi - Level Inheritance
 3 ) Hierarchical Inheritance
 4 ) Multiple Inheritance

 
 
 # declaration of child class
 
 
 # Single Inheritance
 
 If a class is derived from one base class ( Parent Class ), it is called single inheritance.
 
 <b> Syntax:- </b>
 
 
       class Person(object):
          person info

       class Employee(ParentClassName):
          employee info
 
 
 
 <b>Example</b>
 
 

         # Base class (superclass)
         class Vehicle:
             def __init__(self, brand, model):
                 self.brand = brand
                 self.model = model
         
             def display_info(self):
                 print(f"{self.brand} {self.model}")
         
         # Derived class (subclass) inheriting from Vehicle
         class Car(Vehicle):
             def __init__(self, brand, model, color):
                 # Calling the __init__ method of the base class using super()
                 super().__init__(brand, model)
                 self.color = color
         
             def honk(self):
                 print("Beep beep!")
         
         # Example usage
         if __name__ == "__main__":
             # Creating an instance of the Car class
             my_car = Car("Toyota", "Camry", "Blue")
         
             # Accessing attributes from the base class
             my_car.display_info()
         
             # Accessing attribute from the derived class
             print(f"The color of the car is {my_car.color}.")
         
             # Calling method from the derived class
             my_car.honk()

 
 
 
# Constructor in inheritance in python
 
 By default , The Constructor in the parent class is available to the child class
 
 
          class Person:

              def __init__(self):
                  self.name = "sachin"

          class Employee(Person):

              def show(self):
                  print(self.name)

          data = Person()
          data.show()
 
 
 # constructor Overriding in python
 
 if we write constructor in the both classes , parent class and child class then the parent class constructor is not available to the child class.
 in this case only child class constructor is accesible which means child class constructor is replacing class constructor
 
 ( jar child class madhe constructor define asel tar to parent class cha constructor ghet nahi. ani jar parent class cha constructor call kela tar
 error yeil )
 
 
           class Father:

              def __init__(self):
                  self.name = "sachin"

          class Son(Father):

              def __init__(self):
                  self.name = "sagar"

              def show(self):
                  print(self.name)

          data = Son()
          data.show()

           ----------- OUTPUT -----
           sagar
 
 
 
 
 # Multi-Level Inheritance
 
 In multi-level inheritance, the class inherits the feature of another derived class
 
 
        #Base class (superclass)
        class Person:
            def __init__(self, name, age):
                self.name = name
                self.age = age
        
            def display_info(self):
                print(f"Name: {self.name}, Age: {self.age}")
        
        # Derived class inheriting from Person
        class Employee(Person):
            def __init__(self, name, age, employee_id):
                super().__init__(name, age)
                self.employee_id = employee_id
        
            def display_employee_info(self):
                print(f"Employee ID: {self.employee_id}")
        
        # Further derived class inheriting from Employee
        class Manager(Employee):
            def __init__(self, name, age, employee_id, department):
                # Calling the __init__ method of the immediate base class using super()
                super().__init__(name, age, employee_id)
                self.department = department
        
            def display_manager_info(self):
                print(f"Manager in {self.department} department")
        
        # Example usage
        if __name__ == "__main__":
            # Creating an instance of the Manager class
            manager = Manager("Alice", 35, "M56789", "Human Resources")
        
            # Accessing attributes and methods from the base and intermediate classes
            manager.display_info()
            manager.display_employee_info()
        
            # Accessing attribute from the derived class
            manager.display_manager_info()


 
 
 
 # Hierarchical Inheritance
 
 In this clas we create a one class to multiple child class
 
 
 
        class Father:
           member oif class father


       class Son(Father):
           member of class son

       class Daughter(Father):
           member of class daughter

 
 
 
 <b>example</b>
 
            class Father:
               def showF(self):
                   self.name = "sachin"
                   print("Father class mehod")


           class Son(Father):
               def showS(self):
                   print("Son class mehod")



           class Daughter(Father):
               def showD(self):
                   print("Daughter class mehod")

           data = Daughter()
           data.showD()
           data.showF()
 
 
 
 # Multiple Inheritance
 
 if a class is derived from more than one parent classs, then it is called multiple inheritance.
 
 <b>Syntax :-</b>
 
          class ParentClassName1(object):
             member of parnet class

         class ParentClassName2(object):
             member of parent class


         class ChildClassName(ParentClassName1,ParentClassName2):
 
 
 
 ------ Code 

        class Person:
           def __init__(self, name, age):
               self.name = name
               self.age = age
       
           def display_info(self):
               print(f"Name: {self.name}, Age: {self.age}")
       
       class Employee(Person):
           def __init__(self, name, age, employee_id):
               super().__init__(name, age)
               self.employee_id = employee_id
       
           def display_info(self):
               super().display_info()
               print(f"Employee ID: {self.employee_id}")
       
       class Student(Person):
           def __init__(self, name, age, student_id):
               super().__init__(name, age)
               self.student_id = student_id
       
           def display_info(self):
               super().display_info()
               print(f"Student ID: {self.student_id}")
       
       class EmployeeAndStudent(Employee, Student):
           def __init__(self, name, age, employee_id, student_id, role):
               # Using explicit calls to the constructors of both superclasses
               Employee.__init__(self, name, age, employee_id)
               Student.__init__(self, name, age, student_id)
               self.role = role
       
           def display_info(self):
               super(Employee, self).display_info()  # Call display_info from Employee
               super(Student, self).display_info()   # Call display_info from Student
               print(f"Role: {self.role}")
       
       # Example usage
       if __name__ == "__main__":
           person = EmployeeAndStudent(name="Alice", age=25, employee_id="E123", student_id="S456", role="Intern")
           person.display_info()


 
 
 # Encapsulation in Python

 encapsulation ne apan purn access det nahi kahi private thevto

 An objects variable should not always be directly accessible.
 The methods can ensure the correct values are set. if an incorrect value is set, the method can return an error


 private variable direct object chya throught direct accesss nahi karu shakat try kel tar error yeil jas khali kelay pan apan aat madhe use karu shakto 

      class Student:
         __name = "Ravi"
     
     obj = Student()
     print(obj.__name)

how to we can use

      class Student:
          __name = "Ravi"
          
          def __init__(self):
              print(self.__name)
      
      obj = Student()

 apan private method pan baher access karu shakat nahi kel tar error yeil

      class Student:
         __name = "Ravi"
         
         def __init__(self):
             print(self.__name)
             
         def __result(self):
             print("Welcome")
     
     obj = Student()
     obj.__result()

apan atlya at method call karu shakto

       class Student:
           __name = "Ravi"
           
           def __init__(self):
               print(self.__name)
               self.__result() # ithe call kela 
               
           def __result(self):
               print("Welcome")
       
       obj = Student()
       
       OUTPUT:
       Ravi 
       Welcome
 
Definition:

Encapsulation is one of the four fundamental principles of object-oriented programming (OOP), alongside inheritance, polymorphism, and abstraction.
It involves bundling the data (attributes) and methods (functions) that operate on the data into a single unit known as a class.
Access Modifiers:

Public Attributes and Methods: Accessible from outside the class, forming the class's public interface.
Private Attributes and Methods: Indicated by a double underscore (__) prefix, not directly accessible from outside the class.
Access Control:

Encapsulation controls access to the internal details of a class, restricting direct access to private attributes and methods.
Promotes information hiding, allowing changes to the internal implementation without affecting external code.
Getter and Setter Methods:

Getter methods are used to access the values of private attributes.
Setter methods are used to modify the values of private attributes, providing controlled access.
Property Decorator:

The property decorator allows a cleaner syntax for defining getter, setter, and deleter methods for an attribute.
It simplifies the process of encapsulating attribute access.
Benefits:

Enhances Code Reliability: By restricting direct access to internal details, encapsulation helps prevent unintended interference.
Promotes Modularity: Encapsulation encourages breaking down code into smaller, manageable units, promoting modular design.
Facilitates Maintenance: Changes to the internal implementation are isolated, making the code more maintainable over time.
Real-world Analogy:

Encapsulation can be compared to packaging a product in a box with a rulebook. The box protects the product (internal details), and the rulebook guides users on how to interact with it (public interface).
Implementation in Python:

In Python, encapsulation is achieved through access modifiers (public, private, etc.), getter and setter methods, and the property decorator.
 
 
 
 
       class Student:
           def __init__(self):
               self.__name = ""
           
           def getname(self):
               return self.__name
           
           def setname(self,name):
               self.__name = name
       
       data = Student()
       data.setname("testing")
       output = data.getname()
       print(output)
 
 

 # Polymorphism

 **Polymorphism** means same function name ( but different signatures ) being uses for different type


 Method overloading is a form of compile-time polymorphism where a class has multiple methods with the same name but different parameter lists.

 example len function in python we can pass string also we can pass list will give the output here as you can see len function is same but we passign diffrent data

        class MathOperations:
           def add(self, x, y, z=0):
               return x + y + z
       
       # Example usage
       math_operations = MathOperations()
       result1 = math_operations.add(2, 3)        # Calls the add method with two arguments
       result2 = math_operations.add(2, 3, 5)     # Calls the add method with three arguments



**Method overriding**
ya madhe jar donhi class madhe same name ne jar method asel ani  second class madhe first class inherit kela tar first class chay method override karel

         class Ws:
             def displayinfo(self):
                 print("Welcome to Wscubetech")
         
         class IIP(Ws):
             def displayinfo(self):
                 print("Welcome to sachin")
         
         obj = IIP()
         obj.displayinfo()
         
         OUTPUT:
         Welcome to sachin

pan aplyala jar donhi method call karaychya asel tar apan super keywork use karto

        class Ws:
            def displayinfo(self):
                print("Welcome to Wscubetech")
        
        class IIP(Ws):
            def displayinfo(self):
                super().displayinfo()
                print("Welcome to sachin")
        
        obj = IIP()
        obj.displayinfo()

        OUTPUT:
        Welcome to Wscubetech
        Welcome to sachin



# Operator overloading

Operator overloading in Python allows you to redefine or extend the behavior of existing operators for user-defined objects. It enables you to use the same operator symbols (+, -, *, etc.) with instances of your own classes and define how those operations should behave.


Arithmetic Operators:

__add__(self, other): Addition (+)
__sub__(self, other): Subtraction (-)
__mul__(self, other): Multiplication (*)
__truediv__(self, other): Division (/)
__floordiv__(self, other): Floor Division (//)
__mod__(self, other): Modulus (%)
__pow__(self, other, modulo=None): Exponentiation (**)
Comparison Operators:

__eq__(self, other): Equal to (==)
__ne__(self, other): Not equal to (!=)
__lt__(self, other): Less than (<)
__le__(self, other): Less than or equal to (<=)
__gt__(self, other): Greater than (>)
__ge__(self, other): Greater than or equal to (>=)
Unary Operators:

__neg__(self): Unary negation (-)
__pos__(self): Unary plus (+)
__abs__(self): Absolute value (abs())
__invert__(self): Bitwise NOT (~)


         class Point:
             def __init__(self, x, y):
                 self.x = x
                 self.y = y
         
             def __add__(self, other):
                 # Redefine the '+' operator for Point objects
                 return Point(self.x + other.x, self.y + other.y)
         
         # Usage
         p1 = Point(1, 2)
         p2 = Point(3, 4)
         result = p1 + p2
         print(result.x, result.y)  # Output: 4 6

# diffrence between class variable and instance variable

class variable is a associated with class



         class Demo:
             company_name = "Appli"      # this is a class variable
             
             def __init__(self):
                 print("Helo")
                 
             def info(self):
                 print(f"{self.company_name}")
         
         data = Demo()
         data.info()


**Instance variable**

instance variable are associated with instance of the class

       class Demo:
           company_name = "Appli"
           
           def __init__(self,name):
               print("Helo")
               self.raise_salary = 1.3  # instance variable
               
           def info(self):
               print(f"{self.company_name}",self.raise_salary)
       
       data = Demo("Sachin")
       data.raise_salary = 1.5
       data.info()

# Encasulation 


data la encapsulate karun thevne . 
example:

samja A,B,C variable ahe te mi var declare kelay ani 100 line nantar mi punha A declare kela . yamule kay hoil adhicha declare kelela variable
ani attacha declare kelela variable conflict yein hi condition handle karnya sathi apan A,B,C ya variable la aka class madhe encapsulation karto 
te class madhle variable private astat tyana class chya baher access hot nahi class chay atle variable private astat

# Abstraction

abstraction mhanje jya goshtichi garaj ahe tyach dakhavne bakiche hide karne . mhanje user la tevdach dakhavla jat ki tyala jyachi garaj ahe
bakicha sagla tya pasun hide karun thevla jat


# Inheritance

mhaje kuthli gosha virasat madhe milne jas father kadun son kade inheritance houn yene
ya madhe code la repeate lihava lagat nahi . aka class madhun data 


# Polymorphism

function name same pan tyacha kam define kel ahe mhanje input madhe kay yetay tya according action perform karta tyachya according data show karto





______________________________________________________________________________________________
# Class
A python class is a group of attibutes and methods.

1 ) Attributes
Attribute are represented by variable that contains data

jas code python mandhe ( a= 10) tylach yethe attributes mhatat

2 ) What is method

method mhanjech function . tech function class madhe method mhantat 

What is class 

        class ClassName(objects):
          def __init__(self):
              self.variable_name = value

          def method_name(self):
              Body of method
              
              
1 ) class : class keyword in used to create a class
2 ) object : object represents the base clas name from where all class in python are derived this class is also derived from object class this 
             optional
             
3 ) __init__() : hi method initilize karte variable la . hya method la call karnyachi gar nahi automatically call hoto . yacha kam asta variable la
                 initilize karne 
                 
4 ) self         : self variable chay javal current object asto 



Rules  -----------------------------

1 ) The class ame can be any valid identifier
2 ) it cant be python reserverd word
3 ) a valid class name starts with latter 
4 ) class generally start with capital letter


how to declare class




              class Mobile:
                def __init__(self):
                    self.model = "RealMe X"
                def show_model(self):
                     print("Model",self.model)



              Create class with arugment

              class Classname:
                def __init__(self ):
                  self.variable_name = value
                  self.variable_name = "value"

                def method_name(self):
                  body of method
    
   
   @ ) what is object
   
   
   object ha class type cha variable ahe 
   
   
   
   create object
   
                object_name = class_name()
                
                
                
                
                
   Real class
   
   Eample
   
   
   
          class Mobile:
            def __init__(self):
              self.model = "RealMe X"

            def show_mobile(self):
              print("Model",self.model)

          realme = Mobile()
                
                
 ________ HOW it works _____________
 
 realme = Mobile()
 
 1 ) a block of memory is allocated on heap .  the size of allocated memory is to be decided from the attributes and methods are avaialbel in the
     class (Mobile) 
     
 2 ) After allocating memeory block the special method  __init__() is called internally . the method stores the initial data into variable
 
 3 ) The allocated memory location address of the instance is returned in to object
 
 4 ) the memory location is passed to self
 
 
 
_____________ Accessing class member using object __________

1 ) We can access variable and method of a class using class object or instance of class

                        object_name.variable_name
                        realme.mode

                        object_name.method_name()
                        realme.show_model():

_________________ Self variable ______

self is a default variable that contains the memory address of the current object this variable is used to refere to allthe instance variable and method
when we careate aobject of a class the object name contains the memory location of the object
this imemofy location is internally passed to self as self as self know the memory address of the object so we can access variable and method of object
self is the first argument o any objhect method because the first argument is always the object referance . this is automatic whether you call it
self or not


___________ Object _______

Each time you create an object of a class a copy of ech variable defined in the class is created

                class Mobile:
                    def __init__(self):
                        self.model  = "Readme X"

                    def show_model(self):
                        print("Model",self.model)

                realme = Mobile()
                redmi  = Mobile()
                geek   = Mobile()



_______________ Real work Example ______


          class Mobile:
              def __init__(self , m):
                  self.model = m


              def show_model(self , p):
                  price = p
                  print("model", self.model , "Price",price)

          x = Mobile("Realme X")
          x.show_model(1000)
                
                
                
# Constructor

class object banavlyavar constructor automatically call hoto yala call karnyachi garj nahi. ha akdach call hosto jeva instance create hoto
jeva object create karto . 2 instance ( object ) banavle asle tar constructor 2 da call hoto 

Example :
                       
                       class Mobile:
                             def __init__(self): _______________ ha constructor
                                self.model = "RealME  x"

                        realme = Mobile()
                        
                        
 Example :
 
                class Mobile:
                    def __init__(self,m , v = 100):
                        self.m = m
                        self.v = v
                        print("Construtor is call")


                    def show_m(self , mobile , price):
                        self.mobile = mobile
                        self.price = price

                        print(self.mobile)
                        print(self.price)


                m = Mobile("Iphone")
                m.show_m(mobile = "Realme",price = 234)

                ok = Mobile("Samsung")
                ok.show_m(mobile = "Samsung" , price = 321)
                
                
                OUTPUT : 
                Construtor is call
                Realme
                234
                Construtor is call
                Samsung
                321




1 ) Python supports a special type of method called constructor for initializing the instance variable of a class
2 ) A class constructor , if defined is called whenever a program creates an object of that class
3 ) A constructor is called only once at the time of creating an instance . if two instance are created for a class , the constructor will be called once for each instance
                
                
                
                
                
                
 # Type of variable
 
 instance variable la constroctor chay aat initilize ani define kel jat
 
 1 ) Instance  Variable 
                1 ) Intance variable are the variable whose separate copy is created in every object instance varibale are defined and initialized using 
                a constructor with self parameter
                
    
  Example 
  
                  class Mobile:
                    def __init__(self):
                        self.model = "Realme X"

                    def show_model(self):
                        print(self.model)

                realme = Model()
                
  ________________________________________________________________________________________
  2 ) Accessing instance variable
  ________________________________________________________________________________________
    
  with Instance method
  
  to access instance variable , we need instance methods with self as first parameter then we can access instance variable using self.variable_name
  
  Example : 
  
                          class Mobile:
                            def __init__(self):
                                self.model = "Realme X"   _____________ Instance variable
                                
                                print(self.model)
                            def show_model(self):        _______________ Instance method
                                print(self.model)        ________________ Accessing instance variable
                                
                        realme = Mobile()

___________________________________


Access instance variable outside class

___________________________________

class chay baher instance variable access karaycha aslyavar objectname.variable_name

Example 

                        class Mobile:
                            def __init__(self):
                                self.model = "Realme X"
                            def show_model(self):
                                self.model
                        realme = Mobile()


                        print(realme.model) ________________ we access outside of the class
                        

POINT : 
        Intance variable are the variable whose separate copy is created in every object if we modify the copy of instance variable , it will not efect           all the copies in the other instance
        
        
        ( ya madhe pratek object sathi ak navin copy banel copy object madhe changes kelyavar real object var effect padnar nahi ) 
        
                        class Mobile:
                            def __init__(self):
                                self.model = "Realme X"
                            def show_model(self):
                                print(self.model)

                        realme = Mobile()

                        redme = Mobile()

                        geek = Mobile()
                        
                        
Example : 


                                class Mobile:
                                    def __init__(self):
                                        self.model = "RealMe X"  # Instance Variable

                                    def show_model(self):
                                        print(self.model)        # Accessing instance Variable

                                realme = Mobile()
                                redmi = Mobile()
                                geek = Mobile()


                                print(realme.model , f"ID is {id(realme.model)}")
                                print(redmi.model, f"Id Is {id(redmi.model)}")
                                print(geek.model, f"ID is {id(geek.model)}")

                                print()
                                redmi.model = "Redmi 7s"

                                print(realme.model , f"ID is {id(realme.model)}")
                                print(redmi.model, f"Id Is {id(redmi.model)}")
                                print(geek.model, f"ID is {id(geek.model)}")
                                
                                
                                OUTPUT _____________
                                RealMe X ID is 139975711000688
                                RealMe X Id Is 139975711000688
                                RealMe X ID is 139975711000688

                                RealMe X ID is 139975711000688
                                Redmi 7s Id Is 139975711001264
                                RealMe X ID is 139975711000688
   





_______________________________________________________________________________________________
2 ) Class Variable / Static Variable 
_______________________________________________________________________________________________
yamadhe akach object copy aste ti saglay madhe share hote . jar aka object chay variable madhe jar kahi update kel tar te saglay var update hot

1 ) classs variable are the variable whose single copy is avaialble to all the instance of the class if we modify the copy of class variable in an instance , it will effect all the copies in the other instance

Example :

                class Mobile:
                    fp = "Yes"

                    def __init__(self):
                        self.model = "Readme"

                s = Mobile()
                print(s.fp)
                
                
 access class variable outside of the classs
 
 Example :
 
                         class Mobile:
                            fp = "Yes"           # class variable
                            def __init__(self):
                                self.model = "Readme"
                            def show_model(self):
                                print(self.model)
                            @classmethod
                            def is_fp(cls):      # class method
                                cls.fp
                        s = Mobile()
                        print(s.fp)
                        
                        
  POINT : 
        Class variable are the variable whose single copy is available to all the instance of the class . if we modify the copy of class variable in an 
        instance , it will effecct all the copies in the other instance
        
        
        
Example : 

                                class Mobile:
                                    fp = "Yes"   # class variable

                                    def __init__(self):
                                        self.model = "RealMe X"

                                    def show_model(self):
                                        print("Model :", self.model)

                                    @classmethod    # Decorator
                                    def is_fp(cls):
                                        print(cls.fp)

                                realme = Mobile()
                                realme.show_model()
                                Mobile.is_fp()

                                # +++++++++ after modify
                                print()
                                Mobile.fp = "No"
                                Mobile.is_fp()
                                
                                
                                OUTPUT __________________________
                                
                                
                                Model : RealMe X
                                Yes
                                No
                                
                                


__________________________________________________________________________________
# Namespace 
__________________________________________________________________________________
        
In python , namespace represents a memory block where names are mapped to object

class Namespace - A class maintains it own namespace known as class namespace. in the class namespace , the names are mapped to class variables

instance Namespace - Every instance have it's own namespace known as instance namespace . In the instance namespace , the names are mapped to instance variable


____

MARATHI :

        yamadhe namespace ha represents karto mamory block la ani block ha mapped karto object la
        
        class namespace madhe name la class variable shi match kel jat
        
        instance name variable madhe instance variable la namespace shi match kel jat
        
        
        
        
        
 Example :
         
         class Mobile:
                fp  = "Yes"
                
         realme = Mobile()
         redmi  = Mobile()
         geek   = Mobile()
         
         
         
         
         ( Yamadhe  realme , redme , geek he instance name space ahe ) 
         
         
         
Example :

                                class Mobile:
                                    fp = "Yes"

                                    @classmethod
                                    def is_fp(cls):
                                        print("Finger pring " , cls.fp)

                                realme = Mobile()
                                redmi  = Mobile()
                                geek   = Mobile()

                                print("Class FP" , Mobile.fp)
                                print("realme" , realme.fp)
                                print("redmi" , redmi.fp)
                                print("Geek" , geek.fp)

                                print()

                                Mobile.fp = "No"
                                print("Class FP" , Mobile.fp)
                                print("realme" , realme.fp)
                                print("redmi" , redmi.fp)
                                print("Geek" , geek.fp)

                                print()

                                realme.fp = "Not Working"
                                print("Class FP" , Mobile.fp)
                                print("realme" , realme.fp)
                                print("redmi" , redmi.fp)
                                print("Geek" , geek.fp)
                                
                                
                                OUTPUT :_____________________
                                
                                Class FP Yes
                                realme Yes
                                redmi Yes
                                Geek Yes

                                Class FP No
                                realme No
                                redmi No
                                Geek No

                                Class FP No
                                realme Not Working
                                redmi No
                                Geek No










_________________________________________________________________________________________________
# Type Of Methods
_________________________________________________________________________________________________
        
 
 
 1 ) Instance Methods
        - Accessor Methods
        - Mutator Methods
        
 2 ) Class Methods
 
 
 3 ) Static Mehod
 
 
 ________________________________________________________________________________________________
 
 # Instance Method
 
 Instance methods are the methods which act upon the instance variables of the class instance method need to know the memory address of the instance       which is provided through self variable by default as first parameter for the instance method
 
 
                 def method_name(self):
                        function body

  
  # calling instance method W/O Argument
  
  instance methods are bound to object of the class so we call instance method with o bject name
  
  Syntax - object_name.method_name()
  
  
                  class Mobile:
                        def show_model(self):
                                print("RealMe X ")

                  realme = Mobile()
                  realme.show_model()
  
  
  
  # Instance Method with Parameter
  
                  class Mobile:
                        def __init__(self):
                                self.model = "ReadMe X"


                        def show_model(self , p):
                                self.price = p
                                print(self.model, self.price )

                   realme = Mobile()
   
   # calling instance method with argument
   
   Syntax :- object_name.method_name(Actual_argument)
   Ex :-     show_model(1000 )
   
                   class Mobile:
                        def __init__(self):
                                self.model = "RealMe X"

                        def show_model(self , p ):
                                self.price = p
                                print(self.model , self.price )
                   realme = Mobile()
                   realme.show_model(1000)
                   
                   
                   
   
 # Nested Class
 A class within a class is calles as nested calss or nesting of a class
 
 Example :
 
 
                         class OuterClassName():
                                def __init__(self):
                                        self.variable_name = value
                                        self.innterClassObjectName = self.InnerClassName()

                                def method_name(self):
                                        method body



                                class InnerClassName:
                                        def __init__(self):
                                                self.variable_name = value
                                        def method_name(self):
                                                method body
                                                
                                                


Example 2 :
 
 
                                class Army:
                                    def __init__(self):
                                         self.name = "Rahul"
                                         self.gn   = self.Gun()

                                    def show(self):
                                         print(self.name)


                                    class Gun:
                                         def __init__(self):
                                               self.name = "AK47"
                                               self.capacity = "75 Rounds"
                                               self.length   = "34.3 In"

                                         def disp(self):
                                               print(self.name , self.capacity , self.length)

                                a = Army()
                                
                                
Example 3 :

                        class Army:
                            def __init__(self):
                                self.name = "Rahul"
                                self.gn = self.Gun()

                            def show(self):
                                print("Name :",self.name)



                            class Gun:
                                def __init__(self):
                                    self.name = "AK47"
                                    self.capacity = "75 Round"
                                    self.length  = "34.3 In"

                                def disp(self):
                                    print("Gun Name",self.name)
                                    print("Capacity", self.capacity)
                                    print("Length : ", self.length)


                        a = Army()
                        print(a.name)
                        a.show()


                        print(a.gn.name)
                        
                        
                         OUTPUT :::
                         
                         Rahul
                         Name : Rahul
                         AK47
                                                
                                                
                                                

_______________________________________________________________________________________________________________________


# Iheritance


_______________________________________________________________________________________________________________________

1 ) The mechanism of deriving a new calss from an old one  ( existing class ) such that the new class inherit all the member (variable and method ) of odd class is called inheritance or derivation .





old class pasun new class banavne tyala inheriitance mahantat

_________________________________________________________________________________________________________________________________
Other Name


Parent Class         - Base Class or Super Class
Child Class          - Derived Class or Sub Class



# Super Class And Sub Class 

The old class is referred to as the Super class and the new one is called the sub class



# Inheritance

1 ) All classes in python are built in from a single super class called "object"  so whenever we create a class in python  , object will become super class for them internally


Class Mobile(object):
class Mobile:


2 ) The main advantage of inheritance is code reusability




# Why do we need Inheritance




                        class Employee:
                            id = 1

                            @classmethod
                            def getid(cls):
                                return cls.id

                             def setname(self , name):
                                self.name

                             def getname(self , name):
                                self.name

                             def getsalary(self , salary):
                                return self.salary

                             def setovertime ( self , ot):
                                self.ot = ok

                             def getovertime(self):
                                return self.ot
        
        
        
        
 # inherit two classs
 
 
 ________________________________ Parent Class __________
 
 
                                                                                                                                                                                        class Employee:
                                     id = 1
                                     @classmethod

                                     def getid(cls):
                                        return cls , id

                                     def setname(self , name ):
                                        self.name = name

                                     def getname(self):
                                        return self.name

                                     def setsalary(self):
                                         self.salary = salary

                                     def getsalary(self , salary):
                                         return self.salary

                                     def setovertime ( self , ot):
                                         self.ot = ot

                                     def getovertime(self):
                                         return self.ot


                                ________________________ Child Class________

                                class Manager:
                                    def setsalary(self , salary):
                                        self.salary = salary

                                    def getsalary(self):
                                        return self.salary

                                    def getseniorname(self , sname):
                                        self.name = sname

                                    def getseniorname(self):
                                        return self.sname
         
         
         
        
        
 _________________________________________________________________________
 # Type Of Inheritance
 
 1 ) Single Inheritance
 2 ) Multi-level Inheritance
 3 ) Hierarchical inheritance
 4 ) Multiple Inheritance
 
 ________________________________________________________________________
 
 # Declaration of child class
 
 class ChildCLassName(ParentClassName):
      members of child class
      
 class Mobile(object):
      members of child class
      
 class Mobile:
      
      
 
 # Single Inheritance
 
 if a class is derived from one base class ( Parent Class ) , it is called single inheritance
 
 
 Syntax :
 
                 class ParentClassName(object):
                      members of parent class

                 class ChildClassName(ParentClassName):
                      members of child class
      
  Exmaple:
  
  
                          class Father:
                              members of class father

                          class Son(Father):
                              members of class son
 
 

Example :

                        class Father:
                            money = 1000
                            def show(self):
                                print("Parent Class Instance method")

                            @classmethod
                            def showmoney(cls):
                                print("Parent Class Class Method",cls.money)


                            # static method use karta yete ki nahi
                            @staticmethod
                            def stat():
                                a = 10 
                                print("Child class instance method")


                        class Son(Father):
                            def disp(self):
                                print("Child CLass Instance method")

                        s = Son()
                        s.disp()
                        s.show()
                        s.showmoney()
                        s.stat()
                        print("__________________________")
                        f = Father()
                        f.show()
                        
                        
                        OUTPUT ___________________________
                        
                        Child CLass Instance method
                        Parent Class Instance method
                        Parent Class Class Method 1000
                        Child class instance method
                        __________________________
                        Parent Class Instance method




# Constructor in Inheritance

By default , The constructor in the parent class is available to the child class


Ex

                        class Father:
                             def __init__(self):
                                  self.money = 2000
                                  print("Father Class Constructor")

                        class Son(Fateher):
                              def disp(self):
                                  print("Son class Instance method:" self.money)

                        s = Son()
                        s.disp()


_____ CODE

constructor la child class madhe call karaychi garj nahi to automatic call hoto

constructor ha automatic call hoto child class mahe


Example:



                        class Father:
                            def __init__(self , m):
                                self.money = m
                                print("Father Class COnstructor")

                            def show(self):
                                print("Father Class Instance Method")


                        class Son(Father):
                            def disp(self):
                                print("Son class instance method")

                        s = Son(1000)
                        print(s.money)
                        s.show()
                        s.disp()
                        
                        
                        OUTPUT ________________________________
                        
                        Father Class COnstructor
                        1000
                        Father Class Instance Method
                        Son class instance method





# Constructor In Inheritance


By default , The Constructor in the parent class is available to the child class

Samja parent class madhe pan constructor ahe ani child class madhe pan constructor ahe


Example : 

                        class Father:
                            def __init__(self):
                                self.money = 1000
                                print("Father CLass Construtor")

                        class Son(Father):
                            def disp(self):
                                print("Son class instance :",self.money)

                        s = Son()
                        s.disp()




# Constructor Overriding 

1 ) if we write constructor in the both class , parent class and child class then the parent class constroctor is not available to the child class
  ( jar parent ani child class madhe donhi kade constructor asel tar parent class cha constructor available child class sathi ) 
  
2 ) in this case only child class construcotr is accessessible which means child class constructor is replacing parent class constroctor
  
  (mhanje jar donhi kade construcotr define kelele asel tar child class cha constructor ha parent class chya constructor la override karto yalacha 
   constructor overriding mhantat ) 
   

Example :

                class Father:
                     def __init__(self):
                        self.money = 2000
                        print("Father Class Constructor")

                     def disp(self):
                        print(self.mondy)

                s = Son()




Example : 


                class Father:
                    def __init__(self):
                        self.money = 1000

                        print("Father Class Constructor")


                    def show(self):
                        print("Father class instance method")


                class Son(Father):

                    def __init__(self):
                        self.money = 5000
                        self.car = "BWM"
                        print("Son CLass Constructor")

                    def disp(self):
                        print("Son class Instance method")


                #______________________
                s = Son()
                print(s.money)
                print(s.car)
                s.disp()
                s.show()

                # ___________ Pan hya condition madhe
                print("____________________")
                f = Father()
                print(f.money)
                                                
                                                
                                                
                                                
                                                
# how can we call parent class constructor



class Father:
    def __init__(self):
        self.mondy = 2000
        print("Father Class Constructor")
        
        
class Son(Father):
    def __init__(self):
        self.money = 5000
        print("Son Class Constructor")
    
    def disp(self):
        print(self.money)
        
s = Son()
s.disp()


# constructor with super() method

1 ) if we write constroctor in the both classes , parent class and child class then the parent class constructor is not available to the child class
2 ) in this case only child class constructor is accessible which means child class constructor is replacing parent class constructor

3 ) super() method is used to call parent class constructor or methods from the child class
                                                
                                                
( super cha use karto parent class chya constructor use (call ) karnya sathi )                                                 
                                                
                                                
                                                
 Eample :
 
 
                 class Father:
                    def __init__(self):
                        print("Father class constructor")

                    def show(self):
                        print("Father class Instance method")



                class Son(Father):
                    def __init__(self):
                        super().__init__()  # this is calling parent class constructor
                        print("Son class constructor")

                    def disp(self):
                        print("Son class instance method")

                s = Son()
                            
                            
                OUTPUT : 
                
                Father class constructor
                Son class constructor
                                                
                                                
                                                
# Multi-Level Inheritance

in Multi-level inheritance , the class inherits the feature of another derived class ( Child Class ) 

multi leval mhanje  aka class adun inherit houn dusrya class dusrya class madhun inherit houn disrya class kade inherits hoto

A class
  |
B class
  |
C Class



Syntax -

                class ParentClassName:
                     member of parent class


                class ChildClassName(ParentClassName):
                     members of child class


                class GrandChildClassName(ChilclassName):
                      memberf of grand child class


      



Example 1 

                        class Father(object):
                             members of class Father

                        class Son(Father):
                             members of class son

                        class GrandSon(Son):
                             members of class grandSon




Example :



                        class Father:

                            def showF(self):
                                print("Father Class method")

                        class Son(Father):

                            def showS(self):
                                print("Son Class method")

                        class GrandSon(Son):

                            def showG(self):
                                print("Son Class method")  


                        g = GrandSon()        

                        g.showG()
                        g.showF()
                        g.showS()
                        
                        
                        
                        OUTPUT _____________________________
                        
                        Son Class method
                        Father Class method
                        Son Class method




# Hierarchical Inheritance

Ya madhe aka class madhun multiple child class banvto tyala hierachical inheritance mhantat

Syntax :-


                class ParentClasssName(object):
                      members of parent class

                class ChildClassName1(ParentClassName):
                      members of child class 1

                class ChildClassName2(ParentClassName):
                      members of child class 2



Example :


                class Father(object):
                     members of class father

                class Son(Father):
                     members of class son

                class Daugher(Father):
                     members of class Daughter






Example :

yamdhe daughter ani son akmekanche member variable acces karu shakta nahi




                class Father:
                    def showF(self):
                        print("Father Class Method")

                class Son(Father):
                    def showS(self):
                        print("Son Class Method")       



                class Daughter(Father):
                    def showD(self):
                        print("Dougher Class Method")      

                s = Son()
                s.showS()
                s.showF()
                
                
                OUTPUT ______________________
                
                Son Class Method
                Father Class Method




_______________________ Using Constructor _____________
Example :



                        class Father:
                            def __init__(self):
                                print("Father class constructor")

                            def showF(self):
                                print("Father Class Method")

                        class Son(Father):
                            def __init__(self):
                                print("Son class constructor")

                            def showS(self):
                                print("Son Class Method")       


                        class Daughter(Father):
                            def __init__(self):
                                print("Daughter class constructor")

                            def showD(self):
                                print("Dougher Class Method")      

                        s = Daughter()
                        
                        
                        OUTPUT _____________________
                        Daughter class constructor
                        
                        



# Polymorphism



Ploymorphism is a word that came from two greek words, poly means many and morphos means forms

if a variable , object or method perform different behavior according to situation, it is called polymorphism

1 ) Duck typing
2 ) Operator overloading
3 ) Method overloading
4 ) Method overriding



# Dock Typing

In python , we follow a principle - if 'it walks like a duck and talks like a duck , it must be a duck ' which means python doesn't care about
which class of object it is , if it is an object and required behavior is present for that object then it will work . The type of object is distinguished only at runtime . This is called as duck typing .

python doesn't care about which class of object it is , in order to call an existing method on an object . if the method is defined on the object then 
it will be called  .

Marathi:

        Ya madhe he nahi pahila jat ki jo object ahe to kuthlya type cha ahe konchay class cha ahe to phakt hech pahato jo miltoy to object ahe method 
        ahe ani yala phakata call karaychay 
        
        jar kuthla dog ahe ani to duck sarkah chaltoy tari tyala dock mhanun consider kar , 
        
        Ya madhe yala pharak padat nahi ki ha konta object ahe jar yala akhada object bhetla tar to yala direct call karto
        
        
 Example : 
         
                 class Duck:
                    def walk(self):
                        print("Thapak Thapak Thapak ")

                class Horse:
                    def walk(self):
                        print("Tabdak Tabdak Tabdak Tabdak ")

                def myfunction(obj):
                    obj.walk()

                d = Duck()
                myfunction(d)   
                
                
                OUTPUT  : Thapak Thapak Thapak 
                


Example  :-2


                        class Duck:
                            def walk(self):
                                print("Thapak Thapak Thapak ")

                        class Horse:
                            def walk(self):
                                print("Tabdak Tabdak Tabdak Tabdak ")


                        class Cat:
                            def talk(self):
                                print("Meow Meow")


                        def myfunction(obj):
                            obj.walk()

                        d = Duck()
                        myfunction(d)

                        h = Horse()
                        myfunction(h)

                        c  = Cat()
                        myfunction(c)

                        (This class will show error )
                        __________________________________________________________________________
                        
                        Thapak Thapak Thapak 
                        Tabdak Tabdak Tabdak Tabdak
                        
                        AttributeError: 'Cat' object has no attribute 'walk'


        

# Strong Typing

We Can check wether the object passed to the method has the method being invoked or not

hasattr() function is uesed to check wether the object has a method or not 

Syntax :- hasattr(object , attribute)

Where attribute can be a method or variable . if it is found in the object then this method return True else False



Example :



                                class Duck:
                                    def walk(self):
                                        print("Thapak Thapak Thapak ")

                                class Horse:
                                    def walk(self):
                                        print("Tabdak Tabdak Tabdak Tabdak ")

                                class Cat:
                                    def talk(self):
                                        print("Meow Meow")

                                def myfunction(obj):
                                    if hasattr(obj , "walk"): # yane kay hoil jar fun madhe walk asel tar tarach execute hoil otherwise error print hoil
                                        obj.walk()
                                    else:
                                        print("Error")


                                d = Duck()
                                myfunction(d)

                                h = Horse()
                                myfunction(h)

                                c = Cat()
                                myfunction(c)
                                
                                OUTPUT ____________________________
                                
                                Thapak Thapak Thapak 
                                Tabdak Tabdak Tabdak Tabdak 
                                Error





# Method Overloading

( Python madhe method overloading nahiye pan apan yala achive karu shakot code madhe cahnges karun ) 


When more thatn one method with the same name is defined in the same class . it is known as method overloading . 
in python , if a method is written such it can perform more than one task , it is called method overloading . 

Example :- 


# method Overloading

                        class Myclass:
                            def sum(self , a=None, b=None , c=None):
                                if a!=None and b != None and c !=None:
                                    s = a + b + c

                                elif a != None and b !=None:
                                    s = a + b

                                elif a != None:
                                    s = a
                                else:
                                    s = "Invalid"
                                return s

                        obj = Myclass()
                        print(obj.sum(2 , 19,29,))
                        
                        
                        
                        OUTPUT _____ : 50
                        
                        

# method Overriding

if we write method in the both classes , parent class and child class then the parent class method is not avaiable to the child class
in this case only child class method is accessible which means child class method is replacing parent class method

method overriding is used wehn programmer want to modify the existing behavior of a method


Example : 

                        class Myclass:
                            def sum(self , a=None, b=None , c=None):
                                if a!=None and b != None and c !=None:
                                    s = a + b + c

                                elif a != None and b !=None:
                                    s = a + b

                                elif a != None:
                                    s = a
                                else:
                                    s = "Invalid"
                                return s

                        obj = Myclass()
                        print(obj.sum(2 , 19,29,))




# Operator Overloading

if any operator performs additional actions other than what it is meant for , it is called operator overloading

Marathi : 
        yamadhe akhadya operator ( + , * , / ) la apla akhada work assign karu shakatoy 
        
        kuthlahi add , sub , multi  karaycha aslyavar apan magic method la call hoto ( print(dir(int)) ) ( 
        
        
        



Example : 

                        class A:
                            def __init__(self ,x):
                                self.x = x

                            def __add__(self , other):
                                return self.x + other.x

                        class B:
                            def __init__(self ,x):
                                self.x = x

                        a  = A(100)
                        b  = B(100)

                        print(a + b)


# module 

A module is a file containing python definitation and statements .
A module is a file containing group of variables , method , function and classes 
They are executed only the first time the module name is encountered in an import statemnt
the file name is the module name with the suffix .py appended

Ex :- mymodule.py

_______________________________________________________________
Type of Module
_______________________________________________________________


1 ) User-defined Modules
2 ) Build-n Modules

        Ex :- array , math , numpy , sys





# Abstract class VS Interface

1 ) an abstract class can have abstract methods as well as concrete methods , but all methods of an interface are abstract
2 ) we use abstract class when there are some common feature shared by all the objects as they are while we use interface if all the feature need to be iimplemented differently for different objects
3 ) its programmer job to write child class for abstract class while in interface any third party vendor will take responsibility to write child class
4 ) Interface are slow when compared to abstract class



Marathi :
        hi programmer chi responsibility aste jo abstract class banavto to child class bhi banavto 





# Multithereading in python

**Multi-tasking in python**

 Executing multiple task at the same time

 **Type**

 **1 ) Process based multi- tasking**
         1 ) Each task is an independent program/process
         2 ) Used in os level

        process based multi tasking madhe akach veles multiple application open astat example akach veles pycharm, chrome, team ase application open astat

**2 ) Thread based multi-tasking**
        Each task is an independent thread ( Separate part of program )
        used in programmatic level 

        yamadhe akach application asta akach program asto ani tya program che part astat (exmaple calculator madhe addition, multipication, subtraction ) 
        
        

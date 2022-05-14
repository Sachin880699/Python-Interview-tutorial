# Python-Interview-tutorial


# String slicing
 
 string slicing means select range of element from string 
 example suppose i have string sachin i want to print s to i so how i will print
 
 it will skip last element
 
name = "sachin"<br/>
print(name[0:5])



# step Argument

स्टेप arugument  म्हणजे first  पॅरामीटर print  झालयावर किती स्टेप घ्यायच्यात

name = "sachin"<br/>
print(name[0:5:2])

Output : sci


# what is strip method

strip method  चा वापर truncates  किंवा remove  करण्यासाठी होतो

लेफ्ट side  चा space  remove करण्यासाठी आपण name.lstrip() method  use करतो . आणि right  side  चा space remove करण्यासाठी आपण name.rstrip() वापरतो

name = "    sachin     "<br/>
print(name.lstrip())


# string is a immutable
string is a immutable  कारण आपण main  string चांगले करू शकत नाही

example 
असा केला तर error  येईन 

a = "sachin"<br/>
a[0]="S"


पण जर आपण replace  केला तर तो नवी string बनवतो पण आधीची तशीच ठेवतो

a = "sachin"<br/>
print(a.replace("s","a"))<br/>
print(a)

OUTPUT : aachin
         sachin
         
         
# what is pass statement

pass  स्टेटमेंट चा वापर आपण तेव्हा करतो when we don't want to aything

for i in range(1 , 10):<br/>
    if i == 5:<br/>
        pass
        
 # Default Parameter
 
 default  parameter  म्हणजे जे कि आपण function  मध्ये value  pass  करतो . आणि जर function ला call केलं आणि value pass केल्या तर तो default value ला over  right  करतो 
 
 def name(name = "sachin" , age  = 24): <br/>
    print(name , age)<br/>
    
    
name("sachinpaar",20)


# what is list
list is a order collections of item 

append method element हा प्रत्येक वेळेस last  ला add करील
a = ["sachin",'pawar'] <br/>
a.append("10")<br/>
print(a)<br/>


# insert method in list

या मध्ये आपण element position ला add करू शकतो


a = ["sachin",'pawar'] <br/>
a.insert(1,"10") <br/>
print(a) <br/>

OUTPUT : ['sachin', '10', 'pawar']

# pop method 

pop method list मधून लास्ट element ला delete करिन जर कुठलाच argument पास नाही केला तर

a = ["sachin",'pawar','sagar'] <br/>
a.pop()<br/>
print(a)<br/>

OUTPUT : ['sachin', 'pawar']


आणि जर index नंबर पास केला तर त्या position चा element delete करिन

a = ["sachin",'pawar','sagar'] <br/> 
a.pop(1) <br/>
print(a) <br/>

OUTPUT : ['sachin', 'sagar']

# Split method

split method string ला list मध्ये convert करतो

a = "sachin pawar" <br/>
print(a.split(" "))


# Join method

join वापर होतो list join करायला 

a = ["sachin","pawar"] <br/> 
print(" , ".join(a)) <br/>


OUPTUT : sachin , pawar



# List VS Array

list or array means order collections of item

array मध्ये एकाच type चा data store करू शकतो म्हणजे int , किंवा string 

list madhe store any data


# List VS string

1 ) String are immutable means we cant change 
2 ) list are mutable means original element la change karto 


# what is list comprehension in python

creating lists based on existing iterables <br/>

list1 = [1,43,53,65,32,74,23] <br/>
l = [i for i in list1 if i % 2 == 0] <br/>
print(l)

OUTPUT : [32, 74]



# What is tuple 
tuple is emmutable means we cant change in data . we cant append  , remove . tuple आपण तेव्हा असे करतो जेव्हा आपल्याला माहित असता कि आपला डेटा चांगले नाही होणार
emaple . day's tuple त्यासाठी असे करतो कि तो faster आहे list पेक्षा

method we can use in tuple 1 ) count 2 ) index 3 ) length 4 ) slicing


# What is list comprehension
comprehension means आपण list  ला एक line मध्ये create करू शकतो
Example.

fruits = ["apple", "banana", "cherry", "kiwi", "mango"]<br/>

newlist = [x for x in fruits if "a" in x]<br/>

print(newlist)<br/>

creating a list based on existing lists . it is faster than for loop

Example 2 :
   
   fruits = ["apple", "banana", "cherry", "kiwi", "mango"]<br/>

   newlist = [x for x in fruits if x != "apple"]<br/>

   print(newlist)<br/>
   
   OUTPUT : ['banana', 'cherry', 'kiwi', 'mango']
   
# dictionary comprehension 

dictionary comprenhension is a method for transforming one dictionary into another dictionary

keys = ['a','b','c','d','e']<br/>
values = [1,2,3,4,5]  <br/>
myDict = { k:v for (k,v) in zip(keys, values)}  <br/>
print (myDict)<br/>


Output : {'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': 5}

# what is *args

means म्हणजे आपण multiple value pass करू शकतो function ला

def ok(*args):<br/>
    print(type(args))<br/>
ok("sachin",'pawar','application')<br/>

OUTPUT: ('sachin', 'pawar', 'application')

Output he tuple madhe yein


Example 2 :
 
 def total_num(*args):<br/>
    count = 0<br/>
    for res in args:<br/>
        count = count + res<br/>
    print(count)<br/>
total_num(1,2,3,4,5)<br/>




































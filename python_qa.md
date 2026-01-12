***<---------does class by default inherits or extends some other class in python "?--------->***

In Python 3, every class, by default, implicitly inherits from the built-in object class. This means that if you define a class without explicitly specifying a base class, it will automatically inherit from ```object```. 
```
For example: 
class MyClass:
    pass
```

In this case, MyClass implicitly inherits from object. You can verify this by 
checking the __bases__ attribute of the class: 
```
print(MyClass.__bases__)
```

This will output ```(&lt;class 'object'&gt;,)```, confirming that object is its base class. 
This behavior ensures that all classes in Python have a common set of fundamental methods and attributes, such as ```__init_```_, _```_str__```, ```__repr__```, and more, providing a consistent foundation for object-oriented programming. 


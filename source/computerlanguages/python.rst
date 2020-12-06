Python Programming Language
===========================

Basic commands
--------------

Define a variable
*****************

Basic operations on a variable
******************************

Boolean Operation
*****************

Conditional Statements
**********************

Loops
*****

Structural Data
***************

Functions and Procedures
************************

Organization of the code
------------------------

In my perspective, I think the organization of the code in python can be simplified in three parts: (1) class (2) module (3) package

Classes
*******

Python is an object-oriented programming language, which basically means you can define class in it. A class is an abstraction of an object, and we can use the :code:`init` method in the class to initialize an object, then we can use it later.

A typical python class will have the following structures: (I use a person as a class because this is very simple)

.. code-block:: python

    class Person():

        def __init__(self, name, age, job):
            self.name = name
            self.age = age
            self.job = job

        def get_name(self):
            return self.name

        def set_name(self, name):
            self.name = name

        def get_age(self):
            return self.age

        def set_age(self, age):
            self.age = age

        def get_job(self):
            return self.job

        def set_job(self, job):
            self.job = job

        def sayhello(self, name):
            print('Hello, {}, my name is {}'.format(name, self.name))

Let's analyze this code, and it will serve as a template for creating more classes in the future.

First we need to define the name of the class, in this case the name of our class is :code:`Person`, and since we do not have parent class, so there is nothing in the :code:`()`. But if we want to inherit from other class, then we need to put the name of that class in :code:`()`, such as: :code:`class Person(Animal)`. The convenience of inheritance is that we do not need to implement the same properties and methods again, we can just inherit from the :code:`Animal` class. The parent class can be accessed by using :code:`super()` as the parent class, so that we can use the :code:`__init__` method from the paraent class: :code:`super().__init__()`. If you write the same function in the child class, the function in parent class will be overrided.

Then the :code:`__init__` function is the most important function, because it tells python how to build the object, what parameters we need for creating such object. In python, each class properties will need to set a setter and a getter, means that there will be a function called :code:`set_propertyName()` and :code:`get_propertyName()`, because in this way we can protect the properties.

The variable :code:`self` is really powerful in the class, because it is a representation of the object, we can access to all the properties and methods just by using :code:`self` as a normal object. And if we assign a new properties in the method (:code:`self.propertyName`), then it can be accessed within the class. So in this way we can create very complex classes and let python do many different things.

After defining all the properties, now we can make our object do things, this is really interesting part because in OOP we are like HR (human resources), we need to assign tasks to different classes (such as different jobs in a company) in order to make them work together. So by thinking in this way we will improve our ability to organize people. If a programmer can organize information and people together, they can become billinaire (e.g. Bill Gates).

In python there are three types of methods: (1) class method (2) static method (3) common method. The `class method` can only be accessed by the class itself, it cannot be accessed by the object created by the class. The `class method` can modify the properties. The `static method` cannot modify the state of the class or the object, because it doesn't have implicit input parameter :code:`cls`. Usually one can use class method as a *factory* methods, which mean it will returns a class object, but with different initialization function. One can also use static method to create utility methods, for example to check the status of the class or the object.

We can define our class methods and static methods in the following way:

.. code-block:: Python

    @classmethod
    def nameOfClassMethod(cls, para1, para2, .....) # cls means `class`, so if we want to create an object, we can use cls(), which is the same in other places.

    @staticmethod
    def nameOfStaticMethod(para1, para2) # no cls reference here.

You may want to ask: **why do we need to use class?** Can't we do everything by writing functions and just use them? Why do we need to combine properties and methods together and call it a *class*? Well, there are many reasons for that. But remember, every decision, every paradiagm in programming tends to do one thing --- make the reusability of the code higher and easier to maintainance. OOP is a great way for doing that, and it becomes a major programming paradiagm these days. But when you want to solve a problem, OOP is not the only solution, you can write functions, and it may works well, but you will face a point where your functions are just disorganized, and the reusability is terrible. If you think in OOP, then it will be ok, because it just a paradiagm for programming, it doesn't solve anything, just make you think clearer and become more effective.

Modules
*******

In Python, modules are very important in organizing your code. In simple term, a folder which contains :code:`__init__.py` file is considered as a :code:`module`. A python file is also considered as a module, and all the variables and functions in that file is referrable, means you can reuse them in other python codes.

Let's say we have a folder called :code:`hzd_module`, in side this folder we have a python file called :code:`func.py`, which contains two functions: :code:`func1` and :code:`func2`, and also a file called :code:`__init__.py`. Now if we want to create another file :code:`additionalfunc.py`, then I want to use the :code:`func1` in :code:`func.py`, I can write this statement in the beginning of :code:`additionalfunc` in order to refer to the :code:`func1`:

.. code-block:: Python

    from hzd_module.func import func1

Now I can use :code:`func1` in the new python file.

Packages
********

Once we have written many classes and modules, we can bundle them into a package. For example, the package that I wrote for my own research is called :code:`hzdplugins`. You can upload your python package to either PyPI or Conda. Both are very good python package distributors.

When you want to create a python package, you need certain structures. This can be easily found online.

Language sugar
--------------

Language sugar is used to describe some features in a language that can help you write cleaner and more understandable codes.

Decorators
**********

Decorator is a very powerful tool in Python. It is just a function that can modify the behavior of another function.

Let's say that we have a function called :code:`div(a, b)`, which will return the value of :code:`a/b`. If :code:`b!=0`, then we are Ok,
but if :code:`b=0`, then we will meet some trouble. Now let's ask ourselves another questions: How can we check whether b=0 without changing the function :code:`div(a,b)`? You might ask: why do we need to do this? Sometimes when we try to modifiy different functions the same way, the decorator is a really useful tool for doing that.

First let's define the :code:`div(a,b)` function:

.. code-block:: Python

   def div(a,b):
       return a/b

Then we create another function, which input parameter is a function, we call it :code:`func`:

.. code-block:: Python

   def check(func):
       def inside(a, b):
           if b = 0:
               print('You cannot divide 0.')
           else:
               func(a, b)
       return inside

Now we have this :code:`check` function which require a function as input, and also return another function that has different behavior as the original one, in this way we can modify the original function (which is :code:`div`) without actually changing its original code. This is a very useful technique in python programming.

If we want to modify :code:`div`, usually we can do something like this:

.. code-block:: Python

    div = check(div)

Now div has the same functionality as the :code:`inside` function. But there is a simpler way for doing this, just put `@check` above the :code:`def div`, then you have the same effect. So now our code becomes cleaner.

.. code-block:: Python

    @check
    def div(a, b):
        return a/b

So from the introduction above, we will know that:

* A decorator is a function, which take a function as an input, and return a modified function.
* The most important part of a decorator function is in the :code:`inside` function, which will provide most features in the decorator.
* The basic structure of decorator is universal, all we need to modify is the :code:`inside` function.

What can we learn from the decorator syntax? Programming is just trying to find the most efficient way of doing things, there are infinite way for doing the same thing, but they have different complexities and aesthetic, we want to do things fast, efficiently, and also beautifully.


External packages
-----------------

* **numpy**: A package for dealing with arrays in python
* **scipy**:

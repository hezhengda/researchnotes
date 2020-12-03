C Programming Language
======================

The C programming language was originally developed by Dennis Ritchie at Bell Labs between 1972 and 1973 (For more details, you can visit the `wikipedia <https://en.wikipedia.org/wiki/C_(programming_language)>`_). It is a compiled language, which means that in order to run the program written by C, we need to compile it first into an executable (or binary file), then run it as a program. So before we write any C code, we need to things: (1) an edit for writing our C code (2) a compiler for compiling the C code and turn them to an executable. The most common compiler is `gcc <https://gcc.gnu.org/>`_, which can be easily download from the website.

In C code, you can use `//` for commenting in a single line; and you can use `/* ... */` to comment in multiple line.

Basic commands
--------------

Define a variable
*****************

When you define a variable in C, you need to explicity state the type of that variable.

For example if you want to define an integer, you can do: `int x;` It is also important to know that in C language, the semicolon `;` is very important, this symbol means the end of a statement. Withouth `;`, the compiler will complain.

There are other types of variables that you can define in C, for the common ones, I list them in the following:

* `int thisIsInteger = 1;`
* `double thisIsDouble = 1.23;`
* `char thisIsChar = 'a';`
* `float thisIsFloat = 1.3;`
* `long thisIsLong = 143000;`
* `long double thisIsLongDouble = 2222222.522;`
* `bool thisIsBoolean = true`

Sometimes, a variable can be a constant, which means once its value has been assigned, you cannot change the value. In C, you can define a constant variable like this: `const int thisIsConstantInteger = 5;`

Array
#####

If we want to store a series of data, then we need to use array. In C, it is really easy to define an array variable.

For a integer array, it can be defined in this way: `int thisIsIntegerArray[10]`, which means it will have 10 elements, id from 0 to 9. Each element can be accessed by `array[i]`, where `i` is the id of the element.

If we want to create a **multi-dimensional array**, we can do it in this way:

`int thisIsMultiDimensionalIntArray[10][10]`

Pointer
#######

The **Pointer** type in C is a powerful type. It stores the address of a variable, for example if I have a variable called `var`, then `&var` means the address of the variable `var`. Each type of variables has its own type of pointers, for example if I want to define a pointer that can point to a float number, then I can do it in this way: `float* thisIsAPointerForAFloatVariable`.

The pointer can also have its own pointer: `float**`, it may be useful, but it can cause confusion really quickly.

Struct
######

Sometimes when we want to describe a things, for example a human being. We need many different types of data related to that human being. Like name, age, height, sex, etc. So we need to put all these information together in order to access them quickly and efficiently. We can do that easily in C by using the `struct` type.

In here we define a `struct` variable named `Person`:

.. code-block:: C

    struct Person
    {
        char name[50];
        int socialSecurityNumber;
        float salary;
    }


Then we can define a `Person` variable in this way: `struct Person person1, person2`. And we can access the information of each person by this statement: `person1.name`, `person2.salary`.

In C, we can also create a *nesting* struct, which means inside a struct there is another struct, but we can decide whether to do it or not in real examples.

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

External packages
-----------------

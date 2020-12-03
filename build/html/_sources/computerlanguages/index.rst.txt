Computer Languages for Scientific Computing
===========================================

Scientific computing is very different from other types of computing (such as graphical design, gaming or making movies), because in scientific computing, we usually deal with the cluster, the parallel computing environment, and also the softwares and codes that we are using are written by scientists, not professional computer programmers. So we don't usually have very restrict formality or habits of minds.

In my opinion, if you want to become a great computational scientist, you should know the languages in below:

* General-Purpose Programming Languages
    * C & C++ (for computational intensive tasks)
    * Fortran (for computational intensive tasks)
    * Python (for the task management and the 'glue' of everything)

* Domain-Purpose Programming Languages
    * Matlab
    * Mathematica
    * R

* Database-Related Programming Language
    * SQL

Also the parallel programming is also very important, MPI and OpenMPI are very important if you want to write your own code and run it on the supercomputer cluster.

Since I am very lazy in remembering all the details about different languages, I thought it would a great idea to summarize them here, and if I need anything, I can just look it up.

But we have to remember, learning to program is not learning new languages. Programming is thinking, is solving problems. Once you have the algorithm and corresponding data structure, you can code it with any languages. The difference will be in the details, performance and capability of different languages. **Programming = Algorithm + Data Structure**. If you don't have the algorithm, don't try to learn any new language, because that's meaningless. Also when you learn a new language, don't just learn the grammer (you can check them up in this website), learn the idea behind that language, learn a different way of thinking, that's more important in programming.

When you want to learn a new programming languages, the things below are really important:

* Basic commands:
    * Define a variable
    * Basic operation on the variable
    * Boolean operation
    * Conditional statements (if, switch)
    * Loops (for, while, do...while)
    * Structural Data
    * Functions and Procedures

* Organization of the code:
    * Modules
    * Class (Object-Oriented Programming): The OOP is just a way to organize the data (variables and functions), nothing fancy. You can do all the things in procedural programming, but when you want to scale your program, it becomes harder to do.

* External packages
    * pip for python
    * STL for C++

So in this tutorial, I will introduce each langauge as the same procedure above. First we will talk about the basic commands in the langauge, we will give several examples (very simple examples), then we will move on to the organization of larger programs, once we have finished that, we can move on to the more advanced topics, like how to use external packages, and start doing real-world problems. But for more important packages, we will put them in the software section. Also I think when we learn a programming language, we don't really need to remember all the "strange" usage of the language, it just like when we learn english, we don't need try to become Shakespeare, we just need to learn how to communicate with others effectively, so don't put too much attention on the things that you will not use them later. Stay on the fundamentals, because you will use them again and again in the future.

.. toctree::
   :maxdepth: 1
   :caption: General-Purpose Programming Languages:

   c
   c++
   python
   fortran

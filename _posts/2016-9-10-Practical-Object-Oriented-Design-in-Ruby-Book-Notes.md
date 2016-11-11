---
layout: post
title: Practical Object Oriented Design in Ruby Book Notes
---

* The programming techniques that make code a joy to write overlap with those that most efficiently produce software

* The techniques of object-oriented design solve both the moral and the technical dilemmas of programming; following them produces cost- effective software using code that is also a pleasure to work on.

* Design does not matter unless there will be change to the application - but there is always change to the application

* Object-oriented design is about managing dependencies. It is a set of coding techniques that arrange dependencies such that objects can tolerate change.

* Dependent objects would wreak havoc because changing one object forces change upon another, leading another object to change etc.

* You must not only write code for the feature you plan to deliver today, you must also create code that is amenable to being changed later.

* Practical design does not anticipate what will happen to your application, it merely accepts that something will and that, in the present, you cannot know what. It doesn’t guess the future; it preserves your options for accommodating the future. It doesn’t choose; it leaves you room to move.

* The purpose of design is to allow you to do design later and its primary goal is to reduce the cost of change.

<!-- 
* **SOLID**
  * Single Responsibility
  * Open-closed
  * Liskov Subtitution
  * Interface Segregation
  * Dependency Inversion -->

* Studies were done to measure code quality, and indeed these design principles lead to high quality code 

* The Agile experience is that this collaboration produces software that differs from what was initially imagined; the resulting software could not have been anticipated by any other means.

* BUFD stands for Big Up Front Design

* Because Agile processes guarantee change, the ability to make changes to the code is necessary. Otherwise, everytime you would need to rewrite a bunch of code.

* SLOC (source lines of code) to measure programmer contribution says nothing about overall equality

* Object-oriented programming is object-oriented relative to non object-oriented, or procedural, programming.

* Class-based OO languages like Ruby allow you to define a class that provides a blueprint for the construction of similar objects. 

* The trick to getting the most bang for your design buck is to acquire an understanding of the theories of design and to apply these theories appropriately, at the right time, and in the right amounts. Design relies on your ability to translate theory into practice.

## Designing Classes with a Single Responsibility

* The classes you create define a virtual world, and will affect how you think about your application forever.

* Code should be **TRUE**
  * Transparent
    * The consequences of change should be obvious in the code that is changing and in distant code that relies upon it
  * Reasonable
    * The cost of any change should be proportional to the benefits the change achieves
  * Usable
    * Existing code should be usable in new and unexpected contexts
  * Exemplary
    * The code itself should encourage those who change it to perpetuate
    these qualities

* **A class should do the smallest possible useful thing; that is, it should have a single responsibility.**

* A class that has more than one responsibility is difficult to reuse. The various responsibilities are likely thoroughly entangled within the class. If you want to reuse some (but not all) of its behavior, it is impossible to get at only the parts you need. You are faced with two options and neither is particularly appealing.

* How can you determine if the Gear class contains behavior that belongs somewhere else? 
  * **One way is to pretend that it’s sentient (act as if it has feelings) and to interrogate it.**
    * For example, “Please Mr. Gear, what is your ratio?” seems perfectly reasonable, while “Please Mr. Gear, what are your gear_inches?” is on shaky ground, and “Please Mr. Gear, what is your tire (size)?” is just downright ridiculous.

  * **Another way is to attempt to describe it in one sentence**
    * If the explanation requires the use of words such as `and` or `or`, then it most likely has more than one responsibility.

* **When everything in a class is related to its central purpose, the class is said to be highly cohesive or to have a single responsibility.**

* When faced with an imperfect and muddled class like Gear, ask yourself: “What is the future cost of doing nothing today?”


* This “improve it now” versus “improve it later” tension always exists. Applications are never perfectly designed. Every choice has a price. A good designer understands this tension and minimizes costs by making informed tradeoffs between the needs of the present and the possibilities of the future.

* Methods should also only have one single responsibility

* If code inside a method needs a comment, extract that into a seperate method - the method name will serve the same purpose as the old comment.

* Small methods encourage reuse

* If you have a muddled class with too many responsibilities, seperate those responsibilities into different classes.

* An object has a dependency when it knows

* A class should know just enough to do its job and not one thing more

* **Dependency Injection** - moving an explicit dependency to an implicit one.

* When a message requires arguments, not only can you not avoid having knowledge of those arguments, you also require that those arguments be passed in a specific fixed order.
  
  * To avoid this, initialize the arguments using a hash

* Use the fetch method to set defaults instead of using ||

* Depend on things that change less often than you do 

* When a class reveals al, it causes difficult to reuse objects

* Comparison of classes vs kitchens - the class exists to fulfill a single responsibility but implements many methods

* The context that an object expects has a direct effect on how difficult it is to reuse.

* Distinction between what **what** and **how**

* Blind trust is a keystone of object-oriented design. It allows objects to collaborate without binding themselves to context and is necessary in any appliation that expects to gow and change

* Duck types are public interfaces that are not tied to any specific class

* Inheritance at its core is a mechanism for automatic message delegation
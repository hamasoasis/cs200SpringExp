---
title: "Design Principles"
teaching: 0
exercises: 0
questions:
- "What are common design principles, and how do they differ for
research software"
objectives:
- "To introduce common design principles"
keypoints:
- "Design principles abound -- they need to be adapted for research software"
---

# General Design Principles Found on the web

* Encapsulate what varies
* Favor composition over inheritance
* Program to interfaces not implementations
* Loose coupling – interacting components should have minimal knowledge about each other
* SOLID
  * Single responsibility
    * Class/method/function should do only one thing
  * Open/closed
    * Open for extension\, close for modification
  * Liskov substitution
    * Implementations of an interface should give same result
  * Interface segregation
    * Client should not have to use methods it does not need
  * Dependency inversion
    * High level modules should not depend on low level modules\, only on abstractions

# Some Research Software Specific Challenges

![](img/loop.png)

Many parts of the model and software system can be under research

Requirements change throughout the lifecycle as knowledge grows

Verification complicated by floating point representation

Real world is messy

# SOLID Principles Pose Some Difficulties

![](img/solid.png)

# Additional Considerations for Research Software

![](img/RS.png)


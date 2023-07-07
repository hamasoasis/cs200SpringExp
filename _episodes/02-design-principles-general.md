---
title: "General Design Principles"
teaching: 0
exercises: 0
questions:
- "What are most commonly used design principles?"
objectives:
- "To introduce common design principles"
keypoints:
- " "
---

# General Design Principles

## Found on the web

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
    * High level modules should not depend on low level modules\, only
    on abstractions

These are just some of the links that have more details on these
design principles.

https://www.freecodecamp.org/news/solid-design-principles-in-software-development/
https://www.bmc.com/blogs/solid-design-principles/
https://bootcamp.uxdesign.cc/software-design-principles-every-developers-should-know-23d24735518e


# Designing Software – High Level Phases

![](img/elements.png)

As shown in the figure above, software design has three phases:
requirements gathering, decomposition, and understanding connectivity
with that decomposition. These phases come one after another, though
one can iterate over them as needed.

# Requirements Gathering

In the requirement gathering phase the developers gather information
about what is needed from the software. As an extremely simple example
is writing an integer sorter. At first glance it appears that only
requirement is to read in a bunch of integers, sort them in specified
order, and output the sorted numbers. However a few other requirements
may dictate actual implementation. For example:

* How large is the dataset -- will simplest O(N<sup>2</sup>) method
suffice or does one need O(NlogN) method
* Is the dataset large enough that a parallel sorting algorithm needed
* Is it a stand-alone code or is it going to be used in another code
(in other words does it handle I/O or needs to have arguments)
   * If it needs I/O then what is format of input and output files
   * If it has arguments what should the interface look like

It may seem like an obvious approach to take, but if you think a
little about what is involved you will realize that failing to get
these specifications will likely accrue some technical debt which will
have to be paid later through modifications in the code.

# Decomposition

Once requirements are known one can proceed to design components of
the software. In the sorting example the simplest case of small
dataset to sorted through a function call will have just one
components. If it is a stand-alone piece of software then it may be
divided into three components, one for I/O, one for sorting, and the
driver that invokes the other two components. If it is
a stand-alone parallel sorter then it may either incorporate
parallelization in the driver, or may add another component to handle
the parallelization.

#Connectivity

This phase of design is devoted to understanding the interdependencies
between components. In the stand-alone parallel sorting example with a
separate parallelization component we infer the following
connectivity:

* Driver knows all other components and invokes them as needed
* I/O is called by the Driver and no other component. If parallel I/O
  is being used then it needs to have an interface with the
  parallelization component
* Sorter is called by the driver, but it also needs access to the
parallelization component
* Parallelization component is called by the driver and the sorter. It
may also be called by I/O if we are using parallel I/O

One immediate concern that you may have is that this approach is not
compatible with agile methodology. It is not because complete design of a
complex software may need several iterations over the three phases. It
need not all happen before development begins. It can happen anytime
during the development cycle, and in all probability some or all of
the phases may need to be reconsidered as understanding grows.

In the next section we will work through a real life application design.



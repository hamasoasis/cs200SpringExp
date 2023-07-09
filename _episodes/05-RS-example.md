---
title: "Research Software Design Example"
teaching: 0
exercises: 0
questions:
- ""
objectives:
- "To work through an example of research software design"
keypoints:
- "More complex requirements gathering and connectivity and
-implications on design"
---

# More Complex Application Design – Sedov Blast Wave

__Description__

Domain is initialized with pressure spike at the center of the domain.
The resulting shock moves out along the radial direction as an
expanding circle in two dimension, or a sphere in three
dimension. High resolution is needed only at and near the shock, so it
is good to use adaptive mesh refinement.

* __Requirements __
* Adaptive mesh refinement
  * Easiest with finite volume methods
* Driver
* I/O
* Initial condition
* Boundary condition
* Shock Hydrodynamics solver
* Ideal gas equation of state
* Method of verification

![](img/sedov.png)

# Deeper Dive into Requirements

Adaptive mesh refinement (AMR) implies that the spacing between discretized
points varies in different sections of the domain. AMR is a method by
which data and computation needed are compressed for a computation
without significant loss in fidelity. This is especially true when
large swaths of domain have little to no activity, while localized
portion need a very fine resolution. In Sedov blast wave problem high
resolution is needed only along the moving shock, the remainder of the
domain can have low resolution.

A standard way of using AMR is to divide domain into block where the
distance between points within a block is identical, but different
blocks may have different spacing. Blocks may go in and out of
existence depending upon how the needs for higher resolution move
within the domain. AMR is frequently used with explicit methods, where
a block surrounded by a halo of points filled from neighboring blocks
form a stand-alone computational domain. Physics operators cannot
differentiate between such block with halo cells from the entire
domain. Therefore decomposition into blocks serves as a very useful
abstraction for spatial discretization. When using AMR some additional
machinery is needed to reconcile physical quantities at fine-coarse
boundaries. 

The mathematical model used for solving Sedov blast wave problem is
Euler's equations of hydrodynamics in a conservative mode.  For these
eqations, equation of state (EOS) provides closure, and Riemann
solution is needed when there are discontinuited introduced by
shocks. The reason why many implementations of shock hydrodynamics use
Sedov as their test case is because a known analytical solution exists
for how far the shoch has travelled.

To summarize the requirements for solving the Sedov problem we need:
* AMR
  * halo cell filling included at fine-coarse boundaries through
  interpolation
  * reconciliation of fluxes at fine-coarse boundaries
  * regridding with solution evolution
* Riemann solver at discontinuities
* EOS
  * Application of EOS during halo cell fill at fine-coarse boundaries
* Verification against known analytical solution for distance the
shock has traveled.

# Components

* __Deeper Dive into some Components__
* Driver
  * Initialize the domain, supply initial conditions, setup I/O
  * Iterate over blocks
  * Implement connectivity
* Mesh
  * Owns data containers
  * Implements halo cell fill\, including application of boundary conditions
  * Reconciliation of quantities at fine\-coarse block boundaries
  * Remesh when refinement patterns change
* I/O
  * Getting runtime parameters and possibly initial conditions
  * Writing checkpoint and analysis data

* __Binned Components__
* Unchanging or slow changing infrastructure
  * Mesh
  * I/O
  * Driver
  * Comparison utility
* Components evolving with research – physics solvers
  * Initial and boundary conditions
  * Hydrodynamics
  * EOS


# Connectivity

![](img/conn3.png)

# Exploring design space -- Abstractions

![](img/abstract.png)


We consider the possibilities of abstractions as a first step in
design. In this example we are looking at two types of decompositions:
spatial and functional. The spatial decomposition uses a block as
abstraction where for functional components there is no way to
differentiate between the block and the whole domain. Functional
decomposition assumes that physics operators are applied one at a
time, and therefore they form the functional decomposition. Once we
have these abstractions in place we have to consider other design
considerations described below


__Other Design Considerations__

* Data scoping -- which data items are visible to which portions of a component

* Interfaces in the API -- a minimum set of accessor and mutator
  functions needed to provide the needed funcionality

* For example consider the minimal Mesh API needed to provide
  infrastructural support
  * Initialize\_mesh
  * Halo\_fill
  * Access\_to\_data\_containers
  * Reconcile\_fluxes
  * Regrid

# A Design Model for Separation of Concerns

![](img/sepcon.png)

# Separation of Concerns Applied

![](img/sepconapplied.png)

# Takeaways So far

Differentiate between slow changing and fast changing components of your code

Understand the requirements of your infrastructure

Implement separation of concerns

Design with portability\, extensibility\, reproducibility and maintainability in mind







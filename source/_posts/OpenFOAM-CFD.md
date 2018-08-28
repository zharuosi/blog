---
title: Fundamentals of Computational Fluid Dynamics in OpenFOAM
date: 2018-06-30 23:41:13
categories: 曹衣出水
tags: [CFD,OpenFOAM,FVM]
---

## Introduction

OpenFOAM® is the leading free, open source software for computational fluid dynamics (CFD), and other computational science and engineering. Here is a short introduction of the fundamentals of Finite Volume Method (FVM) in OpenFOAM. Two books are recommended:

- *The OpenFOAM Technology Primer*
- *The Finite Volume Method in Computational Fluid Dynamics - An Advanced Introduction with OpenFOAM® and Matlab®*

<!-- more -->
## The scalar transport equation

A mathematical model that describes a fluid flow is defined as a system of partial differential equations. In the model, the physical properties, e.g., pressure, velocity or temperature are dependent variables. The transport equation is used to describe the physical processes which change these properties in different way:

$$ \frac{\partial \phi}{\partial t} + \nabla \cdot (\vec{U} \phi) + \nabla \cdot (D \nabla \phi) = S_{\phi}$$

where $\phi$ is the scalar property, $\vec{U}$ is the velocity vector, $D$ is the diffusion coefficient. Three terms in the left side represent temporal term, convective term and diffusive term, respectively.

The purpose of any numerical method is to obtain an approximation of a solution of
the mathematical model by solving a system of algebraic equations.

## Domain discretization

The continuous space/fields in the mathematical model must be discretized into a finite number of volumes (cells). In the domain $\Omega$, Each finite volume stores an averaged value of the physical property in its cell centre C.

Two major types of meshes are distinguished, i.e., structured and unstructured meshes. Structured mesh has a regular connectivity and unstructured mesh has a irregular connectivity. The structured meshes support direct cell traversal. The cells can be labelled with the indices increasing in the directions of the coordinate axis. The unstructured meshes have no apparent direction in the way the cells are addressed. Their topology is un-ordered. The unstructured meshes allow us to discretize flow domains of very high geometrical complexity and do local refinement conveniently. The quality of a mesh can be evaluated by skewness, Jacobian, smoothness, etc. 

Skewness can be decided based on equilateral volume for triangles or based on the deviation from normalized equilateral angle for prisms and pyramids.
$$ Skewness = \frac{Optimal\; size - Cell\; size}{Optimal\; size} $$
$$ Skewness = \max\{\frac{\theta_{max}-90}{90},\frac{90-\theta_{min}}{90}\} $$

Jacobian, which is short for the Jacobian Matrix Determinate, is a measure of the normals of the element faces relative to each other. Jacobian ratio $J$ is the ratio of maximum determinant of Jacobian to minimum determinant of Jacobian. $J=1.0$ is desired. $J>0.6$ typically indicate the mesh quality is good.

Smoothness means there should not be sudden jumps in the size of the cell to avoid errors at nearby nodes. The change in size should be smooth. Laplacian smoothing is the most commonly used smoothing technique.
 
## Element addressing

The mesh topology determines the way mesh elements are addressed by numerical methods.

### Indirect addressing

The cells and faces of the mesh are defined as sets of indirected indexes to mesh points. The face is defined by the indices of its points, rather than by its points directly. For example, a hexahedral cell is addressed by faces 1 to faces 6 without storing any point related data directly. Otherwise dealing with the mesh information could lead to multiple copies of the same points and faces in memory.

### Owner-neighbour addressing

Owner-neighbour addressing optimization defines the way that the indices in the mesh faces are ordered by using the direction of normal vector of the face. Two global lists are introduced: the face-owner and ther face-neighbour list. The owner cell of a face is the cell with a lower index in the list of mesh cells. The face area normal vector is directed from the owner into the neighbour cell.

### Boundary mesh addressing

Boundary addressing optimization isolates the boundary faces and stores them at the end of the list of mesh faces. The boundary mesh is defined as a set of patches for different interpretation. All the faces of the boundary mesh are directed outwards from the flow domain. They have only a cell owner and no neighbour.

## Equation discretization

Once the domain is dicretized, approximations are applied on the mathematical model which transfer the differential terms into dicrete differential operators. That a numerical method is consistent means, as the size of the cells is reduced, the discrete mathematical model must approach the exact mathematical model (Ferziger and Peric 2002). Time is also discretized into a sequential finite intervals. Let's see the discretization of a simple advection equation for a scalar property $\phi$ and the vector $\mathbf{U}$.
$$ \frac{\partial \phi}{\partial t} + \nabla \cdot (\mathbf{U} \phi) = 0 $$

First we need to integrate it in time and space:
$$ \int_t^{t+\Delta t} \int_{V_p} (\frac{\partial \phi}{\partial t} + \nabla \cdot (\mathbf{U} \phi)) dx dt = 0 $$
where $V_p$ is the cell volume.

The temporal term can be approximated as:
$$ \int_t^{t+\Delta t} \int_{V_p} \frac{\partial \phi}{\partial t}  dx dt \approx V_p \frac{\phi^n - \phi^o}{\Delta t}  $$
where $n$ and $o$ mark the new time step and the old time step respectively, and $\Delta t$ denotes the time step.

The advective term is integrated by applying the Gauss divergence theorem:
$$ \int_t^{t+\Delta t} \int_{V_p} \nabla \cdot (\mathbf{U} \phi) dx dt \approx \sum \phi_f \mathbf{U}_f \mathbf{S} $$
where $f$ is a face of a cell, and $\mathbf{S}$ is the outward-pointing face area normal vector with the magnitude of the face area. The variations of the face interpolated values in time are neglected. Finanly the discrete form is obtained:
$$ V_p \frac{\phi^n - \phi^o}{\Delta t} + \sum \phi_f \mathbf{U}_f \mathbf{S} = 0 $$

If the explicit temporal discretization is used, the spatial terms will be evaluated in the old time step, as:
$$ V_p \frac{\phi^n - \phi^o}{\Delta t} + \sum \phi_f^o \mathbf{U}_f^o \mathbf{S} = 0 $$

If the implicit temporal discretization is used, the spatial terms will be evaluated in the new time step, as:
$$ V_p \frac{\phi^n - \phi^o}{\Delta t} + \sum \phi_f^n \mathbf{U}_f^n \mathbf{S} = 0 $$
It means the variables from the surrounding cells in the new time step are dependent. Afterwards, a system of algebraic equations is constructed and solved.

All face interpolations are based on looping over mesh faces. Cell values are accessed using owner-neighbour addressing of the cells. The face values can be interpolated only once other than twice in the loop, saving the computational time.
$$ \sum \phi_f \mathbf{U}_f \mathbf{S} = \sum^{owner}_f \phi_f \mathbf{U}_f \mathbf{S} - \sum^{neighbour}_f \phi_f \mathbf{U}_f \mathbf{S} $$

## Face interpolation

The values $\phi$ stored in cell centres $C$ are used to interpolate the values in the face centres $C_f$. The face centered value $\phi_f$ can be calculated by:
$$ \phi_f = f_x \phi_P + (1-f_x)\phi_N $$
where $f_x$ is a linear coefficient computed by:
$$ f_x = \frac{||\vec{fN}||}{||\vec{PN}||} $$
Let's take the central differencing scheme (CDS) as an example. The cell face values for a uniform grid can be computed by linear interpolation as:
$$ \phi_e = 0.5 (\phi_P + \phi_E) $$
$$ \phi_w = 0.5 (\phi_W + \phi_P) $$
The Taylor series truncation error of the central differencing scheme is second order accurate if $P_e<2$. $P_e$ is the Péclet number, the ratio of the rate of advection to the rate of diffusion driven by an appropriate gradient.

## Boundary conditions

There is only one cell next to a boundary face, thus this cell will always be the owner of the boundary face, and the normal area vector of a boundary face will always be directed out of the flow domain.

For the Dirichlet boundary conditions, the values of the physical properties are specified and fixed. For the Neumann boundary condition, the values are computed from the internal cell values and the values of the derivative are specified, e.g., zero gradient boundary condition.
$$ \nabla \phi(x_b) = 0 $$
According to the Taylor series approximation:
$$ \phi_P = \phi_b + \nabla \phi (x_b) \delta x + O(\delta^2 x) \approx \phi_b + \nabla \phi (x_b) \delta x = \phi_b $$

In a word, applying the boundary condition is basically either defining a value or a gradient on a certain boundary face. Finally a system of algebraic equations can be generated.





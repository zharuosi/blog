---
title: Fundamentals of Computational Fluid Dynamics in OpenFOAM
date: 2018-06-30 23:41:13
categories: 曹衣出水
tags: [CFD,OpenFOAM,FVM]
---

## Introduction

OpenFOAM® is the leading free, open source software for computational fluid dynamics (CFD), and other computational science and engineering. Here is a short introduction of the fundamentals of Finite Volume Method (FVM) in OpenFOAM. Two books are recommended:

- *The OpenFOAM Technology Primer*
- *Getting Started with OpenFOAM® Technology*

<!-- more -->
## The scalar transport equation

A mathematical model that describes a fluid flow is defined as a system of partial differential equations. In the model, the physical properties, e.g., pressure, velocity or temperature are dependent variables. The transport equation is used to describe the physical processes which change these properties in different way:

$$ \frac{\partial \phi}{\partial t} + \nabla \cdot (\vec{U} \phi) + \nabla \cdot (D \nabla \phi) = S_{\phi}$$

where $\phi$ is the scalar property, $\vec{U}$ is the velocity vector, $D$ is the diffusion coefficient. Three terms in the left side represent temporal term, convective term and diffusive term, respectively.

The purpose of any numerical method is to obtain an approximation of a solution of
the mathematical model by solving a system of algebraic equations.

## Domain discretization

The continuous space/fields in the mathematical model must be discretized into a finite number of volumes (cells). In the domain $\Omega$, Each finite volume stores an averaged value of the physical property in its cell centre C.

Two major types of meshes are distinguished, i.e., strucutured and unstructured meshes. Structured mesh has a regular connectivity and unstructured mesh has a irregular connectivity. The structured meshes support direct cell traversal. The cells can be labeled with the indices increasing in the directions of the coordinate axis. The unstructured meshes have no apparent direction in the way the cells are addressed. Their topology is un-ordered. The unstructured meshes allow us to discretize flow domains of very high geometrical complexity and do local refinement conveniently. The quality of a mesh can be evaluated by skewness, Jacobian, smoothness, etc. 

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






---
title: Fundamentals of Computational Fluid Dynamics in OpenFOAM
date: 2018-06-30 23:41:13
categories: 曹衣出水
tags: [CFD,OpenFOAM,FVM]
---

## Introduction

OpenFOAM® is the leading free, open source software for computational fluid dynamics (CFD), and other computational science and engineering. Here is a short introduction of the fundamentals of CFD in OpenFOAM. Two books are recommended:

- *The OpenFOAM Technology Primer*
- *Getting Started with OpenFOAM® Technology*

## Finite Volume Method in OpenFOAM
<!-- more -->
### The scalar transport equation

A mathematical model that describes a fluid flow is defined as a system of partial differential equations. In the model, the physical properties, e.g., pressure, velocity or temperature are dependent variables. The transport equation is used to describe the physical processes which change these properties in different way:

$$ \frac{\partial \phi}{\partial t} + \nabla \cdot (\vec{U} \phi) + \nabla \cdot (D \nabla \phi) = S_{\phi}$$

where $\phi$ is the scalar property, $\vec{U}$ is the velocity vector, $D$ is the diffusion coefficient. Three terms in the left side represent temporal term, convective term and diffusive term, respectively.

The purpose of any numerical method is to obtain an approximation of a solution of
the mathematical model by solving a system of algebraic equations.

### Domain discretization

The continuous space/fields in the mathematical model must be discretized into a finite number of volumes (cells). In the domain $\Omega$, Each finite volume stores an averaged value of the physical property in its cell centre C.

Two major types of meshes are distinguished, i.e., strucutured and unstructured meshes. Structured mesh has a regular connectivity and unstructured mesh has a irregular connectivity. The structured meshes support direct cell traversal. The cells can be labeled with the indices increasing in the directions of the coordinate axis. The unstructured meshes have no apparent direction in the way the cells are addressed. Their topology is un-ordered. The quality of a mesh can be evaluated by skewness, Jacobian, smoothness, etc. 

Skewness can be decided based on equilateral volume for triangles or based on the deviation from normalized equilateral angle for prisms and pyramids.
$$ Skewness = \frac{Optimal size - cell size}{Optimal size} $$
$$ Skewness = max\{\frac{\theta_{max}-90}{90},\frac{90-\theta_{min}}{90}\} $$

Jacobian, which is short for the Jacobian Matrix Determinate, is a measure of the normals of the element faces relative to each other. Jacobian ratio $J=1.0$ for a cube. $J>0.5$ typically indicate the mesh quality is good. 

Smoothness means there should not be sudden jumps in the size of the cell to avoid errors at nearby nodes. The change in size should be smooth. Laplacian smoothing is the most commonly used smoothing technique.
 











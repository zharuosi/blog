---
title: Introduction to RANS equations
date: 2018-11-08 23:00:31
categories: 曹衣出水
tags: [Marine Hydrodynamics,CFD]
---

## What is RANS

The Reynolds-averaged Navier–Stokes equations (RANS equations) for the three-dimensional incompressible viscous flow consists of the continuity equation and the momentum equations as follows: 

$$ \frac{\partial u_i}{\partial x_i} = 0 $$
$$ \rho \frac{\partial u_i}{\partial t} + \rho u_j \frac{\partial u_i}{\partial x_j} = -\frac{\partial p}{\partial x_i} + \frac{\partial}{\partial x_j} \left[ \mu \left(\frac{\partial u_i}{\partial x_j} + \frac{\partial u_j}{\partial x_i} \right) \right] + \frac{\partial}{\partial x_j} (-\rho \overline{u'_i u'_j}) $$

where $u_i$, i=1,2 in two-dimensional flow, denotes the velocity components along x-,y-axis, respectively. $p$ is the pressure. $rho$ is the water density. $\mu$ is the dynamic viscosity of water. $-\rho \overline{u'_i u'_j}$ are the Reynolds stresses or the apparent turbulent shear stress. The Reynolds stresses can be solved based on the Boussinesq hypothesis using the eddy viscosity turbulence models:
$$ -\rho \overline{u'_i u'_j} = \mu_t \left( \frac{\partial u_i}{\partial x_j} + \frac{\partial u_j}{\partial x_i} \right) - \frac{2}{3}\rho k \delta_{ij} $$
where $\mu_t$ represents the eddy viscosity. $\delta_{ij}$ is the Kronecker delta. $k=\frac{1}{2}\overline{u'_i u'_j}$ is the turbulent kinetic energy, which can be solved from the transport equations. In the present studies, the SST $k-\omega$ model were employed, where $\omega$ is the specific turbulence dissipation rate. In the near region, the transport equations are changed to the standard $k-\omega$ model by setting the damped cross-diffusion derivative term as zero. In the far field, the model is the same as the standard $k-\epsilon$ model, which can avoid the problem that the model is too sensitive to the inlet turbulence properties.
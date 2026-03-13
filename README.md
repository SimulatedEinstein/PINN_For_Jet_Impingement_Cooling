# Physics-Informed Neural Network for Jet-Impingement Heat Transfer on a Cylinder

## Overview

This project implements a **Physics-Informed Neural Network (PINN)** to simulate **jet-embedded uniform inflow impinging on a heated cylinder**.

The model solves the coupled **Navier–Stokes and energy equations** to predict the **velocity field, pressure distribution, and temperature field** around the cylinder. In addition to the flow solution, the code also evaluates the **local Nusselt number distribution along the cylinder surface**, which is a key indicator of convective heat transfer.

The implementation uses **PyTorch** and automatic differentiation to enforce the governing equations and boundary conditions during training.

## Physical Problem

The study considers a **two-dimensional flow past a circular cylinder with a jet embedded in the inlet flow**.

Key parameters used in the simulation:

Reynolds number: Re = 2000

Prandtl number: Pr = 0.71

Cylinder diameter: D = 1

Uniform background inflow velocity: U∞ = 0.6

The jet is superimposed on the background flow using a **Gaussian velocity profile** at the inlet.

The distance between the inlet plane and the cylinder front is defined as

H / D = 2

## Governing Equations

The PINN model enforces the steady **incompressible Navier–Stokes equations** and the **energy equation**.

Continuity equation:

∂u/∂x + ∂v/∂y = 0

Momentum equations:

u ∂u/∂x + v ∂u/∂y = − ∂p/∂x + ν(∂²u/∂x² + ∂²u/∂y²)

u ∂v/∂x + v ∂v/∂y = − ∂p/∂y + ν(∂²v/∂x² + ∂²v/∂y²)

Energy equation:

u ∂T/∂x + v ∂T/∂y = α(∂²T/∂x² + ∂²T/∂y²)

where

u, v = velocity components
p = pressure
T = temperature
ν = kinematic viscosity
α = thermal diffusivity

## Boundary Conditions

The simulation uses the following boundary conditions.

Cylinder wall

No-slip condition:

u = 0
v = 0

Isothermal wall temperature:

T = T_wall

Inlet boundary

The velocity profile is a combination of uniform flow and a Gaussian jet profile.

u(y) = U∞ + jet_profile

The jet temperature is higher than the background fluid.

Outlet boundary

Pressure is fixed at zero to allow flow to exit the domain naturally.

p = 0


Top and bottom boundaries

Weak confinement conditions are applied:

v = 0
T = T∞

## PINN Model

The neural network approximates the following fields:

u(x, y)
v(x, y)
p(x, y)
T(x, y)

Network architecture used in the implementation:

[2, 128, 128, 128, 128, 4]

Inputs:

x, y

Outputs:

velocity components (u, v)
pressure (p)
temperature (T)

The model is trained by minimizing a loss composed of:

Physics residuals (governing equations)
Boundary condition errors


## Training Strategy

The training process involves two stages.

First stage:

Adam optimizer

Second stage:

L-BFGS optimizer for final refinement

This hybrid approach improves convergence and solution accuracy.


## Heat Transfer Analysis

After training, the model computes the **local Nusselt number** along the cylinder surface.

The Nusselt number represents the dimensionless heat transfer rate.

Nu = − (∂T/∂n) D / (T_wall − T_jet)

where

∂T/∂n = temperature gradient normal to the cylinder surface.

The code calculates:

Local Nusselt number distribution along the cylinder surface
Stagnation-point Nusselt number at the jet impact location


## Output Results

The code generates the following outputs.

### Velocity field

Contour plot of velocity magnitude around the cylinder showing flow acceleration and wake formation.

### Temperature field

Contour plot of temperature distribution illustrating thermal transport due to convection and diffusion.

### Nusselt number distribution

Plot of local Nusselt number as a function of surface angle.

### Stagnation heat transfer

The stagnation-point Nusselt number is printed after the simulation.



## Libraries Used

The implementation uses the following Python libraries.

PyTorch
NumPy
Matplotlib


## Applications

This type of simulation is relevant in many engineering applications such as:

jet impingement cooling
gas turbine blade cooling
electronic device thermal management
aerospace heat transfer systems

Physics-Informed Neural Networks provide a promising alternative to traditional CFD solvers for these types of multiphysics problems.

multi-jet cooling configurations
comparison with experimental and CFD data

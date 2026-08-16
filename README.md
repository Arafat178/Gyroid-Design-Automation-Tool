# Gyroid Design Automation Tool

A web-based engineering tool for generating **parametric SolidWorks VBA macros** for Gyroid structures while simultaneously estimating **porosity** from an empirical correlation.

The project combines **Gyroid/TPMS geometry, SolidWorks API automation, parametric CAD, and porosity analysis** into a single lightweight browser application.

---

## Overview

Gyroid structures are a type of **Triply Periodic Minimal Surface (TPMS)** geometry that have attracted significant interest in mechanical engineering because of their complex interconnected geometry, high surface-area-to-volume ratio, lightweight nature, and potential applications in heat transfer, fluid flow, additive manufacturing, and lightweight structures.

However, creating a Gyroid geometry manually in CAD software can be tedious and difficult to parameterize.

This project aims to solve that problem by providing a simple workflow:

```text
User Input
   ↓
Gyroid Parameters
   ↓
Porosity Calculation
   ↓
SolidWorks VBA Generation
   ↓
Copy / Download VBA Macro
   ↓
Automated Gyroid Construction in SolidWorks
```

Instead of manually editing the SolidWorks macro every time the geometry changes, the user can enter the desired parameters in the web interface and generate a ready-to-use VBA macro.

---

# Key Features

### CAD Automation

* Parametric Gyroid generation
* SolidWorks VBA macro generation
* Automatic parameter replacement
* SolidWorks API-based geometry construction
* Surface generation
* Surface transformation
* Surface knitting
* Surface thickening
* Automated 3D visualization

### Porosity Analysis

* Empirical porosity calculation
* Thickness-to-size ratio calculation
* Interactive porosity analysis
* Porosity vs. thickness graph
* Porosity table generation
* Multiple `a`-value comparison

### Code Generation

* Complete VBA macro output
* Syntax-friendly code display
* One-click code copying
* `.bas` macro download
* No manual editing required

### Web Application

* Single HTML file
* HTML + CSS + JavaScript
* No backend
* No database
* No Python required to run the web application
* Responsive interface
* Engineering/CAD-inspired UI

---

# What is a Gyroid?

A Gyroid is a **Triply Periodic Minimal Surface (TPMS)**.

It is a mathematically defined three-dimensional surface that repeats periodically in all three spatial directions.

A commonly used implicit Gyroid equation is:

[ sin(X)\cos(Y) + sin(Y)\cos(Z) + sin(Z)cos(X) = C ]

where (C) controls the level-set of the surface.

Gyroid structures are particularly interesting because they provide a continuous interconnected geometry without the sharp intersections commonly found in conventional lattice structures.

---

# Parameters

The current application uses two primary user-controlled parameters.

## Overall Size — `a`

The unit size Gyroid dimension.

Example:

```text
a = 20 mm
```

## Surface Thickness — `t`

The thickness of the generated Gyroid surface.

Example:

```text
t = 1 mm
```

The SolidWorks macro internally uses SI units, so the web application converts:

[
a_{m}=\frac{a_{mm}}{1000}
]

and

[
t_{m}=\frac{t_{mm}}{1000}
]

For example:

```text
Input:

a = 20 mm
t = 1 mm

Generated VBA:

a = 0.02
t = 0.001
```

---

# Porosity Calculation

The application uses the following empirical correlation to estimate Gyroid porosity:

where:

| Symbol | Meaning             |
| ------ | ------------------- |
| (y)    | Porosity (%)        |
| (t)    | Surface thickness   |
| (a)    | Unit Cell Gyroid size |

Therefore, `t` and `a` must use the same unit when calculating porosity.

For example:

```text
a = 20 mm
t = 1 mm
```

gives:

[
\frac{t}{a}=0.05
]

The application then evaluates the empirical equation automatically.

> **Note:** This is an empirical correlation used for estimation. It is not a direct CAD-volume calculation of porosity.

---

# Porosity Analysis

The application can evaluate the porosity over a range of surface thicknesses.

For a selected `a`, the system evaluates:

```text
t = 0.1 mm
t = 0.2 mm
t = 0.3 mm
...
t = 2.0 mm
```

and calculates:


This allows the user to investigate the relationship between:

```text
Surface Thickness
        ↓
t/a Ratio
        ↓
Porosity
```

---

# Multiple Geometry Comparison

The application can also compare different Gyroid sizes.

The analysis considers:

```text
a = 5 mm
a = 10 mm
a = 15 mm
a = 20 mm
a = 25 mm
a = 30 mm
a = 35 mm
a = 40 mm
a = 45 mm
a = 50 mm
```

For each value of `a`, porosity is evaluated over the selected thickness range.

This allows visualization of:

[
\text{Porosity vs. Thickness}
]

for multiple Gyroid dimensions.

---

# SolidWorks Automation

The core of this project is the SolidWorks VBA macro.

The web application does **not** directly communicate with SolidWorks.

Instead, it generates a VBA macro containing the selected parameters.

The user can then open the generated macro in SolidWorks and execute it.

The workflow is:

```text
Web Application
      │
      │ Generate
      ▼
VBA Macro
      │
      │ Copy / Download
      ▼
SolidWorks
      │
      ▼
3D Sketch
      │
      ▼
Spline Geometry
      │
      ▼
Fill Surface
      │
      ▼
Surface Transformation
      │
      ▼
Surface Replication
      │
      ▼
Knit Surface
      │
      ▼
Thicken
      │
      ▼
Gyroid Solid
```

---

# 🔄 Macro Parameterization

The original SolidWorks macro contains parameters similar to:

```vb
a = 0.02
L = 0.01
m = a / L
t = 0.001
```

The web application dynamically modifies the values of `a` and `t`.

The relationship:

```vb
m = a / L
```

is preserved.

For example, if the user enters:

```text
a = 20 mm
t = 1 mm
```

the generated macro contains:

```vb
a = 0.02
L = 0.01
m = a / L
t = 0.001
```

The rest of the geometry-generation algorithm remains unchanged.

---

# Geometry Generation Workflow

The SolidWorks macro uses a series of geometric operations.

## 1. 3D Sketch

The macro creates a 3D sketch inside SolidWorks.

## 2. Spline Generation

Three-dimensional spline segments are created using predefined control points.

## 3. Fill Surface

The spline boundaries are used to generate the fundamental surface patch.

## 4. Surface Transformation

The generated surface is copied and transformed through translations and rotations.

## 5. Surface Replication

Multiple surface patches are assembled to create the periodic structure.

## 6. Knit Surface

The individual surfaces are combined into a knitted surface.

## 7. Thicken

The knitted surface is converted into a solid geometry by applying the specified thickness.

---

# 🖥️ Web Application Workflow

The user interacts with the application through a simple interface.

### Step 1 — Enter Parameters

```text
Unit Cell Size (a):      20 mm
Surface Thickness (t): 1 mm
```

### Step 2 — Generate

Click:

```text
GENERATE GYROID
```

### Step 3 — Calculate Porosity

The application calculates:

```text
t/a
Porosity (%)
```

### Step 4 — Generate VBA

The application creates the complete SolidWorks VBA macro with the selected parameters.

### Step 5 — Copy or Download

The user can:

```text
COPY CODE
```

or:

```text
DOWNLOAD .BAS
```

### Step 6 — Run in SolidWorks

The generated macro can then be used inside SolidWorks.

---

#  Technologies Used

### Frontend

* HTML5
* CSS3
* JavaScript

### Engineering

* SolidWorks
* SolidWorks API
* VBA
* Parametric CAD
* TPMS / Gyroid geometry

### Mathematical Analysis

* Empirical porosity correlation
* Dimensionless thickness ratio
* Parametric analysis
* Porosity comparison

---

# Potential Engineering Applications

Gyroid structures have a wide range of potential engineering applications.

## Heat Transfer

Potential applications include:

* Compact heat exchangers
* Heat sinks
* Cooling channels
* Thermal management
* Enhanced-convection structures

The high surface-area-to-volume characteristics of TPMS geometries make them particularly interesting for thermal applications.

---

## Fluid Flow

Possible research topics include:

* Pressure drop
* Flow distribution
* Permeability
* Convective heat transfer
* Turbulent flow behavior

---

## Lightweight Structures

Gyroid structures can be investigated for:

* Weight reduction
* Structural optimization
* Energy absorption
* Lightweight components
* Additive-manufactured structures

---

## Additive Manufacturing

The generated geometry can potentially be exported into formats such as:

```text
STL
STEP
3MF
```

for manufacturing and simulation workflows.

---

# Research Potential

The strongest aspect of this project is its potential to evolve beyond a simple CAD macro generator.

The current workflow can be extended into a complete computational engineering pipeline:

```text
Design Parameters
       ↓
Gyroid Generation
       ↓
Porosity Prediction
       ↓
CAD Geometry
       ↓
Mesh Generation
       ↓
CFD / FEA
       ↓
Performance Evaluation
       ↓
Optimization
```

For example, a future study could investigate:

> **Effect of Gyroid surface thickness and unit-cell size on pressure drop and heat-transfer performance.**

A parametric study could automatically generate dozens or hundreds of geometries and evaluate them computationally.

---

# Future Development

## Phase 1 — Current

* [x] Gyroid parameter input
* [x] `a` and `t` parameterization
* [x] Porosity calculation
* [x] Porosity equation implementation
* [x] Porosity graph
* [x] Porosity table
* [x] VBA macro generation
* [x] Copy VBA code
* [x] Download `.bas`
* [x] Responsive web interface

---

## Phase 2 — Geometry Automation

* [ ] Direct SolidWorks API integration
* [ ] Automatic `.SLDPRT` generation
* [ ] Automatic geometry export
* [ ] Automatic STL generation
* [ ] Automatic STEP generation
* [ ] Multiple unit-cell support

---

## Phase 3 — Advanced Gyroid Generator

Replace the current spline-based approach with direct mathematical TPMS generation.

For example:

[sin(x)cos(y) + sin(y)cos(z) + sin(z)cos(x) = C]

This would allow truly parametric Gyroid generation based directly on the mathematical surface.

---

## Phase 4 — Simulation Automation

Integrate the CAD workflow with ANSYS:

```text
Gyroid Generator
      ↓
SolidWorks
      ↓
STEP / Geometry
      ↓
ANSYS
      ↓
Meshing
      ↓
Boundary Conditions
      ↓
CFD / Thermal Analysis
      ↓
Results
```

Potential outputs:

* Pressure drop
* Velocity distribution
* Temperature distribution
* Heat-transfer coefficient
* Nusselt number
* Thermal performance factor

---

## Phase 5 — Optimization

The final goal could be an automated optimization system.

For example:

[
\text{Maximize Heat Transfer}
]

while:

[
\text{Minimize Pressure Drop}
]

and:

[
\text{Maintain Required Porosity}
]

This could eventually lead to:

```text
Input Requirements
       ↓
Automatic Design Generation
       ↓
Automatic Simulation
       ↓
Performance Evaluation
       ↓
Optimization
       ↓
Best Gyroid Geometry
```

---

# Limitations

The current project has several important limitations.

### Empirical Porosity Equation

The porosity is estimated using an empirical correlation rather than calculated directly from the CAD geometry.

Therefore, the predicted porosity should be validated against:

* CAD volume calculations
* Experimental measurements
* Literature/reference data

before using it as a final research result.

### Geometry Generation

The current SolidWorks macro relies on predefined spline geometry and surface transformations.

It is therefore not yet a universal mathematical TPMS generator.

### SolidWorks Dependency

The generated VBA macro requires SolidWorks and its VBA environment.

The web application itself does not require SolidWorks.

---

# Validation Strategy

For research use, the next important step is to compare the empirical porosity prediction against actual CAD geometry.

The recommended validation workflow is:

```text
Input a and t
      ↓
Generate Gyroid
      ↓
Calculate Empirical Porosity
      ↓
Measure CAD Volume
      ↓
Calculate Actual Porosity
      ↓
Compare
```
The difference between empirical and CAD-derived porosity can then be quantified.

This would significantly strengthen the engineering validity of the project.

---

# Project Objectives

The project aims to:

1. Automate Gyroid CAD generation.
2. Reduce repetitive SolidWorks macro editing.
3. Introduce parameter-driven geometry generation.
4. Estimate porosity from geometric parameters.
5. Visualize the relationship between thickness and porosity.
6. Provide a bridge between mathematical modeling and CAD automation.
7. Establish a foundation for automated CFD/FEA studies.
8. Move toward optimization-driven Gyroid design.

---

# Why This Project Matters

The important idea is not simply generating a visually complex Gyroid.

The real objective is **parametric engineering automation**.

A conventional workflow looks like:

```text
Change parameter
      ↓
Manually edit CAD
      ↓
Rebuild
      ↓
Export
      ↓
Analyze
```

This project moves toward:

```text
Change parameter
      ↓
Automatic macro generation
      ↓
Automatic CAD generation
      ↓
Automatic analysis
      ↓
Optimization
```

That transition—from manually built geometry to computationally controlled geometry—is the core engineering value of the project.

---

# Author

**Arafat Hossain**

Mechanical Engineer

Bangladesh

---


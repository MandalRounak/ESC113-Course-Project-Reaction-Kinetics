# Numerical Solution of Reaction Kinetics Using MATLAB

A versatile, interactive MATLAB App Designer tool engineered to simulate chemical reaction kinetics within a batch reactor. The application solves the ordinary differential equations (ODEs) governing reactant and product concentrations over time for arbitrary reaction orders using three distinct numerical schemes: Explicit Euler, Implicit Euler, and 4th-Order Runge-Kutta (RK4).

---

## 🔬 Computational & Mathematical Framework

### 1. Governing Differential Equation
The kinetic behavior of a batch reactor for a reaction of arbitrary order $n$ is mathematically formulated by the following nonlinear ordinary differential equation:

$$\frac{dC_{A}}{dt} = -k \cdot C_{A}^{n}$$

Where:
* $C_{A}$: Instantaneous concentration of reactant A (mol/L)
* $k$: Reaction rate constant
* $n$: Reaction order, varying continuously from 0 to 10

The product concentration $C_{B}(t)$ is computed concurrently at each time interval via the stoichiometric mass balance constraint:

$$C_{B}(t) = C_{A0} - C_{A}(t)$$

### 2. Implemented Numerical Schemes

* **Explicit Euler Method:** Integrates the system explicitly at each step:
  $$C_{A}^{i+1} = C_{A}^{i} + h \cdot \left(-k \cdot (C_{A}^{i})^{n}\right)$$
  * *Characteristics:* Highly efficient with low computational cost per step (single function evaluation), but conditionally stable and prone to non-physical oscillations or negative concentrations if the step size (h) is selected poorly.

* **Implicit Euler Method:** Employs a fully implicit discretization scheme:
  $$C_{A}^{i+1} = C_{A}^{i} - h \cdot k \cdot (C_{A}^{i+1})^{n}$$
  * *Characteristics:* Unconditionally stable and optimal for stiff equations or rapid timescales. Because it is implicit, it resolves the underlying algebraic nonlinearity at each individual time step by iterating via a root-finding Newton-Raphson loop:
    $$G(C_{A,new}) = C_{A,new} - C_{A}^{i} + h \cdot k \cdot (C_{A,new})^{n} = 0$$

* **Runge-Kutta 4th Order (RK4):** Evaluates a weighted average of four internal slope increments across each step interval:
  $$k_{1} = f(t^{i}, C_{A}^{i}), \quad k_{2} = f\left(t^{i} + \frac{h}{2}, C_{A}^{i} + \frac{h k_{1}}{2}\right)$$
  $$k_{3} = f\left(t^{i} + \frac{h}{2}, C_{A}^{i} + \frac{h k_{2}}{2}\right), \quad k_{4} = f(t^{i} + h, C_{A}^{i} + h k_{3})$$
  $$C_{A}^{i+1} = C_{A}^{i} + \frac{h}{6}(k_{1} + 2k_{2} + 2k_{3} + k_{4})$$
  * *Characteristics:* Delivers high-order global accuracy proportional to $h^4$. It acts as a balanced solver for moderately stiff configurations without requiring iterative convergence loops.

---

## 📊 Method Comparison Matrix

The mathematical trade-offs between computational overhead, stability constraints, and precision are summarized below:

| Numerical Method | Convergence Accuracy | Stability Profile | Computational Cost per Step |
| :--- | :--- | :--- | :--- |
| **Explicit Euler** | Low (First-Order proportional to h) | Low (Conditionally Stable) | Low (Single Function Evaluation) |
| **Implicit Euler** | Moderate (First-Order proportional to h) | High (Unconditionally Stable) | High (Requires Newton-Raphson Iterations) |
| **RK4** | High (Fourth-Order proportional to h^4) | Moderate (Stiff Sensitivity) | Moderate (4 Function Evaluations) |

---

## 💻 Interactive App UI & Workflow

The platform provides a complete graphical user interface built with MATLAB App Designer components to easily parameterize models:

```text
+-------------------------------------------------------------+
|  [Inputs Panel]                    [UIAxes Plot Display]    |
|  Rate Constant K     [  0.54 ]     |                        |
|  Initial Conc.       [  0.05 ]     |  Concentration vs Time |
|  Time Step (h)       [  0.10 ]     |     Reactant A (-)     |
|  Total Time (tf)     [ 100.0 ]     |     Product B (--)     |
|  Method Dropdown     [  RK4  ]     +------------------------+
|                                                             |
|  Reaction Order Slider: [0]----(2)--------------------[10]  |
|  [ PLOT BUTTON ]                                            |
+-------------------------------------------------------------+

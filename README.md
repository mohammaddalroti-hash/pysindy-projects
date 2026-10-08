# Data-Driven Discovery of Governing Equations with SINDy

This project recovers the differential equations of physical systems **from time-series data alone**. It uses Sparse Identification of Nonlinear Dynamics (SINDy), implemented with [PySINDy](https://github.com/dynamicslab/pysindy).

👉 **Everything is in one notebook: [`SINDy_equation_discovery.ipynb`](SINDy_equation_discovery.ipynb)** (it renders on GitHub with all outputs and plots).

## Method

1. Simulate the true system to generate state data **X(t)**.
2. Estimate the derivatives **Ẋ** numerically, using plain or smoothed finite differences.
3. Build a library **Θ(X)** of candidate functions: polynomials, trigonometric or rational terms.
4. Solve **Ẋ ≈ Θ(X) Ξ** with sparse regression (STLSQ). Only a few active terms survive, which gives an interpretable model.
5. Validate the identified model on initial conditions that were not used for training.

## Results

| System | Candidate library | Identified model | Validation |
|---|---|---|---|
| **Lorenz attractor** (chaotic) | Polynomials, degree ≤ 2 (30 terms) | Exactly the 7 true terms, all within 0.15 % | Tracks the true trajectory for about 5 Lyapunov times. Reproduces the attractor. |
| **Lorenz + 1 % noise** | Same, with smoothed differentiation | Still 7 terms. Maximum coefficient error drops from 4.6 % to 1.3 % with smoothing | — |
| **Nonlinear pendulum** (undamped and damped) | Linear + {sin, cos} | θ̈ = −0.750 θ̇ − 65.317 sin θ (true: −0.75, −65.33) | Accurate at θ₀ = 90°, outside the small-angle regime |
| **Michaelis–Menten kinetics** | Polynomial vs rational | Rational library gives `ẋ = −1.500 x/(0.8 + x)` exactly | The polynomial model fails to extrapolate. The rational model is exact. |

**Takeaway:** SINDy gives interpretable, physically meaningful models when the derivatives are estimated carefully and the candidate library contains the right kinds of functions. The library is where domain knowledge enters.

## Running

```bash
pip install -r requirements.txt
jupyter notebook SINDy_equation_discovery.ipynb
```

Tested with Python 3.14, pysindy 2.1.0, numpy 2.4, scipy 1.18 and matplotlib 3.10. The whole notebook runs in under a minute.

## Skills demonstrated

System identification · sparse regression · nonlinear and chaotic dynamics · numerical differentiation of noisy data · ODE integration · scientific Python (NumPy, SciPy, Matplotlib, PySINDy)

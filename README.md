# GR-Lab: Schwarzschild Geodesic Verification Engine

**Status:** Public technical specification; implementation and numerical validation are planned, not yet completed.

GR-Lab is a proposed reproducible Python toolkit for null geodesics in Schwarzschild spacetime. The initial goal is to recover known general-relativity predictions through numerical integration and automated tests.

## Mathematical foundation

In the equatorial plane, the Schwarzschild line element is

$$
ds^2=-f(r)c^2dt^2+f(r)^{-1}dr^2+r^2d\phi^2,\qquad
f(r)=1-\frac{2GM}{rc^2}.
$$

For a null geodesic, $ds^2=0$. With affine parameter $\lambda$, the conserved quantities are

$$
E=f(r)c^2\frac{dt}{d\lambda},\qquad L=r^2\frac{d\phi}{d\lambda}.
$$

The radial equation becomes

$$
\left(\frac{dr}{d\lambda}\right)^2+V_{\rm eff}(r)=\frac{E^2}{c^2},
\qquad
V_{\rm eff}(r)=\frac{L^2}{r^2}\left(1-\frac{2GM}{rc^2}\right).
$$

Differentiation gives

$$
\frac{dV_{\rm eff}}{dr}
=L^2\left(-\frac{2}{r^3}+\frac{6GM}{c^2r^4}\right).
$$

The nontrivial stationary point is the photon sphere:

$$
r_{\rm ph}=\frac{3GM}{c^2}.
$$

At this radius,

$$
\left.\frac{d^2V_{\rm eff}}{dr^2}\right|_{r_{\rm ph}}
=-\frac{2L^2}{r_{\rm ph}^4}<0,
$$

so the circular null orbit is unstable. Setting the radial derivative to zero at this orbit and defining $b=Lc/E$ yields

$$
b_{\rm crit}=\frac{3\sqrt{3}GM}{c^2}.
$$

For weak deflection, the leading-order prediction is $\Delta\phi\simeq 4GM/(bc^2)$, valid for $GM/(bc^2)\ll1$.

## Planned technology

Python 3.11+, NumPy, SciPy, SymPy, Matplotlib, pytest and GitHub Actions. Use geometrized units $G=c=1$ internally, with explicit unit conversion at the API boundary.

## Planned repository layout

```text
gr-lab/
├── README.md
├── pyproject.toml
├── src/grlab/
│   ├── __init__.py
│   ├── schwarzschild.py
│   ├── geodesics.py
│   ├── integrator.py
│   └── visualization.py
├── tests/
│   ├── test_photon_sphere.py
│   ├── test_conservation.py
│   └── test_light_deflection.py
├── examples/photon_orbits.py
└── .github/workflows/tests.yml
```

The layout above is a **plan**, not a statement that these files already exist.

## Sprint 1: executable prototype

1. Implement the Schwarzschild metric and null geodesic first-order system.
2. Integrate with `scipy.integrate.solve_ivp`, preserving signed radial velocity through turning points.
3. Add event handling for horizon approach and integration termination.
4. Test the analytic photon-sphere radius $r=3M$ and critical impact parameter $b=3\sqrt{3}M$.
5. Monitor null-constraint and conserved-quantity residuals. Target relative errors below $10^{-8}$ on selected well-conditioned trajectories; document any exceptions.
6. Compare weak-field light deflection with the $4M/b$ leading-order prediction over decreasing $M/b$.
7. Plot representative captured, near-critical and scattered photon trajectories.
8. Add reproducible installation steps and CI tests.

## Proposed Devin workflow

Create GitHub issues -> provide a precise implementation prompt to Devin -> inspect the generated pull request -> run pytest and numerical diagnostics in GitHub Actions -> review plots and errors -> merge only after checks pass.

**Initial Devin prompt:** Implement a Python 3.11 package for Schwarzschild null-geodesic integration in geometrized units, with signed radial velocity, turning-point-safe ODEs, event handling, photon-sphere and critical-impact-parameter tests, conservation diagnostics, weak-field deflection comparison, plots, and documented execution steps.

## Scope and honesty

This repository currently documents a mathematical derivation and an engineering plan. It does **not** yet contain a validated numerical solver or claim a new physical discovery. The aim is to reproduce established results before extending to timelike geodesics or Kerr spacetime.

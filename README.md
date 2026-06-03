# SPDEs mediante Wiener-Chaos y Fourier Neural Operators

Resolución de SPDEs mediante expansión de Wiener–Chaos (WCE) y Fourier Neural
Operators (FNO). Caso de estudio inicial: la ecuación $\Phi^4_1$ (Allen–Cahn
estocástica en 1D),

$$dX = (\Delta X + 3X - X^3)\, dt + dW$$

La expansión de Wiener–Chaos separa la aleatoriedad de la parte determinista,
reescribiendo la solución como una serie de coeficientes deterministas
(*propagator fields*). Un FNO aprende esos coeficientes a partir de datos de
*ground truth* generados.

## Instalación

```bash
git clone <URL_DEL_REPO>
cd spde-wce-fno
uv sync
```

## Referencias

- D. Shi, L. Lin, A. Han, J. M. Hernández-Lobato, Z. Wang, J. Gao.
  *Expanding the Chaos: Neural Operator for Stochastic (Partial) Differential
  Equations* (2026).
- C. Salvi, M. Lemercier, A. Gerasimovičs.
  *Neural Stochastic PDEs: Resolution-Invariant Learning of Continuous
  Spatiotemporal Dynamics* (NeurIPS 2022).
- Z. Li, N. Kovachki, K. Azizzadenesheli, B. Liu, K. Bhattacharya, A. Stuart,
  A. Anandkumar. *Fourier Neural Operator for Parametric Partial Differential
  Equations* (ICLR 2021).
- A. Neufeld, P. Schmocker. *Solving Stochastic Partial Differential Equations
  Using Neural Networks in the Wiener Chaos Expansion* (2024).
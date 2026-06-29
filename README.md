# SPDEs mediante Wiener-Chaos y Fourier Neural Operators

Resolución de SPDEs mediante expansión de Wiener–Chaos (WCE) y Fourier Neural
Operators (FNO). Caso de estudio inicial: la ecuación $\Phi^4_1$ (Allen–Cahn
estocástica en 1D),

$$dX = (\Delta X + 3X - X^3)\, dt + dW$$

La expansión de Wiener–Chaos separa la aleatoriedad de la parte determinista,
reescribiendo la solución como una serie de coeficientes deterministas
(*propagator fields*). Un FNO aprende esos coeficientes a partir de datos de
*ground truth* generados.

## Casos de estudio

- **`phi41_work/`** — ecuación $\Phi^4_1$ (1D). Ground truth por diferencias finitas
  (`phi41_ground_truth_MDF.ipynb`) y modelo F-SPDENO 1D (`phi41_fspdeno*.ipynb`).
- **`navier_stokes_work/`** — Navier–Stokes 2D estocástico en forma de vorticidad.
  - `ns_ground_truth.ipynb` — solver pseudo-espectral Crank–Nicolson (port CPU de
    [`torchspde/generator_sns.py`](https://github.com/crispitagorico/torchspde)) con ruido
    $Q$-Browniano y CI por *Gaussian Random Field*; genera los datasets $W\mapsto X$ y
    $(X_0,W)\mapsto X$.
  - `ns_fspdeno_Wmap.ipynb` / `ns_fspdeno_X0Wmap.ipynb` — F-SPDENO **2D**: features de Wick
    con modos de Fourier 2D, **FNO 2D** (`SpectralConv2d`) y reconstrucción temporal de Haar.

  Adaptaciones respecto al paper (por correr en CPU sin GPU): horizonte temporal corto
  (equivalente a una ventana de las trayectorias largas, *à la* Salvi), ruido coloreado
  dominado por modos bajos y truncamientos $K_{\text{Haar}}$, $S$, $M$ moderados. Todos los
  parámetros están en la celda de configuración de cada notebook.

### Resultados Navier–Stokes (config CPU: $s{=}64$, $T{=}0.5$, $\sigma{=}0.01$, $N{=}400$, $K_{\text{Haar}}{=}24$, ancho 32, 25 épocas)

| Tarea | Baseline (campo medio) | F-SPDENO 2D (este repo) | Paper (Table 2, H200) |
|---|---|---|---|
| $W\mapsto X$ | 0.112 | **0.117** | 0.037 |
| $(X_0,W)\mapsto X$ | 0.919 | **0.398** | 0.031 |

- En $W\mapsto X$ (CI fija, ruido débil) la solución está **dominada por la parte determinista**:
  el FNO reproduce esa evolución (queda $\approx$ baseline), consistente con la observación del
  paper de que un FNO ya es competitivo en esta tarea. El piso temporal es $E_K\approx0.047$.
- En $(X_0,W)\mapsto X$ (CI variable) el FNO **sí aprende el operador** CI$\to$solución
  (reduce el error un ~57% respecto al campo medio). La brecha con el paper se debe a la escala
  CPU (pocas CIs $N{=}400$ $\Rightarrow$ sobreajuste; *early-stopping* guarda el mejor checkpoint)
  frente a GPU + muchas más trayectorias + $K_{\text{Haar}}{=}256$ + ancho 64 del paper.
- **Nota de entrenamiento:** el paper usa `weight_decay=3e-4`, pero con la pérdida $L^2$
  **relativa** (gradiente normalizado) ese término domina y colapsa la red a salida nula
  (error $=1.0$); aquí se usa `weight_decay=0` + recorte de gradiente.

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
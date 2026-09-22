# CLASS with a CPL CDM Equation of State

This modified version of [CLASS](https://github.com/lesgourg/class_public/tree/v3.2.3)
(v3.2.3) gives CDM a time-dependent equation of state of the CPL form:

```math
w_{\mathrm{cdm}}(a)=w_{0,\mathrm{cdm}}+w_{a,\mathrm{cdm}}(1-a).
```

The corresponding density evolution is

```math
\frac{\rho_{\mathrm{cdm}}(a)}{\rho_{\mathrm{cdm},0}}
=a^{-3(1+w_{0,\mathrm{cdm}}+w_{a,\mathrm{cdm}})}
\exp\!\left[-3w_{a,\mathrm{cdm}}(1-a)\right].
```

Here $a=1$ today. Both parameters set to zero recover pressureless CDM.
The parametrization applies to dark matter, not dark energy. The repository
name is retained for continuity; the custom model is a variable-pressure CDM
model and does not add a decay interaction.

This differs from the [late-onset model](https://github.com/Cheng-Hanyu/class_cdm).
The [quadratic extension](https://github.com/Cheng-Hanyu/class_ddm_extension)
adds a second-order term to this equation of state.

## Key Modifications

Relative to official CLASS v3.2.3, the custom C-source and header changes are
confined to the following files:

- [`source/background.c`](source/background.c) — CDM density, pressure, equation of state and its derivative; `background_w_cdm()`; the `w_cdm` and `(.)p_cdm` background output columns.
- [`source/input.c`](source/input.c) — input parsing and default values for the model parameters listed below.
- [`source/perturbations.c`](source/perturbations.c) — CDM density and velocity perturbation equations in Newtonian and synchronous gauges, including the time derivative of the equation of state. The implemented CDM rest-frame sound speed is fixed to zero.
- [`include/background.h`](include/background.h) — model parameters, background indices and the `background_w_cdm()` declaration.

These are modifications of the CDM sector. CLASS's separate, built-in decaying
dark matter sector is not a new feature of this fork. No custom changes to
`source/thermodynamics.c`, `source/output.c`, `source/fourier.c` or
`source/harmonic.c` are needed for the modifications listed above.

## New Input Parameters

| Input name | Symbol | Meaning | Default |
| --- | --- | --- | --- |
| `w0_cdm` | $w_{0,\mathrm{cdm}}$ | Present-day CDM equation of state | `0.0` |
| `wa_cdm` | $w_{a,\mathrm{cdm}}$ | Coefficient of $(1-a)$; $dw_{\mathrm{cdm}}/da=-w_{a,\mathrm{cdm}}$ | `0.0` |

## Compilation

The standalone code requires a C compiler, a C++11 compiler and `make`.
The legacy Python wrapper also requires NumPy, Cython and setuptools in the
active Python environment. Use a separate environment to avoid replacing a
different CLASS installation.

```bash
make -j class                 # standalone executable
make -j all PYTHON=python     # executable, library and Python wrapper
```

The second command installs the wrapper into the active Python environment.
For an in-place wrapper build without installation:

```bash
make -j libclass.a
cd python
python setup.py build_ext --inplace
```

Compiler and OpenMP settings are controlled by the supplied `Makefile`.
See the [upstream installation documentation](https://github.com/lesgourg/class_public/wiki/Installation)
for platform-specific compiler configuration.

## Citation

If you use this code, please cite the original CLASS paper:

- D. Blas, J. Lesgourgues and T. Tram, *The Cosmic Linear Anisotropy Solving System (CLASS). II. Approximation schemes*, JCAP 07 (2011) 034, [arXiv:1104.2933](https://arxiv.org/abs/1104.2933).

Please also identify this repository and the commit used when describing the
modified model. Upstream author acknowledgements, citation requirements and
third-party notices continue to apply.

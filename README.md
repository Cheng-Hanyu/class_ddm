# CLASS with Decaying Dark Matter and Variable CDM Equation of State

This is a modified version of the [**CLASS**](https://github.com/lesgourg/class_public) code (v3.x). It extends the built-in decaying dark matter (DCDM) framework with a variable equation of state for cold dark matter (CDM), parameterized by the CPL form:

$$w_\mathrm{cdm}(a) = w_0 + w_a (1 - a)$$

## Key Modifications

The main changes from the original CLASS code are marked with `/* Hanyu */` in the source. Modified files:

- `source/background.c` — CDM pressure and EoS integration; new `background_w_cdm()` function
- `source/input.c` — reads `w0_cdm`, `wa_cdm` from input file
- `source/perturbations.c` — CDM perturbation equations with variable EoS
- `source/thermodynamics.c` — thermodynamics corrections
- `source/output.c` — outputs `w_cdm`, `(.)p_cdm` background quantities
- `include/background.h` — new `w0_cdm`, `wa_cdm` parameters and background indices

## New Input Parameters

Add to your `.ini` file:

```ini
w0_cdm = -0.0   # CDM EoS parameter w0 (default: 0 → standard CDM)
wa_cdm = 0.0    # CDM EoS parameter wa (default: 0 → standard CDM)
```

## Compilation

```bash
make clean
make -j class       # compile binary
make -j             # compile binary + Python wrapper
```

## Citation

If you use this code, please cite:
- **Cheng et al. (2025)** (in preparation)
- The original CLASS paper: [Blas et al. (2011)](https://arxiv.org/abs/1104.2933)

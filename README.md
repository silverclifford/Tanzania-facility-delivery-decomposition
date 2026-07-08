# Decomposing wealth and residential inequalities in facility delivery in Tanzania

Analysis code for a Blinder–Oaxaca-type decomposition of rural–urban and wealth-related
inequalities in health facility delivery among women in Tanzania, using the
2022 Tanzania Demographic and Health Survey and Malaria Indicator Survey (TDHS-MIS).

This repository contains the code that reproduces all descriptive estimates, the
rural–urban and richest–poorest decompositions, the wealth-by-residence interaction
analysis, and the sensitivity analysis reported in the manuscript.

---

## Study

- **Manuscript:** *Decomposing wealth and residential inequalities in facility delivery among women in Tanzania*
- **Author:** Clifford Silver Tarimo, Department of Science and Laboratory Technology, Dar es Salaam Institute of Technology, Tanzania
- **Data source:** 2022 TDHS-MIS, women's individual recode (`TZIR81FL.DTA`)

---

## Data availability

**The TDHS-MIS data are NOT included in this repository.** The DHS Program's terms of
use prohibit redistribution of the datasets. The data are freely available to
registered users from the DHS Program after a short application:

- https://dhsprogram.com/data/

Once approved, download the **Tanzania 2022** *Individual Recode* in Stata format
(`TZIR81FL.DTA`) and place it in the same folder as the notebook (or update the
`data_dir` path at the top of the notebook).

---

## Analytic approach

Facility delivery (delivery in a public, private, or religious/voluntary health
facility) was modelled with a **weighted linear probability model** estimated by
weighted least squares using the women's sampling weight. The rural–urban and
richest–poorest gaps were partitioned into an **explained (composition)** component
and an **unexplained** component using a regression-based **Blinder–Oaxaca-type
decomposition**:

- The explained component is E = Σ (x̄_A − x̄_B) · β̂, where x̄_A and x̄_B are the
  weighted mean covariate values in the advantaged and disadvantaged groups and β̂ is
  the coefficient vector from a pooled weighted linear probability model.
- The unexplained component is the remainder, U = Δ − E, where Δ is the total
  weighted gap.

The decomposition is implemented directly in `numpy` (no specialised decomposition
package is required).

**Uncertainty.** 95% confidence intervals for all prevalences and decomposition
components are obtained from a **cluster bootstrap** (1,000 replications) that resamples
primary sampling units within strata and applies the sampling weights, so that weighting,
clustering, and stratification are all reflected in the intervals. The notebook also
includes a formal wealth-by-residence interaction test (weighted model with a joint Wald
test based on the bootstrap covariance), linear-probability-model diagnostics, a
missing-data assessment, and sensitivity analyses (ANC 8+ threshold; excluding ANC).

### Variables

| Variable (role) | DHS source | Coding |
|---|---|---|
| Facility delivery (outcome) | `m15_1` | 1 = public/private/religious facility; 0 = home, other home, TBA premises, on the way, other |
| Residence | `v025` | Urban / Rural |
| Region | `v024` | 26 regions |
| Wealth quintile | `v190` | Poorest / Poorer / Middle / Richer / Richest |
| Maternal education | `v106` | No education / Primary / Secondary or higher (secondary and higher merged; higher was 1.1%) |
| ANC attendance | `m14_1` | Number of ANC visits; "no visits" = 0; "don't know" = missing; ANC 4+ vs <4 (ANC 8+ used in sensitivity) |
| Perceived distance | `v467d` | Big problem = 1 / Not a big problem = 0 |
| Sampling weight | `v005` | `v005` / 1,000,000 |
| Primary sampling unit | `v021` | 628 clusters |
| Sampling stratum | `v022` / `v023` | 61 strata |

---

## Environment and dependencies

- **Python 3.9.13**
- pandas
- numpy
- matplotlib
- scipy (chi-square p-value for the interaction test)

Exact versions used are listed in `requirements.txt`. To recreate the environment:

```bash
pip install -r requirements.txt
```

To check your own installed versions:

```python
import pandas, numpy, matplotlib, sys
print("Python:", sys.version.split()[0])
print("pandas:", pandas.__version__)
print("numpy:", numpy.__version__)
print("matplotlib:", matplotlib.__version__)
```

---

## Files

- `DHS_Project_1.ipynb` — main analysis notebook: environment, data loading, outcome and covariate construction, missing-data assessment, weighted prevalences with bootstrap 95% CIs (Table 1), both decompositions with CIs (Table 2), linear-probability-model diagnostics, the formal wealth-by-residence interaction test, sensitivity analyses (ANC 8+; excluding ANC), and figures
- `requirements.txt` — package versions
- `README.md` — this file

---

## How to run

1. Obtain `TZIR81FL.DTA` from the DHS Program (see **Data availability** above).
2. Place it in the repository folder, or edit `data_dir` at the top of the notebook to point to its location.
3. Open `DHS_Project_1.ipynb` in Jupyter and run all cells.

---

## Citation

If you use this code, please cite the manuscript (full citation to be added on
publication) and the DHS Program as the data source.

---

## License

Released under the MIT License (see `LICENSE`).

---

## Contact

Clifford Silver Tarimo — cliffordtarimo94@gmail.com

# Dynamic Factor VAR — Empirical Replication Code

## Overview

This repository contains the R and C++ code for the empirical application in:

Gorgi, P., Koopman, S. J., and Schaumburg, J. (2024). *Vector autoregressions with dynamic factor coefficients and conditionally heteroskedastic errors*. Journal of Econometrics.

The paper introduces a vector autoregressive (VAR) model with time-varying coefficients driven by latent dynamic factors and conditionally heteroskedastic errors.

The code implements the empirical specification with four dynamic factors and two autoregressive lags, using monthly U.S. macroeconomic and financial data.

## Repository contents

- `code/main_script.R` — Main script for data preparation, parameter estimation, model evaluation and calculation of standard errors.
- `code/tvvar_r_functions.R` — Supporting functions for model estimation and state-space calculations.
- `code/tvvar_cpp_functions5.cpp` — C++ functions for filtering and likelihood evaluation.
- `code/lyapunov_cond.cpp` — C++ function for evaluating model stability using a Lyapunov-based simulation procedure.

## Data

The empirical application uses four monthly series obtained from the Federal Reserve Bank of St. Louis (FRED):

| FRED series | Description |
|---|---|
| [INDPRO](https://fred.stlouisfed.org/series/INDPRO) | Industrial Production Index |
| [CPIAUCSL](https://fred.stlouisfed.org/series/CPIAUCSL) | Consumer Price Index |
| [BAA10YM](https://fred.stlouisfed.org/series/BAA10YM) | Moody's Baa corporate bond spread relative to the 10-year Treasury yield |
| [USREC](https://fred.stlouisfed.org/series/USREC) | U.S. recession indicator |

The original data used for the paper cover April 1953 to June 2022. The analysis uses data from December 1969 onward to construct the monthly observations beginning in January 1970.

Industrial production and consumer prices are transformed into monthly log differences multiplied by 100. The corporate bond spread is used in levels. The three model variables are subsequently demeaned. The main script also removes initial observations for lag alignment.

### Data availability

The original data are not included in this repository because some underlying series, particularly the Moody's data used in `BAA10YM`, are subject to redistribution restrictions.

Researchers should download the series directly from FRED, subject to the applicable terms of use.

The main script expects a file named `full_data_updated.csv` in the `data/` folder. This file should contain:

- Column 1: observation dates in `YYYY-MM-DD` format
- Column 2: `INDPRO`
- Column 3: `CPIAUCSL`
- Column 4: `BAA10YM`
- Column 5: `USREC`

The input file must use semicolons as field separators and commas as decimal separators, consistent with the `read.table()` command in the main script. Observations should be ordered chronologically and include December 1969 through June 2022.

Because FRED series can be revised, newly downloaded data may differ from the historical values used in the published analysis.

## Software requirements

The analysis is implemented in R and C++, using Rcpp and RcppArmadillo.

The required R packages are:

- `vars`
- `Rcpp`
- `RcppArmadillo`
- `numDeriv`
- `matrixcalc`
- `dplyr`
- `MASS`

A working C++ compiler compatible with R is required.

## Running the analysis

1. Download or clone this repository.
2. Obtain the four series from FRED and prepare `data/full_data_updated.csv` in the format described above.
3. Open R or RStudio and set the working directory to the repository's main folder.
4. Run `code/main_script.R`.

The main script loads the supporting R and C++ functions, transforms the data, estimates the model by numerical optimisation and calculates information criteria and standard errors.

The default specification uses four dynamic factors, two VAR lags and demeaned data.

## Reproducibility

The code documents the empirical estimation procedure used in the paper. Exact numerical reproduction depends on the availability of the original historical data values and a compatible software environment.

## Funding

This research was supported by the Netherlands Organisation for Scientific Research (NWO), Vidi grant VI.Vidi.191.169.

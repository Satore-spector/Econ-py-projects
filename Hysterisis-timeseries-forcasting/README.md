# Unemployment Hysteresis

Small project looking at whether shocks to the unemployment rate (UNRATE) actually fade out over time, or whether they stick around permanently (that's the "hysteresis" idea). Using average hourly earnings (AHETPI) and real GDP (GDPC1) as controls. Data is quarterly, from FRED.

I basically ran a bunch of different time series models on the same data and compared them, instead of committing to one model upfront.

## Data

`merged_clean_data.csv` - quarterly, columns are:
- `DATE`
- `UNRATE` - unemployment rate
- `AHETPI` - average hourly earnings
- `GDPC1` - real GDP

Pulled from FRED and merged/cleaned before this notebook. Sample runs 1964 Q1 to 2025 Q4 (248 quarters).

## Models

All in `hysteresis_unrate_models.ipynb`:

1. AR(1)
2. MA(1)
3. ARMA(1,4)
4. ARIMA(0,1,0) - random walk on the differenced series, this is the main test for hysteresis. if UNRATE is close to a pure random walk, shocks don't die out
5. DL(4) - distributed lag on AHETPI and GDPC1
6. ADL(1,1) - autoregressive distributed lag
7. ARDL(4,3,3) - bigger ARDL, also computes long-run multipliers
8. VAR(5) - 3-variable system, UNRATE/AHETPI/GDPC1
9. VECM(r=1) - error correction version of the VAR, cointegration rank 1. the alpha coefficient here is basically the speed of adjustment back to the long-run relationship

Model fit stats (AIC/BIC/log-lik or R2 depending on the model) are printed as I go and then collected into one summary table at the end.

## Plots

Added a plots section at the end using matplotlib:
- the three raw series over time
- AR(1) fitted vs actual UNRATE
- residuals from the random walk model (time series + histogram)
- AIC comparison bar chart across the comparable models
- VAR(5) impulse response of UNRATE to a shock in each variable

## Running it

```
pip install numpy pandas matplotlib statsmodels
```

Then just run the notebook top to bottom. Needs `merged_clean_data.csv` in the same folder.

## Notes

Not a polished research paper, just working through the different model classes we covered and seeing which ones actually say something sensible about whether unemployment shocks are permanent or not. ARIMA(0,1,0) and the VECM alpha coefficient are the two things I'd look at first if you want the short answer.

# Week 11.ipynb (Colab)


## What this notebook does

A time series regression exercise on Apple's quarterly revenue:

1. **Load data** — reads `qSales_2024.csv` (Compustat-style quarterly financials: `gvkey`, `datadate`, `fyearq`, `fqtr`, `tic`, `conm`, `saleq`, etc.), 277 rows × 16 columns.
2. **Convert date column** — casts `datadate` to datetime.
3. **Filter to Apple** — subsets to `tic == 'AAPL'`, 92 quarterly rows (2001 Q1 through 2024 Q1).
4. **Visualize** — plots `saleq` vs. `datadate` (blue line with circle markers). The chart shows a clear upward trend with regular seasonal spikes; the notebook's own comment notes these as "spikes from new product launches."
5. **Add a time trend variable** — creates `time` = 1, 2, 3, ... across the 92 rows (triggers a `SettingWithCopyWarning` since `apple_sales` is a filtered slice of `qSales`).
6. **Train/test split** — first 75% of rows as `dt4training`, last 25% as `dt4testing` (the split is created but the test set is never actually used in this PDF).
7. **Fit OLS model** — regresses `saleq` on `time` (plus constant) using `statsmodels`. Fitted equation from the output: `revenue = -13,536.82 + 1,077.61 * time`.
8. **Prediction interval** — calls `get_prediction(x).summary_frame(alpha=0.2)` to get an 80% prediction interval for the training-period fitted values. The notebook's own comment notes "higher confidence intervals means the ranges will also be wider" and recommends 80% confidence.


## Requirements

- Python with `pandas`, `numpy`, `matplotlib`, `statsmodels`
- `qSales_2024.csv` in the working directory (not included in this PDF)

## Known issue

- `apple_sales['time'] = range(1, len(apple_sales)+1)` triggers a `SettingWithCopyWarning` because `apple_sales` is a `.loc` slice of `qSales`, not a copy. It still runs correctly, but pandas recommends `.copy()` or `.loc[row_indexer, col_indexer] = value` to avoid the warning.

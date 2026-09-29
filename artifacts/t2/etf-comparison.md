# T2 ETF Comparison: SPY, TLT, GLD — Fees and Maximum Drawdown

This report upgrades the T1 illustrative comparison (synthetic snapshot) to the T2 ETF Data Pack: it uses real, provider-disclosed annual expense ratios and maximum drawdowns calculated from the daily `adjusted_close` series in the common 2016-09-01 to 2026-08-31 window.

## Source and method

- **Input files:** `data/t2/daily_prices.csv` (7,536 daily observations), `data/t2/fund_info.csv` (three disclosed fund records), `data/t2/data_dictionary.md` (definitions).
- **Preparation date:** 2026-09-25 (T2 ETF Data Pack).
- **Fee disclosures (accessed 2026-09-25):** SPY — fund-information as-of date 2026-09-10, fee-specific effective date **not stated** (gross of waivers/reimbursements); TLT — current prospectus, fee-panel date **not stated**; GLD — **not stated** in the selected field.
- **Drawdown window / frequency / basis:** common period 2016-09-01 through 2026-08-31 (inclusive), 2,512 observations per ETF, US market daily sessions (no weekend/full-closure rows), values are provider `adjusted_close` (split and dividend-distribution adjustments), not an independently constructed total-return index.
- **Calculation:** `artifacts/t2/calculate_drawdown.py`, executed as `python3 artifacts/t2/calculate_drawdown.py`. Input checks passed: only SPY/TLT/GLD, unique ticker/date pairs, strictly ascending dates, positive finite prices, 2,512 rows per ETF, identical date sets and common first/last dates. Method: cumulative maximum of `adjusted_close` from window start; `drawdown_t = (adjusted_close_t / high_t − 1) × 100`; maximum drawdown = minimum of the series. Earliest-trough / earliest-peak tie rule; full precision kept internally, two decimals shown.

## 1. Comparison

| Ticker | Annual expense ratio (%) | Maximum drawdown (%) | Peak date | Trough date |
|---|---:|---:|---|---|
| SPY | 0.0945 | −33.72 | 2020-02-19 | 2020-03-23 |
| TLT | 0.15 | −48.35 | 2020-08-04 | 2023-10-19 |
| GLD | 0.40 | −26.40 | 2026-01-29 | 2026-07-16 |

## 2. Observation

**GLD (−26.40%) has the smallest drawdown loss (closest to zero) in this period**, followed by SPY (−33.72%) and TLT (−48.35%). These drawdowns were calculated here from the `adjusted_close` series, not read from a precomputed snapshot. Comparing the displayed two-decimal results, the three values are distinct — no ties. One limitation: these are daily-close drawdowns within the fixed 2016–2026 window only, and the fee figures are current disclosure snapshots (not ten-year averages and not all-time risk measures).

## 3. Agent Check

This is my own self-check; it is not an independent verification, and the student did not check these data.

- **Chosen ETF:** SPY.
- **Source rows re-read from `data/t2/daily_prices.csv`:** peak `2020-02-19` → `adjusted_close = 307.6394958496094`; trough `2020-03-23` → `adjusted_close = 203.91189575195312`. Peak index ≤ trough index confirmed.
- **Check command (verbatim):**

```bash
python3 -c "
import csv, math
rows = list(csv.DictReader(open('data/t2/daily_prices.csv', newline='', encoding='utf-8')))
spy = [(r['date'], float(r['adjusted_close'])) for r in rows if r['ticker'] == 'SPY']
print('SPY rows:', len(spy))

# 1) Re-read the two reported rows directly from the CSV
pk = next((d, a) for d, a in spy if d == '2020-02-19')
tr = next((d, a) for d, a in spy if d == '2020-03-23')
print('Reported peak row :', pk)
print('Reported trough row:', tr)
print('Peak index <= trough index:', spy.index(pk) <= spy.index(tr))
print('Two-point check (trough/peak - 1)*100 =', repr((tr[1]/pk[1] - 1.0)*100.0))

# 2) Independent full-series recomputation of max drawdown
best = 0.0; cum_high = -math.inf
for d, a in spy:
    if a > cum_high: cum_high = a
    dd = (a / cum_high - 1.0) * 100.0
    if dd < best: best = dd
print('Independent full-series min drawdown_pct =', repr(best))
print('Display:', f'{best:.2f}')
"
```
- **Actual output:** two-point `(trough / peak − 1) × 100 = −33.717257210811404`; independent full-series minimum `= −33.717257210811404`; displayed `−33.72`.
- **Comparison / correction:** both results match the report's full-precision value (−33.717257210811404) exactly; no correction or re-run needed. The full-series method (all dates, `adjusted_close`, cumulative highs, peak-before-trough order) was inspected and is consistent with the reported pair, and the observation holds against all three displayed results.

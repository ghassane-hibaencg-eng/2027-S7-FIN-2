<div align="center">

<img src="images/logo_hassan1.png" alt="Hassan 1st University" height="110"> &nbsp;&nbsp;&nbsp;&nbsp; <img src="images/logo_encg.png" alt="ENCG Settat" height="110">

**Hassan 1st University**
**National School of Business and Management (ENCG) Settat**

# Data Science Project

## EUR/USD exchange rate volatility: can the past predict the size of future movements?

<img src="images/euro_dollar.png" alt="Dollar - Euro" width="380">

**Prepared by:** *(student names)*

**Supervised by:** Mr Abderrahim LARHLIMI

</div>

---

# Forecasting the Size of EUR/USD Exchange Rate Movements

## Table of contents

1. [Introduction and problem statement](#1-introduction-and-problem-statement)
2. [Method](#2-method)
3. [Finding: volatility is persistent](#3-finding-volatility-is-persistent)
4. [Results: GARCH(1,1) is the best model](#4-results-garch11-is-the-best-model)
5. [Limitations and ways to improve](#5-limitations-and-ways-to-improve)
6. [Conclusion](#conclusion)

---

## 1. Introduction and problem statement

Can past exchange rate movements help predict the size of future ones? The question matters for measuring currency risk and sizing a hedge. The direction of a move (up or down) is almost unpredictable; the magnitude of fluctuations, however, may retain some memory. The problem is therefore to forecast, one day ahead, the size of the EUR/USD move rather than its direction.

To do so, we measure size by the absolute value of the return $|r|$, which serves as a proxy for volatility, and we compare four approaches: the historical average volatility (benchmark), a linear regression, a Gradient Boosting model and a GARCH(1,1) model.

## 2. Method

The data come from the U.S. Federal Reserve through the FRED database (series DEXUSEU) and give the number of dollars per euro, recorded every business day from 4 January 1999 to 30 October 2026. The 281 days without a quotation, mostly public holidays, are removed, which yields 6,978 logarithmic returns computed as:

$$r_t = 100 \times \ln\left(\frac{S_t}{S_{t-1}}\right)$$

Over this period, the exchange rate ranges between 0.83 and 1.60 dollars, with no outliers. Finally, the mean absolute return is 0.42 % per day, with a standard deviation of 0.57 %.

<p align="center">
  <img src="images/fig1_taux_rendements.png" alt="Exchange rate, returns and absolute returns" width="85%">
</p>

*Figure 1 – EUR/USD exchange rate, daily logarithmic return and absolute value of the return.*

- **Exchange rate (panel 1):** The euro starts at about $1.17 in 1999, falls to $0.83 around 2000–2001, then climbs to $1.60 in mid-2008, before fluctuating between $1.05 and $1.25 since 2015. Two other breaks are visible: the sharp drop of 2014–2015, from $1.39 to $1.05, and the fall below parity in 2022, to a level of about $0.96. These large swings justify working with returns rather than prices.
- **Returns (panel 2):** Returns fluctuate around zero with no particular trend, but their amplitude changes over time. It is relatively low in 2005–2007, 2013–2014 and 2017–2019, and becomes much larger in 2008–2009. This shows that volatility is not constant over time.
- **Absolute values of returns (panel 3):** High values are concentrated in certain periods, notably 2008–2009, with a maximum of 4.6 %, then 2010–2012, 2015–2016, 2020 and 2022–2023. This means that a day with a large move tends to be followed by other turbulent days, which highlights the phenomenon of volatility clustering.

## 3. Finding: volatility is persistent

If the past helps predict the size of movements, volatility must have memory. The autocorrelation of the return is almost zero (0.008 at lag 1): the past says nothing about the sign. That of $|r|$ is, on the contrary, positive and decays very slowly (about 0.10 at lags 2 to 10, 0.13 at 22 days and still 0.09 at 60 days).

<p align="center">
  <img src="images/fig2_autocorrelation.png" alt="Autocorrelation of r and |r|" width="100%">
</p>

*Figure 2 – Autocorrelation of $r$ (left, almost zero) and of $|r|$ (right, positive and persistent).*

The left-hand chart shows that returns display almost no autocorrelation, as most bars stay within the confidence band. This means that today's return cannot be used to predict tomorrow's. By contrast, the right-hand chart shows a positive autocorrelation of absolute returns, which declines slowly, even after 60 days. This means that large moves tend to cluster together, just like periods of low volatility.

Market regimes confirm this, with an annualised volatility of 16.8 % during the 2008–2009 financial crisis and 10 % during the debt crisis and the ECB–Fed divergence, against 6 to 7 % between 2017 and 2020 and since 2023. Volatility thus retains a relatively long memory, which justifies forecasting future movements from past absolute returns.

## 4. Results: GARCH(1,1) is the best model

| Model                                     | RMSE (%)   | QLIKE       | RMSE/benchmark | RMSE gain (%) |
|-------------------------------------------|:----------:|:-----------:|:--------------:|:-------------:|
| **GARCH(1,1)**                            | **0.3154** | **−0.5592** | **0.928**      | **7.21**      |
| Linear regression                         | 0.3173     | −0.5429     | 0.934          | 6.65          |
| Gradient boosting                         | 0.3173     | −0.5392     | 0.934          | 6.65          |
| Historical average volatility (benchmark) | 0.3400     | −0.3669     | 1.000          | 0.00          |

*Table 1 – Performance on the 2015–2026 test set (lower = better).*

<p align="center">
  <img src="images/fig3_rmse_qlike.png" alt="RMSE and QLIKE by model" width="100%">
</p>

*Figure 3 – Model comparison by RMSE (left) and QLIKE (right).*

**Interpretation:** The results show that the three models that use past information outperform the benchmark based on historical average volatility. The GARCH(1,1) model achieves the best performance, with an RMSE of 0.3154 and a QLIKE of −0.5592, an RMSE improvement of 7.21 % over the benchmark. Linear regression and gradient boosting give almost identical results, which suggests that the relationship between past movements and future volatility is fairly simple and does not necessarily require complex models. GARCH(1,1) therefore appears to be the best-performing and most suitable model for volatility forecasting. Its superiority is confirmed by the Diebold-Mariano test ($p < 0.0001$) and holds in all six situations studied, including when the Covid period is excluded.

<p align="center">
  <img src="images/fig6_prevision_vs_realisee.png" alt="Forecast vs realised volatility" width="100%"><br><br>
  <img src="images/fig6_zooms.png" alt="Zooms on Covid-19 and 2022" width="100%">
</p>

*Figure 6 – Volatility forecast the previous day versus realised volatility, 2015–2026, with zooms on Covid-19 (2020) and 2022.*

The chart highlights the ability of the three models to track the evolution of realised volatility over 2015–2026. Overall, the forecasts reproduce the main trends correctly, notably the rise observed in 2020–2021, the return to lower levels in 2024–2026 and the fluctuations recorded during crisis periods. However, some differences appear depending on the events studied. During the Covid-19 period, the models underestimate the volatility peak: their forecasts stay around 0.25 %, while realised volatility reaches about 0.77 % in March 2020. Likewise, during the war in Ukraine in 2022, the forecasts tend to underestimate volatility surges and to react with some delay. These gaps show that, despite their ability to anticipate general trends, the models perform less well in the face of exceptional shocks and sudden changes in market conditions.

## 5. Limitations and ways to improve

- **Source:** a single daily reading from the Fed, not a closing price at a fixed time. This is an additional source of noise.
- **Public holidays:** a return computed after a holiday covers two calendar days.
- **Regimes:** defined after the fact, hence usable for diagnosis only, never as an explanatory variable.
- **Horizon:** one day only.
- **Next steps:** an asymmetric GARCH (GJR, EGARCH) or a HAR regression; external variables (ECB/Fed rate gap, VIX); forecasts at 5 or 22 days, where noise weighs less; an intraday realised volatility, much less noisy than $|r|$.

## Conclusion

Yes, but with limits. The past helps predict the size of EUR/USD movements, and this is statistically very robust. It does not, however, help predict their direction.

1. **Source of predictability:** it is the persistence of volatility (figure 2): turbulent and calm periods last. An average over a few weeks captures most of it.
2. **Real but modest gain:** about 7 % in RMSE and 0.17 to 0.19 QLIKE points relative to the historical average. The quality ceiling of the oracle is close to the benchmark, which limits any gain.
3. **Choice of model:** a simple model is enough: linear regression and boosting are equivalent. GARCH(1,1) is the best on both metrics, but its advantage is marginal.
4. **Unpredictable shocks:** the models are least useful when a forecast would matter most (Covid, central bank decisions), because they react after the shock.
5. **Use:** useful for sizing a risk or a hedge, especially in calm periods. Useless for betting on the direction of the exchange rate.

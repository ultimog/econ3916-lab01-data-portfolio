# econ3916-lab01-data-portfolio
# The Data Portfolio — Big Mac Index Analysis

## Objective

An empirical assessment of Purchasing Power Parity using The Economist's Big Mac Index, examining whether observed deviations between market exchange rates and burger-implied parity rates represent exploitable currency mispricing or structural differences in non-tradeable input costs.

## Data

The Economist's Big Mac Index, sourced from the publisher's public GitHub repository. The panel comprises 2,056 country-period observations across 57 country labels and 45 survey periods, spanning April 2000 to July 2026. The July 2024 cross-section contains 54 countries.

## Methodology

- Ingested the full panel directly from the upstream CSV, with explicit datetime parsing on the survey date field to preserve ordering and enable temporal aggregation.
- Decomposed the panel into its constituent data structures — cross-sectional (54 countries at a single date), time series (one country across 45 periods), and the full unbalanced panel — to establish which structure supports which class of inferential question.
- Computed implied PPP exchange rates as the ratio of local Big Mac price to the contemporaneous US benchmark price, and valuation percentages as the proportional deviation of implied PPP from the prevailing market rate.
- Extended the valuation calculation across all 45 periods by merging the period-specific US benchmark onto every country-period observation, rather than holding the benchmark fixed at a single cross-section. This avoids comparing historical local prices against a contemporary US price.
- Diagnosed missing data at two levels: column-wise null counts, and panel completeness by comparing each country's observation count against the maximum available periods.
- Classified missingness mechanisms as MCAR, MAR, or MNAR on a per-country basis, examining the temporal distribution of each country's gaps rather than the gap count alone.
- Produced two visualizations: a sorted horizontal bar chart of the July 2024 valuation cross-section, and a multi-country time series of valuation deviations over the full sample period.

## Key Findings

**Systematic undervaluation against the US benchmark.** The median valuation in the July 2024 cross-section is −20.7%, with only 6 of 54 currencies registering above the US price level. This asymmetry is inconsistent with a naive reading of PPP as a short-run equilibrium condition and is more parsimoniously explained by the Balassa-Samuelson effect: a Big Mac is composed predominantly of non-tradeable inputs — local labour, commercial rent, and domestic ingredients — whose prices track national productivity and cannot be arbitraged internationally.

**Persistent extremes rather than mean reversion.** Switzerland records the largest positive deviation in the July 2024 cross-section at +41.8%, and has held the most-overvalued position in every period since January 2015. The consistency of this ranking across a decade of observations argues against interpreting the deviation as a correctable mispricing.

**Repricing is directional for some currencies and cyclical for others.** Time series analysis of Britain, Taiwan, Argentina, and South Korea reveals materially different dynamics under a single summary statistic. Taiwan exhibits the largest sustained repricing, declining monotonically from approximately −30% in 2010 to roughly −60% by the end of the sample. Argentina displays the greatest volatility, swinging across a range exceeding 50 percentage points on multiple occasions while terminating near its starting level — a pattern consistent with recurring currency crises and administered exchange rate regimes rather than gradual price-level convergence. Sterling's discontinuity in 2008 is attributable to the financial crisis rather than to any change in UK non-tradeable costs.

**The panel is materially unbalanced, with non-random attrition.** Only 25 of 57 country labels are observed in all 45 periods. Russia's series terminates at January 2022 after 36 observations, coinciding with McDonald's withdrawal from the market following the invasion of Ukraine — an event that simultaneously moved the rouble sharply. The missingness and the unobserved values are therefore jointly determined, making this MNAR and implying that complete-case analysis would yield biased estimates of average PPP deviation rather than merely less precise ones.

**Two distinct classes of missingness.** Seven GDP-derived columns each contain exactly 254 nulls, reflecting a common set of observations lacking the income data required for the productivity-adjusted index. This is analytically distinct from a country's absence from the survey entirely, and the two should not be pooled when reasoning about coverage.

**A data quality defect in the country identifier.** The `name` field contains both `UAE` (22 observations) and `United Arab Emirates` (17 observations) as separate labels for a single economy. Grouping on this field therefore overstates the country count and understates panel completeness for the affected series. Aggregation keyed on the ISO alpha-3 code would be robust to this inconsistency.

## Limitations

The index measures a single narrowly-defined product and is best understood as a comparison of relative price levels rather than a signal of currency misvaluation. The choice of the United States as numeraire mechanically determines the sign distribution of reported deviations; an alternative benchmark would invert much of the apparent asymmetry. The dataset includes productivity-adjusted columns which this analysis does not exploit; incorporating them would allow valuation to be assessed against the level predicted by income per capita rather than against the US directly.

---
theme: default
layout: cover
highlighter: shiki
colorSchema: light
favicon: favicon/url
title: How we used Polars to build functime, a next gen ML forecasting library
---

# 🔮 functime
How we used **Polars** and **global forecasting** to build\
a **next-generation ML forecasting library**

<div class="absolute bottom-10">

    👤 Luca Baggi
    💼 ML Engineer @xtream
    🛠️ Maintainer @functime

</div>

<div class="absolute right-5 top-5">
<img height="150" width="150"  src="/qr-github.svg">
</div>


---

# 📍 Talk outline

<v-clicks>

### 🔮 The problem with forecasting

### 🐻‍❄️ What is Polars?

### 🚀 Why is Polars so fast?

### 🌏 Why global forecasting?

### 💻 A forecasting workflow with `functime`

### 🔎 Diagnostic tools

### 🙌 Wrapping up

</v-clicks>


---

# 🔮 The problem with forecasting
A new paradigm to evaluate the forecasting process

<v-clicks>

*We spend far too many resources generating, reviewing, adjusting, and approving our forecasts, while almost **invariably failing to achieve the level of accuracy desired**. The evidence now shows that a large proportion of typical business forecasting efforts fail to improve the forecast, or even make it worse. So the conversation needs to change. The focus needs to change.*

*We need to **shift our attention from esoteric model building to the forecasting process itself – its efficiency and its effectiveness**.*

<p align=right>
<a href="https://blogs.sas.com/content/forecasting/2016/10/25/changing-the-paradigm-for-business-forecasting-part-10/">Mike Gilliland</a>
<br>
Board of Directors of the International Institute of Forecasters
</p>

</v-clicks>


---

# 🔮 The problem with forecasting
Reframe the problem

Make forecasting just work at a **reasonable scale** (~90% of use cases):

1. Forecast **thousands of time series** without distributed systems (PySpark).
2. Provide adequate **diagnostic tools**.
2. Smoothly translate form experimentation to production.

<v-click>

🫢 Spoiler alert: we used Polars and global forecasting

</v-click>

<!--
* fit on your local laptop
-->

---

# 🐻‍❄️ What is Polars?
The three plus one key elements

***Dataframes** powered by a **multithreaded**, vectorized **query engine**, written in Rust*

<v-clicks>

1. A dataframe frontend: work with a Python object and not a SQL table.
2. Utilises **all cores** on your machine, **efficiently** (more on this later).
3. Uses 50+ years of relational database research to **optimise the query**.
4. **In-process**, like sqlite (OLTP), duckdb (OLAP) and LanceDB (vector).

</v-clicks>


---

# 🚀 Why is Polars so fast?
The key ingredients

<v-clicks>

1. Efficient **data representation and I/O** with Apache Arrow.
2. Work stealing, AKA **efficient multithreading**.
3. **Query optimisations** through lazy evaluation.

</v-clicks>


---

# 🚀 What makes Polars so fast?
Apache Arrow

A [memory format specification](https://arrow.apache.org/docs/format/Columnar.html), i.e. how to **represent data in memory**, with implementations in a lot of programming languages (e.g. PyArrow).

<v-clicks>

* Column based representation that plays well with SIMD.
* Can represent missing values (unlike `numpy`).
* Nested data types as first-class citizens.

</v-clicks>


---

# 🚀 What makes Polars so fast?
Effective multithreading

<v-clicks>

Polars leverages _work stealing_ so that **no thread is idle if other threads are working**.

A **work scheduler** assigns tasks to each thread. When a thread is idle, the job scheduler takes jobs from other queues and assigns them to it.

</v-clicks>


---

# 🚀 What makes Polars so fast?
Query optimisations

<v-clicks>

The query is *compiled* into a *plan* which is then optimised.

For example, in pandas a `DataFrame.sort("col1").head()` will be eagerly executed:

1. _All rows_ are sorted.
2. The top 5 rows are returned.

</v-clicks>


---

# 🚀 What makes Polars so fast?
Query optimisations


The query is *compiled* into a *plan* which is then optimised.


In Polars, the query optimiser recognises this pattern and uses a *top-k search* algorithm to retrieve just what you need.

<v-click>

For a more thorough overview, watch [Polars' creator latest talk](https://www.youtube.com/watch?v=tqcudsykOGc&t=1916s).

</v-click>

---

# 🌏 Why global forecasting?
A lesson from forecasting competitions

<v-clicks>

Global forecasting simply means fitting a single model on all the time series in your panel dataset.

This approach proved successful in multiple forecasting competitions, most notably M4 ([1](https://www.sciencedirect.com/science/article/abs/pii/S0169207020301850) [2](https://www.sciencedirect.com/science/article/abs/pii/S0169207020301850)) and M5 ([1](https://www.sciencedirect.com/science/article/pii/S0169207021001874#b47)).

</v-clicks>

---

# 🌏 Why global forecasting?
A lesson from forecasting competitions

<v-clicks>

**Gradient boosted regression trees** were fit on tens or even hundreds of thousands of time series and reported great performance, as we are accustomed to.

But global forecasting works well with linear models too, with some **thoughtful and deliberate feature engineering**.

</v-clicks>


---

# 💻 A forecasting workflow with `functime`
Let's get our hands dirty!

`functime` can simply be pip installed:

```bash
# inside a virtual environment
pip install functime
```

---

# 💻 A forecasting workflow with `functime`
Let's get our hands dirty!

Let's import some demo data.

```python
import functime
import polars as pl

url = "https://github.com/TracecatHQ/functime/raw/main/data/commodities.parquet"
y = (
  pl.scan_parquet(url)
  .with_columns(pl.col("time").cast(pl.Date))
)
```

---

# 💻 A forecasting workflow with `functime`
How the data must look like

A functime dataset is just a regular Polars DataFrame, but assumes to have three columns:

* An identifier for the time series (e.g. SKU).
* The time stamp.
* The target value.

Plus all external covariates.

---

# 💻 A forecasting workflow with `functime`
How the data must look like

```
print(y.head().collect())

output
shape: (5, 3)
┌────────────────┬────────────┬────────┐
│ commodity_type ┆ time       ┆ price  │
│ ---            ┆ ---        ┆ ---    │
│ str            ┆ date       ┆ f64    │
╞════════════════╪════════════╪════════╡
│ Aluminum       ┆ 1960-01-01 ┆ 511.47 │
│ Aluminum       ┆ 1960-02-01 ┆ 511.47 │
│ Aluminum       ┆ 1960-03-01 ┆ 511.47 │
│ Aluminum       ┆ 1960-04-01 ┆ 511.47 │
│ Aluminum       ┆ 1960-05-01 ┆ 511.47 │
└────────────────┴────────────┴────────┘
```

---

# 💻 A forecasting workflow with `functime`
Plotting and data manipulation

All data manipulation and plotting methods in functime work out of the box with panel datasets: manually calling `group_by` is not required.

```python
from functime import plotting

plotting.plot_panel(
  data=y
  # accepts all kwargs as plotly
  title=f"Bottom {k} series by coefficient of variation",
  height=125*k,
)
```


---

# 💻 A forecasting workflow with `functime`
Train - Test splitting

Data splitters work with panel datasets out of the box:

```python
from functime.cross_validation import train_test_split

splitter = train_test_split(forecasting_horizon)
y_train, y_test = splitter(y)
```


---

# 💻 A forecasting workflow with `functime`
Define and train a model: a familiar interface

The modeling workflow resembles scikit-learn's:

```python{all|6-9|11|12}
from functime.forecasting import snaive

frequency = "1mo"
forecasting_horizon = 12

forecaster_naive = snaive(
    freq=frequency,
    sp=12,
)

_ = forecaster_naive.fit(y=y_train)
y_bench_pred = forecaster_naive.predict(fh=forecasting_horizon)
```


---

# 💻 A forecasting workflow with `functime`
Define and train a model: a familiar interface

And also offers a `Transformer`/`Pipeline`-like structure to apply data transformations to prevent data leakages when doing cross validation:

```python{all|3|4-8}
from functime.preprocessing import scale, add_fourier_terms, roll

target_transforms = scale()
feature_transforms = roll(
  window_sizes=(6, 12),
  stats=("mean", "std"),
  freq=freq
)
```


---

# 💻 A forecasting workflow with `functime`
Define and train a model: a familiar interface

And also offers a `Transformer`/`Pipeline`-like structure to apply data transformations to prevent data leakages when doing cross validation:

```python{all|5|6|7}
from functime.forecasting import linear_model

forecaster_linear = linear_model(
    freq=freq,
    lags=12,
    target_transform=target_transforms,
    feature_transform=feature_transforms,
)
```

---

# 💻 A forecasting workflow with `functime`
Some commodities

Cross validation is a built-in method in the forecaster:

```python
y_preds, y_resids = forecaster_linear.backtest(
    y=y_train,
    window_size=60, # 5 years of training data in each fold
    test_size=forecasting_horizon,
    step_size=1,
    n_splits=5
)
```


---

# 💻 A forecasting workflow with `functime`
Some commodities

The same goes for generating prediction intervals with conformal predictions:

```python
y_pred_quantiles = forecaster_linear.conformalize(
    y_pred=y_pred,
    y_preds=y_preds,
    y_resids=y_resids,
    alphas=[0.1, 0.9]
)
```

---

# 🔎 Diagnostic tools
The problem with diagnostics with thousands of time series

functime also offers a plethora of methods to *display* and *rank* forecasts across all time series:

```python
from functime.evaluation import rank_residuals, rank_point_forecasts

rank_bench_point_forecast = rank_point_forecasts(
    y_true=y_test,
    y_pred=y_pred,
    sort_by="mape", # a dozen point metrics, including over/underforecast
    descending=True,
)
```

And we're adding support to sample the series or display the top `k` according to a metric.

---

# 🔎 Diagnostic tools
Plotting functions that work with panel datasets

`plotting.plot_backtests` supports natively cross-validation predictions:


```python
plotting.plot_backtests(
    y_train,
    y_linear_preds,
)
```


---

# 🔎 Diagnostic tools
Plotting functions that work with panel datasets

But we also have functions to glance error metrics across time series:

```python
plotting.plot_residuals(y_backtest_residuals)

```

(Histogram and rug plot of residuals, compatibile with backtesting residuals)

---

# 🔎 Diagnostic tools
Plotting functions that work with panel datasets

Forecast value-added plot (i.e. compare residuals against residuals of a naive method).

```python
plotting.plot_fva(y_backtest_residuals)
```

---

# 🔎 Diagnostic tools
Plotting functions that work with panel datasets

Scatterplot of predictions against the coefficient of variation.

```python
plotting.plot_comet(
    y_train=y_train, y_test=y_test, y_pred=y_pred, height=900, width=900
)
```

---

# 🙌 Wrapping up


---
layout: intro
---

# 🙏 Thank you!

Please share your feedback! My address is lucabaggi [at] duck.com

<div class="absolute right-5 top-5">
<img height="150" width="150"  src="/qr-linkedin.svg">
</div>

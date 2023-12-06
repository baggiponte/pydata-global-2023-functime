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

### 📈 `functime`'s answer

### 🐻‍❄️ What is Polars?

### 🌏 What is global forecasting?

### 💻 A forecasting workflow with `functime`

### 🔎 Diagnostic tools

### 🔌 References

</v-clicks>


---

# 🔮 The problem with forecasting
A new paradigm to evaluate the forecasting process

*"We spend far too many resources generating, reviewing, adjusting, and approving our forecasts, while almost **invariably failing to achieve the level of accuracy desired**."* <a href="https://blogs.sas.com/content/forecasting/2016/10/25/changing-the-paradigm-for-business-forecasting-part-10/"><i>(source)</i></a>

<div align="right">

Mike Gilliland<br>
Board of Directors of the International Institute of Forecasters
</div>


---

# 🔮 The problem with forecasting
A new paradigm to evaluate the forecasting process

*"The focus needs to change. We need to shift our attention **from esoteric model building to the forecasting process itself** – its **efficiency** and its **effectiveness**."* <a href="https://blogs.sas.com/content/forecasting/2016/10/25/changing-the-paradigm-for-business-forecasting-part-10/"><i>(source)</i></a>

<div align="right">

Mike Gilliland<br>
Board of Directors of the International Institute of Forecasters
</div>


---

# 📈 `functime`'s answer
Reframe the problem

Make forecasting just work at a **reasonable scale** (~90% of use cases).

<v-clicks>

1. Forecast **thousands of time series** without distributed systems (PySpark).
2. **Feature-engineering** and **diagnostics** API compatible with panel datasets.
3. Smoothly translate form experimentation to production.
</v-clicks>

<v-click>

This can be achieved with two ingredients: **Polars** and **global forecasting**.
</v-click>


---

# 🐻‍❄️ What is Polars?
A brief description

***Dataframes** powered by a **multithreaded**, vectorized **query engine**, written in Rust*

<v-clicks>

1. A dataframe frontend: work with a Python object and not a SQL table.
2. Utilises **all cores** on your machine, **efficiently** (more on this later).
3. Uses 50+ years of relational database research to **optimise the query**.
4. **In-process**, like sqlite (OLTP), duckdb (OLAP) and LanceDB (vector).

</v-clicks>


---

# 🐻‍❄️ What is Polars?
What makes it so fast

<v-clicks>

1. Efficient **data representation and I/O** with Apache Arrow
2. Work stealing, AKA **efficient multithreading**.
3. **Query optimisations** through lazy evaluation (e.g.: `DataFrame.sort("col1").head(5)` in pandas vs Polars).

</v-clicks>


---

# 🌏 What is global forecasting?
A lesson from forecasting competitions

<v-clicks>

Global forecasting just means to **fit a single model on all the time series in your panel dataset**.

This approach proved successful in multiple forecasting competitions, most notably M4 ([1](https://www.sciencedirect.com/science/article/abs/pii/S0169207020301850) [2](https://www.sciencedirect.com/science/article/abs/pii/S0169207020301850)) and M5 ([1](https://www.sciencedirect.com/science/article/pii/S0169207021001874#b47)).

</v-clicks>


---

# 🌏 What is global forecasting?
A lesson from forecasting competitions

**Gradient boosted regression trees** secured the top spots, but linear models work well too, provided some **thoughtful and deliberate feature engineering**.

<v-clicks>

Here's the recipe to make `functime`: a powerful query engine to perform blazingly fast feature engineering, followed by a single `model.fit()`.

Doesn't have to be _best_ model, but fast to iterate on and scalable to thousands of time series on your laptop.

</v-clicks>


---
layout: intro
---

# 💻 Forecasting with `functime`
Time for some dangerous live coding 🥶

### Source code at [https://github.com/baggiponte/pydata-global-2023-functime](https://github.com/baggiponte/pydata-global-2023-functime)


---

# 💻 Forecasting with `functime`
What I could not show

* Prediction intervals with conformal predictions.
* Hyperparameter tuning with `flaml`.
* Advanced feature extraction.
* Censored forecasts.
* LLM data analysis.


---

# 🔌 References
A deep dive into the Arrow ecosystem and Polars internals

* [Apache Arrow and Substrait, the secret foundations of Data Engineering](https://www.youtube.com/watch?v=5_EF4KC8XO4&list=PL8uoeex94UhFcwvAfWHybD7SfNgIUBRo-&index=107) - Alessandro Molina @EuroPython 2023
* [Polars: DataFrames in the multi-core era](https://www.youtube.com/watch?v=tqcudsykOGc&t=1916s) - Ritchie Vink @PyData NYC 2023
* [Is the great dataframe showdown finally over? Enter: Polars](https://www.youtube.com/watch?v=XsxrhGlkyk0) - Luca Baggi (me) @PyConIt 2023


---

# 🔌 References
More PyData Global 2023 talks

* [Polars and time zones: everything you need to know](https://global2023.pydata.org/cfp/talk/LPEHES/) by Marco Gorelli
  * Practical showcase on how to use Polars to master datetimes and time-zones
* [We rewrote tsfresh in Polars and why you should too](https://global2023.pydata.org/cfp/talk/9FEQVK/) by Chris Lo and Mathieu Cayssol
  * 90-minute workshop to dive deeper into functime internals, Polars integration and benchmarking (thanks to Polars-Rust [plugins](https://github.com/pola-rs/pyo3-polars)!)


---

# 🔌 References
Documentation and communities

* [Polars](https://pola.rs/) web site
* Polars [discord server](https://discord.com/invite/4UfP5cfBE7)
* [functime.ai](https://functime.ai/) website and docs
* functime.ai [discord server](https://discord.gg/8pRsc7Ba)


---
layout: intro
---

# 🙏 Thank you!

Please share your feedback! My address is lucabaggi [at] duck.com

<div class="absolute right-5 top-5">
<img height="150" width="150"  src="/qr-linkedin.svg">
</div>

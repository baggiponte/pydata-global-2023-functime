---
theme: default
layout: cover
highlighter: shiki
colorSchema: light
favicon: favicon/url
title: How I used Polars to build built functime, a next gen ML forecasting library
---

# 🔮 functime
Or how we used **Polars** and **global forecasting** to build\
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

---

# 🔎 Diagnostic tools



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

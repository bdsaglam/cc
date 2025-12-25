---
name: marimo
description: This skill should be used when creating or editing marimo notebooks (.py files using marimo). Trigger phrases include "create marimo notebook", "edit marimo notebook", "marimo cell", "marimo slider", "marimo dropdown", "mo.ui", "marimo reactivity", "interactive data analysis", "marimo visualization", "marimo SQL". Use for guidance on marimo's reactive programming model, UI components, layout functions, or visualization patterns.
---

# Marimo Notebook Development

## Core Concepts

Marimo is a reactive notebook where:
- Cells execute automatically when dependencies change
- Variables cannot be redeclared across cells (forms a DAG)
- The last expression in a cell is automatically displayed
- UI elements are reactive without explicit callbacks

## Code Requirements

1. All code must be complete and runnable
2. Import all modules in the first cell, always including `import marimo as mo`
3. Never redeclare variables across cells
4. Access UI element values with `.value` in a separate cell from definition
5. For matplotlib: use `plt.gca()` as the last expression (not `plt.show()`)
6. For plotly/altair: return the figure/chart object directly
7. Don't include comments in markdown or SQL cells

## UI Elements

```python
mo.ui.slider(start, stop, value=None, label=None)
mo.ui.dropdown(options, value=None, label=None)
mo.ui.checkbox(label='', value=False)
mo.ui.text(value='', label=None)
mo.ui.text_area(value='', label=None)
mo.ui.button(value=None, kind='primary')
mo.ui.run_button(label=None)
mo.ui.number(value=None, label=None)
mo.ui.date(value=None, label=None)
mo.ui.radio(options, value=None, label=None)
mo.ui.file(label='', multiple=False)
mo.ui.table(data, sortable=True, filterable=True)
mo.ui.data_explorer(df)
mo.ui.dataframe(df)
mo.ui.altair_chart(chart)
mo.ui.plotly(figure)
mo.ui.tabs(elements: dict)
mo.ui.form(element, label='')
```

## Layout Functions

```python
mo.md(text)           # Display markdown
mo.hstack(elements)   # Stack horizontally
mo.vstack(elements)   # Stack vertically
mo.tabs(elements)     # Tabbed interface
mo.stop(predicate)    # Stop execution conditionally
mo.Html(html)         # Display HTML
mo.image(image)       # Display image
```

## SQL with DuckDB

```python
_df = mo.sql("SELECT * FROM my_dataframe WHERE col > 10")
```

## Examples

### Basic UI with Reactivity

```python
# Cell 1
import marimo as mo
import matplotlib.pyplot as plt
import numpy as np

# Cell 2
n_points = mo.ui.slider(10, 100, value=50, label="Number of points")
n_points

# Cell 3 - auto re-executes when slider changes
x = np.random.rand(n_points.value)
y = np.random.rand(n_points.value)
plt.figure(figsize=(8, 6))
plt.scatter(x, y, alpha=0.7)
plt.title(f"Scatter plot with {n_points.value} points")
plt.gca()
```

### Multiple UI Elements

```python
# Cell 1
import marimo as mo
import pandas as pd
import seaborn as sns

# Cell 2
iris = sns.load_dataset('iris')

# Cell 3
species = mo.ui.dropdown(["All"] + iris["species"].unique().tolist(), value="All", label="Species")
x_feat = mo.ui.dropdown(iris.select_dtypes('number').columns.tolist(), value="sepal_length", label="X")
y_feat = mo.ui.dropdown(iris.select_dtypes('number').columns.tolist(), value="sepal_width", label="Y")
mo.hstack([species, x_feat, y_feat])

# Cell 4
filtered = iris if species.value == "All" else iris[iris["species"] == species.value]
sns.scatterplot(data=filtered, x=x_feat.value, y=y_feat.value, hue="species")
plt.gca()
```

### Interactive Altair Chart

```python
# Cell 1
import marimo as mo
import altair as alt
import pandas as pd

# Cell 2
cars_df = pd.read_csv('https://raw.githubusercontent.com/vega/vega-datasets/master/data/cars.json')
_chart = alt.Chart(cars_df).mark_point().encode(x='Horsepower', y='Miles_per_Gallon', color='Origin')
chart = mo.ui.altair_chart(_chart)
chart

# Cell 3
chart.value  # Selected data
```

### LaTeX in Markdown

```python
mo.md(r"""
The quadratic function $f$ is defined as
$$f(x) = x^2.$$
""")
```

## Data Sources

- Prefer GitHub-hosted datasets (raw.githubusercontent.com)
- Use CORS proxy for external URLs: `https://corsproxy.marimo.app/<url>`
- Consider `vega_datasets` for common example datasets

## Troubleshooting

- **Circular dependencies**: Reorganize code to remove cycles
- **UI value access error**: Move value access to a separate cell from definition
- **Visualization not showing**: Ensure viz object is the last expression

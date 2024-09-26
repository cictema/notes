
## Shift()
---
In Pandas, the shift() function is used to shift the values in a column or row by a specified number of periods. It is commonly used in time series data to compare values across different time steps (e.g., to calculate the difference between consecutive days, months, or years).

```python
DataFrame.shift(periods=1, freq=None, axis=0, fill_value=None)
```

• **periods**: (default = 1) Number of periods to shift. Positive values shift downward, and negative values shift upward.
• **axis**: 0 or ‘index’ for shifting rows (default), 1 or ‘columns’ for shifting columns.
• **fill_value**: The scalar value to fill newly introduced missing values (NaN) resulting from the shift.
• **freq**: (for time series data) Used for frequency conversion if the index is a time-related index (optional).
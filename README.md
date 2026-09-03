# The Analysis

## 1. What are the sales performance across categories and regions?

To find the sales performance across categories and regions. I grouped the category and sales together and sum it up. Likewise, i grouped  the region and sales but this time i looked for the average of sales in the region. It highlight the Category that has the highest sales and also highligt the average sales in each region, showing which Region i shoud pay much attention to,when running a model to help boast the sales and also the category.

## 2.  How does a particular category perform in a particular region

To know how a particular category perform in a particular region. I did a pivot_table making the ROWS to be categories and the COLUMNS to be region and also did a sum of the sales in each category across the regions. which brought out a pivot table that showed the total amount of category in each region.

view my notebook with detailed steps here: [EDA.ipynb](EDA.ipynb)

### Visualize Data

```python
category_region_performance = df.pivot_table(index='Category', columns='Region', values='Sales', aggfunc='sum')

category_region_performance.plot(kind='bar', figsize=(10, 6))
plt.xlabel('Region')
plt.ylabel('Sales')
plt.title('Sales Distribution Across Different Regions')
plt.xticks(rotation=45, ha='right')
plt.show()
```

### Results

![Visualization for sales across different Region](IMAGES/Sales%20Distrubution.png)





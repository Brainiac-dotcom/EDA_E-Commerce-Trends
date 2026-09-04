# Overview
Welcome to my analysis of Exploratory Data Analysis (EDA) of E-Commerce/Retail Sales Trends. The project was created due to my SIWES programm, where i was ask to analyze a dataset of my choice.

The data sourced from https://www.kaggle.com/datasets/vivek468/superstore-dataset-final which provide a foundation for my analysis, containing information on Category,sales,Region. I explore key questions such as  How does a particular category perform in a particular region, average sales per region.

# Objectves
1. To clean and prepare a real-world retail/e-commerce dataset for analysis.
2. To explore sales trends across time (monthly, seasonal, day-of-week patterns).
3. To examine sales performance across product categories and regions.
4. To visualize key patterns and communicate findings clearly.

# Tools I Used

### Python: 
The backbone of the analysis, allowing me to analyze the data and draw insight. I also used the following libraries
1. Pandas Library: This is used to analyze the data
2. Matplotlib Library: I visualized the data
3. Numpuy: Help in numerical calculations
4. plotly.express: helped in data analytic visualization

# Data Preparation and Cleanup
The raw dataset (Superstore Sales Dataset) required several cleaning steps before analysis could begin:

### Import and Clean Up
1.  Encoding Fix
The CSV file failed to load with the default UTF-8 encoding, throwing a UnicodeDecodeError. This was resolved by loading the file with latin1 encoding instead, which correctly handles special characters commonly found in exported retail data:
```python
df= pd.read_csv(r"C:\Users\DELL\Downloads\Sample - Superstore.csv", encoding="latin1")
```
### Converting Object to DateTime
2.  Date Conversion
The Order Date and Ship Date column was originally read in as plain text (object dtype). It was converted to a proper datetime format to allow time-based analysis (e.g., extracting month/year trends):
```python
df["Ship Date"]= pd.to_datetime(df["Ship Date"])
df["Ship Date"]
df["Order Date"] = pd.to_datetime(df["Order Date"])
df["Order Date"]
```

### Conducting EDA on the category that has the higest sales
3. Once the date was coverted from object to datetime, I went ahead to conduct an EDA on categories and sales, which helped in knowing the category the highest sale
```python
highest_sales_category = df.groupby('Category')['Sales'].sum().sort_values(ascending=False)
highest_sales_category
```

### Saved Data
4.  Saving the Cleaned Dataset
Once cleaning was complete, the dataset was exported to a new file to separate raw and cleaned versions:
```python
df.to_csv("Cleaned_Superstore.csv", index=False)
```


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


### Insight

Looking at this chart, West and East are clearly carrying the most weight across all three categories, while South is consistently the weakest performer no matter what's being sold. That's actually the most interesting part to me — South isn't just underperforming in one category, it's behind everywhere, which makes me think it's less about product demand and more about something structural, like fewer customers in that region, less marketing reach, or maybe delivery/logistics issues.

Technology stands out as the strongest category overall, with East pulling in close to 265,000 and West right behind it around 252,000. That's honestly a bit surprising to me — I would've guessed Office Supplies would lead since it's usually a recurring/repeat purchase, but Technology items (which are pricier per unit) seem to be driving more total revenue.

Central is interesting too — it's never the best, but it's never the worst either. It sits fairly con

sistently in the middle across all three categories, which tells me demand there is stable but not really growing or standing out in any one rea.

If I were making a business call from this, I'd say South needs the most attention — either more marketing push or maybe just look into why sales are lagging there specifically, since it's dragging the average down across the board.

## 3. What are the sum sales sub_category in the Three most Competitive category 

To find the sum sales sub_category in the Category, i to group them and draw out the output. the detailed step are in the  EDA link [EDA.ipynb](EDA.ipynb)

### Visualized Data

```python
fig = px.sunburst(Sum_sales_sub_category_in_category, path=['Category','Sub-Category'], values='Sales',
                   title = 'Sales Breakdown: Category + Sub-Category',
                    width =800, height= 800 )
fig.show()                
```

### Results

![Visualization for sales break down](IMAGES/Sales%20Breakdown.png)

### Insight

Office Supplies leads in overall volume, driven primarily by Binders — a low-cost, high-frequency purchase category comparable in size to the entire Technology segment.
Technology sales are concentrated in Phones and Machines, reflecting higher per-unit pricing rather than purchase volume.
Furniture shows the most balanced distribution, with no single sub-category dominating the segment.
Key takeaway: Sales volume is not solely price-driven — high-frequency, low-cost items (e.g., Binders, Paper) contribute significantly to overall revenue, alongside high-value categories like Technology. This suggests both pricing strategy and purchase frequency should inform inventory and marketing decisions.

# What i learned 
Thoughout this project, i deepened my knowledge on how a particular sales  in each region can affect the company in question positively or negatively. Here are some few specific things i learnt

1. Column names must match exactly
I hit KeyError more than once — 'Sales', 'sum', 'quantity', 'subcategories_per_category' — and each time, the fix was the same lesson: pandas is case-sensitive and whitespace-sensitive. By the end, I wasn't panicking at these errors anymore; I was reading the traceback and immediately checking column names.

2. Never overwrite data you'll need later
This one gave me a hard time with Order Date — I overwrote it with .dt.month and lost the original date entirely, which then broke every .dt operation afterward. I now understand why creating a new column (Order Month) instead of overwriting is the safer pattern.

3. Understanding data structure types (Series vs DataFrame)
The .groupby().agg() confusion — where sort_values(by=...) worked sometimes and not others — compelled me to actually understand what type of object my code was producing at each step.

# Insight
1. Regional performance is consistent across every category
West and East are the strongest regions no matter what's being sold, while South consistently underperforms across Furniture, Office Supplies, and Technology alike. Since this pattern holds across every category rather than just one, it points to a regional issue (market reach, logistics, customer base) rather than a product-demand issue.

2. Furniture is the most balanced, least volatile category
Unlike Technology (dominated by 2 sub-categories) and Office Supplies (dominated by Binders), Furniture's sales are spread fairly evenly across Chairs, Tables, Bookcases, and Furnishings 

3. Overall takeaway
The business isn't succeeding because of one "hero" category — it's succeeding through a mix of high-value tech sales, high-frequency supply purchases, and steady furniture demand. The clearest weakness across the whole dataset is regional, not product-based: South is the one dimension where performance drops uniformly, making it the most actionable area for improvement (marketing push, delivery/logistics review, or localized promotions).

# Challenges I Faced
A few recurring issues came up during the cleaning and analysis process, which were valuable learning points:

1. Encoding errors when reading the raw CSV, resolved by switching to latin1 encoding.
2. Column name mismatches (case sensitivity and spacing) caused several KeyError issues during grouping and aggregation — resolved by consistently verifying exact column names with df.columns.tolist() before referencing them.
3. Overwriting original columns during transformation (e.g., replacing Order Date with just the month value) caused downstream errors when the original datetime format was needed again later. This was reolved by re-cleaning the whole dataset
4. Aggregation output type confusion — distinguishing between a Series and DataFrame output from .groupby() operations, which affected how sorting and filtering methods (like .sort_values()) needed to be applied.
5. Installation of *!pip install kaleido==0.2.1* i tried coverting the sunburst chart into image 

# Conclusion
This exploration into the E-Commerce/Retail Sales dataset during my SIWES programm has been incredibly informative. The insight i got enhance my understanding and spot out the issue of the south underperforming across all category. And also it has embended me with the solution to tackle the underperformance of the south region. This project is a good foundation for future exploratoin of E-Commerce/Retail Sales and provide solutions for any set back. 

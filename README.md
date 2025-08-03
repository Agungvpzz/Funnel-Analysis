# Funnel Analysis of Electronics Store eCommerce Events


## Introduction
Understanding where users drop off during their journey is vital for optimizing conversion and improving the overall customer experience. In this analysis, we examine eCommerce event history from an electronics store to identify critical stages where users disengage. By mapping these steps and quantifying drop-offs, we reveal actionable insights for improving funnel efficiency, enhancing engagement, and ultimately boosting sales.

## About the Data
This analysis uses public eCommerce event data from an electronics store, available on [Kaggle](https://www.kaggle.com/datasets/mkechinov/ecommerce-events-history-in-electronics-store). It includes user actions like viewing products, adding to cart, and making purchases. 

To handle missing values:
- brand entries were filled with "unknown"
- category_code entries were filled with "uncategorized"
- These labels help keep the funnel analysis complete and consistent.

Additionally, to better understand purchasing behavior across different price ranges, a new column was created to categorize products into four tiers based on price:
- Low: under $10
- Medium: $10 to <$100
- High: $100 to <$1000
- Premium: $1000 and above

This derived feature allows for segmenting funnel performance by price level and identifying patterns tied to product affordability.

## Funnel Definition
This analysis is based on a three-stage user journey extracted from the electronics store’s eCommerce events:
  - **View**: The user browses a product detail page, showing initial interest.
  - **Cart**: The user adds a product to their shopping cart, indicating intent to purchase.
  - **Purchase**: The user completes the transaction, marking the final conversion.

## Exploratory Data Analysis

### Funnel Proportion
<p align=center>
  <img width="855" height="254" alt="image" src="https://github.com/user-attachments/assets/cba03f03-94ee-40c7-9af4-a2be8f5bea82" />
</p>

Out of ~857,000 interactions:
- 768,093 (89.6%) viewed a product
- 52,623 (6.1%) added to cart
- 36,339 (4.2%) completed purchase

This shows strong interest up front, but major drop-offs at the cart stage.


### Funnel Proportion by Price Category
To enrich the analysis, we derive a new column from price column by cut them into 4 separate categories.
dfl.with_columns(
    pl.when(pl.col("price") < 10).then(pl.lit("low"))
      .when(pl.col("price") < 100).then(pl.lit("medium"))
      .when(pl.col("price") < 1000).then(pl.lit("high"))
      .otherwise(pl.lit("premium"))
      .alias("price_category")
)

<p align=center>
  <img width="881" height="248" alt="image" src="https://github.com/user-attachments/assets/ec320a72-efb1-405a-9bb1-891bc0343309" />
</p>

Low-price products attract lower traffic but higher relative conversion:
- Low View: 23,110 (87.97%)
- Cart: 1,749 (6.66%)
- Purchase: 1,411 (5.37%)

Medium-price products show the highest traffic and conversion:
- Medium View: 447,603 (90.5%)
- Cart: 26,663 (5.4%)
- Purchase: 20,270 (4.1%)

High-price products follow closely in volume, with slightly higher cart activity:
- High View: 291,347 (88.3%)
- Cart: 24,068 (7.3%)
- Purchase: 14,596 (4.4%)

Premium products have minimal reach and very low conversion:
- Premium View: 6,033 (96.7%)
- Cart: 143 (2.29%)
- Purchase: 62 (0.99%)


### Customer Purchase Frequency
<p align=center>
  <img width="870" height="417" alt="image" src="https://github.com/user-attachments/assets/458b3c44-0b58-4156-b33d-cc3b4fd402d9" />
</p>

Repeat purchases are rare among the 393,080 unique customers in the dataset:
- 13,215 customers made a single purchase
- 4,170 customers purchased twice
- The numbers drop rapidly, with only 42 customers reaching 10 purchases

This steep decline suggests limited long-term customer engagement and presents a clear opportunity to improve retention strategies.

# Audible Insights Dashboard - Data Analysis Project

## Tools & Skills:
### #PowerQuery #DAX #DataCleaning #DataTransformation #PowerBI #DataVisualization

## Project Overview
This project focuses on analyzing an Audible audiobook dataset to provide key insights into audiobook prices, ratings, and author productivity. The analysis was performed in Power BI. The dashboard provides a comprehensive view of key metrics, such as total audiobooks, pricing trends, and the distribution of audiobooks across languages and authors.

## Dataset
The dataset contains the following columns:

- Name: The name of the audiobook. (Text)
- Author: The author of the audiobook. (Text)
- Narrator: The name of the narrator (Text)
- releasedate: The release date of the audiobook. (Date)
- audio_language: The primary language of the audiobook. (Text)
- Price: The price of the audiobook. (Currency)
- Time: The time of the audiobook in minutes (Numeric)
- stars: The star rating of the audiobook. (Numeric)
- Ratings: The number of user ratings for each audiobook. (Numeric)

## Data Cleaning and Preprocessing
Before diving into the analysis, the data underwent the following cleaning steps:

Missing Values: Missing or null values in the Price and Ratings columns were handled using imputation techniques (e.g., filling missing prices with the median or mode of the dataset). [Example step, replace if necessary].
Formatting: All dates were formatted uniformly to reflect the correct releasedate structure for timeline analysis.
Data Type Adjustments: Ensured that Price, Ratings, and releasedate columns were in their proper data types (numeric and date formats).
Outlier Detection: Outliers in the Price column (extreme values above a certain threshold) were flagged and evaluated to ensure they were not errors or misentered data points.
Measures and Calculations
Several measures were created in Power BI to enhance the analysis:

Total Audiobooks: A measure to count the total number of audiobooks in the dataset using COUNTROWS('Table').
Total Authors: A similar measure to count the unique number of authors.
Average Price: A measure calculating the average price of audiobooks using AVERAGE('Table'[Price]).
Dashboard Features
1. Bar Chart: Price vs. Ratings
X-Axis: Price
Y-Axis: Ratings
Insight: This visual highlights the relationship between audiobook price and the number of ratings received. The chart shows a general trend where lower-priced audiobooks tend to receive higher numbers of ratings, suggesting that affordability may drive user engagement. The average price ($6.73) is displayed as a reference line, providing additional context.
2. Treemap: Total Audiobooks by Language
Category: audio_language
Values: Count of Audiobooks
Insight: The treemap clearly shows that English-language audiobooks dominate the dataset, comprising a significant portion of the available audiobooks (62K). Other languages, like German and Spanish, are represented in smaller quantities, offering insights into the market distribution of audiobooks across different languages.
3. Card Visual: Total Audiobooks and Total Authors
Measures: Total Audiobooks and Total Authors
Insight: The dataset consists of 87,000 audiobooks and 48,000 authors, showing the vast diversity of content available on Audible. These key metrics help to contextualize the overall size of the dataset.
4. Bar Chart: Total Audiobooks by Author (Top 10)
X-Axis: Author
Y-Axis: Count of Audiobooks
Insight: This visual highlights the most productive authors in the dataset. Notably, SmartReading stands out with 874 audiobooks, followed by other top authors with significantly fewer works. The chart demonstrates the imbalance in audiobook production, with a few highly productive authors dominating the field.
5. Line Chart: Average Price Over Time
X-Axis: Year (from releasedate)
Y-Axis: Average Price
Insight: The line chart reveals trends in audiobook pricing over time. A clear upward trend is visible after 2020, with the average price spiking to $11.71. This insight may indicate changes in the market dynamics or pricing strategies of audiobooks in recent years.
Insights and Conclusions
From this analysis, several important insights were gleaned:

Price vs. Ratings: Lower-priced audiobooks generally receive more ratings, indicating a potential relationship between affordability and user engagement.
Language Distribution: The audiobook market is overwhelmingly dominated by English titles, but there is a significant presence of non-English titles, particularly in languages like German and Spanish.
Top Authors: A small number of authors are responsible for a large portion of audiobook production, with SmartReading as a notable leader.
Price Trends: Audiobook prices have fluctuated over time, with a notable increase in recent years, which may be linked to shifts in content quality or demand.
Conclusion
This Audible Insights Dashboard offers a comprehensive look into audiobook pricing, ratings, language distribution, and author productivity. By visualizing key metrics and trends, this analysis provides valuable insights into the audiobook market. The dashboard is designed for easy interpretation and can be adapted for further interactive analysis if required.

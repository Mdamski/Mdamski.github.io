# Audible - Audiobook Sales Data - Data Analysis Project

## Tools and Skills
### Tools Used: Power Query, DAX, Power BI
### Skills Demonstrated: Data Cleaning, Data Transformation, Data Visualization

## Project Overview
This project focuses on analyzing an Audible audiobook dataset to provide key insights into audiobook prices, ratings, and author productivity. The analysis was performed in Power BI. The dashboard provides a comprehensive view of key metrics, such as total audiobooks, pricing trends, and the distribution of audiobooks across languages and authors.

## Dataset Before Cleaning
The dataset contains the following columns:

- Name: The name of the audiobook. (Text)
- Author: The author of the audiobook. (Text)
- Narrator: The name of the narrator (Text)
- releasedate: The release date of the audiobook. (Text)
- language: The primary language of the audiobook. (Text)
- Price: The price of the audiobook. (Text)
- Time: The time of the audiobook in minutes (Text)
- Stars: The star rating of the audiobook and the number of user ratings. (Text)

## Data Cleaning and Preprocessing
Before diving into the analysis, the data underwent the following cleaning steps:

- Author: Deleted "Writtenby:" before author name.
- Narrator: Deleted "Narratedby:" before narrator name.
- releasedate: Adjusted data type.
- Price: Adjusted data type.
- Time: Transformed from a text format (e.g., "2 hrs and 20 mins") to a numeric format representing total minutes (e.g., "140").
- Stars: Transformed from a text format (e.g., "5 out of 5 stars34 ratings") to two separet columns. One for stars and second for ratings.


## Dataset After Cleaning

- Name: The name of the audiobook. (Text)
- Author: The author of the audiobook. (Text)
- Narrator: The name of the narrator (Text)
- releasedate: The release date of the audiobook. (Date)
- language: The primary language of the audiobook. (Text)
- Price: The price of the audiobook. (Numeric)
- Time: The time of the audiobook in minutes (Numeric)
- Stars: The star rating of the audiobook. (Decimal)
- Ratings: The number of user ratings for each audiobook. (Numeric)

## Dashboard

![image](https://github.com/user-attachments/assets/9e838589-e93c-4293-bee8-1fb93833087a)


## Insights and Conclusions
From this analysis, several important insights were gleaned:

- **Price and Ratings**: Lower-priced audiobooks generally receive more ratings, indicating a potential relationship between affordability and user engagement.
- **Language Distribution**: The audiobook market is overwhelmingly dominated by English titles, but there is a significant presence of non-English titles, particularly in languages like German and Spanish.
- **Top Authors**: A small number of authors are responsible for a large portion of audiobook production, with SmartReading as a notable leader.
- **Price Trends**: Audiobook prices have fluctuated over time, with a notable increase in recent years, which may be linked to shifts in content quality or demand.

These insights can guide pricing strategies and promotional efforts, such as focusing on more affordable titles to boost user engagement or expanding content in non-English languages to tap into underrepresented markets
## Dataset Source
- **Source**: [Kaggle](https://www.kaggle.com/datasets/snehangsude/audible-dataset?select=audible_uncleaned.csv)

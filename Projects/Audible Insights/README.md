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

## Data Cleaning in Power Querry Details 

Detailed list of the applied steps during data cleaning process
```powerquery
let
    Source = Csv.Document(File.Contents("C:\Users\Micha\Desktop\Dane\Audible\audible_uncleaned.csv"),[Delimiter=",", Columns=8, Encoding=65001, QuoteStyle=QuoteStyle.None]),
    #"Promoted Headers" = Table.PromoteHeaders(Source, [PromoteAllScalars=true]),
    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"name", type text}, {"author", type text}, {"narrator", type text}, {"time", type text}, {"releasedate", type date}, {"language", type text}, {"stars", type text}, {"price", type text}}),
    #"Extracted Text After Delimiter" = Table.TransformColumns(#"Changed Type", {{"author", each Text.AfterDelimiter(_, ":"), type text}}),
    #"Extracted Text After Delimiter1" = Table.TransformColumns(#"Extracted Text After Delimiter", {{"narrator", each Text.AfterDelimiter(_, ":"), type text}}),
    #"Added Custom" = Table.AddColumn(#"Extracted Text After Delimiter1", "time_hours", each if Text.Contains([time], "hrs") 
then Number.FromText(Text.BeforeDelimiter([time], " hrs")) 
else 0),
    #"Changed Type1" = Table.TransformColumnTypes(#"Added Custom",{{"time_hours", Int64.Type}}),
    #"Added Custom1" = Table.AddColumn(#"Changed Type1", "time_minutes", each if Text.Contains([time], "mins") then
       if Text.Contains([time], "and") then 
          Number.FromText(Text.BetweenDelimiters([time], "and ", " mins"))
       else 
          Number.FromText(Text.BeforeDelimiter([time], " mins"))
else 0),
    #"Filtered Rows" = Table.SelectRows(#"Added Custom1", each true),
    #"Added Custom2" = Table.AddColumn(#"Filtered Rows", "time_corrected", each [time_hours] * 60 + [time_minutes]),
    #"Changed Type2" = Table.TransformColumnTypes(#"Added Custom2",{{"time_minutes", Int64.Type}, {"time_corrected", Int64.Type}}),
    #"Inserted Text Before Delimiter" = Table.AddColumn(#"Changed Type2", "Text Before Delimiter", each Text.BeforeDelimiter([stars], " out"), type text),
    #"Renamed Columns" = Table.RenameColumns(#"Inserted Text Before Delimiter",{{"Text Before Delimiter", "stars_correction1"}}),
    #"Replaced Value" = Table.ReplaceValue(#"Renamed Columns",".",",",Replacer.ReplaceText,{"stars_correction1"}),
    #"Added Custom3" = Table.AddColumn(#"Replaced Value", "Stars_corrected", each if Text.Contains([stars_correction1], "Not rated yet")
then 0
else Number.FromText([stars_correction1])),
    #"Changed Type3" = Table.TransformColumnTypes(#"Added Custom3",{{"Stars_corrected", type number}}),
    #"Inserted Text Between Delimiters" = Table.AddColumn(#"Changed Type3", "Text Between Delimiters", each Text.BetweenDelimiters([stars], "stars", " ratings"), type text),
    #"Renamed Columns1" = Table.RenameColumns(#"Inserted Text Between Delimiters",{{"Text Between Delimiters", "Ratings_correction1"}}),
    #"Inserted Text Before Delimiter1" = Table.AddColumn(#"Renamed Columns1", "Text Before Delimiter", each Text.BeforeDelimiter([Ratings_correction1], " rating"), type text),
    #"Renamed Columns2" = Table.RenameColumns(#"Inserted Text Before Delimiter1",{{"Text Before Delimiter", "Ratings_correction2"}}),
    #"Changed Type4" = Table.TransformColumnTypes(#"Renamed Columns2",{{"Ratings_correction2", Int64.Type}}),
    #"Added Custom4" = Table.AddColumn(#"Changed Type4", "Ratings_corrected", each if [Ratings_correction2] = null then 0
else [Ratings_correction2]),
    #"Extracted Text Before Delimiter" = Table.TransformColumns(#"Added Custom4", {{"price", each Text.BeforeDelimiter(_, "."), type text}}),
    #"Replaced Value1" = Table.ReplaceValue(#"Extracted Text Before Delimiter",",","",Replacer.ReplaceText,{"price"}),
    #"Changed Type5" = Table.TransformColumnTypes(#"Replaced Value1",{{"price", Currency.Type}}),
    #"Removed Errors" = Table.RemoveRowsWithErrors(#"Changed Type5", {"price"}),
    #"Changed Type6" = Table.TransformColumnTypes(#"Removed Errors",{{"Ratings_corrected", Int64.Type}})
in
    #"Changed Type6"
```
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

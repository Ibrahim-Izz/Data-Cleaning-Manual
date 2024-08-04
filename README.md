# Data Cleaning Manual

## Initial Data Selection and Preparation

1. **Selecting**: Choosing relevant columns or rows.
   - **Power Query**: `Select Columns` option
   - **SQL**: `SELECT column1, column2 FROM table`
   - **Pandas**: `df[['col1', 'col2']]`
   - **Spark**: `df.select("col1", "col2")`
   - **Glue**: `DynamicFrame.select_fields()`

2. **Adding Columns**: Introducing new columns.
   - **Power Query**: `Add Column -> Custom Column`
   - **SQL**: `ALTER TABLE table ADD COLUMN new_col INT`
   - **Pandas**: `df['new_col'] = df['col1'] + df['col2']`
   - **Spark**: `df.withColumn("new_col", df["col1"] + df["col2"])`
   - **Glue**: `DynamicFrame.apply_mapping()` with new column

3. **Renaming Columns**: Standardizing column names.
   - **Power Query**: `Transform -> Rename Columns`
   - **SQL**: `ALTER TABLE table RENAME COLUMN old_name TO new_name`
   - **Pandas**: `df.rename(columns={'old_name': 'new_name'})`
   - **Spark**: `df.withColumnRenamed("old_name", "new_name")`
   - **Glue**: `DynamicFrame.apply_mapping()` with new names

4. **Dropping Columns**: Removing irrelevant columns.
   - **Power Query**: `Remove Columns`
   - **SQL**: `ALTER TABLE table DROP COLUMN col_name`
   - **Pandas**: `df.drop(columns=['col_name'])`
   - **Spark**: `df.drop("col_name")`
   - **Glue**: `DynamicFrame.drop_fields()`

5. **Changing Data Types**: Converting columns to appropriate data types.
   - **Power Query**: `Transform -> Data Type`
   - **SQL**: `ALTER TABLE table MODIFY column_name new_data_type`
   - **Pandas**: `df['col'] = df['col'].astype('int')`
   - **Spark**: `df.withColumn("col", df["col"].cast("int"))`
   - **Glue**: `DynamicFrame.toDF()` and then `df.withColumn("col", df["col"].cast("int"))`

6. **Modify Schema**: Adjusting dataset structure.
   - **Power Query**: `Manage Columns -> Reorder Columns`
   - **SQL**: `ALTER TABLE table MODIFY COLUMN column_name new_data_type`
   - **Pandas**: Adjust columns in DataFrame as needed
   - **Spark**: `df.select("new_col_order")`
   - **Glue**: `DynamicFrame.apply_mapping()` for schema adjustments

7. **Splitting Columns**: Breaking down a column.
   - **Power Query**: `Split Column -> By Delimiter`
   - **SQL**: `SUBSTRING_INDEX(column, delimiter, part)`
   - **Pandas**: `df[['col1', 'col2']] = df['col'].str.split(delimiter, expand=True)`
   - **Spark**: `df.withColumn("col1", split(df["col"], delimiter).getItem(0))`
   - **Glue**: Use `DynamicFrame.apply_mapping()` with split logic

8. **Merging Columns**: Combining multiple columns.
   - **Power Query**: `Add Column -> Merge Columns`
   - **SQL**: `CONCAT(column1, column2)`
   - **Pandas**: `df['merged_col'] = df['col1'].astype(str) + df['col2'].astype(str)`
   - **Spark**: `df.withColumn("merged_col", concat(df["col1"], df["col2"]))`
   - **Glue**: Use `DynamicFrame.apply_mapping()` with merge logic

## Data Cleaning and Transformation

9. **String Case Conversion**: Standardizing text case.
   - **Power Query**: `Transform -> Format -> Uppercase/Lowercase`
   - **SQL**: `UPPER(column)` or `LOWER(column)`
   - **Pandas**: `df['col'] = df['col'].str.upper()`
   - **Spark**: `df.withColumn("col", upper(df["col"]))`
   - **Glue**: Apply transformation using `DynamicFrame.apply_mapping()`

10. **Trimming**: Removing leading/trailing whitespace.
    - **Power Query**: `Transform -> Format -> Trim`
    - **SQL**: `TRIM(column)`
    - **Pandas**: `df['col'] = df['col'].str.strip()`
    - **Spark**: `df.withColumn("col", trim(df["col"]))`
    - **Glue**: Use `DynamicFrame.apply_mapping()` for trimming logic

11. **Extracting Parts of Strings**: Pulling out substrings.
    - **Power Query**: `Add Column -> Extract`
    - **SQL**: `SUBSTRING(column, start, length)`
    - **Pandas**: `df['part'] = df['col'].str.slice(start, end)`
    - **Spark**: `df.withColumn("part", substring(df["col"], start, length))`
    - **Glue**: Use `DynamicFrame.apply_mapping()` for extraction

12. **Removing Duplicates**: Ensuring unique records.
    - **Power Query**: `Remove Rows -> Remove Duplicates`
    - **SQL**: `SELECT DISTINCT column1, column2 FROM table`
    - **Pandas**: `df.drop_duplicates()`
    - **Spark**: `df.dropDuplicates()`
    - **Glue**: Implement deduplication using Spark transformations

13. **Handling Missing Values**: Managing null or missing data.
    - **Power Query**: `Remove Rows -> Remove Blank Rows` or `Replace Values`
    - **SQL**: `IS NULL`, `COALESCE(column, default_value)`
    - **Pandas**: `df.fillna(value)` or `df.dropna()`
    - **Spark**: `df.na.fill(value)` or `df.na.drop()`
    - **Glue**: Handle with Spark transformations or using `DynamicFrame.resolveChoice()`

14. **Conditional Removing/Replacing Values**: Based on conditions.
    - **Power Query**: `Replace Values` with conditional logic
    - **SQL**: `UPDATE table SET column = new_value WHERE condition`
    - **Pandas**: `df.loc[df['col'] < 0, 'col'] = 0`
    - **Spark**: `df.withColumn("col", when(df["col"] < 0, 0).otherwise(df["col"]))`
    - **Glue**: Use `DynamicFrame.apply_mapping()` with conditionals

15. **Filtering Rows**: Based on criteria.
    - **Power Query**: `Filter Rows`
    - **SQL**: `SELECT * FROM table WHERE condition`
    - **Pandas**: `df[df['col'] > 0]`
    - **Spark**: `df.filter(df["col"] > 0)`
    - **Glue**: Use `DynamicFrame.filter()` for row filtering

16. **Counting Rows**: Determining the number of rows.
    - **Power Query**: `Group By -> Count Rows`
    - **SQL**: `SELECT COUNT(*) FROM table`
    - **Pandas**: `df.shape[0]`
    - **Spark**: `df.count()`
    - **Glue**: Use Spark transformations or custom logic

17. **Describing Statistics**: Generating summary statistics.
    - **Power Query**: `Transform -> Statistics`
    - **SQL**: Use aggregate functions like `COUNT()`, `SUM()`, `AVG()`, `MIN()`, `MAX()`
    - **Pandas**: `df.describe()`
    - **Spark**: `df.describe()`
    - **Glue**: Use Spark transformations for statistical summary

18. **Showing a Sample of Rows**: Displaying a subset of rows.
    - **Power Query**: `Home -> Keep Rows -> Keep Top Rows`
    - **SQL**: `SELECT * FROM table LIMIT 10`
    - **Pandas**: `df.head(10)`
    - **Spark**: `df.show(10)`
    - **Glue**: Use `DynamicFrame.toDF().show()` for sampling

## Date and Time Operations

19. **Formatting Date**: Consistent date format.
    - **Power Query**: `Transform -> Date -> Format`
    - **SQL**: `FORMAT(column, 'YYYY-MM-DD')`
    - **Pandas**: `df['date'] = pd.to_datetime(df['date']).dt.strftime('%Y-%m-%d')`
    - **Spark**: `df.withColumn("date", date_format(df["date"], "yyyy-MM-dd"))`
    - **Glue**: Use `DynamicFrame.apply_mapping()` for date formatting

20. **Extracting Date Components**: Deriving year, month, day.
    - **Power Query**: `Add Column -> Date -> Year, Month, Day`
    - **SQL**: `YEAR(column)`, `MONTH(column)`, `DAY(column)`
    - **Pandas**: `df['year'] = df['date'].dt.year`
    - **Spark**: `df.withColumn("year", year(df["date"]))`
    - **Glue**: Use `DynamicFrame.toDF()` with date extraction logic

## Aggregation and Analysis

21. **Aggregating Data**: Summarizing data.
    - **Power Query**: `Group By`
    - **SQL**: `GROUP BY column WITH SUM(), AVG(), COUNT()`
    - **Pandas**: `df.groupby('col').agg({'numeric_col': 'sum'})`
    - **Spark**: `df.groupBy("col").agg(sum("numeric_col"))`
    - **Glue**: Use Spark transformations for aggregation

22. **Calculating Moving Averages**: Trends over time.
    - **Power Query**: `Add Column -> Moving Average`
    - **SQL**: `SELECT column, AVG(column) OVER (PARTITION BY ... ORDER BY ... ROWS BETWEEN ... AND ...)`
    - **Pandas**: `df['moving_avg'] = df['col'].rolling(window=7).mean()`
    - **Spark**: `df.withColumn("moving_avg", avg(df["col"]).over(windowSpec))`
    - **Glue**: Implement moving averages using Spark

23. **Group By & Partition By**: Aggregating data by groups.
    - **Power Query**: `Group By`
    - **SQL**: `GROUP BY column`
    - **Pandas**: `df.groupby('col').sum()`
    - **Spark**: `df.groupBy("col").sum()`
    - **Glue**: Use `DynamicFrame.toDF()` with grouping and partitioning

24. **Pivoting Data**: Reshaping data.
    - **Power Query**: `Pivot Column`
    - **SQL**: Use `CASE WHEN` statements or pivot functions
    - **Pandas**: `df.pivot_table(index='index_col', columns='columns_col', values='values_col')`
    - **Spark**: `df.groupBy("index_col").pivot("columns_col").agg(sum("values_col"))`
    - **Glue**: Implement pivoting logic using Spark

25. **Unpivoting Data**: Reshaping data from wide to long format.
    - **Power Query**: `Unpivot Columns`
    - **SQL**: Use `UNION ALL` for transformation
    - **Pandas**: `df.melt(id_vars=['id_col'], value_vars=['value_col1', 'value_col2'])`
    - **Spark**: `df.selectExpr("id_col", "stack(2, 'col1', col1, 'col2', col2) as (variable, value)")`
    - **Glue**: Use Spark transformations for unpivoting

26. **Merging (Join)**: Combining datasets.
    - **Power Query**: `Merge Queries`
    - **SQL**: `JOIN table1 ON table1.col = table2.col`
    - **Pandas**: `pd.merge(df1, df2, on='key')`
    - **Spark**: `df1.join(df2, df1["key"] == df2["key"])`
    - **Glue**: Use Spark transformations for joining

27. **Appending (Union)**: Combining datasets vertically.
    - **Power Query**: `Append Queries`
    - **SQL**: `UNION ALL SELECT * FROM table1 UNION ALL SELECT * FROM table2`
    - **Pandas**: `pd.concat([df1, df2])`
    - **Spark**: `df1.union(df2)`
    - **Glue**: Use Spark transformations for appending

## Special Cases and Considerations

28. **Removing Values**: Based on conditions.
    - **Power Query**: `Remove Rows -> Remove Blank Rows` or `Replace Values`
    - **SQL**: `DELETE FROM table WHERE condition`
    - **Pandas**: `df = df[df['col'] != 'value']`
    - **Spark**: `df.filter(df["col"] != 'value')`
    - **Glue**: Use Spark transformations for value removal

29. **Replacing Values**: Substituting values.
    - **Power Query**: `Replace Values`
    - **SQL**: `UPDATE table SET column = new_value WHERE condition`
    - **Pandas**: `df['col'] = df['col'].replace(old_value, new_value)`
    - **Spark**: `df.withColumn("col", when(df["col"] == old_value, new_value).otherwise(df["col"]))`
    - **Glue**: Use `DynamicFrame.apply_mapping()` with replace logic

30. **Handling Large Datasets**: Optimization tips.
    - **Power Query**: Use data reduction techniques, aggregate data before transformations
    - **SQL**: Use indexing, partitioning
    - **Pandas**: Use `dask` for larger-than-memory datasets
    - **Spark**: Optimize transformations, use caching where necessary
    - **Glue**: Partitioning and optimize job configurations

31. **Validating Data**: Ensuring data integrity.
    - **Power Query**: Use data profiling and validation functions
    - **SQL**: Implement constraints and checks
    - **Pandas**: Use assertions and validation functions
    - **Spark**: Validate data using DataFrame operations
    - **Glue**: Perform validations in ETL jobs

## References

- [Power Query Documentation](https://docs.microsoft.com/en-us/power-query/)
- [SQL Data Types and Functions](https://www.w3schools.com/sql/sql_datatypes.asp)
- [Pandas Documentation](https://pandas.pydata.org/pandas-docs/stable/)
- [Spark Documentation](https://spark.apache.org/docs/latest/)
- [AWS Glue Documentation](https://docs.aws.amazon.com/glue/latest/dg/what-is-glue.html)

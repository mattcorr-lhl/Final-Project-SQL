# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals

My goal with this project was to gain insights into the sales reports provided by using the techniques I have been developing over the past 6 weeks. As a goal, I would like to see if it is possible to extrapolate some useful information out of the database that could indicate how the company is performing.

## Process
When creating the tables I initially brought all the data (other than obvious numeric-only columns) in as `VARCHAR(128)`s to avoid running into data size limits upon import. Afterward, I went through each row and reduced each column to its minimum viable value.

Once imported, I spent some time with basic `SELECT` queries in an attempt to understand what each column could represent. It became apparent pretty early on that the database was missing significant amounts of data.

Then, I performed some data manipulation queries to clean the data, improving the accuracy of my queries later on. (See `cleaning_data.md`)

Analysis was then performed on the database, first by building queries to answer the questions in `starting_with_questions.md` before moving on to my inquiries into the data, which can be found in `starting_with_data.md`.

Afterward, I performed my QA process on the database to confirm my findings (See `QA.md`)

## Results
We were able to discover some valuable insights about this company including:
- The United States is the most prominent user base, making up a significant amount of sales in both quantity and revenue.
  
- Most of the business revenue occurs in the summer months.
  
- There is no strong correlation between page views and number of products ordered.
  
- Users spend the most time on the `payment.html`, `ordercompleted.html` and `revieworder.html` pages indicating transaction times could be sped up if UX was improved on these pages.
  
- Organic Searches and Direct Sales are the most profitable ways of customers reaching us however, we do sell many more units in terms of volume during "display" sales. 

## Challenges
- The amount of missing data made it increasily difficult to draw conclusions (eg the `city` column missing 8656 values)

- I would frequently encounter issues with the data that required a fix before I could proceed. This often required redoing previous queries, which was time-consuming.



## Future Goals

- To be time efficient I only cleaned the columns I deemed necessary for the project. If I had more time I would like to go through the database with greater attention to detail weeding out any incorrect values, which could improve the accuracy of my results

- I would have liked to better normalize the database by splitting apart some of the larger tables (all_sessions & analytics). This would have made queries simpler to create and less intensive on the SQL server.
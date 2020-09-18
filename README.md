
# <center>Data Scientists Salary Estimate Project</center>

## <center>(Source: September 2020 Glassdoor Estimate)</center>
## <center>Notebook: [ML-DS Salary Estimate](https://github.com/haorzeng1997/Data-Scientist-Salary-Project/blob/master/data%20science%20salary%20estimate%20project%20report.pdf)</center>

- Created a tool to estimate data scientists salaries with MAE ~$16K to help future data scientists estimate salary based on job location, company size, company rating, job title, seniority etc.

- Scraped 1000 jobs posted on Glassdoor.com

- Engineered features from 1000 job descriptions to quantify the value of having hottest data science skills including python, excel, aws, spark, tensorflow. 

- Optimized Linear, Lasso, Ridge and Random Forest Regressors using RandomizedSearchCV.

## Resources Used

Python Version: 3.8

Packages: pandas, numpy, sklearn, matplotlib, seaborn, selenium, pickle

Scraper Github: https://github.com/arapfaik/scraping-glassdoor-selenium

Scraper Article: https://towardsdatascience.com/selenium-tutorial-scraping-glassdoor-com-in-10-minutes-3d0915c6d905

## Data Collection
Scrapped 1000 data scientists jobs from Glassdoor.com. For each job, I have:
- Job title
- Salary Estimate (provided by Glassdoor estimate)
- Job Description
- Rating (Company rating)
- Company
- Location
- Company Headquarters
- Company Size
- Company Founded Date
- Type of Ownership
- Industry
- Sector
- Revenue
- Competitors

## Data Cleaning
I cleanned and reorganized the raw data collected from Glassdoor.com for data science purpose. To prepare for model building, I list the data wrangling plan below:

- Salary parsing
    
    The "Salary Estimate" was retrived as object (e.g. \$78K-\$133K (Glassdoor est.)). I removed     "$", "K", "-" and "(Glassdoor est.)" and only left numerical values. The salary                information will be represented by "max_salary", "min_salary" and "avg_salary". 

- Company name parsing

    The "Company" was collected as "_company name  company rating_" format (e.g. Amazon 4.0).     I removed the rating element from the column which makes the Company name text-only.

- Location parsing

    The "Location" column was collcted as "_city name, state abbreviation_" format (e.g.    Chicago, IL). For data science purpose, I decided to only use state information and removed the city information. Including the city information in the models would potentially decrease the efficiency. Using only state information is much more reasonable and effective.

- Age of Company

    The "Founded" column has information about when the company was founded. Instead of using the specific year, I transformed that information into the age of the company.

- Job description parsing

    The "Job description" column contains text information. However, it was very long. Since the goal of this project is to estimate salary, I only extracted useful information from the column. According to _"14 most used data science tools for 2019" (https://data-flair.training/blogs/data-science-tools/)_, python, r studio, spark, aws, excel, sas, matlab, tableau, tensorflow are widely used by data scientists. Thus, I am interested in how many companies would include those tools in their job description pages and what the correlation between having experiences with these tools and potentially earning a higher salary.

## Exploratory Data Analysis
- Correlation between age of companies/rating of companies/job description length and average salary.
	- I had a few assumptions before doing the correlation analysis:
		- More established companies would pay higher salaries (age vs salary)
		- High rating companies would pay higher salaries (rating vs salary)
		- Jobs with long job descriptions would pay higher salaries (job description length vs salary)
![alt text][logo1]

[logo1]: https://github.com/haorzeng1997/Data-Scientist-Salary-Project/blob/master/graph/correlation.png "correlation"
	- Surprisingly, the correlations are weak. Among age, rating, and description_length, age of the company seems to be correlated with salary. So, we may assume that established companies usually offer higher salaries for data scientists.

- Companies from which states hire more data scientists?
![alt text][logo2]

[logo2]: https://github.com/haorzeng1997/Data-Scientist-Salary-Project/blob/master/graph/job%20state.png "job_state"
	- Companies based on California posts around 1/4 of data scientists jobs. Virginia posts almost 150 out of 1000 data scientists jobs. Because of the pandemic, remote jobs demand is also high.

- Companies from which sectors hire more data scientists?
![alt text][logo3]

[logo3]: https://github.com/haorzeng1997/Data-Scientist-Salary-Project/blob/master/graph/sector.png "sector"
	-	Companies from information technology industry posts ~300 out of 1000 data scientists jobs. The second hottest industry for data scientists is business services industry. 

- Companies of which types of ownership hire more data scientists?
![alt text][logo4]

[logo4]: https://github.com/haorzeng1997/Data-Scientist-Salary-Project/blob/master/graph/type%20of%20ownership.png "type_of_ownership"
	-	Not surprisingly, private companies post more than 500 out of 1000 data scientists jobs.

- What is the most popular programming language for data scientist?
![alt text][logo5]

[logo5]: https://github.com/haorzeng1997/Data-Scientist-Salary-Project/blob/master/graph/python.png "python"
	-	Python appears almost in 700 out of 1000 pieces of job descriptions.

- What words are frequently mentioned across 1000 job descriptions?
![alt text][logo6]

[logo6]: https://github.com/haorzeng1997/Data-Scientist-Salary-Project/blob/master/graph/word%20cloud.png "word cloud"
	-	Experience, data science, machine learning and product sense are areas where job applicants should focus on.
	
## Model Building
I wanted to build a regression model to help future data scientists get their up-to-date salary estimate.

I performed:

-   Multiple linear regression
    Simple linear regression is easy to understand and can be a good starting point for building regression models. 
    
-   Lasso regression
    The goal of lasso regression is to obtain the subset of predictors that minimizes prediction error for a quantitative response variable. If the number of significant features is small, Lasso will perform well. The data may show high levels of muticollinearity, so I tried Lasso regression. The Lasso regularization will actually set less-important predictors to 0.
    
-   Ridge regression
    Ridge Regression is a technique for analyzing multiple regression data that suffer from multicollinearity. In contrast with Lasso, which uses L1 regularization, Ridge uses L2 regularization.
    
-   Random forest regressor
    The data contains many categorical columns. Thus, I assume tree-based models would perform very well. 

I would use  **"negative mean absolute error"**  to score each model. Since I am trying to predict a numerical value, I think mean absolute error (MAE) would be the most direct score to compare.

## Model Performances

The Random Forest regressor outperformed the other models on the test and validation sets.

- Random forest regressor MAE: 16.69
- Multiple linear regression MAE: 21.75
- Ridge regression MAE: 21.97
- Lasso regression MAE: 22.28


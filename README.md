# Overview

Welcome to my analysis of the data job market, focusing on data analyst roles. This project was created out of a desire to navigate and understand the job market more effectively. It delves into the top-paying and in-demand skills to help find optimal job opportunities for data analysts.

The data sourced from a [public hugging face database](https://huggingface.co/datasets/lukebarousse/data_jobs), containing detailed information on job titles, salaries, locations, and essential skills. Through a series of Python scripts, I explore key questions such as the most demanded skills, salary trends, and the intersection of demand and salary in data analytics.

Please note that the data presented here encompasses the entire year of 2023.


# The Questions

Below are the questions I want to answer in my project:

    1.What are the skills most in demand for the top 3 most popular data roles in France ?

    2.How are in-demand skills trending for Data Scientists in France ?

    3.How well do jobs and skills pay for Data Scientists in the US ?

    4.What are the optimal skills for Data Scientists to learn (United States) ? (High Demand AND High Paying)


# Tools I Used


- Python: The backbone of my analysis, allowing me to analyze the data and find critical insights.I also used the following Python libraries: 

    - Pandas Library: This was used to analyze the data.
    - Matplotlib Library: I visualized the data.
    - Seaborn Library: Helped me create more advanced visuals.

- Jupyter Notebooks: The tool I used to run my Python scripts which let me easily include my notes and analysis.

- Visual Studio Code: My go-to for executing my Python scripts.

- Git & GitHub: Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.


# Environment Management with Anaconda

Managing environments effectively is crucial when working on data analytics projects, as different libraries and dependencies can lead to conflicts or compatibility issues. Throughout this project, I relied on Anaconda to create and manage isolated environments, ensuring a stable and reproducible workflow.

By using Anaconda, I was able to:

- Maintain Dependency Compatibility: Different libraries often require specific versions of dependencies. With Anaconda environments, I could install and manage packages without affecting my system-wide Python setup.

- Ensure Reproducibility: Creating a dedicated environment allowed me to document the exact package versions used, making it easier to reproduce results or share my work with others.

- Work with Multiple Libraries: Throughout the project, I utilized various libraries such as pandas for data manipulation, matplotlib and seaborn for visualization, and scikit-learn for machine learning. Anaconda made it seamless to install and switch between different toolsets as needed.

- Avoid Conflicts: Some libraries require specific versions of dependencies that may not be compatible with others. By isolating my project in an Anaconda environment, I prevented conflicts that could have disrupted my workflow.

# Data Preparation and Cleanup

This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.

## Import & Clean Up Data

I start by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.

```python
# Importing Libraries
import ast
import pandas as pd
import seaborn as sns
from datasets import load_dataset
import matplotlib.pyplot as plt  

# Loading Data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```

## Filter France Jobs

To focus my analysis on the France job market, I apply filters to the dataset, narrowing down to roles based in France.

```python
df_FR = df[df['job_country'] == 'France']
```

# The Analysis

## 1. What are the most demanded skills for the top 3 most popular data roles in France ?

To find the most demanded skills for the top 3 most popular data roles. I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting.

# Visualize Data

```python
fig, ax = plt.subplots(len(job_titles), 1)


for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)[::-1]
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')

plt.show()
```

## Results

![Vis Skills](Visualisations/Skill%20Demand/Skill_Demand_Likelihood.png)
*Bar graph visualizing the salary for the top 3 data roles and their top 5 skills associated with each.*

## Insights 

- SQL is a Highly Demanded Skill Across Roles: SQL is prominently featured in job postings for Data Analysts (45%), Data Engineers (49%), and Data Scientists (33%). This indicates that SQL is a critical skill for professionals in data-related roles in France.

- Python is a Key Skill for Data Engineers and Data Scientists: Python is highly requested for Data Engineers (57%) and Data Scientists (67%). This suggests that Python is essential for roles that involve more complex data processing and analysis.

- Cloud Platforms are Important for Data Engineers: Skills related to cloud platforms like AWS (27%) and Spark (35%) are significant for Data Engineers. This reflects the growing importance of cloud computing in data engineering tasks.

## 2. How are in-demand skills trending for Data Scientists in France ?


To find how skills are trending in 2023 for Data Analysts, I filtered data analyst positions and grouped the skills by the month of the job postings. This got me the top 5 skills of data analysts by month, showing how popular skills were throughout 2023.

## Visualize Data

```python
fig, ax = plt.subplots(len(job_titles), 1)

from matplotlib.ticker import PercentFormatter

df_plot = df_DA_US_percent.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, legend='full', palette='tab10')

plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.show()
```

## Results

![skill_trend](Visualisations\Skill%20Trend\Skill_Trend.png)
*Bar graph visualizing the trending top skills for data analysts in the US in 2023.*

## Insights 

- Python is the Most In-Demand Skill: Python consistently has the highest likelihood of being mentioned in job postings for Data Scientists throughout the year, indicating its critical importance in the field.

- SQL and Git are Also Highly Valued: SQL and Git show significant presence in job postings, suggesting that data manipulation and version control are essential skills for Data Scientists in France.

- R and Spark Have Moderate Demand: While R and Spark are mentioned in job postings, their likelihood is lower compared to Python, SQL, and Git. This indicates that while they are important, they are not as universally required as the top three skills.


# 3. How well do jobs and skills pay for Data Scientists in the US  ?

For this section i decided to focus my analysis in the US as most of the job postings from the database are from the US.

To identify the highest-paying roles and skills, I only got jobs in the United States and looked at their median salary. But first I looked at the salary distributions of common data jobs like Data Scientist, Data Engineer, and Data Analyst, to get an idea of which jobs are paid the most.


## Visualize Data


```python
sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', order=job_order)

ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```

## Results

![skill_dist](Visualisations\Salary%20Analysis\Salary_Distribution.png)
*Box plot visualizing the salary distributions for the top 6 data job titles.*



## Insights


- Senior Roles Command Higher Salaries: Senior positions such as Senior Data Scientist and Senior Data Engineer have higher salary distributions compared to their non-senior counterparts. This indicates that experience and seniority significantly impact earning potential in data-related roles.

- Data Scientists Earn More Than Data Engineers: The salary distribution for Data Scientists is generally higher than that for Data Engineers, suggesting that Data Scientist roles may offer higher compensation on average.

- Data Analysts Have the Lowest Salary Range: Data Analysts, both senior and non-senior, have the lowest salary distributions among the listed roles. This reflects the relative entry-level nature of these positions compared to more specialized roles like Data Scientists and Data Engineers.


## Highest Paid & Most Demanded Skills for Data Analysts

Next, I narrowed my analysis and focused only on data scientist roles. I looked at the highest-paid skills and the most in-demand skills. I used two bar charts to showcase these.

## Visualize Data

```python
fig, ax = plt.subplots(2, 1)  

# Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')

# Top 10 Most In-Demand Skills for Data Analystsr')
sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, hue='median', ax=ax[1], palette='light:b')

plt.show()
```

## Results

Here's the breakdown of the highest-paid & most in-demand skills for data scientists in the US:

![high_paid](Visualisations\Salary%20Analysis\Salary_Skillls_In_Demand.png)
*Separate bar graphs visualizing the highest paid skills and most in-demand skills for data scientists in the US.*


## Insights

- Specialized Tools Command Higher Salaries: Skills like Asana, Airtable, and Watson are associated with higher median salaries, indicating that niche or specialized tools can significantly boost earning potential for Data Scientists in the US.

- Core Technical Skills Are Most In-Demand: Skills such as TensorFlow, Spark, SQL, and Python are highly in-demand, reflecting their fundamental importance in the day-to-day work of Data Scientists. Despite their high demand, these skills do not always correlate with the highest salaries.

- Discrepancy Between High-Paid and High-Demand Skills: There is a noticeable gap between the highest-paid skills and the most in-demand skills. This suggests that while certain specialized skills can command higher salaries, the core technical skills remain essential for most Data Scientist roles


# 4. What are the most optimal skills to learn for Data Scientists ?

To identify the most optimal skills to learn ( the ones that are the highest paid and highest in demand) I calculated the percent of skill demand and the median salary of these skills. To easily identify which are the most optimal skills to learn.

Then to go deeper in the analysis,let's visualize the different technologies as well in the graph. We'll add color labels based on the technology (e.g., {Programming: Python})

## Visualize Data

```python
from matplotlib.ticker import PercentFormatter

# Create a scatter plot
scatter = sns.scatterplot(
    data=df_DA_skills_tech_high_demand,
    x='skill_percent',
    y='median_salary',
    hue='technology',  # Color by technology
    palette='bright',  # Use a bright palette for distinct colors
    legend='full'  # Ensure the legend is shown
)
plt.show()
```

## Results

![opt_skills](Visualisations\Optimal%20Skills\Optimal_Skills.png)
*A scatter plot visualizing the most optimal skills (high paying & high demand) for data scientists in the US with color labels for technology.*


## Insights

- TensorFlow and Spark are Highly Valued: TensorFlow and Spark are among the most optimal skills for Data Scientists, with a significant percentage of job postings requiring these skills. This indicates their importance in advanced data processing and machine learning tasks.

- Core Programming Skills are Essential: Skills like Python and SQL are crucial, appearing in a large percentage of job postings. This underscores the importance of strong programming and data manipulation capabilities in the Data Science field.

- Cloud and Analyst Tools are Growing in Importance: Cloud technologies (e.g., AWS) and analyst tools (e.g., Tableau, Excel) are increasingly relevant, reflecting the growing need for data scientists to be proficient in cloud computing and data visualization.


# What I Learned

Throughout this project, I deepened my understanding of the data scientist job market and enhanced my technical skills in Python, especially in data manipulation and visualization. Here are a few specific things I learned:

- Advanced Python Usage: Utilizing libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visualization, and other libraries helped me perform complex data analysis tasks more efficiently.

- Data Cleaning Importance: I learned that thorough data cleaning and preparation are crucial before any analysis can be conducted, ensuring the accuracy of insights derived from the data.

- Strategic Skill Analysis: The project emphasized the importance of aligning one's skills with market demand. Understanding the relationship between skill demand, salary, and job availability allows for more strategic career planning in the tech industry.


# Insights

This project provided several general insights into the data job market for analysts:

- Core Technical Skills are Fundamental: Skills like SQL, Python, and data visualization tools (e.g., Tableau, Excel) are essential for data analysts. These skills are frequently mentioned in job postings and are critical for data manipulation, analysis, and reporting.

- Experience and Specialization Enhance Earning Potential: Senior roles, such as Senior Data Analyst, command higher salaries compared to entry-level positions. Additionally, specialized skills in tools like Asana, Airtable, or advanced analytics platforms can further boost earning potential.

- Growing Importance of Cloud and Advanced Analytics: There is an increasing demand for proficiency in cloud technologies (e.g., AWS) and advanced analytics tools (e.g., TensorFlow, Spark). This trend indicates that data analysts who can leverage these technologies are likely to have a competitive edge in the job market.


# Challenges I Faced


This project was not without its challenges, but it provided good learning opportunities:


- Data Inconsistencies: Handling missing or inconsistent data entries requires careful consideration and thorough data-cleaning techniques to ensure the integrity of the analysis.

- Complex Data Visualization: Designing effective visual representations of complex datasets was challenging but critical for conveying insights clearly and compellingly.

- Balancing Breadth and Depth: Deciding how deeply to dive into each analysis while maintaining a broad overview of the data landscape required constant balancing to ensure comprehensive coverage without getting lost in details.


# Conclusion

This exploration into the data analyst job market has been incredibly informative, highlighting the critical skills and trends that shape this evolving field. The insights I got enhance my understanding and provide actionable guidance for anyone looking to advance their career in data analytics. As the market continues to change, ongoing analysis will be essential to stay ahead in data analytics. This project is a good foundation for future explorations and underscores the importance of continuous learning and adaptation in the data field.
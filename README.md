# Overview

This project is about diving deep into the job market and the specific details about the job postings so that one may quickly and efficiently understand each job posting as well determine whether an individual is a good match for the job and if the job is a good match for them.

Throughout this project, I explore different areas of focus such as the skills demanded, the existing salary trends, and the demand/salary for data analytical roles

# The Questions

Below are the questions I want to answer in this project:

1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the optimal skills for Data Analysts to learn? In other words, which skills are in high demand and offer a high salary?

# Tools I Used

The following are the tools used to help build this project.

- Python: This language served as the foundation for which I was able to perform the following analysis. It allowed me to gather and collect key insights on the questions I wanted to focus on as well as critically analyze the data and the findings from the analysis. Below are the libraries I used to help me achieve this:
    - Pandas: Used for analyzing the data
    - Matplotlib: Used to create visualizations of the data
    - Seaborn: Used for more advanced visualizations of the data and manipulating those visualizations

# Data Preparation and Cleanup

During this project, I used the following code to prepare the data and clean it for proper analysis:

### Import & Clean Up Data
```
# Importing Libraries

import pandas as pd
import ast
from datasets import load_dataset
import matplotlib.pyplot as plt
import seaborn as sns

# Loading Data

dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Cleaning Data

df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x : ast.literal_eval(x) if pd.notna(x) else(x))
```
### Filter US Jobs
```
df_US = df[df['job_country'] == 'United States']
```

# The Analysis

Each notebook aims to answer the following specific questions: 

## 1. What are the most demanded skills for the top 3 most popular data roles?

To find the most demanded skills for the top 3 most popular roles, I filtered out those positions by which ones were the most popular, and got the otp 5 skills for these Top3 Roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting.

You can find the detailed steps in here:
[2_Skills_Demand.ipynb](3_Projects\2_Skills_Demand.ipynb)


### Visualize Data

```
fig, ax = plt.subplots(len(job_titles), 1)

sns.set_theme(style='ticks')

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)
    df_plot.plot(kind='barh', x='job_skills', y='skill_percent', ax=ax[i], title=job_title)
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')
    ax[i].set_title(job_title)
    ax[i].set_xlabel('')
    ax[i].set_ylabel('')
    ax[i].get_legend().remove()
    ax[i].set_xlim(0, 78)

    for n, value in enumerate(df_plot['skill_percent']):
        ax[i].text(value + 1, n, f'{value:.0f}%', va='center')
    
    if i != len(job_titles) - 1: 
        ax[i].set_xticks([])
    
fig.suptitle('Likelihood of Skills Requested in US Job Postings', fontsize=15)
plt.tight_layout()
plt.show()
```

### Results
![Visualization of Top Skills for Data Nerds](3_Projects\Images\Skill_Demand_All_Data_Roles.png)

### Insights

- Python is versatile skill, highly demanded across all three roles, but most prominently for Data Scientists (72%) and Data Engineers (65%)
- SQL is the most requested skill for Data Analysts and Data Scientists, with it in over half the job postings for both roles. For Data Engineers, Python is the most sought-after skill, appearing in 69% of job postings.
- Data Engineers require more specialized, technical skills (AWS, Azure, Spark) compared to Data Analysts and Data Scientists who are expected to be proficient in more general data management and analysis tools (Excel, Tableau)

## 2. How are in-demand skills trending for Data Analysts?

I tracked 2023 Data Analyst skill trends by filtering job postings, grouping skills by month, and identifying the top five skills to show monthly popularity.

You can find the detailed steps in here:
[3_Skills_Trend.ipynb](3_Projects\3_Skills_Trend.ipynb)

### Visualize the Data
```

from matplotlib.ticker import PercentFormatter

df_plot = df_da_us_percent.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, palette='tab10')

plt.title('Trending Top Skills for Data Analysts in the US')
plt.ylabel('Likelihood in Job Posting')
plt.xlabel('2023')
plt.legend().remove()

ax=plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.show()

```

### Results
![Trending Top Skills for Data Analyst in the US](3_Projects\Images\Skill_Trend_Da.png)

*Line graph visualizing the trending top skills for data analysts in the US in 2023.*

### Insights
- SQL remains the most consistently demanded skill throughout the year, although it shows a gradual decrease in demand
- Excel experienced a significant increase in demand startting around September, surpassing both Python and Tableau by the end of the year
- Both Python and Tableau show relatively stable demand throughout the year with some fluctuations but remain essential for data analysts. SAS, while less demanded compared to others, shows a slight upward trend towards the year's end

## 3. How well do jobs and skills pay for Data Analysts

Here is where I analyzed U.S. data jobs by median salary, focusing on high-paying roles like Data Scientist, Data Engineer, and Data Analyst.

You can find the detailed steps in here:
[4_Salary_Analysis.ipynb](3_Projects\4_Salary_Analysis.ipynb)

### Salary Analysis for Data Nerds

### Visualize Data

```
sns.boxplot(data=df_us_top6, x= 'salary_year_avg', y='job_title_short', order=job_order)

ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```

### Results
![Salary Distributions of Data Jobs in the US](3_Projects\Images\Salary_Analysis_Data_Roles.png)
*Box plot visualizing the salary distributions for the top 6 data job titles.*

### Insights
- There is signifcant variation in salary ranges across different job titles. Senior Data Scientist positions tend to ave the highest salary potential, with up to $600k, indicating the high value placed on advanced data skills and experience in the industry

- Senior Data Engineer and Senior Data Scientist roles show a considerable number of outliers on the higher end of the salary spectrum, suggesting that exceptional skills or circumstances can lead to high pay in these roles. In contrast, Data Analyst roles demonstrate more consistency in salary, with fewer outliers

- The median salaries increase iwth the seniority and specialization of the roles. Senior roles (Senior Data Scientist, Senior Data Engineer) not only have higher median salaries but also larger differences in typical salaries reflecting greater variance in compensation as responsibilities increase.

### Highest Paid and Most Demanded Skills for Data Analysts

### Visualize the Data

```
#Plot for Top 10 Highest Paid Skills

sns.barplot(data=df_da_top_pay, x='median', y=df_da_top_pay.index, ax=ax[0], hue='median', palette='dark:b_r', legend=False)

#Plot for Top 10 Most in Demand Skills

sns.barplot(data=df_da_skills, x='median', y=df_da_skills.index, ax=ax[1], hue='median', palette='light:b', legend=False)
```
### Results
![Highest Paid and Most Demanded Skills for Data Analysts](3_Projects\Images\Highest_Paid_and_Most_Demanded_Skills_DA.png) 

*Two Separate Bar Charts - Top one showing the Highest Paid Skills and the Bottom One showing the Most Demanded Skills

### Insights

- The top graph shows specialized technical skills like 'dplyr', 'Bitbucket', 'Gitlab' are associated with higher salaries, some reaching up to $200k , suggesting that advanced technical proficiency can increase earning potential

- The bottom graph highlights that foundational skills like 'Excel', 'PowerPoint', and 'SQL' are the most in-demand, even though they may not offer the highest salaries. This demonstrates the importance of these core skills for employability in data analysis roles

- There's a clear distinction between the skills that are highest paid and those that are most in-demand. Data analysts aiming to maximize their career potential should consider developing a diverse skill set that includes both high-paying specialized skills and widely demanded foundational skills

## 4. What is the most optimal skill to learn for Data Analysts?

Here I identified the best skills to learn by calculating their demand percentage and median salary, highlighting those that are highest paid and most in demand.

You can find the detailed steps in here:
[5_Salary_Analysis.ipynb](3_Projects\5_Optimal_Skills.ipynb)

#### Visualize Data

```
from adjustText import adjust_text
from matplotlib.ticker import PercentFormatter

sns.scatterplot(
    data=df_plot,
    x='skill_percent',
    y='median_salary',
    hue='technology'
    )

for i, txt in enumerate(df_da_skills_high_demand.index):
    texts.append(plt.text(df_da_skills_high_demand['skill_percent'].iloc[i], df_da_skills_high_demand['median_salary'].iloc[i], txt))

```

#### Results
![Most Optimal Skills for Data Analysts in the US](3_Projects\Images\Most_Optimal_Skills_for_Data_Analysts_in_the_US_with_Coloring_by_Technology.png)

*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in the US.*

#### Insights

- The scatter plot shows that most of the "programming" skills (colored blue) tend to cluster at higher salary levels compared to other categories, indicating that programming expertise might offer greater salary benefits within the data analytics field

- Analyst tools (colored green), including Tableau and Power BI, are prevalent in job postings and offer competitive salaries, showing that visualization and data analysis software are crucial for current data roles. This category not only has good salaries but is also versatile across different of data tasks.

- The database skills (colord orange), such as Oracle and SQL server, are associated with some of the highest salaries among data analyst tools. This indicates a significant demand and valuation for data management and manipulation expertise in the industry

# What I learned

Throughout this project, I was able to find incredible insights on the job market and the skills demanded for data analyst positions and other positions in the field of analytics as well. But more importantly, I learned how to use my skills in Python to create deep and insightful analysis. Here are some things that I learned:
 - Python and Git/GitHub: Using Python and the number of libraries that go along with it allows for some increadible analysis, but utilizing Git/GitHub also served as a way of telling me how important storage was for these scripts and why it is so effective.
 - The Importance of Annotation: Writing comments was something that I was already familiar with, but the importance was something that I had to relearn. Especially if it has been a couple of hours since you have looked at a specific block of code, annotating your notes as comments served as a helpful reminder as to why you did something before or what made something work.
 - Strategically Approaching a Dataset: Approaching the dataset in a careful manner allows one to determine the best course of action. Rather than jumping the gun, it is much better to create a clear outline and framework of what you want to achieve beforehand. That way, everything is taken care of easily.

# Insights
- Correlation between Skills and Salary: During this project, I was able to see which skills earned the most money and the job titles those came with. This helped me understand the demand that recruiters may be looking for.
- Understanding Demand Trends: Understanding the trends in demand for certain positions is important because if you see that your position is in less and less demand, it could be worth investigating if a switch is worth it.
- Calculating Self-Worth: Thinking of the skills you have is easy, but determining how much those skills are worth is a different question. Looking at the visualizations allowed me to see how much one would be worth should a certain skill be acquired.

# Challengs Faced
- Dynamic Skill Trends: Skill demand and salary trends fluctuated, requiring adaptable methods to capture shifts accurately over time.
- Time Constraints: With a limited timeframe, prioritizing key areas without sacrificing detail was essential to meet project goals.
- Determing Best Data Visualizations: Designing clear, effective visualizations for complex datasets was challenging but essential for conveying insights compellingly.

# Conclusion

This analysis of the data analyst job market has revealed valuable insights into essential skills and emerging trends in the field.The findings enhance my understanding and offer practical guidance for career growth in data analytics. As the market shifts, ongoing analysis will be crucial, and this project lays a solid foundation for future research, underscoring the need for continuous learning and adaptability.
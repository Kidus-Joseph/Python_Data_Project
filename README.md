# The Analysis

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

### Insights
- SQL remains the most consistently demanded skill throughout the year, although it shows a gradual decrease in demand
- Excel experienced a significant increase in demand startting around September, surpassing both Python and Tableau by the end of the year
- Both Python and Tableau show relatively stable demand throughout the year with some fluctuations but remain essential for data analysts. SAS, while less demanded compared to others, shows a slight upward trend towards the year's end

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
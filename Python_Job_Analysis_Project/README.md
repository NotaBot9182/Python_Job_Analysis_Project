# Overview

Welcome to my analysis of the data job market, focusing on data analyst roles. This project was created out of a desire to navigate and understand the job market more effectively. It delves into the top-paying and in-demand skills to help find optimal job opportunities for data analysts.

The data sourced from [Luke Barousse's Python Course](https://lukebarousse.com/python) which provides a foundation for my analysis, containing detailed information on job titles, salaries, locations, and essential skills. Through a series of Python scripts, I explore key questions such as the most demanded skills, salary trends, and the intersection of demand and salary in data analytics.

# The Questions

Below are the questions I want to answer in my project:

1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the optimal skills for data analysts to learn? (High Demand AND High Paying) 

# Tools I Used

For my deep dive into the data analyst job market, I harnessed the power of several key tools:

- **Python:** The backbone of my analysis, allowing me to analyze the data and find critical insights.I also used the following Python libraries:
    - **Pandas Library:** This was used to analyze the data. 
    - **Matplotlib Library:** I visualized the data.
    - **Seaborn Library:** Helped me create more advanced visuals. 
- **Jupyter Notebooks:** The tool I used to run my Python scripts which let me easily include my notes and analysis.
- **Visual Studio Code:** My go-to for executing my Python scripts.
- **Git & GitHub:** Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.

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

# The Analysis

Each Jupyter notebook for this project aimed at investigating specific aspects of the data job market. Here’s how I approached each question:

## 1.What are the most demanded skills for the top 3 most popular data roles?

To find the most demanded skills for the top 3 most popular data roles. I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting.

View my notebook with detailed steps here: [2_Skill_Demand](Python_Job_Analysis_Project\2_Skill_Demand.ipynb)

### Visualize Data
```python
fig,ax=plt.subplots(len(job_titles),1)
sns.set_theme(style='ticks')

for i,job in enumerate(job_titles):
    df_plot=df_skills_perc[df_skills_perc['job_title_short']==job].head()
    #df_plot.plot(kind='barh',x='job_skills',y='percentage',ax=ax[i],legend=False,title=job)
    sns.barplot(data=df_plot,x='percentage',y='job_skills',palette='dark:b_r',ax=ax[i],hue='count')
    ax[i].set_ylabel('')
    ax[i].set_xlabel('')
    ax[i].set_title(job)
    ax[i].legend().remove()
    ax[i].set_xlim(0,15)

    for n,v in enumerate(df_plot['percentage']):
        ax[i].text(v+0.1,n,f'{v:.1f}%',color='black',va='center')
    
    if i!=len(job_titles)-1:
        ax[i].set_xticks([])
    

fig.tight_layout()
plt.show()

```
### Results

![Image](Python_Job_Analysis_Project\Pictures\2_Skill_demand.png)

### Insights:
- 🌍 SQL and Python are universal: They are the top two most required skills across all three data roles.
- 🧠 Data Scientists need Python most (13.1%): Their focus is on building models and algorithms, supported by R, SQL, and AWS.
- 🎯 Specialization happens after the top two: Analysts focus on reporting tools, Scientists on statistics/modeling, and Engineers on cloud infrastructure.

## 2.How are in-demand skills trending for Data Analysts?

### Visualize code

```python
sns.lineplot(data=df_plot,dashes=False,palette='tab10')
sns.set_theme(style='ticks')

plt.title('Top 5 Skills Trend for Data Analyst Jobs in India')
plt.xlabel('2023')
plt.ylabel('Percentage of Job Postings (%)')
plt.legend().remove()
sns.despine()

for i in range(5):
    plt.text(11.1,df_plot.iloc[-1,i], df_plot.columns[i], color=sns.color_palette('tab10')[i], va='center')
```

![](Python_Job_Analysis_Project\Pictures\3_Skills_demand_line_chart.png)

## Insights:

- 👑 SQL is the Undisputed Leader: Throughout the entire year, SQL consistently remained the most highly demanded skill, appearing in over 50% of job postings every month and peaking at nearly 70% in May.
- 📈 The "May Spike" Anomaly: There was a very noticeable, simultaneous spike in demand for both SQL and Excel specifically in May. Excel nearly reached 60% before dropping sharply back to its baseline in June.
- 🚀 Late-Year Surge for Power BI: While Power BI remained the lowest-demanded skill among the top 5 for the vast majority of the year, it experienced a sharp, steep increase in demand between November and December, closing the gap with Tableau.

## 3.How well do jobs and skills pay for Data Analysts?

### Visualize Data
```python
sns.boxplot(data=df_In_top6,x='salary_year_avg',y='job_title_short',order=job_order)
sns.set_theme(style='ticks')

plt.title('Salary Distribution by Job Title (India)')
plt.xlabel('Average Yearly Salary (USD)')
plt.ylabel('')
plt.xlim(0,300000)
plt.show()
```
![](Python_Job_Analysis_Project\Pictures\4_Salary_Analysis_Box_plot.png)

## Insights:
- 🤖 Machine Learning Engineers (Widest Variance): This role has the largest pay spread. The middle 50% earn between ~$80k and $165k, but total pay stretches from $35k to nearly $270k—offering massive earning potential but huge variability based on the specific job. 💸
- 💻 Software Engineers (Tight Core Pay + Outliers): Most salaries are tightly clustered in a narrow band between $65k and $80k. However, a select few distinct outliers earn significantly more, hitting up to $200k. 🚀
- 📊 Senior Data Engineers (Highly Concentrated): Unlike the other roles, this dataset collapses into a single line at ~$150k. This means core salaries are heavily concentrated at that exact number, flanked by scattered outliers both higher and lower. 🎯

## 4.What is the most optimal skill to learn for Data Analysts?

### Visualise Data

```python
from adjustText import adjust_text

plt.figure(figsize=(10,6))

plt.scatter(df_DA_skills_high_demand['skill_percent'], df_DA_skills_high_demand['median_salary'])
plt.xlabel('Percent of Data Analyst Jobs')
plt.ylabel('Median Salary ($USD)')  # Assuming this is the label you want for y-axis
plt.title('Most Optimal Skills for Data Analysts in the US',fontsize=20,fontweight='bold')

# Get current axes, set limits, and format axes
ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))  # Example formatting y-axis

# Add labels to points and collect them in a list
texts = []
for i, txt in enumerate(df_DA_skills_high_demand.index):
    texts.append(plt.text(df_DA_skills_high_demand['skill_percent'].iloc[i], df_DA_skills_high_demand['median_salary'].iloc[i], " " + txt))

# Adjust text to avoid overlap and add arrows
adjust_text(texts, arrowprops=dict(arrowstyle='->', color='gray'))

plt.tight_layout()
plt.show()
```

![](Python_Job_Analysis_Project\Pictures\5_Optimal_Skills.png)

## Insights:
- 📊 Foundational Skills are High Demand, Average Pay: SQL, Excel, and Python are the absolute core of the data analyst toolkit, appearing in roughly 38% to 50% of all job postings. However, because these are baseline requirements, their median salaries sit right in the middle of the pack at around $95K to $98K.
- 📈 BI Tools Offer a Solid Salary Bump: Visualization tools like Power BI and Tableau sit in a great "sweet spot." They are moderately requested (around 18% to 22% of jobs) but command noticeably higher median salaries (~$108K to $112K) compared to foundational programming and spreadsheet skills.
- 🚀 Niche Tech Commands Premium Outlier Pay: MongoDB is the massive outlier on this chart. While it has very low demand (required in less than 10% of roles), it pays the absolute highest median salary at over $160K, proving that rare, specialized database management skills are highly lucrative.
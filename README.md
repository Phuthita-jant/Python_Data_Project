# Overview
Welcome to my analysis of the data jobs market, with a focus on Data Analyst roles. This project was driven by my curiosity to better understand and navigate the data job market. It explores the top-paying and most in-demand skills to help job seekers identify opportunities in data analytics. 

The data was sourced from Luke Barousse’s Python Course, which provides valuable insights for this analysis. The dataset contains detailed information on job titles, salaries, locations, and preferred skills. Using this data, I explored the most in-demand skills and salary trends within the data analytics field.

# The Questions
Here are the questions I aim to answer in this project:
1. What skills are most in demand for the top three data roles?
2. How do skill demand trends change over time for Data Analysts?
3. What are the typical salary ranges for Data Analysts?
4. Which skills offer the best combination of high demand and high compensation for Data Analysts?

# Tool I Used
To analyze the data analyst job market in depth, I leveraged several key tools:

- **Python**: The primary tool used for this analysis, enabling data processing and the extraction of meaningful insights. I utilized the following Python libraries:
   - **Pandas:** Used for data manipulation and analysis
   - **Matplotlib:** Used for data visualization
   - **Seaborn:** Used to create more advanced and visually appealing charts
- **Jupyter Notebooks:** Used to run Python scripts while documenting notes and analysis in an interactive environment.
- **Visual Studio Code:** Used to write, test, and execute Python scripts.
- **Git & GitHub:** Used for version control and for sharing the analysis code.

# Data Preparation
This section details the steps taken to prepare the data for analysis.

## Import and cleanup Data
I started by importing the necessary libraries, loading the dataset, and cleaning the data to ensure it was ready for analysis.

```python
import ast 
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
from datasets import load_dataset

#load dataset
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

#clean data
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```
## Filter TH jobs
To focus specifically on the Thailand job market, I filtered the dataset to include only roles based in the Thailand.

```python
df_TH = df[df['job_country'] == 'Thailand']
```

# The Analysis
In this project, each Jupyter notebook explores a specific aspect of the data job market. Below is my approach to each question:

## 1. What skills are most in demand for the top three data roles?
Driven by my curiosity about the data job market, I explored the most in-demand skills across the three most popular data roles. By narrowing the dataset to the most frequently posted positions, I identified the top five skills for each role. This analysis highlights which skills matter most for different data roles, offering guidance on where to focus when targeting a specific career path.

View the full notebook with detailed steps here: [2_Skill_Demand](3_Project/2_Skill_Demand.ipynb).

### Visualize Data
```python
#plot skills perc
fig, ax = plt.subplots(len(job_titles), 1)

sns.set_theme(style='ticks')
for i, job_title in enumerate(job_titles):
    df_plot = df_perc[df_perc['job_title_short'] == job_title].head(5)[::-1]
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_percent', palette='dark:b_r')
    ax[i].set_title(job_title)
    ax[i].invert_yaxis()
    ax[i].set_ylabel('')
    ax[i].set_xlabel('')
    ax[i].get_legend().remove()
    ax[i].set_xlim(0, 70)

#remove x_axis ticks for readability
    if i != (len(job_titles) - 1):
        ax[i].set_xticks([])

#label percentages on bars
    for n, v in enumerate(df_plot['skill_percent']):
        ax[i].text(v + 1, n, f'{v:.0f}%', va='center')

fig.suptitle('Likelihood of Preferred Skill for Job Postings in Thailand', fontsize=15)
fig.tight_layout(h_pad=0.8)
plt.show()
```
### Results
![Likelihood of Skills Requested in Thailand Job Postings](3_Project/Images/2_Skill_Demand.png)

*Bar graph comparing salaries across the top three data roles, highlighting the five most important skills for each role.*

### Insights:
- SQL was identified as a core skill across data roles, appearing in over half of job postings for both Data Analysts and Data Engineers. In contrast, Python was the most in-demand skill for Data Scientists, featured in approximately 49% of postings.
- Data Engineers require more specialized technical skills, such as AWS, Hadoop, and Spark, reflecting their focus on data infrastructure. Meanwhile, Data Analysts and Data Scientists are expected to be proficient in more general data analysis and management tools, including Excel and Tableau.
- Data Scientists also show demand for statistical and analytical programming skills, such as SAS and R.

## 2. How do skill demand trends change over time for Data Analysts?
To understand how data analyst skills trended throughout 2023, I filtered the dataset to focus on data analyst roles and grouped skills by the month of each job posting. This allowed me to identify the top five skills for each month, revealing how skill demand evolved over the year.

View the notebook with detailed steps here: [3_Skills_Trend](3_Project/3_Skills_Trend.ipynb).

### Visualize Data
```python
#plot percentage skills trend
from matplotlib.ticker import PercentFormatter

df_plot =df_DA_TH_perc.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, legend='full', palette='tab10')
sns.set_theme(style='ticks')
sns.despine()

plt.title('Trending Skills for Data Analyst in Thailand')
plt.ylabel('Likelihood of Job Posts')
plt.xlabel('2023')
plt.legend().remove()
plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

#annotate the plot with the top 5 skills
for i in range(5):
    plt.text(11.2, df_plot.iloc[-1, i], df_plot.columns[i], color='black')

plt.show()
```
### Results
![Trending Skills for Data Analysts in Thailand]
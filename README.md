# Table of Content
- [Table of Content](#table-of-content)
- [Introduction](#introduction)
- [Tools Used](#tools-used)
- [ETL](#etl)
- [The Analysis](#the-analysis)
  - [:one: What are the most demanded skills for the top 3 most popular data roles?](#one-what-are-the-most-demanded-skills-for-the-top-3-most-popular-data-roles)
  - [:two: How are in-demand skills trending for Data Analysts?](#two-how-are-in-demand-skills-trending-for-data-analysts)
  - [:three:.:one: How well do jobs and skills pay for Data Analysts?](#threeone-how-well-do-jobs-and-skills-pay-for-data-analysts)
  - [:three:.:two: Highest Paid \& Most Demanded Skills for Data Analysts?](#threetwo-highest-paid--most-demanded-skills-for-data-analysts)
  - [:four: What is the most optimal skill to learn for Data Analysts?](#four-what-is-the-most-optimal-skill-to-learn-for-data-analysts)
- [What I learned](#what-i-learned)
- [Challenges I Faced](#challenges-i-faced)
- [Conclusion](#conclusion)
# Introduction
This project was created under the guidance of Luke Barousse's course on [Youtube](https://www.youtube.com/watch?v=wUSDVGivd-8) to discover insights of data analyst roles in Australia. The dataset used was from Luke's [Hugging Face](https://huggingface.co/datasets/lukebarousse/data_jobs).
Below are the questions I want to answer in my project:
1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the optimal skills for data analysts to learn? (High Demand AND High Paying)  

# Tools Used
🐍 Python: The backbone of this project, allowing me to analyze the data and find critical insights.I also used the following Python libraries:  
   - Pandas Library: This was used to analyze the data.
   - Matplotlib Library: This was used to visualized the data.
   - Seaborn Library: This was used to create more advanced visuals.  

📓 Jupyter Notebooks: The tool I used to run my Python scripts which enables me to include my notes and analysis.  

💻 Visual Studio Code: A lightweight, versatile code editor. I utilized     Visual Studio Code to edit project scripts and manage images, ensuring seamless integration and synchronization with GitHub for version control and collaboration.  

🐙 Git & Github: My go-to for version control and tracking my project progress.

# ETL
Extraced data from [Hugging Face](https://huggingface.co/datasets/lukebarousse/data_jobs), performed initial data cleaning to ensure accuracy and usability.

**Import and Clean Data**

```python
# importing libraries
import ast
import pandas as pd
from datasets import load_dataset
import matplotlib.pyplot as plt
import seaborn as sns

# load data
dataset= load_dataset('lukebarousse/data_jobs')
df=dataset['train'].to_pandas()

# data cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```

**Filter Australia Jobs**
This project focuses on postings from the Australia market. So I applied filters to the dataset and narrowed down the roles to Australia only.
```python
df_US = df[df['job_country'] == 'Australia']
```
# The Analysis

Each Jupyter Notebook in this project focused on exploring distinct aspects of the data job market. Here's the approach I used for each analysis:  

## :one: What are the most demanded skills for the top 3 most popular data roles?

To identify the most in-demand skills for the top three most popular data roles, I filtered the data to isolate these roles and extracted the top five skills associated with each. This analysis highlights the most sought-after job titles and their key skill requirements, providing valuable insights into the skills to prioritize based on my target role.

View my notebook with detailed steps here: [2_Skills_Demand.ipynb](/2_Project/2_Skills_Demand.ipynb)


**Visualize Data**

```python
# plotting 3 rows 1 column
fig, ax = plt.subplots(len(job_titles), 1)
sns.set_theme(style='ticks')
for i, job_title in enumerate(job_titles):
    # find jobs that are on the job_titles list
    df_plot = df_skills_percent[df_skills_percent['job_title_short'] == job_title].head(5)
    # plot chart using seaborn
    sns.barplot(
        data=df_plot,
        x='skill_percent',
        y='job_skills',
        ax=ax[i],
        hue='skill_count',
        palette='dark:b_r'
    )
    ax[i].set_title(job_title)
    ax[i].set_xlabel('')
    ax[i].set_ylabel('')
    ax[i].get_legend().remove()
    ax[i].set_xlim(0,78)
    # add labels to each bar by looping through df_plot
    for index, value in enumerate(df_plot['skill_percent']):
        ax[i].text(value +1 , index, f'{value:.0f}%', va='center') 
        # value +1 so its a bit far away from the bar
        # va='center' to position labels in the center
        # f'{value:.0f}%' converts the numbers to a float with no decimal places
    # remove xaxis and only keeping the bottom one
    if i != len(job_titles) -1:
        ax[i].set_xticks([])      
fig.suptitle('Likelihood of Skills Requested in Australia Job Postings', fontsize=15)
fig.tight_layout(h_pad=0.5) # fix overlap
plt.show()
```
**Results**

![image_skill_demand_all_roles](/2_Project/images/job_skills.png)  
*Bar charts illustrating the top three in-demand positions along with their five most sought-after skills.*

**Insights**
- Across all roles, **SQL** and **Python** are foundational skills, so proficiency in these is essential.
- Visualization tools like **Power BI** and **Tableau** are specifically important for **Data Analyst** positions.
- **Cloud platforms** (AWS, Azure) and **big data tools** (e.g., Spark) are crucial for engineering roles, especially senior ones.
- Transitioning between roles (e.g., from Data Analyst to Data Engineer) may require enhancing cloud computing and big data skills.


## :two: How are in-demand skills trending for Data Analysts?  
To analyze skill trends for Data Analysts in 2023, I filtered job postings for data analyst roles and grouped the listed skills by the month they were posted. This allowed me to identify the top five skills for each month, revealing how their popularity fluctuated throughout the year.  

View my notebook with detailed steps here: [3_Skills_Trend.ipynb](/2_Project/3_Skills_Trend.ipynb)

**Visualize Data**

```python
# Plot
df_plot = df_DA_aus_percent.iloc[:, :5]
# create the lineplot and store the axes object
sns.set_theme(style='ticks')
sns.lineplot(
    data=df_plot,
    dashes=False,
    palette='tab10'
)
sns.despine()
plt.title('Trending Top Skills for Data Analysts in Australia')
plt.ylabel('Likelihood in Job Posting')
plt.xlabel('2023')
# add legend with matching colors
handles, labels = ax.get_legend_handles_labels()  # extract the line handles and labels
plt.legend(
    title="Skills",
    handles=handles,  # ensure legend uses the line colors
    labels=df_plot.columns.tolist(),  # use column names for labels
    loc="upper left",  # position legend at the upper left
    bbox_to_anchor=(1.05, 1),  # move legend outside the plot
    fontsize=10
)
# change y-axis to percentage
from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))
plt.tight_layout()
plt.show()
```  

**Results**

![Skills_trend](/2_Project/images/trending_skills.png)  
*Line chart visualizing the trending top skills for data analysts in Austrlia in 2023.*

**Insights**
- **SQL** is consistently the most in-demand skill for data analysts, suggesting it’s a non-negotiable requirement for job seekers.
- The spikes in **Power BI** and **Python** indicate these tools are highly valued, likely for specific data visualization and analysis projects.
- While not spiking in demand, **Excel remains relevant**, proving its continued use in day-to-day data tasks.
- Lower demand for Tableau may indicate it’s a **nice-to-have** skill or used in more specific contexts compared to Power BI.

## :three:.:one: How well do jobs and skills pay for Data Analysts?  

To determine the highest-paying roles and skills, I focused on job postings in Australia and analyzed their median salaries. I began by examining the salary distributions for common data roles, such as Data Scientist, Data Engineer, and Data Analyst, to identify which positions offer the highest compensation.

View my notebook with detailed steps here: [4_Salary_Analysis.ipynb](/2_Project/4_Salary_Analysis.ipynb)

**Visualize Data**

```python
# plot with seaborn, sort by salary median
sns.boxplot(data=df_aus_top10, x='salary_year_avg', y='job_title_short', order=job_order)
sns.set_theme(style='ticks')
plt.title('Salary Distribution in the Australia')
plt.xlabel('Yearly Salary ($USD)')
plt.ylabel('')
plt.xlim(0,600000)
ticks_x=plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```

**Results**

![salary_distribution](/2_Project/images/salary_distribution.png)  
*Box plot visualizing the salary distributions for the top 10 data job titles.*

**Insights**
- **Senior Data Scientist and Senior Data Engineer** are the most lucrative positions, reflecting the demand for experienced professionals in these fields.
- The wide salary range suggests **opportunities for both entry-level and highly experienced professionals**, depending on specialization and industry.
- **Cloud Engineers** have a competitive median salary, suggesting growing demand in cloud infrastructure roles.
- **Data Analysts and Business Analysts are compensated at lower ranges**, aligning with their position as foundational roles in data and business operations.

## :three:.:two: Highest Paid & Most Demanded Skills for Data Analysts?
I then refined my analysis to focus exclusively on data analyst roles, examining both the highest-paid skills and the most in-demand skills. To present these insights, I utilized two bar charts for clear visualization.  

**Visualize Data**  

```python
fig, ax = plt.subplots(2,1)
#set seaborn theme
sns.set_theme(style='ticks')
# plot df_DA_top_pay
# use seaborn.barplot
sns.barplot(
    data=df_DA_top_pay,
    x='median',
    y=df_DA_top_pay.index,
    ax=ax[0],
    hue ='median',
    palette='dark:b_r') # '_r' to reverse colroing
ax[0].legend().remove() # remove legend
ax[0].set_title('Top 10 Highest Paid Skills for Data Analysts')
ax[0].set_ylabel('')
ax[0].set_xlabel('')
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x,_: f'${int(x/1000)}K'))
# plot df_DA_skills
# use seaborn.barplot
sns.barplot(
    data=df_DA_skills,
    x='median',
    y=df_DA_skills.index,
    ax=ax[1],
    hue ='median',
    palette='light:b'
)
ax[1].legend().remove() # remove legend
ax[1].set_title('Top 10 Most In-Demand Skills for Data Analysts')
ax[1].set_ylabel('')
ax[1].set_xlabel('Median Salary ($USD)')
ax[1].set_xlim(ax[0].get_xlim())
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x,_: f'${int(x/1000)}K'))
plt.tight_layout()
plt.show()
```

**Results**

![top_paid_and_top_demand_skills](/2_Project/images/top_paid_skills.png)  
*Two separate bar graphs visualizing the highest paid skills and most in-demand skills for data analysts in Australia.*  

**Insights**
- **Tableau** commands the highest salaries but is less frequently requested, indicating it may be a specialized skill valued by select employers.
- **Go and Python** are both highly paid and in high demand, making them crucial skills to prioritize for aspiring data analysts.
- Foundational for data analysis roles, **SQL and Snowflake** appear in both charts, showing their universal relevance.
- Tools like **Tableau and Power BI** remain important, with Tableau offering higher salaries and Power BI being more in demand.
- Despite the rise of advanced tools, **Excel** remains both well-paid and highly requested, making it a critical skill for business-oriented analysts.

## :four: What is the most optimal skill to learn for Data Analysts?  
To determine the most optimal skills to learn—those that are both highly paid and in high demand—I calculated the percentage of job postings requiring each skill alongside their median salaries. This analysis highlights the skills that offer the best combination of demand and compensation.  

View my notebook with detailed steps here: [5_Optimal_skills.ipynb](/2_Project/4_Salary_Analysis.ipynb)

**Visualize Data**
```python
from adjustText import adjust_text
df_DA_skills_high_demand.plot(kind='scatter',x='skill_percent',y='median_salary')
# empty list to collect x and y values and label names in the for loop below
texts = []
# create a loop to find skill location (x,y)
for i, txt in enumerate(df_DA_skills_high_demand.index):   texts.append(plt.text(df_DA_skills_high_demand['skill_percent'].iloc[i],df_DA_skills_high_demand['median_salary'].iloc[i],txt))
adjust_text(texts, arrowprops=dict(arrowstyle='->', color='gray', lw=1))
from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))
ax.xaxis.set_major_formatter(PercentFormatter(decimals=0))
# set axis labels, title and legend
plt.xlabel('Percent of Data Analyst Job Postings')
plt.ylabel('Median Yearly Salary ($USD)')
plt.title('Most Optimal Skills for Data Analysts in Australia')
plt.tight_layout()
plt.show()
```
**Results**

![optimal_skills](/2_Project/images/optimal_skills.png)  
*A scatter plot visualizing the most optimal skills (high paying and high demand) for data analysts in Australia.*  

**Insights**
- **SQL** is essential, its dominance in job postings (~100%) makes it a non-negotiable skill for data analysts.
- **Tableau** provides the highest pay but is less in demand, making it a strategic skill for roles requiring advanced visualization.
- **Python and Snowflake** are highly compensated and widely applicable, making them ideal for professionals seeking high-paying, versatile skills.
- Despite being a traditional tool, **Excel** remains relevant with high demand (~80%) demonstrates its ongoing importance in data analysis.
- **Avoid over-investing** in niche skills like C, it may not offer significant returns due to low demand and salary potential.

# What I learned
# Challenges I Faced
# Conclusion

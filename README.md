# The Analysis

## 1. What are the most demanded skills for the top 3 most popular data roles?

View my notebook with detailed steps here: [2_Skills_Count.ipynb](2_Project/2_Skills_Count.ipynb)


### Visualize Data

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

        
fig.suptitle('Likelihood of Skills Requested in US Job POstings', fontsize=15)
fig.tight_layout(h_pad=0.5) # fix overlap
plt.show()
```
### Results
![image_skill_demand_all_roles](/2_Project/images/skill_demand_all_data_roles.png)  

### Insights


# The Analysis

## 2. How are in-demand skills trending for Data Analysts?
### Visualize Data
```python
df_plot = df_DA_US_percent.iloc[:,:5]

sns.lineplot(
    data=df_plot,
    dashes=False,
    palette='tab10'
)
sns.set_theme(style='ticks')
sns.despine()

plt.title('Trending Top Skills for Data Analysts in the US')
plt.ylabel('Likelihood in Job Posting')
plt.xlabel('2023')
plt.legend().remove()

# change yaxis to percentage
from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))

# use for loop to add label to each line
for i in range (5):
    plt.text(11.2,df_plot.iloc[-1,i],df_plot.columns[i])
```

![Skills_trend](/2_Project/images/top_skill_trends.png)  
*Line chart visualizing the trending top skills for data analysts in the US in 2023.*

### Insights
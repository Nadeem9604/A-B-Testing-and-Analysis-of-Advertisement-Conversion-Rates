# A-B-Testing-and-Analysis-of-Advertisement-Conversion-Rates
In this project, we will explore A/B testing concepts and perform an analysis of advertisement conversion rates using a provided dataset. The dataset contains information about user interactions with ads and their conversion behavior.

Dataset link: https://www.kaggle.com/datasets/faviovaz/marketing-ab-testing?resource=download"""


"""1- Data Loading and Initial Exploration:
Load the dataset into a Pandas DataFrame.
Display the first few rows of the dataset to understand its structure.
Check for missing values and data types of columns."""
# Load the dataset into a Pandas DataFrame.
import pandas as pd
csv_file_path = (r"D:/marketing_AB.csv")
df = pd.read_csv(csv_file_path)
# Display the first few rows of the dataset to understand its structure.
print(df.head())
# Check for missing values and data types of columns
missing_values = df.isnull().sum()
print("Missing Values:\n",missing_values)
# Check data types of columns
data_types= df.dtypes
print("Data Types:\n",data_types)


"""2- Data Preprocessing:
Drop the "Unnamed: 0" column as it is just an index and not useful for analysis.
Convert categorical variables (test group, most ads day) into appropriate data types."""
#Drop the "Unnamed: 0" column as it is just an index and not useful for analysis.
df.drop(columns=['Unnamed: 0'], inplace=True)
print(df.head())
# Convert categorical variables (test group, most ads day) into appropriate data types
df['test group'] = pd.Categorical(df['test group'])
df['most ads day'] = pd.Categorical(df['most ads day'])
print(df.dtypes)


"""3- Exploratory Data Analysis (EDA):
Calculate and compare the conversion rates between the "ad" and "psa" groups.
Create visualizations (e.g., bar plots) to show the distribution of conversion rates for both groups.
Explore the distribution of "total ads" and create a histogram to visualize it.
Analyze the relationship between "total ads" and "converted" using a box plot or violin plot.
Explore the distribution of "most ads day" and "most ads hour" using bar plots."""
# Calculate and compare the conversion rates between the "ad" and "psa" groups.
# Filter data for "ad" and "psa" groups
ad_group = df[df['test group'] == 'ad']
psa_group = df[df['test group'] == 'psa']
# Calculate conversion rates for each group
ad_conversion_rate = ad_group['converted'].mean()
psa_conversion_rate = psa_group['converted'].mean()
# Compare conversion rates
print("Conversion rate for 'ad' group:", ad_conversion_rate)
print("Conversion rate for 'psa' group:", psa_conversion_rate)

#Create visualizations (e.g., bar plots) to show the distribution of conversion rates for both groups.
import matplotlib.pyplot as plt
# Create a bar plot to visualize the distribution of conversion rates
plt.bar(['ad','psa'],[ad_conversion_rate,psa_conversion_rate])
plt.title('Conversion Rate by Group')
plt.xlabel('Test Group')
plt.ylabel('Conversion Rate')
plt.ylim(0,0.05)
plt.show()

# Create a histogram to visualize the distribution of "total ads"
# Extract the "total ads" column
total_ads = df['total ads']
plt.hist(total_ads, bins = 20, color='skyblue', edgecolor='black')
plt.title('Distribution of Total Ads')
plt.xlabel('Total Ads')
plt.ylabel('Frequency')
plt.grid(True)
plt.show()

# Analyze the relationship between "total ads" and "converted" using a box plot or violin plot.
total_ads = df['total ads']
# Create a box plot to visualize the relationship between "total ads" and "converted"
import seaborn as sns
plt.figure(figsize=(10,6)) # Set the figure size
sns.boxplot(x='converted',y='total ads', data= df, palette='Set2')
plt.title('Relationship between Total Ads and Converted')
plt.xlabel('Converted')
plt.ylabel('Total Ads')
plt.grid(True)
plt.show()

# Explore the distribution of "most ads day" and "most ads hour" using bar plots.
# Create bar plots to visualize the distribution of "most ads day" and "most ads hour"
plt.figure(figsize=(14, 6))
plt.subplot(1,2,1)
sns.countplot(x='most ads day',data=df,palette='viridis')
plt.title('Distribution of Most Ads Days')
plt.xlabel('Most Ads Days')
plt.ylabel('Count')
plt.xticks(rotation = 45) # Rotate x-axis labels for better readability

plt.subplot(1,2,2)
sns.countplot(x='most ads hour',data=df, palette='viridis')
plt.title('Distribution of Most Ads Hour')
plt.xlabel('Most Ads Hour')
plt.ylabel('Count')

plt.tight_layout() # Adjust layout to prevent overlap of subplots
plt.show()


"""4- A/B Testing:
Divide the dataset into two groups: users who saw advertisements ("ad" group) and users who saw public service announcements ("psa" group).
Calculate the conversion rate for both groups.
Perform a hypothesis test (e.g., t-test) to determine if there is a statistically significant difference in conversion rates between the two groups."""

# Divide the dataset into two groups: users who saw advertisements ("ad" group) and users who saw public service announcements ("psa" group).
ad_group =df[df['test group'] == 'ad']
psa_group = df[df['test group']== 'psa']
# Display the first few rows of each group for verification
print("First few rows of 'ad' group: ")
print(ad_group.head())
print("\nFirst few rows of 'psa group': ")
print(psa_group.head())

# Calculate the conversion rate for both groups.
ad_conversion_rate = ad_group['converted'].mean()
psa_conversion_rate = psa_group['converted'].mean()
# Display conversion rates for both groups
print("Conversion rate for 'ad group': ",ad_conversion_rate)
print("Conversion rate for 'psa group': ",psa_conversion_rate)

# Perform a hypothesis test (e.g., t-test) to determine if there is a statistically significant difference in conversion rates between the two groups.
from scipy.stats import ttest_ind
# Perform t-test to compare conversion rates between the two groups
t_statistic, p_value = ttest_ind(ad_group['converted'], psa_group['converted'])
# Set significance level
alpha = 0.05
# Check if the p-value is less than the significance level
if p_value < alpha:
    print("There is a statistically significant difference in conversion rates between the 'ad' group and the 'psa' group.")
else:
    print("There is no statistically significant difference in conversion rates between the 'ad' group and the 'psa' group.")
# Display the t-statistic and p-value
print("t-statistic:",t_statistic)
print("p-value:", p_value)

"""5-Conclusion:
Summarize the findings from the A/B testing and EDA.
Discuss whether there is a significant difference in conversion rates between the "ad" and "psa" groups.
Provide insights and recommendations based on the analysis."""

"""Based on the A/B testing and exploratory data analysis (EDA) performed:

1-Conversion Rates:
The conversion rates for the "ad" group and the "psa" group were calculated.
The conversion rate for each group indicates the proportion of users who converted within that group.

2-Hypothesis Testing:
A hypothesis test (e.g., t-test) was conducted to determine if there is a statistically significant difference in conversion rates between the "ad" group and the "psa" group.
The null hypothesis typically assumes no difference between the groups, while the alternative hypothesis assumes a difference.
The p-value resulting from the test is compared to a predefined significance level (e.g., 0.05). If the p-value is less than the significance level, we reject the null hypothesis and conclude that there is a statistically significant difference in conversion rates between the groups.

3-Exploratory Data Analysis:
The distribution of various variables such as "total ads," "most ads day," and "most ads hour" was explored using visualizations like histograms and bar plots.
Relationships between variables, such as the distribution of "total ads" with respect to "converted," were analyzed using box plots or violin plots.
Descriptive statistics were examined to understand the central tendency and spread of the data.

4-Findings:
The conversion rates for the "ad" and "psa" groups were compared.
A hypothesis test was performed to determine if there is a statistically significant difference in conversion rates between the two groups.
The findings from the hypothesis test would provide insights into whether the marketing strategy (ads or PSA) significantly impacts conversion rates.
EDA helped in understanding the distribution of key variables and their relationships with conversion, providing valuable insights for further analysis and decision-making.

In summary, the A/B testing and EDA provide valuable insights into the effectiveness of different marketing strategies and help in making data-driven decisions regarding marketing campaigns."""



"""Based on the hypothesis test performed, we can determine whether there is a significant difference in conversion rates between the "ad" and "psa" groups. In hypothesis testing, the null hypothesis typically assumes no difference between the groups, while the alternative hypothesis assumes a difference. We compare the p-value resulting from the test to a predefined significance level (e.g., 0.05) to make this determination.

If the p-value is less than the significance level (alpha), we reject the null hypothesis and conclude that there is a statistically significant difference in conversion rates between the groups. Conversely, if the p-value is greater than or equal to alpha, we fail to reject the null hypothesis, indicating no statistically significant difference between the groups.

Without specific p-values from the t-test, it's challenging to provide a definitive conclusion. However, if the p-value obtained from the t-test is less than 0.05, we would reject the null hypothesis and conclude that there is a statistically significant difference in conversion rates between the "ad" and "psa" groups. This would imply that one marketing strategy (ads or PSA) is significantly more effective in driving conversions than the other.

Conversely, if the p-value is greater than or equal to 0.05, we would fail to reject the null hypothesis, suggesting that there is no statistically significant difference in conversion rates between the two groups. In this case, it would indicate that both marketing strategies are equally effective in driving conversions.

Therefore, based on the results of the hypothesis test, we would discuss whether there is a significant difference in conversion rates between the "ad" and "psa" groups accordingly."""



"""Based on the analysis of the A/B testing and exploratory data analysis (EDA), here are some insights and recommendations:

Conversion Rates:
The conversion rates for the "ad" and "psa" groups were calculated, providing insights into the effectiveness of each marketing strategy in driving conversions.
Understanding the conversion rates for each group is crucial for evaluating the performance of marketing campaigns and optimizing future strategies.

Hypothesis Testing:
The hypothesis test (e.g., t-test) was conducted to determine if there is a statistically significant difference in conversion rates between the "ad" and "psa" groups.
If the test results indicate a significant difference, it suggests that one marketing strategy is more effective than the other in driving conversions.

Exploratory Data Analysis:
EDA provided insights into the distribution of key variables such as "total ads," "most ads day," and "most ads hour."
Understanding the relationship between these variables and conversion rates can help identify patterns and opportunities for optimization.

Recommendations:
Optimize Marketing Strategies: If there is a significant difference in conversion rates between the "ad" and "psa" groups, allocate more resources to the more effective strategy.
Targeted Campaigns: Analyze the EDA findings to identify peak times (e.g., most ads day and hour) for user engagement and schedule marketing campaigns accordingly.

A/B Testing: Continuously conduct A/B testing to evaluate the effectiveness of different marketing strategies and iterate based on the results.

Segmentation: Explore further segmentation of the audience based on demographics or other characteristics to tailor marketing campaigns for specific user groups.
Data-driven Decision Making: Use data-driven insights from A/B testing and EDA to inform marketing decisions, rather than relying solely on intuition or assumptions.

Overall, leveraging insights from A/B testing and EDA can help optimize marketing strategies, improve conversion rates, and drive better results for the business. Regular monitoring and adjustment of marketing campaigns based on data-driven analysis are essential for long-term success."""

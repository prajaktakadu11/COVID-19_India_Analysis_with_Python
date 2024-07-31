# COVID-19_India_Analysis_with_Python

# Table of content
- Introduction
- Objective
- Tools and Technologies
- Data Import and Cleaning
- Data Transformation
- Analysis and Visualizations
- Project Insights

# Introduction :
The COVID-19 pandemic has affected every corner of the world, and India has been one of the countries significantly impacted. Analyzing COVID-19 data can provide crucial insights into the spread of the virus, the effectiveness of measures taken, and help in planning future responses. This project aims to analyze and visualize COVID-19 data in India using Python. The project involves data cleaning, transformation, and analysis to derive meaningful insights and trends.

# Objectives  :
- To analyze the spread of COVID-19 across different states in India.
- To visualize the trends in confirmed cases, recoveries, and deaths.
- To identify the states with the highest and lowest vaccination rates.
- To compare vaccination rates between males and females.
- To provide a comprehensive dashboard to track COVID-19 metrics.

# Tools and Technologies :
- Python: For data analysis and visualization.
- Pandas: For data manipulation and cleaning.
- Matplotlib and Seaborn: For creating visualizations.
- Plotly: For interactive visualizations.
- Jupyter Notebook: For coding and documentation.

# Data Import and Cleaning :
The initial step involves importing the data into Python using Pandas. The data is then cleaned to handle missing values, incorrect data formats, and any inconsistencies. Date columns are converted to datetime format, and relevant columns are extracted for analysis.
   ```
   # Importing dataset

   covid_df = pd.read_csv('covid_data.csv')
   vaccine_df = pd.read_csv('vaccine_data.csv')
   ```

# Data Transformation :
- To facilitate analysis, additional columns are created. For instance, the 'Active_cases' column is calculated as the difference between confirmed cases and the sum of recoveries and deaths.
   ```
   covid_df['Active_cases'] = covid_df['Confirmed'] - (covid_df['Cured'] + covid_df['Deaths'])
   ```
- For instance, the pivot table is created to summarize the data by State/UnionTerritory using the maximum values of Cured, Deaths, and Confirmed cases. Then calculates the recovery and mortality rates.
   ```
   # Create pivot table
   Statewise = pd.pivot_table(covid_df, values=['Cured', 'Deaths', 'Confirmed'], index='State/UnionTerritory', aggfunc=max)
   ```
   ```
   # Calculate recovery and mortality rates
   Statewise['Recovery_rate'] = (Statewise['Cured'] * 100) / Statewise['Confirmed']
   Statewise['Mortality_rate'] = (Statewise['Deaths'] * 100) / Statewise['Confirmed']
   ```

# Analysis and Visualizations :
1. Top 10 States with Most Active Cases
   This analysis identifies the states with the highest number of active COVID-19 cases in India.
   ```
   top_10_active_cases = covid_df.groupby('State/UnionTerritory').max()[['Active_cases', 'Date']].sort_values(by='Active_cases', ascending=False).reset_index()
   plt.figure(figsize=(16,9))
   sns.barplot(data=top_10_active_cases.iloc[:10], x='State/UnionTerritory', y='Active_cases', palette='viridis')
   plt.title('Top 10 States with Most Active Cases in India')
   plt.xlabel('State/UnionTerritory')
   plt.ylabel('Active Cases')
   plt.xticks(rotation=45)
   plt.show()
   ```
   
2. Top 10 States with Most Death Cases
   This analysis highlights the states with the highest number of deaths due to COVID-19.
   ```
   top_10_deaths = covid_df.groupby('State/UnionTerritory').max()[['Deaths', 'Date']].sort_values(by='Deaths', ascending=False).reset_index()

   plt.figure(figsize=(16,9))
   sns.barplot(data=top_10_deaths.iloc[:10], x='State/UnionTerritory', y='Deaths', palette='Reds')
   plt.title('Top 10 States with Most Death Cases in India')
   plt.xlabel('State/UnionTerritory')
   plt.ylabel('Death Cases')
   plt.xticks(rotation=45)
   plt.show()
   ```

3. Top 5 Affected States Over Time
   A line plot showing the trends of active cases in the top 5 affected states over time.
   ```
   top_5_states = ['Maharashtra', 'Karnataka', 'Kerala', 'Tamil Nadu', 'Uttar Pradesh']
   filtered_df = covid_df[covid_df['State/UnionTerritory'].isin(top_5_states)]

   plt.figure(figsize=(12,9))
   sns.lineplot(data=filtered_df, x='Date', y='Active_cases', hue='State/UnionTerritory')
   plt.title('Top 5 Affected States in India Over Time')
   plt.xlabel('Date')
   plt.ylabel('Active Cases')
   plt.xticks(rotation=45)
   plt.legend(title='State/UnionTerritory', loc='upper left')
   plt.show()
   ```

4. Vaccination Analysis
   Male vs Female Vaccination: A pie chart comparing the vaccination numbers between males and females.
   ```
   male_vaccinated = vaccine_df['Male(Individuals Vaccinated)'].sum()
   female_vaccinated = vaccine_df['Female(Individuals Vaccinated)'].sum()

   fig = px.pie(names=['Male', 'Female'], values=[male_vaccinated, female_vaccinated], title='Male vs Female Vaccination')
   fig.show()
   ```

5. Top 10 Vaccinated States
   A bar chart showing the top 10 states with the highest number of vaccinations.
   ```
   max_vac = vaccine_df.groupby('State')['Total'].sum().to_frame('Total').sort_values(by='Total', ascending=False).reset_index()

   plt.figure(figsize=(16,9))
   sns.barplot(data=max_vac.iloc[:10], x='State', y='Total', palette='Blues')
   plt.title('Top 10 Vaccinated States in India')
   plt.xlabel('State')
   plt.ylabel('Total Vaccinations')
   plt.xticks(rotation=45)
   plt.show()
   ```

6. Top 5 Least Vaccinated States
   A bar chart showing the top 5 states with the lowest number of vaccinations.
   ```
   min_vac = vaccine_df.groupby('State')['Total'].sum().to_frame('Total').sort_values(by='Total', ascending=True).reset_index()

   plt.figure(figsize=(15,9))
   sns.barplot(data=min_vac.iloc[:5], x='State', y='Total', palette='Oranges')
   plt.title('Top 5 Least Vaccinated States in India')
   plt.xlabel('State')
   plt.ylabel('Total Vaccinations')
   plt.xticks(rotation=45)
   plt.show()
   ```

# Project Insights :
The analysis provides several key insights:

- Maharashtra has consistently had the highest number of active cases and deaths.
- Vaccination rates vary significantly across states, with some states achieving higher vaccination coverage than others.
- There is a gender disparity in vaccination rates, with more males being vaccinated compared to females.

These insights can help policymakers and health officials to allocate resources more effectively, implement targeted measures in states with high case counts, and address vaccination disparities.


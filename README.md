# Playing It Smart: Insights from Smart Device Usage for Bellabeat 
## Google Data Analytics Professional Certificate Capstone Project
---

## Overview
This project analyzes Fitbit smart device data to gain insights into user activity, sedentary behavior, and sleep patterns. The goal is to provide actionable insights for [Bellabeat](https://bellabeat.com/), a wellness technology company, to inform marketing, engagement strategies, and product development.

---

## 1. Ask

**Business Task:**  
Analyze smart device usage to understand how consumers interact with them and provide insights to Bellabeat's product strategy and marketing efforts.

**Stakeholders:**  
- **Urška Sršen** – Bellabeat’s cofounder and Chief Creative Officer  
- **Sando Mur** – Mathematician and Bellabeat’s cofounder; key member of the executive team  
- **Bellabeat Marketing Analytics Team**

**Guiding Questions:**  
1. What are some trends in smart device usage?  
2. How could these trends apply to Bellabeat customers?  
3. How could these trends help influence Bellabeat marketing strategy?

---

## 2. Prepare

**Data Source:**  
- Dataset: Publicly available [FitBit Fitness Tracker Data (Kaggle)](https://www.kaggle.com/datasets/arashnic/fitbit).  
- Contains 18 CSV files of daily activity, sleep, and weight metrics for 30 participants.  
- Data covers a 30-day period between April and May 2016.

These files were chosen because they provide a direct link between daily activity and sleep metrics, essential for understanding wellness behaviors.

**Limitations:**  
- 30-day snapshot in 2016; may not reflect current trends  
- Small sample size (30 participants)  
- Data is from non-Bellabeat devices, which may have tracking differences  
- No demographic data provided

---

## 3. Process

**Data Used:**  
For this analysis, two primary datasets were selected (from April to May 2016):  
- *dailyActivity_merged.csv* – daily activity, calories, steps  
- *sleepDay_merged.csv* – sleep duration and quality
These were chosen because they provide the most relevant overlap between daily activity and sleep metrics, allowing for meaningful behavioral insights.  

**Applications:**  
- **Excel** – initial exploration of the datasets for missing values, duplicates, and formatting issues  
- **R** – cleaning, transformation, exploratory analysis, and visualizations  
- **Tableau** – building an interactive dashboard for stakeholder presentation

**Initial Pass Through (Excel)**
- Loaded `dailyActivity_merged.csv` and `sleepDay_merged.csv`  
- Checked date formats and aligned columns  
- Verified duplicates and missing data  
- Noted inconsistencies that would require cleaning in R

**Cleaning and Exploration (R)**
For full R code, click [here](https://github.com/shsilv/Bellabeat_Capstone_Project/blob/main/R%20code).

- Load the tidyverse package and data files
- Ensure data has been loaded correctly
- Standardized column names using `janitor::clean_names()`
- Removed duplicates and invalid entries:
```r
daily_activity <- daily_activity %>%
  filter(calories > 0, sedentary_minutes < 1440)

sleep_day <- sleep_day %>%
  distinct() %>%
  filter(total_minutes_asleep > 0,
    total_time_in_bed > 0,
    total_minutes_asleep <= total_time_in_bed)
```
- Converted date columns to proper formats:
daily_activity <- daily_activity %>%
  mutate(activity_date = lubridate::mdy(activity_date))

sleep_day <- sleep_day %>%
  mutate(sleep_day = lubridate::mdy_hms(sleep_day),
         sleep_day = as.Date(sleep_day))
```
-Merged datasets on ID and date:
```r
daily_summary <- daily_activity %>%
  inner_join(sleep_day, by = c("id" = "id", "activity_date" = "sleep_day"))
```

---

## 4. Analyze

**Key R Summaries**
```r
summary(daily_summary)
glimpse(daily_summary)
```
- Checked ranges and distributions of steps, sedentary minutes, sleep duration and calories

**Visualizations and Insights**
#### 1. Total Steps by Day
<img width="1344" height="960" alt="download" src="https://github.com/user-attachments/assets/a2e6245b-edcf-42b7-a1da-d43220c519ac" />
Step counts fluctuate daily with some midweek peaks. Tracking total steps over time highlights overall activity trends.

#### 2. Average Sedentary Minutes by Day of Week
<img width="1344" height="960" alt="download" src="https://github.com/user-attachments/assets/783670cb-21da-4a45-b00d-f516cfe08aa7" />
Users are less sedentary on weekends compared to weekdays, suggesting workweek routines contribute to inactivity.

#### 3. Distribution of Average Sleep Duration by Day
<img width="1344" height="960" alt="download" src="https://github.com/user-attachments/assets/812c3da8-fbb2-45fe-815c-f8d39bb0560a" />
Sleep duration is fairly consistent with most days averaging between 6.5- 8 hours of sleep, indicating general healthy sleep habits.

#### 4. Relationship Between Daily Steps and Sleep Duration
<img width="1344" height="960" alt="download" src="https://github.com/user-attachments/assets/0dc533ab-3239-40f2-9d0c-3999d5d537b4" />
Higher step counts generally align with longer sleep duration, supporting a connection between physical activity and sleep quality.

---

## 5. Share
View the dashboard on Tableau Public [here](https://public.tableau.com/app/profile/sarah.hannah.silverstein/viz/Bellabeat_Capstone_Project_17585732966110/InsightsDashboard?publish=yes).

---

## 6. Act
**Insights and Recommendations**
1. Encourage Activity During Workdays: Sedentary minutes peak on weekdays, so Bellabeat could send reminders to move or offer micro-workout prompts during typical high-sedentary hours.  
2. Highlight and Gamify Logging Consistency: Many users log activity inconsistently—some achieve step goals only on the few days they log. Bellabeat could gamify step tracking by rewarding streaks, consistent logging, or “completion badges” to motivate regular engagement.  
3. Reward Step Goal Achievement: Since most users don’t hit 10,000 steps daily, gamified step challenges could incentivize reaching daily targets, boosting overall activity.  
4. Promote Sleep Health: Users with higher activity tend to sleep longer. Bellabeat can emphasize the link between movement and better sleep in app notifications or marketing campaigns.  
5. Marketing Messaging: Tailor communications around peak sedentary periods (weekdays) and highlight weekends for optional group challenges or community activity promotions.

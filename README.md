# Employment Attrition Analysis of Salifort-Motors 

## Executive Summary

![Executive Summary Document](https://github.com/Mitch-P-Analyst/Salifort-Motors/blob/main/Executive%20Summary.pdf)

I analyzed a 15k-row HR dataset to understand attrition at Salifort Motors. 

Employees with **more projects, higher monthly hours, longer tenure, and lower satisfaction were much more likely to leave**.

Recommendations: 
- Limit project assignments at ≤5
- Limit average monthly hours ≤230
- Rebalance workload for 3–4 year employees
- Monitor satisfaction thresholds for predicting employees at risk of attrition.

Best Model:
- XGBoost classifier (cross-validated) 
    - Achieved strong precision, recall (
    - Ordered top features by gain (log-loss reduction)
        - Satisfaction
        - Number of Projects
        - Tenure
        - Previous Satisfaction
        - Average Monthly Hours.

Impact:
- Reduces risk among the highest-attrition groups.

Nest Steps:
- Source information on **Type of Employee Departure** to further analysis attrition. 
    - Fired
    - Redundant
    - Resignation

## Overview

Google Advanced Data Analytics Capstone project. Built around the fictional french Salifort-Motors organisation, this repository utilises skills taught through Google's Advanced Data Analytics course such as project proposals, EDA, machine learning model building, statistical testing, PACE planning and stakeholder summaries to analyse and predict employee attrition.

## Provided Context
   
### About the company
Salifort Motors is a fictional French-based alternative energy vehicle manufacturer. Its global workforce of over 100,000 employees research, design, construct, validate, and distribute electric, solar, algae, and hydrogen-based vehicles. Salifort’s end-to-end vertical integration model has made it a global leader at the intersection of alternative energy and automobiles.        

#### Dataset
A ficitional datasets sourced from Kaggle, hosting 15,000 rows and 10 columns with the structure listed below:

[Kaggle Hr Analytics Job Prediction](https://www.kaggle.com/datasets/mfaisalqureshi/hr-analytics-and-job-prediction?select=HR_comma_sep.csv).


Variable  |Description |
-----|-----|
satisfaction_level|Employee-reported job satisfaction level [0&ndash;1]|
last_evaluation|Score of employee's last performance review [0&ndash;1]|
number_project|Number of projects employee contributes to|
average_monthly_hours|Average number of hours employee worked per month|
time_spend_company|How long the employee has been with the company (years)
Work_accident|Whether or not the employee experienced an accident while at work
left|Whether or not the employee left the company
promotion_last_5years|Whether or not the employee was promoted in the last 5 years

### Scenario

Currently, there is a high rate of turnover among Salifort employees. **(Note: In this context, turnover data includes both employees who choose to quit their job and employees who are let go)**. Salifort’s senior leadership team is concerned about how many employees are leaving the company. Salifort strives to create a corporate culture that supports employee success and professional development. Further, the high turnover rate is costly in the financial sense. Salifort makes a big investment in recruiting, training, and upskilling its employees. 

If Salifort could **predict whether an employee will leave the company**, and discover the reasons behind their departure, they could better understand the problem and develop a solution. Therefore, **discover what’s likely to make the employee leave the company.**

As a first step, the leadership team asks Human Resources to survey a sample of employees to learn more about what might be driving turnover.  

Next, the leadership team asks you to analyze the survey data and come up with ideas for how to increase employee retention. To help with this, they suggest you design a model that **predicts whether an employee will leave the company based on their job title, department, number of projects, average monthly hours, and any other relevant data points.** A good model will help the company **increase retention and job satisfaction for current employees**, and save money and time training new employees. 

As a specialist in data analysis, the leadership team leaves it up to you to choose an approach for building the most effective model to predict employee departure. For example, you could build and evaluate a statistical model such as logistic regression. Or, you could build and evaluate machine learning models such as decision tree, random forest, and XGBoost. Or, you could choose to deploy both statistical and machine learning models. 

For any approach, you’ll need to analyze the key factors driving employee turnover, build an effective model, and share recommendations for next steps with the leadership team. 

### Your business case
As a data specialist working for Salifort Motors, you have received the results of a recent employee survey. The senior leadership team has tasked you with analyzing the data to come up with ideas for how to increase employee retention. To help with this, they would like you to design a model that predicts whether an employee will leave the company based on their  department, number of projects, average monthly hours, and any other data points you deem helpful. 

### Deliverables
For this deliverable, you are asked to choose a method to approach this data challenge based on your prior course work. 

- Select either a regression model or a tree-based machine learning model to predict whether an employee will leave the company. 

- Identify the top features that impact employees to leave the company

### Stakeholders
- Salifort Leaderteam & Executive team
    - High overview of project and summaries

- Human Resources Department
    - Coworkers & Mangement
        - Mid-level overview if required


## Project Process

### Pace: Planning
The provided Kaggle dataset contain 15,000 rows across 10 variable columns. Initial analysis identifed necessary data structure, types and values to further investiage trends.

- Exploratory Data Analysis (EDA)
    - Cleaned provided dataset, identifying structural and information limitations
        - Duplicates / Null Values / Datatypes / Outliers

### pAce: Analyze
Visual analysis of variable trends was conducted with Matplotlib and Seaborn over a series of 5 figures. Observations were conducted between independent variables and target variable `left`, cross-variable relationships, `satisfaction_level` distributions and further associations.

- Continued EDA
    - Identify target variable class imbalance
- Visualisations to observed assocations and trends among variables
    - Figures
        - [Figure_1_Employee_Satisfaction_by_Factors](Outputs/Figure_1_Employee_Satisfaction_by_Factors.png)
        - [Figure_2_Cross_Variable_Relationships](Outputs/Figure_2_Cross_Variable_Relationships.png)
        - [Figure_3_Employees_Who_Left_Salifort_Motors](Outputs/Figure_3_Employees_Who_Left_Salifort_Motors.png)
        - [Figure_4_Distribution_of_Satisfaction_Levels_of_Departed_Employees](Outputs/Figure_4_Distribution_of_Satisfaction_Levels_of_Departed_Employees.png)
            - ![Figure_4_Distribution_of_Satisfaction_Levels_of_Departed_Employees](https://github.com/Mitch-P-Analyst/Salifort-Motors/blob/main/Outputs/Figure_4_Distribution_of_Satisfaction_Levels_of_Departed_Employees.png?raw=true)
        - [Figure_5_Satisfaction_Bands_of_Departed_Employees_vs_Number_of_Assigned_Project](Outputs/Figure_5_Satisfaction_Bands_of_Departed_Employees_vs_Number_of_Assigned_Project.png)
    - Key Observations
        - Three identifible clusters of employees who **departed** Salifort Motors
            - Low Satisfaction Cluster (x <= 0.12)
            - Mid Satisfaction Cluster (0.31 <= x <= 0.48)
            - High Satisfaction Cluster (0.70 <= x)
        - Majority of all **Low Satisfaction Level Employees who departed** were on **6 or 7 projects**.
        - Employee satisfaction level has visible associations with the **number of projects**; strong association with assignment to **6 and 7 different projects** and moderate association with assignment to **2 different projects.** 
        - Additional factor **tenure** has moderate associations with employee satisfaction level for duration of **3 and 4 years**.
    - Limitations
        - Unable to differentiate between employees who left voluntarily and those who were fired or made redunant.
        
Inference from these visualisations in the Analyze stage show departed employees with **low levels of satisfaction** were assigned to **6 or 7 projects**. The majority of whom are employees with **4 years of tenure, working higher average monthly hours**. These observed associations became a focal point for further analysis as we discover what factors are leading to employee attrition.

### paCe: Construct
To identify variable assocaitions with the target variable `left`, two classification prediction models were produced to identify potential relationships and assess utility for future Human Resources assessments on at-risk employees.

- Clear baseline model
    - **Logistic Regression**
        - Poor prediction results
        - Identified strong odds-ratios for independent variables
            - **Satisfaction level increase of 0.1 is associated with ≈33% lower odds of leaving**
            - **Each additional year of tenure is associated with ≈30% higher odds of leaving**
            - **A promotion in the last five years reduces the odds of leaving by roughly ≈76%.**
    
- Descriptive precise metrics
    - **XGBoost**
        - Cross Valdiation perofrmed to assess best hyperparameter metrics
            - Very strong prediction results
        - Identified strong Feature Importance assessed by **Gain**. (Gain is the average reduction in log loss from splits using a feature (averaged over all trees). Higher gain ⇒ feature splits reduce log loss more.)
            - Satisfaction Level
                - **44.0%** 
            - Number of Projects
                - **23.3%**
            - Tenure
                - **13.8%**
            - Previous Satisfaction Level
                - **9.6%**
            - Average Monthly Hours
                - **6.9%**
            - All Department Types
                - **≈2.5%**

**Interpretation**: *These percentages are not effect sizes; they show how often and how usefully a feature was used across the boosted trees to reduce log-loss.*

- Prediction Evaluation Metrics
![model_predicition_metrics](https://github.com/Mitch-P-Analyst/Salifort-Motors/blob/main/Outputs/model_prediction_metrics.png?raw=true)

As seen in model metrics below, the XGBoost Classification model produced extremely high evaluation metrics. Higher than the Logistic Regression model across metrics `Precision, Recall, F1, Accuracy`.

Given this consistent improvement across all metrics, the **XGBoost** is the preferred model for predicting future employee attrition at Salifort Motors.

### pacE: Execute

#### Conclusion

To conclude, after combining perspectives from these models, we identified a consistent pattern.

Employees working on **higher number of projects, a workload with higher monthly hours, longer tenure, lower satisfaction, are much more likely to leave**.

Departed employees with the lowest satifaction levels were assigned to **6 or 7 different projects**. Examining all employees, past and present, from data provided, employees assigned to 6 or 7 projects are associated with the **highest average monthly hours** and are **tenured employees around ≈4 years**. 

Employees assigned to **7 projects** have an **attrition rate of 100%**

![Project_statistics](https://github.com/Mitch-P-Analyst/Salifort-Motors/blob/main/Outputs/Project_Statistics.png?raw=true)

![Conclusion_Charts](https://github.com/Mitch-P-Analyst/Salifort-Motors/blob/main/Outputs/Figure_6_Conclusion_Charts.png?raw=true)

These results point to clear opportunties for improving attrition rates by **managing workload of tenured employees of ~3 - 4 years to address satisfaction levels**.

#### Recommendations

Reflecting upon the above visualisations and numbers presented in **Project Statistics**, this investigation recommends addressing the key variables that are supported by analysis to associate with **employee departure.**

- Rebalance Project Assignment
    - Employees with the highest levels of mean Satisfaction Levels are assigned to 4 or 5 projects. **Restrict employees to a maximum of 5 projects.**
- Restrict Employee Hours
    - Restrict **Average Monthly Hours** for employees to the ranges presented by most satisfied employees, **a maximum of 230 hours**.

#### Next Steps

Further steps to improve this investigation may include

- Sourcing information on **Type of Employee Departure**. 
    - Fired
    - Redundant
    - Resignation
    
The data shows significant quantities of employees departed with medium and high satisfaction levels, primarily from collection of employees assigned to 2 project and in the departments of sales and human resources. 

Distinguishing between how employees left Salifort may assist in analyzing where employee resources may be reassigned to compensate over-worked employees working on 6 or 7 projects.

- Assess Employee Previous Satisfaction Levels
The XGBoost model identifies **Previous Satisfaction Levels** as a key indicator towards predicting an employees eventual departure. Analysis of threshold upon satisficlations could provide potential risk management of employee attrition to monitor work environment and maintain sufficient satisfaction levels.

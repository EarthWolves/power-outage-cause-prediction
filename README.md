# Predicting Power Outage Causes Across U.S. States

## Introduction 

The dataset we chose for our project is 'Power Outages' Dataset, which contains data on major power outages across different states of the United States during January 2000 and July 2016. The dataset contains about 1534 datapoints and 57 features, of which we have selected the following columns to perform our project on.


|Column                |Description|
|---                |---        |
|`'MONTH'`                |Month an outage occurred|
|`'U.S._STATE'`                |State the outage occurred in|
|`'POSTAL.CODE'`| Represents the postal code of the U.S. states|
|`'ANOMALY.LEVEL'`                |Oceanic El Niño/La Niña (ONI) index referring to the cold and warm episodes by season|
|`'CAUSE.CATEGORY'`                |Categories of all the events causing the major power outages|
|`'OUTAGE.DURATION'`                |Duration of outage events (in minutes)|
|`'CUSTOMERS.AFFECTED'`                |Number of customers affected by the power outage event|
|`'TOTAL.PRICE'` | Monthly electricity price combined across all sectors (cents/kilowatt-hour) |
|`'CLIMATE.CATEGORY'`|This represents the climate episodes corresponding to the years|


## Data Cleaning and EDA
The first and foremost step to ensure no undesirable outcomes in the data, and making the data more interpretable.

### Data Cleaning 
Most data collected by public institution is already preprocessed, but upon analysis, we came across a few unwanted characteristics that we took care of in the following steps:

1) **Selecting only relevant features** We dropped features that we didn't want and only kept the features as listed: `'MONTH'` , `'POSTAL.CODE'`, `'U.S._STATE'` , `'ANOMALY.LEVEL'`, `'CAUSE.CATEGORY'` , `'OUTAGE.DURATION'` , `'CUSTOMERS.AFFECTED'`, `'RES.PRICE'`.

2) **Renaming columns**: We renamed the columns to be more readable and interpretable and then combined the row with the units and combined it with the corresponding columns

The first couple of rows of the dataframe after cleaning are as follows:


| | Climate Region | State | State Code | Cause Category | ... | Anomaly Level (numeric) | Residential Price (cents / kilowatt-hour) | Customers Affected | Month |
|---|---|---|---|---|---|---|---|---|---|
| 1 | East North Central | Minnesota | MN | severe weather | ... | -0.3 | 11.6 | 70000.0 | 7.0 |
| 2 | East North Central | Minnesota | MN | intentional attack | ... | -0.1 | 12.12 | NaN | 5.0 |
| 3 | East North Central | Minnesota | MN | severe weather | ... | -1.5 | 10.87 | 70000.0 | 10.0 |
| 4 | East North Central | Minnesota | MN | severe weather | ... | -0.1 | 11.79 | 68200.0 | 6.0 |
| 5 | East North Central | Minnesota | MN | severe weather | ... | 1.2 | 13.07 | 250000.0 | 7.0 |

## Exploratory Data Analysis
### Univariate Analysis

For a holistic univariate analysis, we performed quite a few visualizations and have listed the the most useful below

The first plot below displays the **trends in cause of power outage from the years 2000 to 2016**
<iframe
  src="plots/Univariate_Outages_Years.html"
  width="1200"
  height="700"
  frameborder="0"
></iframe>

The second plot below is a **chlorpleth map that has varying shades of color - darker the state, the more power outages it has witnessed**. (Note: For a more relative and accurate representation, the values of frequencies were converted to log)

<iframe
  src="plots/us_power_outages_map.html"
  width="800"
  height="500"
  frameborder="0"
></iframe>

The third plot below performs similarly to the previous chlorpleth map, but this one is a chloropleth map for **total power outage duration per state, with the darker the state, the more total duration of power outages it has faced**. (Note: For a more relative and accurate representation, the values of frequencies were converted to log)
<iframe
  src="plots/combined_power_outages_state.html"
  width="800"
  height="500"
  frameborder="0"
></iframe>

### Bivariate Analysis

For a holistic bivariate analysis, we performed quite a few visualizations and the most significant results are as below:

The plot below is a sunburst plot for the top 12 states by frequency of power outages. The proportion of each state is segmented by the different power outage causes to give a visual idea of the most common root cause for power outages in each state and how many major causes are prevalent (Single dominating cause in case of Michigan, and Multiple dominating causes in case of California) 

<iframe
  src="plots/Sunburst_State_Cause.html"
  width="1000"
  height="700"
  frameborder="0"
></iframe>


The following plot is a box-and-whisker plot for the distribution of power outage durations for each of the 12 states with the most number of power outages. This gives us an insight on how tight or spread the distribution of outages is and an insight into what potential outliers might be present for each state 

<iframe
  src="plots/Bivariate_Duration_State.html"
  width="800"
  height="450"
  frameborder="0"
></iframe>

### Interesting Aggregates

The following is a pivot table generated that shows the average outage duration (in minutes) for different combinations of climate categories and cause categories, revealing how different factors impact power restoration times. The data reveals that fuel supply emergencies cause the longest outages, with warm regions experiencing nearly 16-day interruptions (22,799 minutes). Severe weather consistently produces extended outages across all climate types, though warm regions suffer the most (4,416 minutes). Climate significantly influences outage patterns - normal regions face longer equipment failures, while cold regions experience prolonged public appeals. Intentional attacks show an interesting pattern, with decreasing duration as temperatures rise.

| CLIMATE.CATEGORY | equipment failure | fuel supply emergency | intentional attack | islanding | public appeal | severe weather | system operability disruption |
|------------------|-------------------|------------------------|-------------------|-----------|---------------|----------------|------------------------------|
| cold             | 308.24            | 17433.0                | 497.28            | 259.27    | 2125.91       | 3279.95        | 601.86                       |
| normal           | 3201.43           | 7658.82                | 426.82            | 142.18    | 1376.53       | 4059.33        | 941.02                       |
| warm             | 505.0             | 22799.67               | 312.56            | 209.83    | 596.23        | 4416.69        | 478.2                        |

## Assessment of Missingness

### NMAR Analysis

After analyzing different columns, we thought `'CUSTOMERS.AFFECTED'`'s missingness to be NMAR. Since the power outage durations can be as extreme as 108k minutes, some companies might tend to not record or report such long durations of outages to avoid negative publicity. It might also be the case that data where only a few customers would have been affected might not be reported due to the numbers being small and insignificant. About a third of the rows of the top 50 highest reported outage durations had their corresponding `'CUSTOMERS.AFFECTED'` value empty, and about half of them either were not reported or had suspiciously low values of under 10 customers affected. This led us to believe that `'CUSTOMERS.AFFECTED'` could be NMAR on `'OUTAGE.DURATION'`.

#### What data could we collect to explain the missingness? 

The main features that could help us identify the true cause of missingness would be gettint to know the Utility company policies and thresholds with regards to collecting and reporting the data. Other data features like data reporting history and analysis of trends, potential state-to-state varying regulatory requirements, company structure, media coverage, etc. 

### Missingness Dependency

<iframe
  src="plots/climate_customers_affected_hist.html"
  width="800"
  height="450"
  frameborder="0"
></iframe>

<iframe
  src="plots/climate_customers_affected_missing_assessment2.html"
  width="1000"
  height="500"
  frameborder="0"
></iframe>

<iframe
  src="plots/climate_total_price_hist.html"
  width="800"
  height="450"
  frameborder="0"
></iframe>

<iframe
  src="plots/climate_total_price_missing_assessment.html"
  width="1000"
  height="500"
  frameborder="0"
></iframe>


   
## Hypothesis Testing

### Introducing our Hypothesis

In this analysis, we set out to investigate whether certain factors, such as states, cause categories, and months, influence the duration of power outages in the United States. Specifically, we aimed to test whether the distribution of outage durations is proportionally related to the frequency of outages in different states, cause categories, and months. We hypothesized that certain factors could cause systematic differences in outage durations, suggesting that some regions, causes, or times of the year might experience longer outages than others due to inherent infrastructural or environmental challenges.

When conducting this hypothesis testing, we are making a key assumption: If the system is fair and there is no underlying cause driving differences in outage duration, then states with more outages should, on average, experience longer outage durations. This assumption is based on the idea that, generally speaking, states with more frequent outages might face more systemic challenges (e.g. infrastructure limitations), which could result in longer durations.

Thus, under the null hypothesis, we assume that the distribution of outage durations should be proportional to the distribution of outage frequencies across states.


### Hypothesis Testing (Across States)

**Null Hypothesis** : The distribution of power outage durations across states is proportional to the distribution of the number of outages per state.

Any observed difference (TVD) is due to random variation rather than a true underlying effect.

**Alternative Hypothesis** : The distribution of power outage durations is not proportional to the number of outages per state.

**Test Statistic** : TVD 

#### Test Visualization

<iframe
  src="plots/HT_1_State.html"
  width="800"
  height="380"
  frameborder="0"
></iframe>

Our observed TVD lied in the extreme right tail of Null distribution (obtained from permutation tests) which provides strong evidence that outage durations are not randomly assigned across states.

Hence we **reject the Null Hypothesis**. There are likely systematic factors (e.g., infrastructure & weather) that cause certain states to have disproportionately longer outages than their outage frequency would suggest.


### Hypothesis Testing (Across Cause Categories)

**Null Hypothesis**: The distribution of power outage durations across different cause categories is proportional to the distribution of the number of outages in each cause category. Any observed difference (TVD) is due to random variation rather than a true underlying effect.

**Alternative Hypothesis**: The distribution of power outage durations is not proportional to the number of outages in each cause category.

**Test Statistic**: TVD

#### Test Visualization

<iframe
  src="plots/HT_2_Cause_Cat.html"
  width="800"
  height="380"
  frameborder="0"
></iframe>

Our observed TVD lied in the extreme right tail of the null distribution (obtained from permutation tests), which provides strong evidence that outage durations are not randomly assigned across cause categories.

Hence, we **reject the Null hypothesis**. There are likely systematic factors (e.g., infrastructure, severity of weather events, and other causes) that lead to disproportionately longer outages in certain cause categories compared to their outage frequency.

### Hypothesis Testing (Across Month)

**Null Hypothesis**: The distribution of power outage durations across different months is proportional to the distribution of the number of outages in each month. Any observed difference (TVD) is due to random variation rather than a true underlying effect.

**Alternative Hypothesis**: The distribution of power outage durations is not proportional to the number of outages in each month.

**Test Statistic**: TVD

<iframe
  src="plots/HT_3_Clim_Cat.html"
  width="800"
  height="380"
  frameborder="0"
></iframe>

Our observed TVD did not lie in the extreme right tail of the null distribution (obtained from permutation tests), and the p-value was not sufficiently small. This suggests that the observed difference in outage durations across months is likely due to random variation.

We fail to reject the null hypothesis. The analysis indicates that the outage duration and frequency seem to have a relationship across months, and there is no strong evidence to suggest that certain months cause disproportionately longer outages than would be expected based on the number of outages.

### Conclusion

**Across States**: We found strong evidence against the null hypothesis, indicating that certain states have disproportionately longer outage durations than would be expected based on their outage frequency. This suggests that inherent differences between states, such as infrastructure likely contribute to longer durations.

**Across Cause Categories**: We observed that certain causes of outages (e.g., severe weather events) are associated with longer durations, supporting the hypothesis that specific causes lead to more prolonged outages.

**Across Months**: Interestingly, we failed to reject the null hypothesis for months, as the p-value was not sufficiently small, indicating that the variation in outage durations across months is likely due to random variation. This suggests that, unlike states and causes, month-to-month differences in outage duration do not exhibit a strong systematic pattern.


## Framing a Prediction Problem 
We aim to train a model that can predict the cause of power outages based on a variety of simple yet effective features. By identifying patterns in historical data, we seek to provide accurate predictions early, enabling faster outage cause determination. This allows authorities to take swift action, allocate resources efficiently, and enhance predictive maintenance by eliminating the root cause before failures occur.

In large-scale outage scenarios—such as a sudden city-wide blackout with a surge in complaints—there’s often no time for extensive forensic analysis before action must be taken. Our model bridges this gap by offering data-driven insights in real time, helping utilities quickly assess the likely cause and respond accordingly. In the long run, such predictive capabilities contribute to a more resilient power grid, reducing downtime and improving overall service reliability.

## Baseline Model

### Model Used 
**Our model is a Decision Tree Classifier pipeline designed to predict the `CAUSE.CATEGORY` of power outages based on three features**

### Features in the Model

**Total features: 3**
  - `Outage Duration`: (Quantative Feature) Numeric variable representing the length of time the outage lasted
     This may correlate with certain types of causes (e.g., severe weather events typically cause longer outages than equipment failures)
    
  - `US State`: (Nominal Feature) Categorical variable representing the state where the outage occurred.
    Different states have varying infrastructure, regulations, and environmental conditions that influence outage causes.
    
  - `Month`: (Norminal Feature) Categorical variable representing the month when the outage occurred
     This might capture seasonal patterns that might affect outage causes (e.g., storm seasons, high electricity demand periods)

### Feature Engineering
- `US State`: One-hot encoded using `OneHotEncoder(drop='first')` to convert this nominal variable into binary features while avoiding multicollinearity by dropping one reference category.
- `Month`: The month are already labelled nominally (1-12), hence no transforming the data.
- `Outage Duration`: Maintained as a raw numeric feature without scaling.

These encodings were implemented using a `ColumnTransformer` within a scikit-learn `Pipeline` to ensure proper feature transformation during both training and prediction phases.

### Model Performance
- **Training Data Performance**: ~80% Accuracy
- **Test Data Performance**: ~68% Accuracy
- **F1-score**: ~0.64

### Model Improvements
1. Using random forest classifier instead of using decision trees, giving us more liberty with the hyperparameters and less variance in the model's estimates.
   
2. **Cross-validation**: Using k-fold cross-validation ensures more reliable performance evaluation across different datasets, and decreasing the chance of overfitting.
   
3. **Efficient parameter search**: RandomizedSearchCV with `'n'` iterations provided a smart exploration of the hyperparameter space without the computational burden of exhaustive grid search.


## Final Model
**Our Final model is a RandomForest Classifier pipeline designed to predict the `CAUSE.CATEGORY` of power outages now based on 4 features**

We maintain the first 3 features we use in the baseline model, and on top of that we also use 'Customers Affected' as a new feature. 

### Features:
 - `Customers Affected`: (Quantative Feature) Numeric variable representing the number of customers affected due to the outage.
    Some causes of outage might affect a wider area for a longer time hence, customers affected might have a correlation with the outage cause.
  
 - `Outage Duration`: (Quantative Feature) Numeric variable representing the length of time the outage lasted
    
 - `US State`: (Nominal Feature) Categorical variable representing the state where the outage occurred.
    
 - `Month`: (Norminal Feature) Categorical variable representing the month when the outage occurred

### Feature Engineering
-  'Customers Affected': No scaling the data. `np.nan` is used if this feature has a missing value
- `US State`: One-hot encoded using `OneHotEncoder(drop='first')` to convert this nominal variable into binary features while avoiding multicollinearity by dropping one reference category.
- `Month`: The month are already labelled nominally (1-12), hence no transforming the data.
- `Outage Duration`: Maintained as a raw numeric feature without scaling.

### Model Performance
- **Training Data Performance**: ~90.2% Accuracy
- **Test Data Performance**: ~86.1% Accuracy
- **F1-score**: ~0.84

### Hyperparamters
We used `RandomizedSearchCV` to find the best `max_depth`, `min_sample_split`, and `criterion` for the RandomForest Classifier

- Ideally, lower `max_depth` is preferred, since higher `max_depth` leads to overfitting.
- Conversely, a higher `min_sample_split` is set to prevent overfitting.
- `criterion` measures the quality of the node split (Nodes of DecisionTrees in the RandomForest). It is selected between `gini` and `entropy`.

**Hyperparameters used**:
- `max_depth` : 20
- `min_sample_split` : 15
- `criterion` : gini

These hyperparamters help the model overfit less and perform a better prediction over unseen data.



## Fairness Analysis
<iframe
  src="plots/Accuracy_Unfair_Region.html"
  width="800"
  height="380"
  frameborder="0"
></iframe>
<iframe
  src="plots/Datapoints_for_Accuracy_Region.html"
  width="800"
  height="380"
  frameborder="0"
></iframe>
<iframe
  src="plots/accuracy_by_climate_region_map.html"
  width="900"
  height="500"
  frameborder="0"
></iframe>

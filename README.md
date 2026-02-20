# A3-Study

See [AI_USAGE_NOTES.md](AI_USAGE_NOTES.md) for notes on how we used AI in this project.

## Experiment Setup
Link to Our Revisit Study: https://weavergoldman.com/a3-study/

<img width="2560" height="1357" alt="image" src="https://github.com/user-attachments/assets/72f60e83-68c9-4ee1-bedd-ad4b3cd301bf" />

Our experiment was to determine whether it was easier or more difficult to identify the month with the highest average in a time series dataset with a line graph or colorfield, and for the ordered or permuted versions of each. 

Each user sees 20 line graphs and 20 colorfields, with half of each being permuted. The data for these graphs is generated with a random winning month for each, and the following parameters:

Parameter | Value
--- | ---
k (# distractor months) | 4
d (difficulty) | 2
noise_level | 2

We randomized the overall order of the graphs for each user.

An example of each type of graph can be found below:

Line Graph Non Permuted
<img width="1543" height="638" alt="image" src="https://github.com/user-attachments/assets/7d63babb-4742-41eb-b444-9621e456f878" />

Line Graph Permuted
<img width="1535" height="647" alt="image" src="https://github.com/user-attachments/assets/833748bb-cc13-4353-a6f7-34f959dfa3e7" />


Colorfield Non Permuted
<img width="1559" height="374" alt="image" src="https://github.com/user-attachments/assets/e5b13139-36a7-4f63-9770-a185ae7b7681" />

Colorfield Permuted
<img width="1556" height="379" alt="image" src="https://github.com/user-attachments/assets/b2995d84-cbcf-4408-9ad6-872f87bec451" />



## Data Analysis
We used mixed logistical regression with BinomialBayesMixedGLM to analyze our data because it has a binary outcome as opposed to a continous value. We use this to get the predicted probability and standard deviation of the trial results. This is shown in our results figure via the red model prediction dot and line. 

We also calculated the emperical mean and bootstrapped 95% confidence intervals to get upper and lower error bounds. This is shown in our result figure via the black error bars. 

Using these two different ways of calculating error helps us report confidence in our findings. We used these metrics instead of average log2Error or Cleveland and McGill’s log-base-2 error because of binary true/false responses of whether our participants had guessed the correct month. We don’t have continuous values that allow for calculating how wrong participants were because they were either right or wrong and therefore, it doesn't make sense to use these two ways of calculating error. 

We also used Generalized Estimating Equations to calculate p-values and see if our results were statistically significant. 

<img width="790" height="588" alt="analysis_results" src="https://github.com/user-attachments/assets/88c589e1-ba7f-47c0-865b-20ee67ab4a7e" />

Results:
| chart_type   | permuted  | mean_accuracy | ci_lower | ci_upper | predicted_probability | sd_probability |
|-------------|-----------|---------------|----------|----------|----------------------|----------------|
| colorfield  | ordered   | 0.55          | 0.47     | 0.62     | 0.543                | 0.029          |
| colorfield  | permuted  | 0.36          | 0.31     | 0.41     | 0.341                | 0.046          |
| line graph  | ordered   | 0.39          | 0.31     | 0.46     | 0.371                | 0.048          |
| line graph  | permuted  | 0.28          | 0.21     | 0.36     | 0.224                | 0.064          |

Analysis of statistical significance:
| Term                                           | Beta (log-odds) | Std.Err. | 95% CI           | p      |
|------------------------------------------------|-----------------|----------|------------------|--------|
| Intercept                                      | 1.222           | 0.154    | [2.51, 4.59]     | 0.192  |
| chart_type[T.line chart]                       | 0.523           | 0.167    | [1.216, 2.34]    | < .001 |
| permuted[T.permuted]                           | 0.460           | 0.189    | [1.094, 2.294]   | < .001 |
| chart_type[T.line chart]:permuted[T.permuted]  | 1.322           | 0.236    | [2.362, 5.957]   | 0.237  |

Descriptive statistics show that colorfields had higher mean accuracy than line graphs, both when ordered and permuted. Specifically, ordered colorfields achieved a mean accuracy of 55% (95% CI: 0.47–0.62), while ordered line graphs achieved a lower mean accuracy of 39% (95% CI: 0.31–0.46). Permutation reduced accuracy for both chart types, with permuted colorfields at 36% (95% CI: 0.31–0.41) and permuted line graphs at 28% (95% CI: 0.21–0.36).

According to our logistic regression analysis, chart type and permutation were both significant predictors of accuracy: line charts had lower odds of being answered correctly compared to colorfields (β = 0.523, p < 0.001), and permuted charts had lower odds than ordered charts (β = 0.460, p < 0.001). The interaction between chart type and permutation was not statistically significant (β = 1.322, p = 0.237), suggesting that the effect of permutation on accuracy did not differ consistently between chart types.

These results are consistent with previous findings (https://dl.acm.org/doi/epdf/10.1145/2207676.2208556) showing that colorfields tend to enable higher accuracy than line graphs. While the effect of permutation in our study differs from that in the original experiment, likely due to differences in how permuted charts were generated, the overall pattern indicates that both chart type and data ordering influence participant performance.

The clean data can be found in `data/clean_data.csv`.

## Technical Achievements:
- Generated data to make graphs with a winning month, some number of distracter months, and some noise level to generate a diverse data set to run our experiment.
    - Used perlin noise to generate smooth noise for our data, which is more realistic and complex than the underlying step function that creates each time series.
    - Used cubic splines to create smoother time series data from the original noise and step function data.
- Generating permuted graphs for both line graphs and colorfields.
- We used d3 within React components to create our line graphs and colorfields, while the data was pre-generated with python. The config file dynamically references different data series in the data file to create the different graphs.
- Using python libraries pandas, statsmodels, matplotlib, and seaborn for data analysis to calculate two different error metrics and plot our results.
- Processed tidy csv output from reVISit to create a clean dataframe for analysis.
- Used a mixed logistical regression model to analyze our data and get the predicted probability and standard deviation of the trial results.
- Used bootstrapping to calculate the emperical mean and 95% confidence intervals for our data.
- Used Generalized Estimating Equations to calculate p-values.

## Design Achievements:
- Two different graph types: Colorfield and Line Graph
    - Colorfield: A grid of colored squares where the color intensity represents the value of the data point. This allows for quick visual identification of intensity.
    - Line Graph: A graph that uses lines to connect individual data points, showing trends over time. This allows for a clear visualization of the data's progression and can help identify peaks and troughs.
- Two different graph arrangements: Ordered and Permuted
    - Ordered: The data points are arranged in chronological order.
    - Permuted: The data points are randomly arranged within each month. The goal is to make the graph seem more uniform in each month.
- Plotted results in a bar graph to show both ordered and permuted results for both line graphs and colorfields, with two types of error bars. 
- Used a red and blue color scheme to make sure the colors were easily distinguishable for all users, including those with color blindness.
- Minimized distracting elements in the graphs to make sure that the only thing participants were focusing on was the data itself.
- Added bars to the graphs to indicate the bounds of each month to make it easier for participants to identify the mean of each month.

## Observations:
- While conducting the study, one participant mentioned that they percieved the dark red (more intense colors) as much more intense than the lighter colors, adding that they expected the difference between dark red and light red to be much larger than it was. 
    - This observation suggests that the difference in color intensity may be percieved as much greater than the linear difference in corresponding values. To account for this, it may be helpful to change the color scale to only be a gradient from white to red



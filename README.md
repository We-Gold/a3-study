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

Line Graph
<img width="1517" height="637" alt="line graph" src="https://github.com/user-attachments/assets/5703353c-c852-4178-b3dd-a485bad0b65a" />

Colorfield
<img width="1546" height="382" alt="colorfield" src="https://github.com/user-attachments/assets/3a073924-6bf1-49db-85eb-04e2d0f148f4" />


## Data Analysis
We used mixed logistical regression with BinomialBayesMixedGLM to analyze our data because it has a binary outcome as opposed to a continous value. We use this to get the predicted probability and standard deviation of the trial results. This is shown in our results figure via the red model prediction dot and line. 

We also calculated the emperical mean and bootstrapped 95% confidence intervals to get upper and lower error bounds. This is shown in our result figure via the black error bars. 

Using these two different ways of calculating error helps us report confidence in our findings. We used these metrics instead of average log2Error or Cleveland and McGill’s log-base-2 error because of binary true/false responses of whether our participants had guessed the correct month. We don’t have continuous values that allow for calculating how wrong participants were because they were either right or wrong and therefore, it doesn't make sense to use these two ways of calculating error. 

<img width="790" height="588" alt="analysis_results" src="https://github.com/user-attachments/assets/88c589e1-ba7f-47c0-865b-20ee67ab4a7e" />

| chart_type   | permuted  | mean_accuracy | ci_lower | ci_upper | predicted_probability | sd_probability |
|-------------|-----------|---------------|----------|----------|----------------------|----------------|
| colorfield  | ordered   | 0.55          | 0.47     | 0.62     | 0.543                | 0.029          |
| colorfield  | permuted  | 0.36          | 0.31     | 0.41     | 0.341                | 0.046          |
| line graph  | ordered   | 0.39          | 0.31     | 0.46     | 0.371                | 0.048          |
| line graph  | permuted  | 0.28          | 0.21     | 0.36     | 0.224                | 0.064          |

This successfully replicated the experiment from https://dl.acm.org/doi/epdf/10.1145/2207676.2208556 which shows that colorfields have a higher mean correctness than line graphs.


## Technical Achievements:
- Generated data to make graphs with a winning month, some number of distracter months, and some noise level to generate a diverse data set to run our experiment.
    - Used perlin noise to generate smooth noise for our data, which is more realistic and complex than the underlying step function that creates each time series.
- Generating permuted graphs for both line graphs and colorfields.
- Use of React to display graphs.
- Using python libraries pandas, statsmodels, matplotlib, and seaborn for data analysis to calculate two different error metrics and plot our results.
- Processed tidy csv output from reVISit to create a clean dataframe for analysis.
- Used a mixed logistical regression model to analyze our data and get the predicted probability and standard deviation of the trial results.
- Used bootstrapping to calculate the emperical mean and 95% confidence intervals for our data.

## Design Achievements:
- Two different graph types: Colorfield and Line Graph
    - Colorfield: A grid of colored squares where the color intensity represents the value of the data point. This allows for quick visual identification of intensity.
    - Line Graph: A graph that uses lines to connect individual data points, showing trends over time. This allows for a clear visualization of the data's progression and can help identify peaks and troughs.
- Two different graph arrangements: Ordered and Permuted
    - Ordered: The data points are arranged in chronological order.
    - Permuted: The data points are randomly arranged within each month. The goal is to make the graph seem more uniform in each month.
- Plotted results in a bar graph to show both ordered and permuted results for both line graphs and colorfields, with two types of error bars. 

## Observations:
- While conducting the study, one participant mentioned that they percieved the dark red (more intense colors) as much more intense than the lighter colors, adding that they expected the difference between dark red and light red to be much larger than it was. 
    - This observation suggests that the difference in color intensity may be percieved as much greater than the linear difference in corresponding values. To account for this, it may be helpful to scale the color intesity mapping so that it gets exponentially more intense as the values deviate from the center of the range.



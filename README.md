# reVISit study – Interactive, Web-Based User Studies. 

AI Usage Note: Right now, we have a few sample plot components in the config. These are loading the data directly. For the study, what we want to do is to show 20 line charts, and 20 colorfields. Half of each are permuted. The data file should be referenced for this purpose. Can you come up with a plan for how to adapt our current code for the full study setup?

Create your own interactive, web-based data visualization user studies by cloning/forking and editing configuration files and adding stimuli in the `public` folder. 

reVISit introduces reVISit.spec a DSL for specifying study setups (consent forms, training, trials, etc) for interactive web based studies. You describe your experimental setup in reVISit.spec, add your stimuli as images, forms, html pages, or React components, build and deploy – and you're ready to run your study. For tutorials and documentation, see the [reVISit website](https://revisit.dev). 

## Build Instructions

To run this demo experiment locally, you will need to install node on your computer. 

* Clone `https://github.com/revisit-studies/study`
* Run `yarn install`. If you don't have yarn installed, run `npm i -g yarn`. 
* To run locally, run `yarn serve`.
* Go to [http://localhost:8080](http://localhost:8080) to view it in your browser. The page will reload when you make changes. 

## Release Instructions

Releasing reVISit.dev happens automatically when a PR is merged into the `main` branch. The name of the pull request should be the title of the release, e.g. `v1.0.0`. Releasing creates a tag with the same name as the PR, but the official GitGub release should be created manually. The `main` branch is protected and requires two reviews before merging.

The workflow for release looks as follows:
Develop features on feature branch
| PRs
Dev branch
| PR (1 per release)
Main branch
| Run release workflow on merge
References are updated and commit is tagged


## Experiment Setup
Link to Our Revisit Study: https://weavergoldman.com/a3-study/
Our experiment was to determine whether it was easier or more difficult to identify the month with the highest average for a line graph or colorfield, and for the ordered or permuted versions of each. Each graph was permuted 20 times for a total of 40 graphs per experiment.
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
- Generating permuted graphs for both line graphs and colorfields.
- Use of React to display graphs.
- Using python libraries pandas, statsmodels, matplotlib, and seaborn for data analysis to calculate two different error metrics and plot our results.

## Design Achievements:
- Two different graph types: Colorfield and Line Graph
- Plotted results in a bar graph to show both ordered and permuted results for both line graphs and colorfields, with two types of error bars. 

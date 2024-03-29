# Predicting Churn of a Music Streaming Service

## About

Sparkify is an online music streaming service. It has customers which listen to music, rate it, change their subscription and sometimes cancel their account. The goal of this project is to predict clients which are likely to cancel their account, such that Sparkify can make them certain offers in order to keep them on their platform.

For more background and details see the corresponding [blog post](https://timwinkler99.github.io/portfolio/predicting-churn-sparkify).

## Getting Started

1. Create conda environment
2. Install dependencies (pandas, numpy, pyspark, ...)
   * Need to install Java for Pyspark. The easiest way to do this is using brew `brew install openjdk`

## Structure

### Load and Prepare Data

After loading the data into a notebook, we'll notice some columns require formatting before we can use them to learn about how the users interact with sparkify. This includes the location column, which we split up into city and state, and the userAgent column, from which we extract the end device with which the user accessed sparkify.

Furthermore, we delete all rows without a userId since we are not able to distinguish between who made the actions.

### Exploratory Data Analysis

To learn about how users interact with the platform, we divide each users logs into different regimes. The first regime starts when the first log appears and it is either a paid or free regime. When the user upgrades or downgrades his account, a new regime starts. Hence, we are able to see for example, if a users who downgrades to a free account, submitted a high number of thumbs down. The result is stored in the `user_regime` table.

The statement to obtain the `user_regime` table got a little bit more complicated. Removing the final select statement and querying each pretable might give you an idea about what's happining.

To get the data ready for the ML model, the `user_regime` table is simply aggregated to have one row per user ID.

#### Troubleshooting & Abnormalities

While performing this aggregation, it occured that some logs of a user have the same exact timestamp, which caused mistakes in partition statements. This was resolved by partitioning the log per user and assigning row numbers.

Related to this issue, we came across the fact that if the action of a user failed, the error is recorded in a preceeding log. This means, that for any log, we have to check what the message of the next log is, to filter out actions that were ultimately not performed.


### Modeling

In version 3.3.2 spark supports a variety of [classification algorithms](https://spark.apache.org/docs/3.3.2/ml-classification-regression.html#classification-and-regression). We will focus on: Logistic Regression, Random Forest, Gradient Boosted Tree and Linear Support Vector Machine classification algorithms.

The process of fitting a model is relatively straight forward:

1. Split the data
2. Combine them in a vector
3. Normalize that vector
4. Initialise model
5. Set up a parameter grid, an evaluation metric and a cross validator
6. Fit model

To evaluate the models, we compute the **accuracy** and the **F1-score** for each model.


### Results

Running the 4 models provides us with the following results:

| model | accuracy | f1-score |
|-------|----------|----------|
| lrm   | 0.7429   | 0.6868   |
| rfc   | 0.7714   | 0.7312   |
| gbt   | 0.8571   | 0.8513   |
| svm   | 0.6571   | 0.5212   |

Clearly the gradient-boosted tree model provides the highest accuracy and f1-score.

# Acknowledgements

The data for this project was provided by Udacity and is part of the Data Science Nanodegree.
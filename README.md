# Predicting Churn of a Music Streaming Service

## About

Sparkify is an online music streaming service. It has customers which listen to music, rate it, change their subscription and sometimes cancel their account. The goal of this project is to predict clients which are likely to cancel their account, such that Sparkify can make them certain offers in order to keep them on their platform.

## Getting Started

1. Create conda environment
2. Install dependencies (pandas, numpy, pyspark, ...)
   * Need to install Java for Pyspark. The easiest way to do this is using brew `brew install openjdk`

## Structure

### Load and Prepare Data

After loading the data into a notebook, we'll notice some columns require formatting before we can use them to learn about how the users interact with sparkify. This includes the location column, which we split up into city and state, and the userAgent column, from which we extract the end device with which the user accessed sparkify.

Furthermore, we delete all rows without a userId since we are not able to distinguish between who made the actions.


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

* Accuracy is defined as $$\frac{\text{no. of correct predictions}}{\text{no. of total predictions}}$$
* The F1-score combines the measure of accuracy and recall $$\frac{1}{N} \sum_{i=0}^{N-1}2\frac{|P_i\cap L_i|}{|P_i|\cdot|L_i|},$$
where $L_0, L_1, ..., L_{N-1}$ are the label sets and $P_0, P_1, ..., P_{N-1}$ the prediction sets


For more details see the [Spark Documentation](https://spark.apache.org/docs/2.2.0/mllib-evaluation-metrics.html#multilabel-classification).

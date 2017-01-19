INTRODUCTION
------------

This is document describing the analysis I conducted for my final
project for the Johns Hopkins’ Coursera course “Practical Machine
Learning” in the Data Science specialization. One thing that people
regularly do is quantify how much of a particular activity they do, but
they rarely quantify how well they do it. In this project, your goal
will be to use data from accelerometers on the belt, forearm, arm, and
dumbell of 6 participants.

The goal of your project is to predict the manner in which they did the
exercise. I describe how to built my models, how I used cross
validation, what I think the expected out of sample error is, and why I
made the choices I did. I also used my prediction model to predict 20
different test cases.

Data
----

The training data for this project are available here:

<https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv>

The test data are available here:

<https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv>

Load Data and conduct initial data exploration
----------------------------------------------

**Set working directory**

setwd("C:/Users/Tachibana/Documents/GitHub/Practical\_Machine\_learning")

**Load Data**

trainUrl &lt;-
"<http://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv>"

testUrl &lt;-
"<http://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv>"

training &lt;- data.frame(read.csv(trainUrl, header=TRUE))

testing &lt;- data.frame(read.csv(testUrl, header=TRUE))

head(training)

**Partioning the training set into two**

library(caret)

set.seed(975)

inTrain = createDataPartition(training$classe, p = 0.7)\[\[1\]\]

training = training\[ inTrain,\]

testing = training\[-inTrain,\]

**Clean Data**

**Remove NearZeroVariance variables**

Find columns with near zero variance and remove them
----------------------------------------------------

df\_nzv &lt;- nearZeroVar(df, saveMetrics=TRUE)

remaining &lt;- df\_nzv\[which(df\_nzv$nzv==FALSE),\]

df\_all\_var &lt;- subset(df , select=rownames(remaining))

Remove Columsn with NAs
-----------------------

df\_rm\_na &lt;- df\_all\_var\[ , colSums(is.na(df\_all\_var)) == 0\]

Find Correlated variables
=========================

df\_corr &lt;- cor(subset(df\_rm\_na, select=-classe))

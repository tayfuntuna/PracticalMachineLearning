PracticalMachineLearning
========================
R
<code>
library(AppliedPredictiveModeling)
library(caret)
library(randomForest)
Download files and import them as dataset into RStudio environment from the menu
trainData <- read.csv("D:\\aa\\pml-training.csv", na.strings=c("NA",""), header=TRUE) 
testData<- read.csv("D:\\aa\\pml-testing.csv", na.strings=c("NA",""), header=TRUE) 
</code>

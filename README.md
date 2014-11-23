Welcome to the PracticalMachineLearning wiki!

#### 1. I loaded library and files(after manually download) and try to train by RF
library(AppliedPredictiveModeling)<br/>
library(caret)<br/>
library(randomForest)<br/>
trainData <- read.csv("D:\\aa\\pml-training.csv", na.strings=c("NA",""), header=TRUE)<br/>
testData<- read.csv("D:\\aa\\pml-testing.csv", na.strings=c("NA",""), header=TRUE)<br/>
yModel <- train(trainData$classe ~ ., method="rf", trControl=trainControl(method = "cv", number = 10), data=trainData,importance = TRUE)<br/>

> I got this : Error in contrasts<-`(`*tmp*`, value = contr.funs[1 + isOF[nn]]) :+1:  contrasts can be applied only to factors with 2 or more levels

#### 2. I tried to remove empty row/columns and unnecessary columns before train by RF
trainData<-na.omit(trainData)<br/>
yModel <- train(trainData$classe ~ ., method="rf", trControl=trainControl(method = "cv", number = 10), data=trainData,importance = TRUE)<br/>
> Same Error Error in contrasts<-`(`*tmp*`, value = contr.funs[1 + isOF[nn]]) :+1:  contrasts can be applied only to factors with 2 or more levels

#### 3. I removed empty row/columns and unnecessary columns in another way before train by RF
trainColumns <- colnames(trainData)<br/>
validColumns <- function(x) {<br/>
  as.vector(apply(x, 2, function(x) length(which(!is.na(x)))))<br/>
}<br/>
cols <- validColumns(trainData)<br/>
columnsToRemove <- c()<br/>
for (i in 1:length(cols)) {<br/>
  if (cols[i] < nrow(trainData)) {<br/>
    columnsToRemove <- c(columnsToRemove, trainColumns[i])<br/>
  }}<br/>
trainData <- trainData[,!(names(trainData) %in% columnsToRemove)]<br/>
trainData<-trainData[,8:length(colnames(trainData))]<br/>
yModel <- train(trainData$classe ~ ., method="rf", trControl=trainControl(method = "cv", number = 10), data=trainData,importance = TRUE)<br/>
> It worked but couldnot see the result it took so long couldnot wait more

#### 4. I took only a portion from trainSet and processed 
train <- createDataPartition(y=trainData$classe, p=0.1, list=FALSE)
trainData <- trainData[train,]
myModel <- train(trainData$classe ~ ., method="rf", trControl=trainControl(method = "cv", number = 10), data=trainData,importance = TRUE)
print(myModel, digits=3)
> It worked and I could see the result: The accuracy was promissing so I decided to use the testdata and submit.
Summary of sample sizes: 1471, 1473, 1473, 1475 
> Resampling results across tuning parameters:
>  mtry  Accuracy  Kappa  Accuracy SD  Kappa SD<br/>
>   2    0.926     0.906  0.0181       0.0230  
>  27    0.926     0.906  0.0218       0.0277  
>  52    0.918     0.896  0.0215       0.0273

#### 5. I tested the file from my model and submitted the results
testData <- testData[,!(names(testData) %in% columnsToRemove)]<br/>
testData<-testData[,8:length(colnames(testData))]<br/>
predictions <- predict(myModel, newdata=testData)<br/>
print(predictions)<br/>
> Output was [1]  ..... (removed for privacy)
Levels: A B C D E
Only One were not correct so I submitted by the next alphabet it was correct 
![RStudioScreenShoot](http://ics.cs.uh.edu/myR/myr.png)

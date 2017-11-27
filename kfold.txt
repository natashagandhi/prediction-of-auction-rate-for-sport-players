databowler<-read.csv(file.choose())

#naiveBayes


outputData = 0
#Perform 5 fold cross validation
for(i in 1:5){
  #Segement your data by fold using the which() function
  folds <- cut(seq(1,nrow(databowler)),breaks=5,labels=FALSE)
  testIndexes <- which(folds==i,arr.ind=TRUE)
  testData <- databowler[testIndexes, ]
  trainData <- databowler[-testIndexes, ]
  
  classifier = naiveBayes(Category ~ ., data=trainData)
  pred = predict(classifier, testData)
  pred
  misClassifyError = mean(pred != testData$Category)
  Accuracy = 1-misClassifyError
  Accuracy
  outputData[i] = Accuracy
  #Use the test and train data partitions however you desire...
}
summary(outputData)


#randomForest
#Perform 5 fold cross validation
for(i in 1:5){
  #Segement your data by fold using the which() function 
  testIndexes <- which(folds==i,arr.ind=TRUE)
  testData <- databowler[testIndexes, ]
  trainData <- databowler[-testIndexes, ]
  
  rf = randomForest(Category ~ . , data = trainData,na.action=na.exclude)
  pred = predict(rf, testData)
  pred
  misClassifyError = mean(pred != testData$Category)
  Accuracy = 1-misClassifyError
  Accuracy
  outputData[i] = Accuracy
  #Use the test and train data partitions however you desire...
}
summary(outputData)


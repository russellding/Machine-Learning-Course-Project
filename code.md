Course Project Page for Machine Learning Course Offered by John Hopkins University

The project focuses on studying whether the records obtained from accelerometers on the belt, forearm, arm, and dumbell could be used to predict whether the participants are doing their workout correctly or not. Their manners doing the exericse are classified into 5 different categories (A,B,C,D and E). And, only type A is the correct way.
#Step 1: Data Collection and Cleaning
The original data is stored on the webpage as csv files. The very first step is loading the data into R for analysis.

Even though the original link uses https, it seems working fine to amend the link to http

trainRawData<-read.csv("http://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv")

testRawData<-read.csv("http://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv")

A glance at the training raw data, I notice that there are  a few entries with "NA" value. These records may impact our later analysis, so that I decide to remove these factors containing "NA" value. (If you run dim(trainRawData), you will notice that there are totally 160 coulmns. Reducing certian factors is necessary to speed up the algoithm!) Also, some columes, such as "X" and "raw_timestamp" is not actually related to the exercise manner. Removing these logically non-related factors are essential.

First, we remove the columns with "NA" value by (Need to remove test data first! Or there will be no reference for test data if raw data "NA" all removed.)
> testRawData <- testRawData[, colSums(is.na(trainRawData)) == 0]

Then clean the training data with "NA"
> trainRawData <- trainRawData[, colSums(is.na(trainRawData)) == 0]

It is possible that the testData may still have "NA" value. We need to repeat the procedure again:
> trainRawData <- trainRawData[, colSums(is.na(testRawData)) == 0]
> testRawData <- testRawData[, colSums(is.na(testRawData)) == 0]

Now, we have:
> dim(trainRawData)
[1] 19622    60
> dim(testRawData)
[1] 20 60
All the columns(factors) are the same in two datasets excpet the last column. Now, we need to exam the logical connection between the factors and the exercise manner to remove those with no logically connection.

> names(trainRawData)
 [1] "X"                    "user_name"            "raw_timestamp_part_1"
 [4] "raw_timestamp_part_2" "cvtd_timestamp"       "new_window"          
 [7] "num_window"           "roll_belt"            "pitch_belt"          
[10] "yaw_belt"             "total_accel_belt"     "gyros_belt_x"        
[13] "gyros_belt_y"         "gyros_belt_z"         "accel_belt_x"        
[16] "accel_belt_y"         "accel_belt_z"         "magnet_belt_x"       
[19] "magnet_belt_y"        "magnet_belt_z"        "roll_arm"            
[22] "pitch_arm"            "yaw_arm"              "total_accel_arm"     
[25] "gyros_arm_x"          "gyros_arm_y"          "gyros_arm_z"         
[28] "accel_arm_x"          "accel_arm_y"          "accel_arm_z"         
[31] "magnet_arm_x"         "magnet_arm_y"         "magnet_arm_z"        
[34] "roll_dumbbell"        "pitch_dumbbell"       "yaw_dumbbell"        
[37] "total_accel_dumbbell" "gyros_dumbbell_x"     "gyros_dumbbell_y"    
[40] "gyros_dumbbell_z"     "accel_dumbbell_x"     "accel_dumbbell_y"    
[43] "accel_dumbbell_z"     "magnet_dumbbell_x"    "magnet_dumbbell_y"   
[46] "magnet_dumbbell_z"    "roll_forearm"         "pitch_forearm"       
[49] "yaw_forearm"          "total_accel_forearm"  "gyros_forearm_x"     
[52] "gyros_forearm_y"      "gyros_forearm_z"      "accel_forearm_x"     
[55] "accel_forearm_y"      "accel_forearm_z"      "magnet_forearm_x"    
[58] "magnet_forearm_y"     "magnet_forearm_z"     "classe"              

Here, I removed "X", "raw_timestamp_part_1","raw_timestamp_part_2","cvtd_timestamp", "new_window" and "num_window".
> new<-testRawData[,-1]
> new<-new[,-2]
> new<-new[,-2]
> new<-new[,-2]
> new<-new[,-2]
> new<-new[,-2]
> testRawData<-new

Do the same on training data:
> new<-trainRawData[,-1]
> new<-new[,-2]
> new<-new[,-2]
> new<-new[,-2]
> new<-new[,-2]
> new<-new[,-2]
> trainRawData<-new

#Step 2:Model Construction and Prediction

I first choose Random Forests Algothrim since this algorithm can help to pick the best factors.

> library(caret)
Loading required package: lattice
Loading required package: ggplot2
Find out what's changed in ggplot2 with
news(Version == "1.0.1", package = "ggplot2")
> set.seed(12345)
> inTrain<-createDataPartition(y=trainRawData$classe, p=0.7, list=FALSE)
> training<-trainRawData[inTrain,]
> View(training)
> testing<-trainRawData[-inTrain,]
> modFit<-train(classe~., data=training, method="rf", prox=TRUE)
Loading required package: randomForest
randomForest 4.6-10
Type rfNews() to see new features/changes/bug fixes.
modFit



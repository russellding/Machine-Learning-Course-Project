Course Project Page for Machine Learning Course Offered by John Hopkins University

The project focuses on studying whether the records obtained from accelerometers on the belt, forearm, arm, and dumbell could be used to predict whether the participants are doing their workout correctly or not. Their manners doing the exericse are classified into 5 different categories (A,B,C,D and E). And, only type A is the correct way.
#Step 1: Data Collection
The original data is stored on the webpage as csv files. The very first step is loading the data into R for analysis.

Even though the original link uses https, it seems working fine to amend the link to http

trainRaw<-read.csv("http://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv")

testdata<-read.csv("http://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv")


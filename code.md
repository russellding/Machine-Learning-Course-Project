# Machine-Learning-Course-Project
Course Project Page for Machine Learning Course Offered by John Hopkins University
Step 1: Data Collection
The original data is stored on the webpage as csv files. The very first step is loading the data into R for analysis.

#Even though the original link uses https, it seems working fine to amend the link to http
trainRaw<-read.csv("http://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv")

testdata<-read.csv("https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv")


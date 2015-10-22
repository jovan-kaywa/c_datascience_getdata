---
title: "Readmefor Johns Hopkins course "Getting and Cleaning Data" on Coursera"
author: "Jovan Milicevic"
date: "October 20, 2015"
output: html_document
---

Here is the explanation of every step needed in order to create run_analysis.R script that you can use to generate .csv file with tidy data which has average values per variable, grouped by subject and activity

```{r}
# liraries dplyr (to work with tables) and respahe2 (to make tidy data)
# to install dplyr libray just uncomment next line
# install.packages("dplyr")
# load dplyr libray

library(dplyr)

# load reshape2 [ackage]
library(reshape2)
# create data folder to store all our downloaded data files
if(!file.exists("data")){dir.create("data")}

# download project zip file and rename it to getdata.zip
fileUrl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download.file(fileUrl,destfile="./data/getdata.zip",method="curl")
# add date of download to be able to check if data changed later 
dateDownloaded <- date()
# unzip data
unzip("data/getdata.zip", exdir = "./data/")

# read all needed data test and train files, and also activity labels
X_test <- read.table("data/UCI HAR Dataset/test/X_test.txt", quote="\"")
subject_test <- read.table("data/UCI HAR Dataset/test/subject_test.txt", quote="\"")
y_test <- read.table("data/UCI HAR Dataset/test/y_test.txt", quote="\"")

X_train <- read.table("data/UCI HAR Dataset/train/X_train.txt", quote="\"")
subject_train <- read.table("data/UCI HAR Dataset/train/subject_train.txt", quote="\"")
y_train <- read.table("data/UCI HAR Dataset/train/y_train.txt", quote="\"")

activity_labels <- read.table("data/UCI HAR Dataset/activity_labels.txt", quote="\"")

# read features.txt to get variables description
features <- read.table("data/UCI HAR Dataset/features.txt", quote="\"")

# Merge files
# https://class.coursera.org/getdata-033/forum/thread?thread_id=141
# merge test subject and activity files to create a dataset which 
# have all needed information

#merge test file with subjects
testSubjectMerge <- cbind (X_test,subject_test)
# check variables, the subject variable is on the end
head(testSubjectMerge)

#merge test file including subjects with activities
testFinal <- cbind (testSubjectMerge,y_test)
# check variables, the activity variable is on the end
head(testFinal)

#merge train file with subjects
trainSubjectMerge <- cbind (X_train,subject_train)
# check variables, the subject variable is on the end
head(trainSubjectMerge)

#merge test file including subjects with activities
trainFinal <- cbind (trainSubjectMerge,y_train)
# check variables, the activity variable is on the end
head(trainFinal)

# merge test and train files to create complete dataset
testTrainFinal <- rbind(testFinal,trainFinal)

# add missing values for our two new variables Subject and Activity
featuresSubject <- rbind( features, data.frame("V1"=562, "V2"="Subject"))
featuresFinal <- rbind( featuresSubject, data.frame("V1"=563, "V2"="Activity"))

# rename variables in merged file with featerus descriptions
# Link http://stackoverflow.com/questions/25468003/rename-columns-from-external-file-in-r
names(testTrainFinal) <- featuresFinal$V2


# Error: found duplicated column name
# fix duplicated column names
# http://stackoverflow.com/questions/28549045/dplyr-select-error-found-duplicated-column-name
valid_column_names <- make.names(names=names(testTrainFinal), unique=TRUE, allow_ = TRUE)
names(testTrainFinal) <- valid_column_names

# filter only needed columns with standard deviation and mena in name, 
# and keep Activity and Subject variables too
testTrainFinalClean<-select(testTrainFinal,matches('std|mean|Activity|Subject'))

# rename activity to use descriptive activity names
testTrainFinalActivity <- merge(testTrainFinalClean, activity_labels, by.x = 'Activity', by.y = 'V1', all = FALSE)

# rename last variable
names(testTrainFinalActivity)[89] <- "Activity"

# remove first variable
# http://www.cookbook-r.com/Manipulating_data/Adding_and_removing_columns_from_a_data_frame/
testTrainFinalActivity[[1]] <- NULL

# make another dataset with grouped Subject and Activity variables
activitySubjectGroups <- group_by(testTrainFinalActivity,Subject,Activity)

#summarize each variable to get their mean values
summary <- summarize_each(activitySubjectGroups,funs(mean))

# make tidy dataset
# http://seananderson.ca/2013/10/19/reshape.html
tidy <- melt(summary, id.vars = c("Subject", "Activity"))
View(tidy)
```
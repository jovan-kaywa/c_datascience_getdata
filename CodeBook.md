---
title: "'Getting and Cleaning Data Cook' CookBook"
author: "Jovan Milicevic"
date: "October 22, 2015"
---

## Project Description

This is a "Getting and Cleaning Data Cook" project which use "Human Activity Recognition Using Smartphones Dataset"


##Study design and data processing

###Collection of the raw data
The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data. 

The sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window). The sensor acceleration signal, which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. From each window, a vector of features was obtained by calculating variables from the time and frequency domain. See 'features_info.txt' for more details. 

###Notes on the original (raw) data 
For each record it is provided:
- Triaxial acceleration from the accelerometer (total acceleration) and the estimated body acceleration.
- Triaxial Angular velocity from the gyroscope. 
- A 561-feature vector with time and frequency domain variables. 
- Its activity label. 

##Creating the tidy datafile

###Guide to create the tidy data file
1. Download the [dataset](https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip)
2. Unpack it in your working directory (you can also just run the script and it will download and unpack the files)
3. Merge training set, subjects and activites
4. Merge test set, subjects and activites
5. Merge test and training sets
6. Rename all variables with labels from the features.txt file
7. Make variable names unique
8.filter only needed columns with standard deviation and mena in name, and keep Activity and Subject variables too
9. Make another dataset with grouped Subject and Activity variables
10. Summarize each variable to get their mean values
11. Make tidy dataset
12. Create .csv table with tidy dataset

###Cleaning of the data
During the cleaning process, only variables that contains data about Average and Standard devitation are kept. [Please check the readme file for all details](https://github.com/jovan-kaywa/c_datascience_getdata/blob/master/README.md)

##Description of the variables in the tiny_data.txt file
General description of the file including:
 - Dimensions of the dataset
 - Summary of the data
 - Variables present in the dataset

(you can easily use Rcode for this, just load the dataset and provide the information directly form the tidy data file)

###Variable 1 (repeat this section for all variables in the dataset)
Short description of what the variable describes.

Some information on the variable including:
 - Class of the variable
 - Unique values/levels of the variable
 - Unit of measurement (if no unit of measurement list this as well)
 - In case names follow some schema, describe how entries were constructed (for example time-body-gyroscope-z has 4 levels of descriptors. Describe these 4 levels). 

(you can easily use Rcode for this, just load the dataset and provide the information directly form the tidy data file)

####Notes on variable 1:
If available, some additional notes on the variable not covered elsewehere. If no notes are present leave this section out.

##Sources
Sources you used if any, otherise leave out.

##Annex
If you used any code in the codebook that had the echo=FALSE attribute post this here (make sure you set the results parameter to 'hide' as you do not want the results to show again)
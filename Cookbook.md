---
title: "Codebook for run_analysis.R"
author: "Rebecca Kirstein"
date: "8/23/2015"
output:
  html_document:
    keep_md: yes
---

## Project Description
This project is about collecting and processing data for wearable computing. The data we are woring with is from accelerometers from 
Samsung Galaxy S smartphones. A complete description of the data collection is located at UCI, Machine Learning Repository at
http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones. The goal for this project is to work with the raw
data, process it by turning it into tidy data that is ready for analysis and do a simple analysis of mean and standard deviation collected features.
A detailed description of collection, data, and analysis is described below. 

##Study design and data processing
For this project the data that was available on multiple text files. The first task was to understand exactly what was in each file and how it fit into the 
big picture. It was like solving a puzzle. If the data was not put together properly, it would be useless. There were multiple supporting files such as 
the readme and features.info that helped with understanding. The abstract on the collection was: "Human Activity Recognition database built from the recordings
of 30 subjects performing activities of daily living (ADL) while carrying a waist-mounted smartphone with embedded inertial sensors." 

###Collection of the raw data 
The data was provided by the course as a zip file that contained the following files:

- 'README.txt'

- 'features_info.txt': Shows information about the variables used on the feature vector.

- 'features.txt': List of all features.

- 'activity_labels.txt': Links the class labels with their activity name.

- 'train/X_train.txt': Training set.

- 'train/y_train.txt': Training labels.

- 'test/X_test.txt': Test set.

- 'test/y_test.txt': Test labels.

The following files are available for the train and test data. Their descriptions are equivalent. 

- 'train/subject_train.txt': Each row identifies the subject who performed the activity for each window sample. Its range is from 1 to 30. 

The following files were available but were not used for this project. This project was interested in mean and standard deiviations for for activities for each participant.

- 'train/Inertial Signals/total_acc_x_train.txt': The acceleration signal from the smartphone accelerometer X axis in standard gravity units 'g'. Every row shows
   a 128 element vector. The same description applies for the 'total_acc_x_train.txt' and 'total_acc_z_train.txt' files for the Y and Z axis. 

- 'train/Inertial Signals/body_acc_x_train.txt': The body acceleration signal obtained by subtracting the gravity from the total acceleration. 

- 'train/Inertial Signals/body_gyro_x_train.txt': The angular velocity vector measured by the gyroscope for each window sample. The units are radians/second.

###Notes on the original (raw) data 
The zip files was unzipped and the r code was run from the UCIHAR Dataset folder.

##Creating the tidy datafile
R Studio was used as the development environment. To create the tidy data the files had to be loaded in to R Studio with the read.table command. The following rocess is in cronilogical
order:
1. Read the data for train:, X_train.txt, y_train.txt, and subject_train.txt to the following dataframes: training_X, training_Y and subject_train respectfully. 
2. Read the data for test:, X_test.txt, y_test.txt, and subject_test.txt to the following dataframes: test_X, test_Y and subject_trainTest(I should have changed this to subject_test but
   I did not notice) respectfully.
3. Read the features.txt file into a data frame called features to be used to create labels for the train_X and test_X dataframes.
4. Create a char variable vector "names" from the V2 variable of the features dataframe and replace all of the "()" with "". Other than that leave the vairable names intact.  
   They are descriptive and all of the information is necessary. For example: in the variable name tBodyAcc-mean-Y, t is the time, BodyAccwas the signal for the Y axial
   and mean was the value.
5. Use the function colnames() to add names to each of the vaiables in the dataframes, Train_X and Test_X.
6. For the training_Y and Test_Y use colnames() to label the single variable "activity".
7. For the subjectTrain and subjectTrainTest use colnames() to label the single variable "id".
8. Use cbind() to append the training_Y and subject_Train to the front of the training_X dataframe.
9. Use cbind() to append the test_Y and subject_TrainTest to the front of the test_X dataframe.
10.Use rbind() to append test_X to the end of Train_X and create a new dataframe called combo.
11.Using the combo dataframe remove all variables except the ones containing ""mean" to create a new datframe called slimCombo using thr grep() function combined with the colnames() 
   function.
12.Using the combo dataframe remove all variables except the ones containing "std"" to create a new datframe called slimCombo2 using thr grep() function combined with the colnames() 
   function.
13 Use cbind()to append slimCombo, slimCombo2 and combo "activity" and "id" variables to a new datatframe called myCombo.
14.The last step is to change the variable values for the activities to the values in the activities.txt file using a simple statement to replace each value with the matching descriptive
   string

myCombo is your tidy data frame and it is used to create the tidy data analysis in step 5 of the project.
To create the tidy_data analysis we were asked to create a second independent tidy data set with the average of each variable for each activity and each subject.

This required one combined statement to first group the data by id then activity and summarize each of the remaining variables to get the average values for each mean and standard variation
instance creating a new dataframe called tidy_Data_summary.

  ###Guide to create the tidy data file
Use the write.table() comand to create the file tidy_data.txt.

###Cleaning of the data
run_analysis.R contains the code for cleaning the data, doing the analysis and outputting the final tidy_data txt file. [link to the readme document that describes the code in greater detail]()

##Description of the variables in the tiny_data.txt file
General description of the file including:
 - Dimensions of the dataset: 180 observations of 81 variables 
 - Summary of the data: the data has 30 subjects performing 6 activities and the ave value of ether the means or standard deviation for 79 collection points 
 - Variables present in the dataset:
 id = subject and is an "int"
 activity = for 6 activities and is a "chr"
 These variables of the feature vector for each pattern and all are "num":  
'-XYZ' is used to denote 3-axial signals in the X, Y and Z directions.
sanple for rest:
	tBodyAcc-X
	tBodyAcc-Y
	tBodyAcc-Z

tGravityAcc-XYZ
tBodyAccJerk-XYZ
tBodyGyro-XYZ
tBodyGyroJerk-XYZ

tBodyAccMag
tGravityAccMag
tBodyAccJerkMag
tBodyGyroMag
tBodyGyroJerkMag
fBodyAcc-XYZ
fBodyAccJerk-XYZ
fBodyGyro-XYZ
fBodyAccMag
fBodyAccJerkMag
fBodyGyroMag
fBodyGyroJerkMag

variables averaging the signals in a signal window sample

meanFreq
gravityMean
tBodyAccMean
tBodyAccJerkMean
tBodyGyroMean
tBodyGyroJerkMean

Here is the complete list:
"id" "activity" "tBodyAcc-mean-X" "tBodyAcc-mean-Y" "tBodyAcc-mean-Z" "tGravityAcc-mean-X" "tGravityAcc-mean-Y" "tGravityAcc-mean-Z" "tBodyAccJerk-mean-X" "tBodyAccJerk-mean-Y" 
"tBodyAccJerk-mean-Z" "tBodyGyro-mean-X" "tBodyGyro-mean-Y" "tBodyGyro-mean-Z" "tBodyGyroJerk-mean-X" "tBodyGyroJerk-mean-Y" "tBodyGyroJerk-mean-Z" "tBodyAccMag-mean" 
"tGravityAccMag-mean" "tBodyAccJerkMag-mean" "tBodyGyroMag-mean" "tBodyGyroJerkMag-mean" "fBodyAcc-mean-X" "fBodyAcc-mean-Y" "fBodyAcc-mean-Z" "fBodyAcc-meanFreq-X" 
"fBodyAcc-meanFreq-Y" "fBodyAcc-meanFreq-Z" "fBodyAccJerk-mean-X" "fBodyAccJerk-mean-Y" "fBodyAccJerk-mean-Z" "fBodyAccJerk-meanFreq-X" "fBodyAccJerk-meanFreq-Y"
"fBodyAccJerk-meanFreq-Z" "fBodyGyro-mean-X" "fBodyGyro-mean-Y" "fBodyGyro-mean-Z" "fBodyGyro-meanFreq-X" "fBodyGyro-meanFreq-Y" "fBodyGyro-meanFreq-Z" "fBodyAccMag-mean"
"fBodyAccMag-meanFreq" "fBodyBodyAccJerkMag-mean" "fBodyBodyAccJerkMag-meanFreq" "fBodyBodyGyroMag-mean" "fBodyBodyGyroMag-meanFreq" "fBodyBodyGyroJerkMag-mean" 
"fBodyBodyGyroJerkMag-meanFreq" "tBodyAcc-std-X" "tBodyAcc-std-Y" "tBodyAcc-std-Z" "tGravityAcc-std-X" "tGravityAcc-std-Y" "tGravityAcc-std-Z" "tBodyAccJerk-std-X" 
"tBodyAccJerk-std-Y" "tBodyAccJerk-std-Z" "tBodyGyro-std-X" "tBodyGyro-std-Y" "tBodyGyro-std-Z" "tBodyGyroJerk-std-X" "tBodyGyroJerk-std-Y" "tBodyGyroJerk-std-Z" 
"tBodyAccMag-std" "tGravityAccMag-std" "tBodyAccJerkMag-std" "tBodyGyroMag-std" "tBodyGyroJerkMag-std" "fBodyAcc-std-X" "fBodyAcc-std-Y" "fBodyAcc-std-Z" "fBodyAccJerk-std-X" 
"fBodyAccJerk-std-Y" "fBodyAccJerk-std-Z" "fBodyGyro-std-X" "fBodyGyro-std-Y" "fBodyGyro-std-Z" "fBodyAccMag-std" "fBodyBodyAccJerkMag-std" "fBodyBodyGyroMag-std" 
"fBodyBodyGyroJerkMag-std"

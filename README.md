<<<<<<< HEAD
====================================================================================================================================================================
### Final project for Getting and cleaning data course####################
### by Rebecca Kirstein, M.ED, C.P.E. NCSU ###############################

I used commentting throughout my code that explains what will happen. I ran the code from the UCIHAR Dataset folder that was withn the getdata-projectfiles-UCICHAR Datataset zip file.

### Libraries used
library(dplyr)
library(reshape2)

###Read in my data for train
training_X <- read.table('train/X_train.txt', header=F, fill=T)
training_Y <- read.table('train/Y_train.txt', header=F, fill=T)
subject_train <- read.table('train/subject_train.txt', header=F, fill=T)

###Read in my data for test
test_X <- read.table('test/X_test.txt', header=F, fill=T)
test_Y <- read.table('test/y_test.txt', header=F, fill=T)
subject_trainTest <- read.table('test/subject_test.txt', header=F, fill=T)

###Read in features 
features <- read.table('features.txt', header=F, fill=T)

###create col variable names for training_X and test_X
names <- sub("\\(\\)", "", features$V2) ## removes () from V2 of features
colnames(training_X) <- names
colnames(test_X) <- names

###create col variable names for training_Y = activity, subjectTrain = id, subjectTrainTest = id
colnames(training_Y) <- c("activity")
colnames(test_Y) <- c("activity")
colnames(subject_train) <- c("id")
colnames(subject_trainTest) <- c("id")

###combine the three datasets from training and test by column into two datasets respectively
comboX <- cbind (training_Y, subject_train,training_X)

comboY <- cbind (test_Y, subject_trainTest,test_X)

###merge the two new datasets from comboX and combY by row into one dataset, combo 
combo <- rbind(comboX,comboY) 

###create a new dataset that only has the required col
slimCombo <- combo[, grep("mean", (colnames(combo)))]
slimCombo2 <- combo[, grep("std", (colnames(combo)))]
myCombo <- cbind(combo[, c("activity", "id")], slimCombo, slimCombo2)

### change activity values from numbers to actual values in text file:
## 1 WALKING
## 2 WALKING_UPSTAIRS
## 3 WALKING_DOWNSTAIRS
## 4 SITTING
## 5 STANDING
## 6 LAYING

myCombo$activity[myCombo$activity == 1] <- "WALKING"
myCombo$activity[myCombo$activity == 2] <- "WALKING_UPSTAIRS"
myCombo$activity[myCombo$activity == 3] <- "WALKING_DOWNSTAIRS"
myCombo$activity[myCombo$activity == 4] <- "SITTING"
myCombo$activity[myCombo$activity == 5] <- "STANDING"
myCombo$activity[myCombo$activity == 6] <- "LAYING"

### now, to part 5, new tidy data with the ave of each var for each activity and each subject
tidy_Data_summary <- myCombo %>% 
        group_by(id, activity) %>%
        summarise_each(funs(mean(., na.rm = TRUE)))

###output tidy_Data_summary to my final product tidy_data.txt
write.table(tidy_Data_summary, file ="tidy_data.txt")
=======
# Final-Project-GACD
Final project for Getting and Cleaning Data Coursera Course
>>>>>>> 6c4a593dada17ab9f51c83e26044b99bd4f9b0e7

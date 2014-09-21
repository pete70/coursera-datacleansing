#Introduction
This output dataset is created from multiple numerical observations obtained during an experiment where individuals (referred onwards as 'subjects') were wearing smartphones and sensor data was recorded during various activities. Specifically, the values were recorded from smartphone's accelerometer and gyroscope sensors. Various transformations were performed on the raw data before it was taken input in the run_analysis.R script.

#Input
The following files were utilized as input:

|File|Usage|
|---|---|
|features.txt|The names of features were extracted from this file.|
|activity_labels.txt|The names of six (6) activites were extracted from this file.|
|train/y_train.txt and test/y_test.txt|These file contained the activity index for each data value (training and test)|
|train/subject_train.txt and test/subject_test.txt|These files contained the Subject index for each data value (training and test)|
|train/X_train.txt and test/X_test.txt|These files contained the actual transformed sensor data for each feature (training and test)|


#Method
1. The R script (run_analysis.R) loaded the data in test and training files and merged them using rbind function. 
2. The next step was give proper column names from features.txt file. Although it was Task #4 of the project, it was done ahead of time to facilitate Task #2.
3. Next subsetting technique was used to take extract columns which have word "mean" or "std" in them. This created two new data frames called allMeans and allStdev (representing all columns which contain means and all columns which contain standard deviation).
4. 



#Output Data Layout

|Column|Description|
|----|----|
|Subject|The identifier of Subject for which the observations were recorded. These range for 1 to 30.|
|ActivityName|The names of activities during which observations were taken. These include six (6) activites: WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, and LAYING.|
|feature|The names of various features of data observed. These are names of transformed values as given in input, but are renamed for more readability.|
|meanOfFeature|The average of each value of feature observed for given subject and activity.|


###Main Code
Following is sequential outline of code in R script.

#####Task 1. Merges the training and the test sets to create one data set.
  * Load observation data
  * Merge train and test
  * Release memory

#####Task 4. Appropriately labels the data set with descriptive variable names. 
This task is executed ahead of given order to help Task #2.
  * Apply feature names as column names

#####Task 2. Extracts only the measurements on the mean and standard deviation for each measurement.
  * Subset the columns with means into allMeans data frame
  * Subset the columns with standard deviation into allStdev data frame

#####Task 3. Uses descriptive activity names to name the activities in the data set
  
  * Load activity names and numbers
  * Attach activity names to the merged dataset
  * Release memory

#####Task 5. From the data set in previous step, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

  * Attach subjects to merged dataframe
  * Use aggregate function to group data by Subject and Activity 
  * Make the data tidy. Output: 4 columns: Subject | ActivityName  | feature | meanOfFeature
  * Release memory
  * Save output to file for final submission
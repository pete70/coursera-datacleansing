#Introduction
This output dataset is created from multiple numerical observations obtained during an experiment where individuals (referred onwards as 'subjects') were wearing smartphones and sensor data was recorded during various activities. Specifically, the values were recorded from smartphone's accelerometer and gyroscope sensors. Various transformations were performed on the raw data before it was taken input in the run_analysis.R script.

#Objective
Following are the objectives as given by the instructor.
* Merge the training and the test sets to create one data set.
* Extract only the measurements on the mean and standard deviation for each measurement.
* Use descriptive activity names to name the activities in the data set
* Appropriately label the data set with descriptive variable names.
* From the data set in previous step, create a second, independent tidy data set with the average of each variable for each activity and each subject.


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
The R script utilized the following methods and steps to accomplish the task.

**Loading**:

* The R script (run_analysis.R) loaded the data in test and training files and merged them using rbind function. 
* The next step was give proper column names from features.txt file. Although it was Task #4 of the project, it was done ahead of time to facilitate Task #2.

**Extraction**:

* Next, subsetting technique was used to take extract columns which have word "mean" or "std" in them. This created two new data frames called allMeans and allStdev (representing all columns which contain means and all columns which contain standard deviation).

**Enrichment**:

* Next, the script loads the list of activities and corresponding index (factors) of activity for each data value. Factors are created and their textual representation is then merged in the ongoing dataset.
* The script loads the subjects and corresponding index (factors) of activity for each data value. These subject ids are then merged in the ongoing dataset.

**Aggregation**
* Data is aggregated to list mean of each feature observation for every subject and activity.

**Transformation**
* Using tidyr, the data is transformed in a 'normalized' form where features are converted from columns to rows.


#Output 
Following is the layout of output file.

|Column|Description|
|----|----|
|Subject|The identifier of Subject for which the observations were recorded. These range for 1 to 30.|
|ActivityName|The names of activities during which observations were taken. These include six (6) activites: WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, and LAYING.|
|feature|The names of various features of data observed. These are names of transformed values as given in input, but are renamed for more readability.|
|meanOfFeature|The average of each value of feature observed for given subject and activity.|


#Abbreviations used
Following abberviations are used in 'feature' column.

|Abbreviation|Meaning|
|---|---|
|mean| Mean value|
|std| Standard deviation|
|mad| Median absolute deviation |
|max| Largest value in array|
|min| Smallest value in array|
|sma| Signal magnitude area|
|energy| Energy measure. Sum of the squares divided by the number of values. |
|iqr| Interquartile range |
|entropy| Signal entropy|
|arCoeff| Autorregresion coefficients with Burg order equal to 4|
|correlation| correlation coefficient between two signals|
|maxInds| index of the frequency component with largest magnitude|
|meanFreq| Weighted average of the frequency components to obtain a mean frequency|
|skewness| skewness of the frequency domain signal |
|kurtosis| kurtosis of the frequency domain signal |
|bandsEnergy| Energy of a frequency interval within the 64 bins of the FFT of each window.|
|angle| Angle between to vectors.|

#Outline of Code
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
  

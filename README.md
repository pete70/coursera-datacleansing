#Files:
|File|Purpose|
|---|---|
|1. README.md| This file. Contains overview of code|
|2. run_analysis.R|R code for solving the project|
|3. codebook.md|Data dictionary for the R output file.|

#Description of R code:

##Packages
* sqldf: For fast data loading
* tidyr: For making tidy data

##Code Organization
The code is broken in two areas. 
* Utility Functions - functions for data loading, etc.
* Main Code - the code which solves the given 5 tasks of the project
 

###Utility Functions

|Function|Purpose|
|--------|-------|
|loadFeatureNames|This function loads the features (variable) labels and makes them readable - without sacrificing compactness for further computations.|
|loadActivityNames|This function load activity names e.g. WALKING, SITTING, etc.|
|loadActivityNumbers|This function loads the activity index for each data value (training and test), and returns a combined set.|
|loadSubjects|This function loads the Subject index for each data value (training and test), and returns a combined set.|
|loadObs_Slow|This function loads the fixed width data using base R utility function read.fwf.|
|loadObs_Fast|This function loads the fixed width data using sqldf package.|

#####Quick note on performance of data loading:
The base R function read.fwf is significantly slower compare to the sqldf. Thus function loadObs_Slow is only included here for reference.
Performance benchmark on Linux: 

|Method|Time taken|Notes|
|------|----------|-----|
|read.fwf|140 seconds|Time take to load train file with optimized buffer size of 500|
|sqldf|20 seconds|Time take to load train file|

###Main Code
Following is the layout of main code.

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
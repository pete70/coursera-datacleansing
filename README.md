#Files:
1. README.md (This file)
2. run_analysis.R (R code for solving the project)
3. codebook.md (Data dictionary for the R output file)

#Description of R code:

##Packages
* sqldf: For fast data loading
* tidyr: For making tidy data

##Code Organization
The code is broken in two areas. 
* Utility Functions - functions for data loading, etc.
* Main Code - the code which solves the given 5 tasks of the project
 

### U T I L I T Y  F U N C T I O N S 

|Function|Purpose|
|--------|-------|
|loadFeatureNames|This function loads the features (variable) labels and makes them readable - without sacrificing compactness for further computations.|
|loadActivityNames|This function load activity names e.g. WALKING, SITTING, etc.|

# This function loads the activity index for each data value (training and test), and returns a combined set
loadActivityNumbers <- function(){
  f1 <- read.table('train/y_train.txt')
  f2 <- read.table('test/y_test.txt')
  rbind(f1,f2)
}

# This function loads the Subject index for each data value (training and test), and returns a combined set
loadSubjects <- function(){
  f1 <- read.table('train/subject_train.txt')
  f2 <- read.table('test/subject_test.txt')
  rbind(f1,f2)
}

# This function loads the fixed width data using base R utility function read.fwf. 
# This is slow compare to the alternative, and only included here for reference.
# Performance benchmark on Linux: 
#     read.fwf    time take to load train file: 140 seconds (with optimized buffer size of 500)
#     sqldf       time take to load train file: 20 seconds
loadObs_Slow <- function(filePath)
{
  read.fwf(filePath, rep(c(-1,15),561), buffersize=500)
}

# This function loads the fixed width data using sqldf package. 
# This is slow compare to the alternative, and only included here for reference.
loadObs_Fast <- function(filePath)
{
  file_train <- file(filePath)
  
  sqlc <- paste("create table datatab(", paste(paste("f", seq(1,561), " real ", sep=""), collapse=","), ")")              
  sqld <- paste(paste("substr(V1,",seq(2,8962,16),",15)", sep=""),collapse=",")
  sqld <- paste("insert into datatab select", sqld, "from file_train") 
  attr(file_train, "file.format") <- list(sep = ";")
  train <- sqldf(c(sqlc,sqld, "select * from datatab"), dbname =tempfile(), file.format = list(header=F, sep=" ", eol="\n") )
  close(file_train)
  train
}


#================================
#     M A I N   C O D E 
#================================

#Task 1. Merges the training and the test sets to create one data set.
#Task 2. Extracts only the measurements on the mean and standard deviation for each measurement.
#Task 3. Uses descriptive activity names to name the activities in the data set
#Task 4. Appropriately labels the data set with descriptive variable names.
#Task 5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

#================================
#Task 1. Merges the training and the test sets to create one data set.

  traindata <- loadObs_Fast("train/X_train.txt")
  testdata <- loadObs_Fast("test/X_test.txt")

  # Merge train and test
  merged <- rbind(traindata, testdata)

  # Release memory
  rm(testdata)
  rm(traindata)


#================================
#Task 4. Appropriately labels the data set with descriptive variable names. (Executed ahead to help Task #2)

  # Apply feature names as column names
  colnames(merged) <- loadFeatureNames()


#================================
#Task 2. Extracts only the measurements on the mean and standard deviation for each measurement.

  allMeans <- subset(merged, select = grepl("mean", names(merged)))
  allStdev <- subset(merged, select = grepl("std", names(merged))) 


#================================
#Task 3. Uses descriptive activity names to name the activities in the data set
  
  # load activity names and numbers
  aNames <- loadActivityNames()
  aNum <- loadActivityNumbers()
  aNum.f <- factor(aNum[[1]], labels=aNames)
  
  #attach activity names to the merged dataset
  merged <- cbind(aNum.f, merged)
  colnames(merged)[1] <- "Activity-Name"

  # Release memory
  rm(aNames)
  rm(aNum)
  rm(aNum.f)

#================================
#Task 5. From the data set in previous step, creates a second, independent tidy data set 
#         with the average of each variable for each activity and each subject.

  # attach subjects to merged dataframe
  sub <- loadSubjects()
  merged <- cbind(sub, merged)
  colnames(merged)[1] <- "Subject"

  # cleanup memory
  rm(sub)

  # group by
  #groups <- group_by(m, "Subject", "Activity-Name")
  #summarise(groups, mean(m[,1]))
  messydata <- aggregate(merged[,3:563],list(merged$"Subject",merged$"Activity-Name"), mean)
  colnames(messydata)[1:2] <- c("Subject", "ActivityName")
    
  # Make the data tidy
  # output will be: 4 columns: Subject | ActivityName  | feature | meanOfFeature
  tidydata <-gather(messydata, feature, meanOfFeature, -Subject, -ActivityName)

  # Release memory
  rm(merged)
  rm(messydata)

  # Save output to file for final submission
  write.csv(tidydata, file="tidydata.txt", row.names=F)

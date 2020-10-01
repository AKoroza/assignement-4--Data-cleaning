### Getting and Cleaning Data Assigement

Assigement description
----------------------

The purpose of this project is to demonstrate your ability to collect,
work with, and clean a data set. Review criteria less The submitted data
set is tidy. The Github repo contains the required scripts. GitHub
contains a code book that modifies and updates the available codebooks
with the data to indicate all the variables and summaries calculated,
along with units, and any other relevant information. The README that
explains the analysis files is clear and understandable. The work
submitted for this project is the work of the student who submitted it.

The purpose of this project is to demonstrate your ability to collect,
work with, and clean a data set. The goal is to prepare tidy data that
can be used for later analysis. You will be graded by your peers on a
series of yes/no questions related to the project. You will be required
to submit: 1) a tidy data set as described below, 2) a link to a Github
repository with your script for performing the analysis, and 3) a code
book that describes the variables, the data, and any transformations or
work that you performed to clean up the data called CodeBook.md. You
should also include a README.md in the repo with your scripts. This repo
explains how all of the scripts work and how they are connected.

One of the most exciting areas in all of data science right now is
wearable computing - see for example this article . Companies like
Fitbit, Nike, and Jawbone Up are racing to develop the most advanced
algorithms to attract new users. The data linked to from the course
website represent data collected from the accelerometers from the
Samsung Galaxy S smartphone. A full description is available at the site
where the data was obtained:

<a href="http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones" class="uri">http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones</a>

Here are the data for the project:

<a href="https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip" class="uri">https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip</a>

You should create one R script called run\_analysis.R that does the
following.

Merges the training and the test sets to create one data set. Extracts
only the measurements on the mean and standard deviation for each
measurement. Uses descriptive activity names to name the activities in
the data set Appropriately labels the data set with descriptive variable
names. From the data set in step 4, creates a second, independent tidy
data set with the average of each variable for each activity and each
subject.

Idenifires
----------

-   subject - integer - The ID of the test subject

\*\* 1 - 30

-   activity - string - The type of activity that they were performing
    when the measurements were taken

\*\* Walking

\*\* Walking Upstairs

\*\* Walking Downstairs

\*\* Sitting

\*\* Standing

\*\* Laying

Mean values of the Variables of corresponding measurement based on the subject and activity. Variables are numeric.
-------------------------------------------------------------------------------------------------------------------

-   tBodyAccmeanX

-   tBodyAccmeanY

-   tBodyAccmeanZ

-   tBodyAccstdX

-   tBodyAccstdY

-   tBodyAccstdZ

-   tGravityAccmeanX

-   tGravityAccmeanY

-   tGravityAccmeanZ

-   tGravityAccstdX

-   tGravityAccstdY

-   tGravityAccstdZ

-   tBodyAccJerkmeanX

-   tBodyAccJerkmeanY

-   tBodyAccJerkmeanZ

-   tBodyAccJerkstdX

-   tBodyAccJerkstdY

-   tBodyAccJerkstdZ

-   tBodyGyromeanX

-   tBodyGyromeanY

-   tBodyGyromeanZ

-   tBodyGyrostdX

-   tBodyGyrostdY

-   tBodyGyrostdZ

-   tBodyGyroJerkmeanX

-   tBodyGyroJerkmeanY

-   tBodyGyroJerkmeanZ

-   tBodyGyroJerkstdX

-   tBodyGyroJerkstdY

-   tBodyGyroJerkstdZ

-   tBodyAccMagmean

-   tBodyAccMagstd

-   tGravityAccMagmean

-   tGravityAccMagstd

-   tBodyAccJerkMagmean

-   tBodyAccJerkMagstd

-   tBodyGyroMagmean

-   tBodyGyroMagstd

-   tBodyGyroJerkMagmean

-   tBodyGyroJerkMagstd

-   fBodyAccmeanX

-   fBodyAccmeanY

-   fBodyAccmeanZ

-   fBodyAccstdX

-   fBodyAccstdY

-   fBodyAccstdZ

-   fBodyAccJerkmeanX

-   fBodyAccJerkmeanY

-   fBodyAccJerkmeanZ

-   fBodyAccJerkstdX

-   fBodyAccJerkstdY

-   fBodyAccJerkstdZ

-   fBodyGyromeanX

-   fBodyGyromeanY

-   fBodyGyromeanZ

-   fBodyGyrostdX

-   fBodyGyrostdY

-   fBodyGyrostdZ

-   fBodyAccMagmean

-   fBodyAccMagstd

-   fBodyBodyAccJerkMagmean

-   fBodyBodyAccJerkMagstd

-   fBodyBodyGyroMagmean

-   fBodyBodyGyroMagstd

-   fBodyBodyGyroJerkMagmean

-   fBodyBodyGyroJerkMagstd

Load packages
-------------

    library(dplyr)

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

    library(data.table)

    ## 
    ## Attaching package: 'data.table'

    ## The following objects are masked from 'package:dplyr':
    ## 
    ##     between, first, last

    library(tidyr)

1. download and unzip data
--------------------------

    if(!file.exists("./data")){dir.create("./data")}
    fileUrl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
    download.file(fileUrl,destfile="./data/Dataset.zip",method="curl")

Unzip DataSet to /data directory
--------------------------------

    unzip(zipfile="./data/Dataset.zip",exdir="./data")
    filesPath <- "/Users/olisza/Desktop/Coursera R/assignement-4--Data-cleaning/data/UCI HAR Dataset"
    setwd(filesPath)

Read the above files and create data tables
-------------------------------------------

Read subject files
==================

    dataSubjectTrain <- tbl_df(read.table(file.path(filesPath, "train", "subject_train.txt")))

    ## Warning: `tbl_df()` is deprecated as of dplyr 1.0.0.
    ## Please use `tibble::as_tibble()` instead.
    ## This warning is displayed once every 8 hours.
    ## Call `lifecycle::last_warnings()` to see where this warning was generated.

    dataSubjectTest  <- tbl_df(read.table(file.path(filesPath, "test" , "subject_test.txt" )))

Read activity files
===================

    dataActivityTrain <- tbl_df(read.table(file.path(filesPath, "train", "y_train.txt")))
    dataActivityTest  <- tbl_df(read.table(file.path(filesPath, "test" , "y_test.txt" )))

\#Read data files

    dataTrain <- tbl_df(read.table(file.path(filesPath, "train", "X_train.txt" )))
    dataTest  <- tbl_df(read.table(file.path(filesPath, "test" , "X_test.txt" )))

merge the training and test sets to create one data set for both Activity and Subject files this will merge the training and the test sets by row binding and rename variables “subject” and “activityNum”
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

    alldataSubject <- rbind(dataSubjectTrain, dataSubjectTest)
    setnames(alldataSubject, "V1", "subject")
    alldataActivity<- rbind(dataActivityTrain, dataActivityTest)
    setnames(alldataActivity, "V1", "activityNum")

\#combine the DATA training and test files

    dataTable <- rbind(dataTrain, dataTest)

name variables according to feature e.g.(V1 = “tBodyAcc-mean()-X”)
==================================================================

    dataFeatures <- tbl_df(read.table(file.path(filesPath, "features.txt")))
    setnames(dataFeatures, names(dataFeatures), c("featureNum", "featureName"))
    colnames(dataTable) <- dataFeatures$featureName

\#column names for activity labels

    activityLabels<- tbl_df(read.table(file.path(filesPath, "activity_labels.txt")))
    setnames(activityLabels, names(activityLabels), c("activityNum","activityName"))

Merge columns
=============

    alldataSubjAct<- cbind(alldataSubject, alldataActivity)
    dataTable <- cbind(alldataSubjAct, dataTable)

extract only measurements on the mean and standard deviation for each measurment
--------------------------------------------------------------------------------

enter name of activity into dataTable
-------------------------------------

Reading “features.txt” and extracting only the mean and standard deviation
==========================================================================

    dataFeaturesMeanStd <- grep("mean\\(\\)|std\\(\\)",dataFeatures$featureName,value=TRUE) 

Taking only measurements for the mean and standard deviation and add “subject”,“activityNum”
============================================================================================

    dataFeaturesMeanStd <- union(c("subject","activityNum"), dataFeaturesMeanStd)
    dataTable<- subset(dataTable,select=dataFeaturesMeanStd) 

Use descriptive activity names to name the activities in the data
-----------------------------------------------------------------

\#\#enter name of activity into dataTable

    dataTable <- merge(activityLabels, dataTable , by="activityNum", all.x=TRUE)
    dataTable$activityName <- as.character(dataTable$activityName)

create dataTable with variable means sorted by subject and Activity, appropriately labels the data set with descriptive variable names
--------------------------------------------------------------------------------------------------------------------------------------

    dataTable$activityName <- as.character(dataTable$activityName)
    dataAggr<- aggregate(. ~ subject - activityName, data = dataTable, mean) 
    dataTable<- tbl_df(arrange(dataAggr,subject,activityName))

check data
----------

    head(str(dataTable),2)

    ## tibble [180 × 69] (S3: tbl_df/tbl/data.frame)
    ##  $ subject                    : int [1:180] 1 1 1 1 1 1 2 2 2 2 ...
    ##  $ activityName               : chr [1:180] "LAYING" "SITTING" "STANDING" "WALKING" ...
    ##  $ activityNum                : num [1:180] 6 4 5 1 3 2 6 4 5 1 ...
    ##  $ tBodyAcc-mean()-X          : num [1:180] 0.222 0.261 0.279 0.277 0.289 ...
    ##  $ tBodyAcc-mean()-Y          : num [1:180] -0.04051 -0.00131 -0.01614 -0.01738 -0.00992 ...
    ##  $ tBodyAcc-mean()-Z          : num [1:180] -0.113 -0.105 -0.111 -0.111 -0.108 ...
    ##  $ tBodyAcc-std()-X           : num [1:180] -0.928 -0.977 -0.996 -0.284 0.03 ...
    ##  $ tBodyAcc-std()-Y           : num [1:180] -0.8368 -0.9226 -0.9732 0.1145 -0.0319 ...
    ##  $ tBodyAcc-std()-Z           : num [1:180] -0.826 -0.94 -0.98 -0.26 -0.23 ...
    ##  $ tGravityAcc-mean()-X       : num [1:180] -0.249 0.832 0.943 0.935 0.932 ...
    ##  $ tGravityAcc-mean()-Y       : num [1:180] 0.706 0.204 -0.273 -0.282 -0.267 ...
    ##  $ tGravityAcc-mean()-Z       : num [1:180] 0.4458 0.332 0.0135 -0.0681 -0.0621 ...
    ##  $ tGravityAcc-std()-X        : num [1:180] -0.897 -0.968 -0.994 -0.977 -0.951 ...
    ##  $ tGravityAcc-std()-Y        : num [1:180] -0.908 -0.936 -0.981 -0.971 -0.937 ...
    ##  $ tGravityAcc-std()-Z        : num [1:180] -0.852 -0.949 -0.976 -0.948 -0.896 ...
    ##  $ tBodyAccJerk-mean()-X      : num [1:180] 0.0811 0.0775 0.0754 0.074 0.0542 ...
    ##  $ tBodyAccJerk-mean()-Y      : num [1:180] 0.003838 -0.000619 0.007976 0.028272 0.02965 ...
    ##  $ tBodyAccJerk-mean()-Z      : num [1:180] 0.01083 -0.00337 -0.00369 -0.00417 -0.01097 ...
    ##  $ tBodyAccJerk-std()-X       : num [1:180] -0.9585 -0.9864 -0.9946 -0.1136 -0.0123 ...
    ##  $ tBodyAccJerk-std()-Y       : num [1:180] -0.924 -0.981 -0.986 0.067 -0.102 ...
    ##  $ tBodyAccJerk-std()-Z       : num [1:180] -0.955 -0.988 -0.992 -0.503 -0.346 ...
    ##  $ tBodyGyro-mean()-X         : num [1:180] -0.0166 -0.0454 -0.024 -0.0418 -0.0351 ...
    ##  $ tBodyGyro-mean()-Y         : num [1:180] -0.0645 -0.0919 -0.0594 -0.0695 -0.0909 ...
    ##  $ tBodyGyro-mean()-Z         : num [1:180] 0.1487 0.0629 0.0748 0.0849 0.0901 ...
    ##  $ tBodyGyro-std()-X          : num [1:180] -0.874 -0.977 -0.987 -0.474 -0.458 ...
    ##  $ tBodyGyro-std()-Y          : num [1:180] -0.9511 -0.9665 -0.9877 -0.0546 -0.1263 ...
    ##  $ tBodyGyro-std()-Z          : num [1:180] -0.908 -0.941 -0.981 -0.344 -0.125 ...
    ##  $ tBodyGyroJerk-mean()-X     : num [1:180] -0.1073 -0.0937 -0.0996 -0.09 -0.074 ...
    ##  $ tBodyGyroJerk-mean()-Y     : num [1:180] -0.0415 -0.0402 -0.0441 -0.0398 -0.044 ...
    ##  $ tBodyGyroJerk-mean()-Z     : num [1:180] -0.0741 -0.0467 -0.049 -0.0461 -0.027 ...
    ##  $ tBodyGyroJerk-std()-X      : num [1:180] -0.919 -0.992 -0.993 -0.207 -0.487 ...
    ##  $ tBodyGyroJerk-std()-Y      : num [1:180] -0.968 -0.99 -0.995 -0.304 -0.239 ...
    ##  $ tBodyGyroJerk-std()-Z      : num [1:180] -0.958 -0.988 -0.992 -0.404 -0.269 ...
    ##  $ tBodyAccMag-mean()         : num [1:180] -0.8419 -0.9485 -0.9843 -0.137 0.0272 ...
    ##  $ tBodyAccMag-std()          : num [1:180] -0.7951 -0.9271 -0.9819 -0.2197 0.0199 ...
    ##  $ tGravityAccMag-mean()      : num [1:180] -0.8419 -0.9485 -0.9843 -0.137 0.0272 ...
    ##  $ tGravityAccMag-std()       : num [1:180] -0.7951 -0.9271 -0.9819 -0.2197 0.0199 ...
    ##  $ tBodyAccJerkMag-mean()     : num [1:180] -0.9544 -0.9874 -0.9924 -0.1414 -0.0894 ...
    ##  $ tBodyAccJerkMag-std()      : num [1:180] -0.9282 -0.9841 -0.9931 -0.0745 -0.0258 ...
    ##  $ tBodyGyroMag-mean()        : num [1:180] -0.8748 -0.9309 -0.9765 -0.161 -0.0757 ...
    ##  $ tBodyGyroMag-std()         : num [1:180] -0.819 -0.935 -0.979 -0.187 -0.226 ...
    ##  $ tBodyGyroJerkMag-mean()    : num [1:180] -0.963 -0.992 -0.995 -0.299 -0.295 ...
    ##  $ tBodyGyroJerkMag-std()     : num [1:180] -0.936 -0.988 -0.995 -0.325 -0.307 ...
    ##  $ fBodyAcc-mean()-X          : num [1:180] -0.9391 -0.9796 -0.9952 -0.2028 0.0382 ...
    ##  $ fBodyAcc-mean()-Y          : num [1:180] -0.86707 -0.94408 -0.97707 0.08971 0.00155 ...
    ##  $ fBodyAcc-mean()-Z          : num [1:180] -0.883 -0.959 -0.985 -0.332 -0.226 ...
    ##  $ fBodyAcc-std()-X           : num [1:180] -0.9244 -0.9764 -0.996 -0.3191 0.0243 ...
    ##  $ fBodyAcc-std()-Y           : num [1:180] -0.834 -0.917 -0.972 0.056 -0.113 ...
    ##  $ fBodyAcc-std()-Z           : num [1:180] -0.813 -0.934 -0.978 -0.28 -0.298 ...
    ##  $ fBodyAccJerk-mean()-X      : num [1:180] -0.9571 -0.9866 -0.9946 -0.1705 -0.0277 ...
    ##  $ fBodyAccJerk-mean()-Y      : num [1:180] -0.9225 -0.9816 -0.9854 -0.0352 -0.1287 ...
    ##  $ fBodyAccJerk-mean()-Z      : num [1:180] -0.948 -0.986 -0.991 -0.469 -0.288 ...
    ##  $ fBodyAccJerk-std()-X       : num [1:180] -0.9642 -0.9875 -0.9951 -0.1336 -0.0863 ...
    ##  $ fBodyAccJerk-std()-Y       : num [1:180] -0.932 -0.983 -0.987 0.107 -0.135 ...
    ##  $ fBodyAccJerk-std()-Z       : num [1:180] -0.961 -0.988 -0.992 -0.535 -0.402 ...
    ##  $ fBodyGyro-mean()-X         : num [1:180] -0.85 -0.976 -0.986 -0.339 -0.352 ...
    ##  $ fBodyGyro-mean()-Y         : num [1:180] -0.9522 -0.9758 -0.989 -0.1031 -0.0557 ...
    ##  $ fBodyGyro-mean()-Z         : num [1:180] -0.9093 -0.9513 -0.9808 -0.2559 -0.0319 ...
    ##  $ fBodyGyro-std()-X          : num [1:180] -0.882 -0.978 -0.987 -0.517 -0.495 ...
    ##  $ fBodyGyro-std()-Y          : num [1:180] -0.9512 -0.9623 -0.9871 -0.0335 -0.1814 ...
    ##  $ fBodyGyro-std()-Z          : num [1:180] -0.917 -0.944 -0.982 -0.437 -0.238 ...
    ##  $ fBodyAccMag-mean()         : num [1:180] -0.8618 -0.9478 -0.9854 -0.1286 0.0966 ...
    ##  $ fBodyAccMag-std()          : num [1:180] -0.798 -0.928 -0.982 -0.398 -0.187 ...
    ##  $ fBodyBodyAccJerkMag-mean() : num [1:180] -0.9333 -0.9853 -0.9925 -0.0571 0.0262 ...
    ##  $ fBodyBodyAccJerkMag-std()  : num [1:180] -0.922 -0.982 -0.993 -0.103 -0.104 ...
    ##  $ fBodyBodyGyroMag-mean()    : num [1:180] -0.862 -0.958 -0.985 -0.199 -0.186 ...
    ##  $ fBodyBodyGyroMag-std()     : num [1:180] -0.824 -0.932 -0.978 -0.321 -0.398 ...
    ##  $ fBodyBodyGyroJerkMag-mean(): num [1:180] -0.942 -0.99 -0.995 -0.319 -0.282 ...
    ##  $ fBodyBodyGyroJerkMag-std() : num [1:180] -0.933 -0.987 -0.995 -0.382 -0.392 ...

    ## NULL

    head(str(dataTable),6)

    ## tibble [180 × 69] (S3: tbl_df/tbl/data.frame)
    ##  $ subject                    : int [1:180] 1 1 1 1 1 1 2 2 2 2 ...
    ##  $ activityName               : chr [1:180] "LAYING" "SITTING" "STANDING" "WALKING" ...
    ##  $ activityNum                : num [1:180] 6 4 5 1 3 2 6 4 5 1 ...
    ##  $ tBodyAcc-mean()-X          : num [1:180] 0.222 0.261 0.279 0.277 0.289 ...
    ##  $ tBodyAcc-mean()-Y          : num [1:180] -0.04051 -0.00131 -0.01614 -0.01738 -0.00992 ...
    ##  $ tBodyAcc-mean()-Z          : num [1:180] -0.113 -0.105 -0.111 -0.111 -0.108 ...
    ##  $ tBodyAcc-std()-X           : num [1:180] -0.928 -0.977 -0.996 -0.284 0.03 ...
    ##  $ tBodyAcc-std()-Y           : num [1:180] -0.8368 -0.9226 -0.9732 0.1145 -0.0319 ...
    ##  $ tBodyAcc-std()-Z           : num [1:180] -0.826 -0.94 -0.98 -0.26 -0.23 ...
    ##  $ tGravityAcc-mean()-X       : num [1:180] -0.249 0.832 0.943 0.935 0.932 ...
    ##  $ tGravityAcc-mean()-Y       : num [1:180] 0.706 0.204 -0.273 -0.282 -0.267 ...
    ##  $ tGravityAcc-mean()-Z       : num [1:180] 0.4458 0.332 0.0135 -0.0681 -0.0621 ...
    ##  $ tGravityAcc-std()-X        : num [1:180] -0.897 -0.968 -0.994 -0.977 -0.951 ...
    ##  $ tGravityAcc-std()-Y        : num [1:180] -0.908 -0.936 -0.981 -0.971 -0.937 ...
    ##  $ tGravityAcc-std()-Z        : num [1:180] -0.852 -0.949 -0.976 -0.948 -0.896 ...
    ##  $ tBodyAccJerk-mean()-X      : num [1:180] 0.0811 0.0775 0.0754 0.074 0.0542 ...
    ##  $ tBodyAccJerk-mean()-Y      : num [1:180] 0.003838 -0.000619 0.007976 0.028272 0.02965 ...
    ##  $ tBodyAccJerk-mean()-Z      : num [1:180] 0.01083 -0.00337 -0.00369 -0.00417 -0.01097 ...
    ##  $ tBodyAccJerk-std()-X       : num [1:180] -0.9585 -0.9864 -0.9946 -0.1136 -0.0123 ...
    ##  $ tBodyAccJerk-std()-Y       : num [1:180] -0.924 -0.981 -0.986 0.067 -0.102 ...
    ##  $ tBodyAccJerk-std()-Z       : num [1:180] -0.955 -0.988 -0.992 -0.503 -0.346 ...
    ##  $ tBodyGyro-mean()-X         : num [1:180] -0.0166 -0.0454 -0.024 -0.0418 -0.0351 ...
    ##  $ tBodyGyro-mean()-Y         : num [1:180] -0.0645 -0.0919 -0.0594 -0.0695 -0.0909 ...
    ##  $ tBodyGyro-mean()-Z         : num [1:180] 0.1487 0.0629 0.0748 0.0849 0.0901 ...
    ##  $ tBodyGyro-std()-X          : num [1:180] -0.874 -0.977 -0.987 -0.474 -0.458 ...
    ##  $ tBodyGyro-std()-Y          : num [1:180] -0.9511 -0.9665 -0.9877 -0.0546 -0.1263 ...
    ##  $ tBodyGyro-std()-Z          : num [1:180] -0.908 -0.941 -0.981 -0.344 -0.125 ...
    ##  $ tBodyGyroJerk-mean()-X     : num [1:180] -0.1073 -0.0937 -0.0996 -0.09 -0.074 ...
    ##  $ tBodyGyroJerk-mean()-Y     : num [1:180] -0.0415 -0.0402 -0.0441 -0.0398 -0.044 ...
    ##  $ tBodyGyroJerk-mean()-Z     : num [1:180] -0.0741 -0.0467 -0.049 -0.0461 -0.027 ...
    ##  $ tBodyGyroJerk-std()-X      : num [1:180] -0.919 -0.992 -0.993 -0.207 -0.487 ...
    ##  $ tBodyGyroJerk-std()-Y      : num [1:180] -0.968 -0.99 -0.995 -0.304 -0.239 ...
    ##  $ tBodyGyroJerk-std()-Z      : num [1:180] -0.958 -0.988 -0.992 -0.404 -0.269 ...
    ##  $ tBodyAccMag-mean()         : num [1:180] -0.8419 -0.9485 -0.9843 -0.137 0.0272 ...
    ##  $ tBodyAccMag-std()          : num [1:180] -0.7951 -0.9271 -0.9819 -0.2197 0.0199 ...
    ##  $ tGravityAccMag-mean()      : num [1:180] -0.8419 -0.9485 -0.9843 -0.137 0.0272 ...
    ##  $ tGravityAccMag-std()       : num [1:180] -0.7951 -0.9271 -0.9819 -0.2197 0.0199 ...
    ##  $ tBodyAccJerkMag-mean()     : num [1:180] -0.9544 -0.9874 -0.9924 -0.1414 -0.0894 ...
    ##  $ tBodyAccJerkMag-std()      : num [1:180] -0.9282 -0.9841 -0.9931 -0.0745 -0.0258 ...
    ##  $ tBodyGyroMag-mean()        : num [1:180] -0.8748 -0.9309 -0.9765 -0.161 -0.0757 ...
    ##  $ tBodyGyroMag-std()         : num [1:180] -0.819 -0.935 -0.979 -0.187 -0.226 ...
    ##  $ tBodyGyroJerkMag-mean()    : num [1:180] -0.963 -0.992 -0.995 -0.299 -0.295 ...
    ##  $ tBodyGyroJerkMag-std()     : num [1:180] -0.936 -0.988 -0.995 -0.325 -0.307 ...
    ##  $ fBodyAcc-mean()-X          : num [1:180] -0.9391 -0.9796 -0.9952 -0.2028 0.0382 ...
    ##  $ fBodyAcc-mean()-Y          : num [1:180] -0.86707 -0.94408 -0.97707 0.08971 0.00155 ...
    ##  $ fBodyAcc-mean()-Z          : num [1:180] -0.883 -0.959 -0.985 -0.332 -0.226 ...
    ##  $ fBodyAcc-std()-X           : num [1:180] -0.9244 -0.9764 -0.996 -0.3191 0.0243 ...
    ##  $ fBodyAcc-std()-Y           : num [1:180] -0.834 -0.917 -0.972 0.056 -0.113 ...
    ##  $ fBodyAcc-std()-Z           : num [1:180] -0.813 -0.934 -0.978 -0.28 -0.298 ...
    ##  $ fBodyAccJerk-mean()-X      : num [1:180] -0.9571 -0.9866 -0.9946 -0.1705 -0.0277 ...
    ##  $ fBodyAccJerk-mean()-Y      : num [1:180] -0.9225 -0.9816 -0.9854 -0.0352 -0.1287 ...
    ##  $ fBodyAccJerk-mean()-Z      : num [1:180] -0.948 -0.986 -0.991 -0.469 -0.288 ...
    ##  $ fBodyAccJerk-std()-X       : num [1:180] -0.9642 -0.9875 -0.9951 -0.1336 -0.0863 ...
    ##  $ fBodyAccJerk-std()-Y       : num [1:180] -0.932 -0.983 -0.987 0.107 -0.135 ...
    ##  $ fBodyAccJerk-std()-Z       : num [1:180] -0.961 -0.988 -0.992 -0.535 -0.402 ...
    ##  $ fBodyGyro-mean()-X         : num [1:180] -0.85 -0.976 -0.986 -0.339 -0.352 ...
    ##  $ fBodyGyro-mean()-Y         : num [1:180] -0.9522 -0.9758 -0.989 -0.1031 -0.0557 ...
    ##  $ fBodyGyro-mean()-Z         : num [1:180] -0.9093 -0.9513 -0.9808 -0.2559 -0.0319 ...
    ##  $ fBodyGyro-std()-X          : num [1:180] -0.882 -0.978 -0.987 -0.517 -0.495 ...
    ##  $ fBodyGyro-std()-Y          : num [1:180] -0.9512 -0.9623 -0.9871 -0.0335 -0.1814 ...
    ##  $ fBodyGyro-std()-Z          : num [1:180] -0.917 -0.944 -0.982 -0.437 -0.238 ...
    ##  $ fBodyAccMag-mean()         : num [1:180] -0.8618 -0.9478 -0.9854 -0.1286 0.0966 ...
    ##  $ fBodyAccMag-std()          : num [1:180] -0.798 -0.928 -0.982 -0.398 -0.187 ...
    ##  $ fBodyBodyAccJerkMag-mean() : num [1:180] -0.9333 -0.9853 -0.9925 -0.0571 0.0262 ...
    ##  $ fBodyBodyAccJerkMag-std()  : num [1:180] -0.922 -0.982 -0.993 -0.103 -0.104 ...
    ##  $ fBodyBodyGyroMag-mean()    : num [1:180] -0.862 -0.958 -0.985 -0.199 -0.186 ...
    ##  $ fBodyBodyGyroMag-std()     : num [1:180] -0.824 -0.932 -0.978 -0.321 -0.398 ...
    ##  $ fBodyBodyGyroJerkMag-mean(): num [1:180] -0.942 -0.99 -0.995 -0.319 -0.282 ...
    ##  $ fBodyBodyGyroJerkMag-std() : num [1:180] -0.933 -0.987 -0.995 -0.382 -0.392 ...

    ## NULL

from data set in step 4 creates a second, independent tidy data set with the average of each variable for each activity and each subject write to text file on disk
-------------------------------------------------------------------------------------------------------------------------------------------------------------------

    write.table(dataTable,"TidyDAta.txt",row.name=FALSE)

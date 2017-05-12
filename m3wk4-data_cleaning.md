MY markdown documents
================

GitHub Documents
----------------

This is an R Markdown format used for publishing markdown documents to GitHub. When you click the **Knit** button all R code chunks are run and a markdown file (.md) suitable for publishing to GitHub is generated.

Including Code
--------------

You can include R code in the document as follows:

\`\`\`\# 1. Downloading, unzipping dataset and merge data sets \#Downloading data if(!file.exists("./data")){dir.create("./data")} fileUrl &lt;- "<https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip>" download.file(fileUrl,destfile="./data/Dataset.zip")

Unzip dataSet to /data directory
================================

unzip(zipfile="./data/Dataset.zip",exdir="./data")

Merging the training and the test sets to create one data set:
==============================================================

Reading trainings tables:
=========================

x\_train &lt;- read.table("./data/UCI HAR Dataset/train/X\_train.txt") y\_train &lt;- read.table("./data/UCI HAR Dataset/train/y\_train.txt") subject\_train &lt;- read.table("./data/UCI HAR Dataset/train/subject\_train.txt")

Reading testing tables:
=======================

x\_test &lt;- read.table("./data/UCI HAR Dataset/test/X\_test.txt") y\_test &lt;- read.table("./data/UCI HAR Dataset/test/y\_test.txt") subject\_test &lt;- read.table("./data/UCI HAR Dataset/test/subject\_test.txt")

Reading feature vector:
=======================

features &lt;- read.table('./data/UCI HAR Dataset/features.txt')

Reading activity labels:
========================

activityLabels = read.table('./data/UCI HAR Dataset/activity\_labels.txt')

colnames(x\_train) &lt;- features\[,2\] colnames(y\_train) &lt;-"activityId" colnames(subject\_train) &lt;- "subjectId"

colnames(x\_test) &lt;- features\[,2\] colnames(y\_test) &lt;- "activityId" colnames(subject\_test) &lt;- "subjectId"

colnames(activityLabels) &lt;- c('activityId','activityType')

Merging all data in one set:
============================

merge\_train &lt;- cbind(y\_train, subject\_train, x\_train) merge\_test &lt;- cbind(y\_test, subject\_test, x\_test) setAllInOne &lt;- rbind(merge\_train, merge\_test)

2. Extracting only the measurements on the mean and standard deviation for each measurement
===========================================================================================

Reading column names
====================

colNames &lt;- colnames(setAllInOne) \# Create vector for defining ID, mean and standard deviation: mean\_and\_std &lt;- (grepl("activityId" , colNames) | grepl("subjectId" , colNames) | grepl("mean.." , colNames) | grepl("std.." , colNames) )

3. Uses descriptive activity names to name the activities in the data set
=========================================================================

Making necessary subset from setAllInOne:
=========================================

setForMeanAndStd &lt;- setAllInOne\[ , mean\_and\_std == TRUE\]

4. Appropriately labels the data set with descriptive variable names.
=====================================================================

setWithActivityNames &lt;- merge(setForMeanAndStd, activityLabels, by='activityId', all.x=TRUE) \# 5. Creating a second, independent tidy data set with the average of each variable for each activity and each subject: \#Making second tidy data set secTidySet &lt;- aggregate(. ~subjectId + activityId, setWithActivityNames, mean) secTidySet &lt;- secTidySet\[order(secTidySet*s**u**b**j**e**c**t**I**d*, *s**e**c**T**i**d**y**S**e**t*activityId),\]

Writing second tidy data set in txt file
========================================

write.table(secTidySet, "secTidySet.txt", row.name=FALSE)

rmarkdown::render('m3wk4-data\_cleaning.R')

add this chunk to end of m3wk4-data\_cleaning.rmd
=================================================

file.rename(from="m3wk4-data\_cleaning.rmd", to="m3wk4-data\_cleaning.md") \`\`\`

Including Plots
---------------

Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.

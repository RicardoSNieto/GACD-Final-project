# GACD-Final-project

% Opening datasets
activity_labels <- read.table("./UCI HAR Dataset/activity_labels.txt")
features <- read.table("./UCI HAR Dataset/features.txt")
subject_test <- read.table("./UCI HAR Dataset/test/subject_test.txt")
x_test <- read.table("./UCI HAR Dataset/test/X_test.txt")
y_test <- read.table("./UCI HAR Dataset/test/y_test.txt")
subject_train <- read.table("./UCI HAR Dataset/train/subject_train.txt")
x_train <- read.table("./UCI HAR Dataset/train/X_train.txt")
y_train <- read.table("./UCI HAR Dataset/train/y_train.txt")

%Part 1 
%Merging rows
R1 <- rbind(x_train, x_test)
R2 <- rbind(y_train, y_test)
%Merging columns
Subject <- rbind(subject_train, subject_test)
fullData <- cbind(Subject, R2, R1)

%Grouping mean and std
allNames <- c("subject", "activity", as.character(features$V2))
meanStdColumns <- grep("subject|activity|[Mm]ean|std", allNames, value = FALSE)
%Extracting mean and std
reducedSet <- fullData[ ,meanStdColumns]

%Renaming
names(activity_labels) <- c("activityNumber", "activityName")
reducedSet$V1.1 <- activity_labels$activityName[reducedSet$V1.1]

reducedNames <- allNames[meanStdColumns]    
reducedNames <- gsub("mean", "Mean", reducedNames)
reducedNames <- gsub("std", "Std", reducedNames)
reducedNames <- gsub("gravity", "Gravity", reducedNames)
reducedNames <- gsub("[[:punct:]]", "", reducedNames)
reducedNames <- gsub("^t", "time", reducedNames)
reducedNames <- gsub("^f", "frequency", reducedNames)
reducedNames <- gsub("^anglet", "angleTime", reducedNames)
names(reducedSet) <- reducedNames 

%Grouping and tidying
tidyDataset <- reducedSet %>% group_by(activity, subject) %>% 
  summarise_all(list(mean = mean))

%Final dataset
write.table(tidyDataset, file = "tidyDataset.txt", row.names = FALSE

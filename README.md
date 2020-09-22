Getting and Cleaning Data Final Project

Description of run_analysis.R:

1. Reads train and test datasets.

2. Assigns variable names to each of the datasets to merge.

3. Merges datasets into r1(measurements), r2(activity) and subject

4. Obtains a reduced set with the mean and std. 

5. Setss descriptive column names

6. Extracts data by selected columns, remerges desired columns and replaces activity column to it's name by refering activity label.

7. Generates 'Tidy Dataset' with the average (mean) of each variable for each subject and each activity. 

8. Creates file tidy_dataset.txt.

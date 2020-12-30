# R-Assignment---Getting-and-Cleaning-Data-Course-Project
Where the assignment for Getting and Cleaning Data Course Project is stored

This script serves to tidy up sets of Samsung data. It works in the following way:

##defining all required tables
x_test<-read.table("UCI HAR Dataset/test/X_test.txt")
x_train<-read.table("UCI HAR Dataset/train/X_train.txt")
features<-read.table("UCI HAR Dataset/features.txt")
subject_train<-read.table("UCI HAR Dataset/train/subject_train.txt")
subject_test<-read.table("UCI HAR Dataset/test/subject_test.txt")
y_test<-read.table("UCI HAR Dataset/test/Y_test.txt")
y_train<-read.table("UCI HAR Dataset/train/Y_train.txt")
activity_labels<-read.table("UCI HAR Dataset/activity_labels.txt")

##combining all variables of both groups (adding new columns)
x_train2<-cbind(x_train,y_train,subject_train)
x_test2<-cbind(x_test,y_test,subject_test)

##combining both train and test observations (add on in rows)
x_all<-rbind(x_train2,x_test2)

##Naming the columns in new table
i=1
list=NULL
for(i in 1:561){
  list <- c(list,features[i,2])
}
list<-c(list,"Activity_Class","SubjectNew")
colnames(x_all)<-list

##Only take columns of mean and standard deviation
takelist <- c(grep("mean" ,names(x_all)),grep("std" ,names(x_all)),grep("Activity_Class" ,names(x_all)),grep("SubjectNew" ,names(x_all)))
usethistable <- x_all[takelist]

##Providing activity name and subject
combine <- merge(usethistable,activity_labels,by.x="Activity_Class",by.y="V1")
newcolnamelist <- colnames(combine)
newcolnamelist <- c(newcolnamelist[1:(ncol(combine)-1)],"Activity_Type")
colnames(combine)<-newcolnamelist

##find means grouped by activity type, then subject
final<-aggregate(combine[2:80],list(combine$Activity_Type,combine$SubjectNew),mean)
final

#install R postgreSQL package for db connectivity/querying
install.packages("RPostgreSQL")

#load the package you just installed
library(RPostgreSQL)

#load the drivers and create the connection string
drv <- dbDriver("PostgreSQL")
dbname <- "kaggle"; dbuser <- "admin"; dbpass <- "<obscured>";
dbhost <- "obscuredhost.com"; dbport <- 11111;
con <- dbConnect(drv, host=dbhost, port=dbport, dbname=dbname, user=dbuser, password=dbpass)

#Set working directory
setwd("~/Dropbox/Kaggle/titanic")

#Import files & set file vars
train <- read.csv("~/Dropbox/Kaggle/titanic/train.csv")
test <- read.csv("~/Dropbox/Kaggle/titanic/test.csv")

## Add survived column to test set with value of 0--in order to combine test & train data must match
test$Survived <- rep(0, 418)
combi <- rbind(train, test)

## insert training data into a table
dbWriteTable(con, "titanic", train, row.names=FALSE)

###############################

###### Data exploration ######

##############################


#query data from database--one variable @ a time
#fetch = put results in data frame; -1=fetch all results
#make sure you use double quotes since the table will be created with double quotes for col names
train <- fetch(dbSendQuery(con, 'SELECT * from titanic'), n=100)

#make R treat age as numeric and survived as a factor (it can't be a numeric range)
train$Age <- as.numeric(train$Age)
train$Survived <- as.factor(train$Survived)

#start creating barplots to see where there might be indicators
age <- table(train$Survived, train$Age)
barplot(age, xlab="Passenger Age", col=c("red","green"), legend = rownames(age))

class <- table(train$Survived, train$Pclass)
barplot(class, xlab="Passenger Class", col=c("red","green"), legend = rownames(class))

sex <- table(train$Survived, train$Sex)
barplot(sex, xlab="Passenger Sex", col=c("red","green"), legend = rownames(sex))

#keep exploring other variables!

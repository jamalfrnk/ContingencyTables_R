# Descriptive Statistics_R
# Malcolm Frank 
# Software Engineer, Infinitely Deep Robotics Group, 2018

## Project Title: Summarizing categorical data using contingency tables

### Overview: 
In this walkthrough, we will learn a few tools for summarzing categorical data using contingency tables. Each table in this repo was generated using R Studio. 

### Purpose:
We want to examine the following key features associated with categorical data:
     `Frequencies - The number of observations for a particular category`
     `Proportions - The percent that each category accounts for out of the whole`
     `Marginals - The totals in a cross tabulation by row or column`
     `Visualization - Wholistic understanding of the aforementioned features of the data through statistics and graphical representations` 
    
#### Replication Requirements
Input the following commands into RStudio to download the required packages for this walkthrough:
`library(readxl) # for reading in excel sheets` 
`library(ggplot2) # for generating visualiations`

### Sample Data:

The following sample dataset needs to be imported. [TitanicSurvival.csv](https://github.com/jamalfrnk/ContingencyTables_R/tree/master/TitanicSurvival.csv.csv)

This can be done directly from RStudio's "Import Dataset" button, or the "read.csv" command:
    
     TitanicSurvival = read.csv('TitanicSurvival.csv')
     
#### Frequencies 
To produce contingency tables which calculate counts for each combination of categorical variables we can use R's `table` function.

       # counts for sex (gender) categories
       table(TitanicSurvival.csv$sex)
       ##
       ##      female    male
       ##         466     843 
 
 If we want to understand the number of males and females of a particular age group we can produce a cross classification table:
 
       # cross classification counts for age by sex (gender)
       table(TitanicSurvival.csv$age, TitanicSurvival.csv$sex)
       ##
       ##                female    male
       ##    0.166700006      1       0
       ##    0.333299994      0       1
       ##    0.416700006      0       1
       ##    0.666700006      0       1
       ##    0.75             2       1
       ##    0.833299994      0       3
       ##    0.916700006      1       1
       ##    1                5       5
       ##    2                7       5
       ##    3                3       4
       ##    4                5       5
The above cross classification table is only a snippet of the whole. It conveys the total number of male or female passengers on the the Titanic of a particular age. This snippet range begins at the age of 1.6months and ends at 4years old. The entire table would range the ages of males and females aboard the Titanic from 1.6 months to 80years old.


We can also produce multidimensional tables based on three or more categorical variables. For this we leverage the `ftable` function to print the results more attractively. In this case we assess the count of passengers by sex (gender), passengerClass, survived:

       ## customer counts across sex (gender) by passengerClass and survived
       table1 <- table(TitanicSurvival.csv$sex, TitanicSurvival.csv$passengerClass, TitanicSurvival.csv$survived) 
       ftable(table1)
                    
                    no yes                  
    ## female 1st    5 139
    ##        2nd   12  94
    ##        3rd  110 106
    ## male   1st  118  61
    ##        2nd  146  25
    ##        3rd  418  75
    
    
#### Proportions 
   
We can also produce contingency tables that present the proportions (percentages) of each category or combination of categories. To do this we simply feed the frequency tables produced by `table` function to the `prop.table` function. The following reproduces the previous tables but calculates the proportions rather than counts:

     # percentage of sex (gender) categories
     table2 <- table(TitanicSurvival.csv$sex)
     prop.table(table2)
     ##
     ##        female         male 
     ##     0.3559969    0.6440031
     
     # percentage of sex (gender) of a particular age group 
     table3 <- table(TitanicSurvival.csv$sex, TitanicSurvival.csv$age)
     prop.table(table3)
     ##
     ##      
     ##                  25           26
     ## female 0.0057361377 0.0086042065
     ## male   0.0267686424 0.0200764818
     # Please note: This is only a snippet of my result contingency table. The data here represents the percentage of females : males of age 25 and 26. 

     
     # customer percentages across sex (gender) by passengerClass and survived
     # using table1 from previous code chunk
     ftable(round(prop.table(table1), 3))
     #               no   yes               
     # female 1st  0.004 0.106
     #        2nd  0.009 0.072
     #        3rd  0.084 0.081
     # male   1st  0.090 0.047
     #        2nd  0.112 0.019
     #        3rd  0.319 0.057 


#### Marginals

Marginals show the total counts or percentages across columns or rows in a contingency table. For example, if we go back to `table3` which is cross classification counts for sex (gender) by age:

     ##
     ##      
     ##                  25           26
     ## female 0.0057361377 0.0086042065
     ## male   0.0267686424 0.0200764818
     
We can produce the marginal frequencies with `margin.table()` and the percentages for these marginal frequencies with `prop.table()` using the `margin` argument:

     # FREQUENCY MARGINALS 
     # row marginals - totals for each age across sex (gender)
     margin.table(table3, 1)
     ##    female   male 
     ##       388    658 
     
     # column marginals - totals for each sex (gender) across age
     margin.table(table3, 2)
     ## 24        24.5          25 
     ## 47           1          34 
     ## 26        26.5          27 
     ## 30           1          30 


     # PERCENTAGE MARGINALS 
     # row marginals - row percentages across sex (gender)
     prop.table(table3, margin = 1)
     #                24        24.5
     # female 0.054123711 0.000000000
     # male   0.039513678 0.001519757
        
     #                25          26
     # female 0.015463918 0.023195876
     # male   0.042553191 0.031914894
        
     #              26.5          27
     # female 0.000000000 0.025773196
     # male   0.001519757 0.030395137
     
     # column marginals - colun percentages across age 
     prop.table(table3, margin = 2)
     #             23.5        24      24.5
     # female 0.0000000 0.4468085 0.0000000
     # male   1.0000000 0.5531915 1.0000000
        
     #              25        26      26.5
     # female 0.1764706 0.3000000 0.0000000
     # male   0.8235294 0.7000000 1.0000000
        
     #              27        28      28.5
     # female 0.3333333 0.2812500 0.0000000
     # male   0.6666667 0.7187500 1.0000000
     
     
     
#### Visualization
Bar charts are most often used to visualize categorical variables. Her we can asses the count of passengers by passengerClass:

     ggplot(TitanicSurvival.csv, aes(x = passengerClass)) + geom_bar() + theme(axis.text.x = element_text(angle = 45, hjust = 1))
     
   ![Bar Graph with Count of Titanic Passengers categorized by passengerClass](C:\Users\B005401\Documents\TitanPassenger20%by20%passengerClass.png)

We use the `head` function to see the first few lines of the dataset.
       
       head(TitanicSurvival)
    ##                                 X survived    sex     age passengerClass
    ## 1   Allen, Miss. Elisabeth Walton      yes female 29.0000            1st
    ## 2  Allison, Master. Hudson Trevor      yes   male  0.9167            1st
    ## 3    Allison, Miss. Helen Loraine       no female  2.0000            1st
    ## 4 Allison, Mr. Hudson Joshua Crei       no   male 30.0000            1st
    ## 5 Allison, Mrs. Hudson J C (Bessi       no female 25.0000            1st
    ## 6             Anderson, Mr. Harry      yes   male 48.0000            1st

We can see the name of each passenger and wheter they survived, along with their age, sex, and cabin class.


Next, we will use the `xtab` function to make some contingency tables. We can stratify by survival status and sex: 
   
    xtabs(~survived + sex, data=TitanicSurvival)

    ##         sex
    ## survived female male
    ##      no     127  682
    ##      yes    339  161

Or by passenger class:

    xtabs(~survived + passengerClass, data=TitanicSurvival)

    ##         passengerClass
    ## survived 1st 2nd 3rd
    ##      no  123 158 528
    ##      yes 200 119 181

Or by all three, to yield a multi-way table:

    xtabs(~survived + passengerClass + sex, data=TitanicSurvival)

    ## , , sex = female
    ## 
    ##         passengerClass
    ## survived 1st 2nd 3rd
    ##      no    5  12 110
    ##      yes 139  94 106
    ## 
    ## , , sex = male
    ## 
    ##         passengerClass
    ## survived 1st 2nd 3rd
    ##      no  118 146 418
    ##      yes  61  25  75
   
   
  We can also turn the table counts into proportions using the `prop.table` command.
   
    table1 = xtabs(~survived + sex, data=TitanicSurvival)
    prop.table(table1, margin=1)

    ##         sex
    ## survived    female      male
    ##      no  0.1569839 0.8430161
    ##      yes 0.6780000 0.3220000

The first command says to store the table of raw counts in a variable called table1. The second says turn the counts into proportions, standardizing so that the rows (margin=1) sum to 1. 


We can also standardize along the columns. We’re thinking of sex as the predictor and survival as the response, and therefore we want to see how the relative changes for men versus women:

    prop.table(table1, margin=2)

    ##         sex
    ## survived    female      male
    ##      no  0.2725322 0.8090154
    ##      yes 0.7274678 0.1909846


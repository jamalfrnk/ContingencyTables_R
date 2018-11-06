# ContingencyTables_R
# Malcolm Frank 
# Software Engineer, Infinitely Deep Robotics Group, 2018

## Project Title: Summarizing categorical data using contingency tables

### Overview: 
In this walkthrough, we will learn a few tools for summarzing categorical data using contingency tables. In addition, we will learn to look at subsets of a dataset defined by a specific variable, using the subset() function. Finally, we will explore the cut() function and the role it plays when transforming a numerical variable into a categorical variable. 


### Sample Data:

The following sample dataset needs to be imported. [TitanicSurvival.csv](https://github.com/jamalfrnk/ContingencyTables_R/tree/master/TitanicSurvival.csv.csv)

This can be done directly from RStudio's "Import Dataset" button, or the "read.csv" command:
    TitanicSurvival = read.csv('TitanicSurvival.csv')
    
 
We use the head() function to see the first few lines of the dataset.
  head(TitanicSurvival.csv) 
    ##                                 X survived    sex     age passengerClass
    ## 1   Allen, Miss. Elisabeth Walton      yes female 29.0000            1st
    ## 2  Allison, Master. Hudson Trevor      yes   male  0.9167            1st
    ## 3    Allison, Miss. Helen Loraine       no female  2.0000            1st
    ## 4 Allison, Mr. Hudson Joshua Crei       no   male 30.0000            1st
    ## 5 Allison, Mrs. Hudson J C (Bessi       no female 25.0000            1st
    ## 6             Anderson, Mr. Harry      yes   male 48.0000            1st

We can see the name of each passenger and wheter they survived, along with their age, sex, and cabin class.


Next, we will use the xtab() function to make some contingency tables. We can stratify by survival status and sex: 
   xtabs(~survived + sex, data=TitanicSurvival.csv)
   
   ##         sex
   ## survived female male
   ##      no     127  682
   ##      yes    339  161


Or by passenger class:

  xtabs(~survived + passengerClass, data=TitanicSurvival.csv)

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
   
   
  We can also turn the table counts into proportions using the prop.table command.
   
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

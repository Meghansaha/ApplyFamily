---
title: "<b>Apply Family Notes</b>"
output:
  html_document:
    toc: yes
    df_print: paged
    keep_md: true
  github_document:
    theme: cosmo
    toc: yes
    toc_float:
      collapsed: yes
      smooth_scroll: yes
---





#   **Purpose**

In this notebook session, I'll be going over the "Apply" Family in base R. The "Apply Functions" refer to a group of functions that come with base R that allow you to do repetitive actions within different objects(i.e. data frames, lists, etc.)

The functions I'll go over will be:

-   apply()
-   lapply()
-   mapply()
-   rapply()
-   sapply()
-   tapply()
-   vapply()

# **Apply Functions, Inputs, & Outputs**

A quick overview:

<table class=" lightable-minimal" style='font-family: "Trebuchet MS", verdana, sans-serif; margin-left: auto; margin-right: auto;'>
 <thead>
  <tr>
   <th style="text-align:left;"> Name </th>
   <th style="text-align:left;"> What it does </th>
   <th style="text-align:left;"> Input </th>
   <th style="text-align:left;"> Output </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> apply() </td>
   <td style="text-align:left;"> Applies a function to the rows or columns of the object </td>
   <td style="text-align:left;"> Data Frame or Matrix </td>
   <td style="text-align:left;"> Matrix or Array </td>
  </tr>
  <tr>
   <td style="text-align:left;"> lapply() </td>
   <td style="text-align:left;"> Applies a function to all elements within the input </td>
   <td style="text-align:left;"> Data Frame, List, Vector </td>
   <td style="text-align:left;"> List </td>
  </tr>
  <tr>
   <td style="text-align:left;"> mapply() </td>
   <td style="text-align:left;"> Applies a function to multiple lists or vectors - can be considered a multivariate version of 'sapply' </td>
   <td style="text-align:left;"> Multiple Lists or Vectors (i.e. a Data Frame) </td>
   <td style="text-align:left;"> List or Vector </td>
  </tr>
  <tr>
   <td style="text-align:left;"> rapply() </td>
   <td style="text-align:left;"> Applies a function recursively through a list - nested lists </td>
   <td style="text-align:left;"> Nested Lists </td>
   <td style="text-align:left;"> Nested List or Vector depending on arguments passed </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sapply() </td>
   <td style="text-align:left;"> A simpler version of 'lapply' that works on list, data frames, and vectors </td>
   <td style="text-align:left;"> Data Frame, List, Vector </td>
   <td style="text-align:left;"> Matrix or Vector </td>
  </tr>
  <tr>
   <td style="text-align:left;"> tapply() </td>
   <td style="text-align:left;"> Applies a function over a ragged/jagged array (an array that has more than one dimension with varying lengths) </td>
   <td style="text-align:left;"> Data Frame or Vector that can be split (divided into groups/factors) </td>
   <td style="text-align:left;"> Array </td>
  </tr>
  <tr>
   <td style="text-align:left;"> vapply() </td>
   <td style="text-align:left;"> Similar to 'sapply', but you can pre-specify the type of value that is output, making it a bit faster </td>
   <td style="text-align:left;"> Data Frame, List, Vector </td>
   <td style="text-align:left;"> Data Frame, List, Vector </td>
  </tr>
</tbody>
</table>

# **Apply Function: apply()**

The **apply()** function is used to apply a function to all rows or columns of an object. Consequently, only objects with more than one dimension can be used with apply, so a data frame or matrix.


`apply(X, MARGIN, FUN)`


Where:

<table class=" lightable-minimal" style='font-family: "Trebuchet MS", verdana, sans-serif; margin-left: auto; margin-right: auto;'>
 <thead>
  <tr>
   <th style="text-align:left;"> Argument </th>
   <th style="text-align:left;"> Description </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> X </td>
   <td style="text-align:left;"> Data Frame or Matrix </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MARGIN </td>
   <td style="text-align:left;"> '1' or '2' or 'c(1,2)' where 1 = Rows and 2 = Columns </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FUN </td>
   <td style="text-align:left;"> The function you want to be applied to the data frame or matrix in question </td>
  </tr>
</tbody>
</table>


## **Data Frame Example**

Creating a mock data frame:


```r
City <- c("Buffalo","NYC","Seattle","Austin","Orlando","Minneapolis")
Cases <- c(500,2012,1876,635,4512,823)
Controls <- c(3426,5210,6753,5633,2013,1890)

records <- data.frame(City,Cases,Controls, row.names = NULL)

records
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["City"],"name":[1],"type":["chr"],"align":["left"]},{"label":["Cases"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["Controls"],"name":[3],"type":["dbl"],"align":["right"]}],"data":[{"1":"Buffalo","2":"500","3":"3426"},{"1":"NYC","2":"2012","3":"5210"},{"1":"Seattle","2":"1876","3":"6753"},{"1":"Austin","2":"635","3":"5633"},{"1":"Orlando","2":"4512","3":"2013"},{"1":"Minneapolis","2":"823","3":"1890"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


We can use the apply function to calculate column sums...


```r
apply(records[,2:3], 2,sum)
```

```
##    Cases Controls 
##    10358    24925
```


...Or row sums. (Note that these both produce vectors)


```r
apply(records[,2:3], 1,sum)
```

```
## [1] 3926 7222 8629 6268 6525 2713
```


We can name the vectors in one line of code with the `names<-` function 

```r
`City Totals` <-  `names<-`(apply(records[,2:3], 1,sum), records$City)

`City Totals`
```

```
##     Buffalo         NYC     Seattle      Austin     Orlando Minneapolis 
##        3926        7222        8629        6268        6525        2713
```

## **Applying Statistic (More Complex) Functions**

We can also do different statistical procedures based on each test's requirement. Let's do a T-test:


```r
#Making a mock data set for T-test
Exam_one_grades <- c(67,53)
Exam_two_grades <- c(90,89)
Exam_three_grades <- c(89,95)
Exam_four_grades <- c(95,87)
Exam_five_grades <- c(100,99)
Student <- c("Student1","Student2")

Grades <- tibble(Student,Exam_one_grades,Exam_two_grades,Exam_three_grades,Exam_four_grades,Exam_five_grades)

Grades
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["Student"],"name":[1],"type":["chr"],"align":["left"]},{"label":["Exam_one_grades"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["Exam_two_grades"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["Exam_three_grades"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["Exam_four_grades"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["Exam_five_grades"],"name":[6],"type":["dbl"],"align":["right"]}],"data":[{"1":"Student1","2":"67","3":"90","4":"89","5":"95","6":"100"},{"1":"Student2","2":"53","3":"89","4":"95","5":"87","6":"99"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

Let's just say we want the P.value of one sample T-tests for each student and we want to place it in this dataset as a new column. What's important to note is that arguments that would normally be passed through to your functions, go as seperate arguments at the end of the apply function, after you declare which function you want used.


```r
#Turning off scientific notation formatting
options(scipen = 999)

#Getting index of columns that end with the word "grades"
Gradeindexes <- grep(("grades$"),names(Grades))

#Using apply to apply the t.test.
testresults <- apply(Grades[,Gradeindexes],1,t.test, alternative = "two.sided", conf.level = 0.95)

#Using do.call to bind p values to the data set. because results are in a list, need to use lapply.

Grades$`P Values` <- do.call(rbind, lapply(testresults, function(x){x$p.value}))

Grades
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["Student"],"name":[1],"type":["chr"],"align":["left"]},{"label":["Exam_one_grades"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["Exam_two_grades"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["Exam_three_grades"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["Exam_four_grades"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["Exam_five_grades"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["P Values"],"name":[7],"type":["dbl[,1]"],"align":["right"]}],"data":[{"1":"Student1","2":"67","3":"90","4":"89","5":"95","6":"100","7":"0.00009843551"},{"1":"Student2","2":"53","3":"89","4":"95","5":"87","6":"99","7":"0.00049395503"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

# **Lapply Function: lapply()**
TBC

# **Mapply Function: mapply()**
TBC

# **Rapply Function: rapply()**
TBC

# **Sapply Function: sapply()**
TBC

# **Tapply Function: tapply()**
TBC

# **Vapply Function: vapply**
TBC

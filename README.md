# RANDOMISATION LIST

<div style="text-align: justify">

This page is devoted to the edition of a randomisation list for a randomised clinical trial (RCT). We focus here on randomisation within blocks, in order to provide a balance between arms throughout the study. R codes are proposed below.

<br>

### TEST

<br>

<ul>

<li> <details> <summary> RANDO 1</summary></li>

BLA BLA BLA

<li> <details> <summary> RANDO STRATIFICATION</summary></li>

  <ul>

  <li> Strate = 1 vari </li>
  
  BLA BLA
  
  <li> Strate > 1 variable <li>
  
  BLA BLA
  
  </ul>


</ul>


<details>
<summary>RANDOMISATION WITHOUT STRATIFICATION</summary>
<br>

*In order to edit a randomisation list for a RCT comparing an experimental treatment against placebo (2 arms), using random block sizes of 6, 8, 10 and 12 (meaning that each block is defined with 3, 4, 5 or 6 occurences of each arm), we can compute the following code:*

```r

library(blockrand)

randomisation_list <- function(myseed, Npat, labelArms = c("A","B"), block = 1:4) 
{
set.seed(myseed)
mylist <- blockrand(n=Npat,
                    num.levels = length(labelArms),
                    levels = labelArms,
                    block.sizes = block)
}

edit_list <- randomisation_list(myseed=9478, Npat=210, labelArms=c("Placebo","Experimental treatment"), block=c(3,4,5,6))
table(edit_list*treatment) 


```

**Input parameters:**
* Npat : number of patients in the randomisation list
* labelArms : vector of labels for randomised arms (vector size = number of arms)
* block : vector of integers defining the block sizes (number of occurence of each arm)


</details>	


<details>
<summary>RANDOMISATION WITH STRATIFICATION</summary>

<br>

<details>
<summary>Stratification with one variable</summary>
<br>

*In order to edit a randomisation list for a RCT comparing an experimental treatment against placebo (2 arms), using random block sizes of 4, 6 and 8 (meaning that each block is defined with 2, 3, or 4 occurences of each arm), and considering a randomisation stratified with gender (male, female), we can compute the following code:*

```r

library(blockrand)

randomisation_list_strat <- function(myseed, Npat, labelArms = c("A","B"), block = 1:4, strat = c("Stratum1","Stratum2")) 
{
set.seed(myseed)

for (i in 1:length(strat)) {
  listrand <- blockrand(n=Npat, 
                        num.levels = length(labelArms), 
                        levels = labelArms, 
                        block.sizes = block,
                        stratum = strat[i])
  if (i > 1) {
    mylist <- rbind(mylist, listrand)
  }
  else {
    mylist <- listrand
  }
}
}

edit_list <- randomisation_list(myseed=72048, Npat=128, labelArms=c("Placebo","Experimental treatment"), block=2:4, stratum=c("Male","Female"))
table(edit_list$stratum, edit_list*treatment) 

```

**Input parameters:**
* Npat : number of patients in the randomisation list
* labelArms : vector of labels for randomised arms (vector size = number of arms)
* block : vector of integers defining the block sizes (number of occurence of each arm)
* strat : vector of labels for stratum (vector size = number of stratum)

</details>

<details>
<summary>Stratification with more than one variable</summary>
<br>

*In order to edit a randomisation list for a RCT comparing an experimental treatment against placebo (2 arms), using random block sizes of 4, 6 and 8 (meaning that each block is defined with 2, 3, or 4 occurences of each arm), and considering a randomisation stratified with age (< 40 years, > or = 40 years) and centre (3 centres), we can compute the following code:*

```r

library(blockrand)

randomisation_list_strat <- function(myseed, Npat, labelArms = c("A","B"), block = 1:4, strat = c("Stratum1","Stratum2")) 
{
set.seed(myseed)

for (i in 1:length(strat)) {
  listrand <- blockrand(n=Npat, 
                        num.levels = length(labelArms), 
                        levels = labelArms, 
                        block.sizes = block,
                        stratum = strat[i])
  if (i > 1) {
    mylist <- rbind(mylist, listrand)
  }
  else {
    mylist <- listrand
  }
}
}

edit_list <- randomisation_list(myseed=74792, Npat=172, labelArms=c("Placebo","Experimental treatment"), block=2:4, 
                                stratum=c("<40 and centre 1","<40 and centre 2","<40 and centre 3",
                                          "40+ and centre 1","40+ and centre 2","40+ and centre 3"))
table(edit_list$stratum, edit_list*treatment) 

```

**Input parameters:**
* Npat : number of patients in the randomisation list
* labelArms : vector of labels for randomised arms (vector size = number of arms)
* block : vector of integers defining the block sizes (number of occurence of each arm)
* strat : vector of labels for stratum (vector size = number of stratum)


</details>	

</details>

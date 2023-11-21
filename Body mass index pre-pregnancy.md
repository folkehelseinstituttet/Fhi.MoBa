# Body mass index pre-pregnancy

#### Table of Contents
- _[1. Code in Stata](#code-in-stata)_ <br>

## Code in Stata
##### MoBa is not responsible for any errors in the study results that are caused by errors in code or documentation at the MoBa Wiki page.
```stata
/***************************************************************************************************
* Variable: Pre pregnancy BMI
* Purpose: Generate a variable for pre-pregnancy BMI by use of data from Q1
* Author: Ingeborg Forthun 
* Date: 22.09.2014 (version 8 of MoBa)
* Questionnaires used: First questionnaire (Q1)
* Variables used: AA85 AA87
****************************************************************************************************/

/*
AA85: What did you weigh at the time you became pregnant (in kilograms)?
AA87: How tall are you?
*/

use "Q1.dta", clear

generate height=AA87/100

*Height (in meters) lower than 1 is coded missing
replace height=. if height<1 

generate weight=AA85

*Weight in kilos lower than 35 and higher than 200 is coded missing
replace weight=. if (weight<35|weight>200) & weight!=.

generate bmi_x=(weight/(height*height))

*bmi is rounded to nearest .1 decimal. 
generate bmi=round(bmi_x, 0.1)

brow height weight bmi_x bmi

drop height bmi_x

label variable bmi "Pre pregnancy BMI"

sum bmi

*In order to make a variable for categories of pre pregnancy BMI one can use:
recode bmi (10.0/18.4=0 "<18.5")(18.5/24.9=1 "18.5-24.9") (25.0/29.9=2 "25-29.9") (30.0/max=3 ">=30"), generate (bmi_cat4)
tab bmi_cat4, missing
```

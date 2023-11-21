# Urinary infection in pregnancy

#### Table of Contents
- _[1. Code in Stata](#code-in-stata)_

## Code in Stata
This code was written for the purpose of harmonizing data from MoBa with data from the Danish National Birth Cohort (DNBC) for the MOBAND CP study. The harmonization was regarded as partial and not all information from MoBa was captured when harmonizing the data.

The respondents are asked in what week of pregnancy she had the infection in the third questionnaire in MoBa, but not in the first and fourth questionnaire in MoBa. However, it is possible to extract pregnancy week from the first questionnaire in MoBa from questions regarding use of medication during the urinary infection. This is not done in the code below as only 80 percent of the respondents reporting a urinary infection have filled in information on pregnancy weeks they used medication.

##### MoBa is not responsible for any errors in the study results that are caused by errors in code or documentation at the MoBa Wiki page.

```stata
/***************************************************************************************************
* Variable: Urinary infection
* Purpose: Generate a variable for urinary infection by use of data from Q1, Q3 and Q4
* Author: Ingeborg Forthun 
* Date: 23.09.2014 (version 8 of MoBa)
* Questionnaires used: first questionnaire (Q1), third questionnaire (Q3) and fourth qestionnaire (Q4)
* Variables used: Q1, AA780 AA789 
*                 Q3, CC592-CC596
*                 Q4, DD450
****************************************************************************************************/

/*
First questionnaire
Do you have or have you had any of the following illness or heath problems? 
AA780: Kidney infection/pyelonephritis
AA789: Urinary tract infections/cystitis

Values (for both the above):
1=Yes

Third questionnaire
Do you have or have you had any of the following illnesses or problems after the 13th week of pregnancy?
Bladder infection/cystitis 
CC592: Pregnancy week 13-16
CC593: Pregnancy week 17-20
CC594: Pregnancy week 21-24
CC595: Pregnancy week 25-28
CC596: Pregnancy week 29+

Values (for all the above): 
1=Yes

Fourth questionnaire
Have you had any of the following problems/illnesses since you completed the previous questionnaire?
Cystitits
DD450: Yes, last part of pregnancy

Values:
1=Yes
*/

*Urinary infection in pregnancy at time of first questionnaire

use "Q1.dta", clear

generate uti_1=0
replace uti_1=1 if (AA780==1|AA789==1)

label define yesno 0 "No" 1 "Yes"
label values uti_1 yesno

label variable uti_1 "Urinary infection in pregnancy at time of questionnaire 1"

tab uti_1, missing

*Urinary infection in pregnancy from week 13 to time of the third questionnaire

use "Q3.dta", clear

generate uti_2=0
replace uti_2=1 if (CC592==1|CC593==1|CC594==1|CC595==1|CC596==1)

label define yesno 0 "No" 1 "Yes"
label values uti_2 yesno

label variable uti_2 "Urinary infection in pregnancy from week 13 upp till questionnaire 3"

tab uti_2, missing

*Urinary infection in the last part of pregnancy

use "Q4.dta", clear

generate uti_3=0
replace uti_3=1 if DD450==1

label define yesno 0 "No" 1 "Yes"
label values uti_3 yesno

label variable uti_3 "Urinary infection in the last part of pregnancy"

tab uti_3, missing
```


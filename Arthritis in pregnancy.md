# Arthritis in pregnancy

#### Table of Contents
- _[1. Code in Stata](#code-in-stata)_ 
  
### Code in Stata
This code was written for the purpose of harmonizing data from MoBa with data from the Danish National Birth Cohort (DNBC) for the MOBAND CP study. The harmonization was regarded as partial due to differences in format in questions in MoBa and the DNBC, but no information from MoBa was considered lost when harmonizing the data.

##### MoBa is not responsible for any errors in the study results that are caused by errors in code or documentation at the MoBa Wiki page.

```stata
/***************************************************************************************************
* Variable: Arthritis 
* Purpose: Generate a variable for arthritis by use of data from Q1
* Author: Ingeborg Forthun 
* Date: 22.09.2014 (version 8 of MoBa)
* Questionnaires used: First questionnaire (Q1)
* Variables used: AA644 AA645 AA647-AA652
****************************************************************************************************/

/*
Do you have or have you had any of the following illnesses or health problems?
If you have taken medication (tablets, mixtures, suppositories, inhalers, creams,
etc.) in conjunction with the illness or health problem give the name(s) of the medication(s)
and when you took them. 

Arthritis (rheumatoid arthritis)/Bechterev's reflex

AA644: Before pregnancy
AA645: During pregnancy

Use of medication
AA647: Last 6 months before pregnancy
AA648: Pregnancy week 0-4
AA649: Pregnancy week 5-8
AA650: Pregnancy 9-12
AA651: Pregnancy week 13+

Values (for all the variables listed above):
1=Yes

AA652: Number of days used 
*/

use "Q1.dta", clear

*Before and/or during pregnancy

gen arthritis=0

foreach v of var AA644-AA651 {
replace arthritis=1 if (`v'==1|(AA652>0 & AA652!=.))
}

label define yesno 0 "No" 1 "Yes"
label values arthritis yesno

label variable arthritis "Arthritis before and/or during pregnancy at time of questionnaire 1" 

tab arthritis

*During pregnancy
gen arthritis_preg=0

foreach v of var AA645 AA648 AA649 AA650 AA651 {
replace arthritis_preg=1 if `v'==1
}

label values arthritis_preg yesno

label variable arthritis_preg "Arthritis during pregnancy at time of questionnaire 1" 

tab arthritis_preg
```

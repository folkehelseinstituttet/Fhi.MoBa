# Diabetes in pregnancy

#### Table of Contents
- _[1. Code in Stata](#code-in-stata)_ <br>

## Code in Stata
This code was written for the purpose of harmonizing data from MoBa with data from the Danish National Birth Cohort (DNBC) for the MOBAND CP study. The harmonization was regarded as partial as not all available information was used from MoBa. Information about diabetes and use of medication before pregnancy is also available in the first questionnaire (Q1), but is not used in the code included here.
It is not possible to distinguish between different types of diabetes as the respondents in MoBa only are asked whether they have “Diabetes treated with insulin” or “Diabetes not treated with insulin”. Information on types of diabetes (type-I, type-II, gestational and unspecified) is included in the Medical Birth Registry (MBR).

##### MoBa is not responsible for any errors in the study results that are caused by errors in code or documentation at the MoBa Wiki page.

```stata
/***************************************************************************************************
* Variable: Diabetes, information from questionnaire
* Purpose: Generate a variable for diabetes by use of data from Q1
* Author: Ingeborg Forthun 
* Date: 22.09.2014 (version 8 of MoBa)
* Questionnaires used: First questionnaire (Q1)
* Variables used: AA510, AA513-AA516, AA519, AA522-AA525
****************************************************************************************************/



/*
Diabetes treated with insulin

AA510: During pregnancy

Use of medication
AA513: Pregnancy week 0-4
AA514: Pregnancy week 5-8
AA515: Pregnancy week 9-12
AA516: Pregnancy week 13+

Values (for all the above):
1=Yes

Diabetes not treated with insulin

AA519: During pregnancy

Use of medication
AA522: Pregnancy week 0-4
AA523: Pregnancy week 5-8
AA524: Pregnancy week 9-12
AA525: Pregnancy week 13+

Values (for all the above):
1=Yes
*/

use "Q1.dta", clear

*Diabetes during pregnancy

*It is not possible to distinguish between 0 (No) and missing. 
gen diabetes=0

/*diabetes_int is coded 1 (Yes) if the respondent has checked the box for diabetes during pregnancy or/and
  use of medication.*/
foreach v of var AA510 AA513-AA516 AA519 AA522-AA525 {
replace diabetes=1 if `v'==1
}

*Add label 
label define yesno 1 "Yes" 0 "No"
label values diabetes yesno

label variable diabetes "Diabetes in pregnancy at time of questionnaire 1"

*Frequency tabel
tab diabetes

*Use of medication during pregnancy

/*
diabetes_med is coded 1 (Diabetes and use of medication) if the respondent has checked one of the boxes indicating use of medication during pregnancy.
diabetes_med is coded 2 (Diabetes and no use of medication) if the respondent has checked the box indicating diabetes during 
pregnancy, but has not reported use of medication. 
diabetes_med is coded 0 (No diabetes if the respondent has not reported diabetes during pregnancy. 
*/

gen diabetes_med=0

foreach v of var AA513-AA516 AA522-AA525 {
replace diabetes_med=1 if `v'==1
}

replace diabetes_med=2 if diabetes_med==0 & (AA510==1|AA519==1)

*Add label 
label define med_diabetes 0 "No diabetes" 1 "Diabetes and use of medication" 2 "Diabetes and no use of medication"
label values diabetes_med med_diabetes

label variable diabetes_med "Use of diabetes medication during pregnancy upp till questionnaire 1"

*Frequency table
tab diabetes_med, mis
```

# Epilepsy in pregnancy

#### Table of Contents
- _[1. Code in Stata](#code-in-stata)_ 

## Code in Stata
This code was written for the purpose of harmonizing data from MoBa with data from the Danish National Birth Cohort (DNBC) for the MOBAND CP study. The harmonization was regarded as partial, but no information from MoBa was considered lost when harmonizing the data.
Information on maternal epilepsy in pregnancy is also available in the Medical Birth Registry (MBR).

##### MoBa is not responsible for any errors in the study results that are caused by errors in code or documentation at the MoBa Wiki page.

```stata
/***************************************************************************************************
* Variable: Epilepsy
* Programmer: Ingeborg Forthun, Ingeborg.Forthun@fhi.no
* Published: 16.09.2014 (version 8 of MoBa)
* Questionnaires used: First questionnaire (Q1)
* Variables used: Q1, AA837 AA838 AA839 AA840
****************************************************************************************************/

/*Epilepsy during pregnancy

Epilepsy
AA834: During pregnancy

Use of medication
AA837: Pregnancy week 0-4
AA838: Pregnancy week 5-8
AA839: Pregnancy week 9-12
AA840: Pregnancy week 13+

Values (for all the above):
1=Yes
*/

use "Q1.dta", clear

*Epilepsy during pregnancy

*It is not possible to distinguish between 0 (No) and missing. 
gen epilepsy=0

/*epilepsy_int is coded 1 (Yes) if the respondent has checked the box for epilepsy during pregnancy or/and
  use of medication.*/
foreach v of var AA834 AA837-AA840 {
replace epilepsy=1 if `v'==1
}

*Add label 
label define yesno 1 "Yes" 0 "No"
label values epilepsy yesno

label variable epilepsy "Epilepsy in pregnancy at time of questionnaire 1"

*Frequency tabel 
tab epilepsy

*Use of medication during pregnancy

gen epilepsy_med=0

/*
epilepsy_med is coded 1 (Epilepsy and use of medication) if the respondent has checked one of the boxes indicating use of medication during pregnancy.
epilspy_med is coded 2 (Epilepsy and no use of medication) if the respondent has checked the box indicating epilepsy during 
pregnancy, but has not reported use of medication. 
epilepsy_med is coded 0 (No epilepsy) if the respondent has not reported epilepsy during pregnancy. 
*/

foreach v of var AA837-AA840 {
replace epilepsy_med=1 if `v'==1
}

replace epilepsy_med=2 if epilepsy_med==0 & AA834==1

*Add label 
label define med_epilepsy 0 "No epilepsy" 1 "Epilepsy and use of medication" 2 "Epilepsy and no use of medication"
label values epilepsy_med med_epilepsy 

label variable epilepsy_med "Use of epilepsy medication during pregnancy upp till questionnaire 1"

*Frequency tabel 
tab epilepsy_med, missing
```

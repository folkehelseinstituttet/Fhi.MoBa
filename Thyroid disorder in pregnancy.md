# Thyroid disorder in pregnancy

#### Table of Contents
- _[1. Code in Stata](#code-in-stata)_ <br>

## Code in Stata
This code was written for the purpose of harmonizing data from MoBa with data from the Danish National Birth Cohort (DNBC) for the MOBAND CP study. The harmonization was regarded as partial, but all information was kept from MoBa when harmonizing the data.

##### MoBa is not responsible for any errors in the study results that are caused by errors in code or documentation at the MoBa Wiki page.

```stata
/***************************************************************************************************
* Variable: Thyroid disorders
* Purpose: Generate a variable for thyroid disorders by use of data from Q1
* Author: Ingeborg Forthun 
* Date: 23.09.2014 (version 8 of MoBa)
* Questionnaires used: First questionnaire (Q1)
* Variables used: AA563 AA564 AA566-AA571 AA1755 AA1756 AA1759-AA1763 AA1764 AA1765 AA1766 AA1769-AA1774
****************************************************************************************************/

/*
Do you have or have you had any of the following illnesses or health problems?
If you have taken medication (tablets, mixtures, suppositories, inhalers, creams. etc.)
in conjunction with the illness or heatlh problem give the name(s) of the medication(s)
and when you took them. 

Hypothyroidism or hyperthyroidism

Version B/C/D/E

AA563: Before pregnancy
AA564: During pregnancy

Use of medication
AA566: Last 6 months before pregnancy
AA567: Pregnancy week 0-4
AA568: Pregnancy week 5-8
AA569: Pregnancy week 9-12
AA570: Pregnancy week 13+

Values (for all the above):
1=Yes

AA571: Number of days used

Version A
Hyperthyroidism

AA1756: Before pregnancy
AA1755: During pregnancy

Use of medication 
AA1759: Last 6 months before pregnancy
AA1760: Pregnancy week 0-4
AA1761: Pregnancy week 5-8
AA1762: Pregnancy week 9-12
AA1763: Pregnancy week 13+

Values (for all the above):
1=Yes

AA1764: Number of days used

Hypothyroidism

AA1765: Before pregnancy
AA1766: During pregnancy

Use of medication 
AA1769: Last 6 months before pregnancy
AA1770: Pregnancy week 0-4
AA1771: Pregnancy week 5-8
AA1772: Pregnancy week 9-12
AA1773: Pregnancy week 13+

Values (for all the above):
1=Yes

AA1774: Number of days used
*/

*Thyroid disorders before and/or during pregnancy 

use "Q1.dta", clear

gen thyroid=0

foreach v of var AA563 AA564 AA566-AA570 AA1755 AA1756 AA1759-AA1763 AA1765 AA1766 AA1769-AA1773 {
replace thyroid=1 if (`v'==1|(AA1764>0 & AA1764!=.)|(AA571>0 & AA571!=.)|(AA1774>0 & AA1774!=.))
}

*Add label 
label define yesno 0 "No" 1 "Yes"
label values thyroid yesno

label variable thyroid "Thyroid disorder before or during pregnancy at time of questionnaire 1"

*Frequency table
tab thyroid 

*Thyroid disorders during pregnancy

gen thyroid_preg=0

foreach v of var AA564 AA567-AA570 AA1755 AA1760-AA1763 AA1766 AA1770-AA1773 {
replace thyroid_preg=1 if `v'==1
}

*Add label 
label values thyroid_preg yesno

label variable thyroid_preg "Thyroid disorder in pregnancy at time of questionnaire 1"

*Frequency table
tab thyroid_preg

/*
thyroid_med is coded 0 (No thyroid disorder) if the respondent has not reported hyper- or hypothyrodism.

thyroid_med is coded 1 (Thyroid disorder and use of medication) if the respondent has reported to have used medication
for hyper- or hypothyrodism. 

thyroid_med is coded 2 (Thyroid disorder and no use of medication) if the respondent has reported hyper- or hypothyrodism,
but has not reported use of medication. 
*/

gen thyroid_med=0

foreach v of var AA567-AA570 AA1760-AA1763 AA1770-AA1773 {
replace thyroid_med=1 if `v'==1
}

replace thyroid_med=2 if thyroid_med==0 & (AA564==1|AA1755==1|AA1766==1)

*Add label 
label define med_thyroid 0 "No thyroid disorder" 1 "Thyroid disorder and use of medication" 2 "Thyroid disorder and no use of medication"
label values thyroid_med med_thyroid

label variable thyroid_med "Use of medication during pregnancy up till questionnaire 1"

*Frequency tabel 
tab thyroid_med, missing
```


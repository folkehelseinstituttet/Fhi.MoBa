# Inflammatory bowel disease in pregnancy

#### Table of Contents
- _[1. Code in Stata](#code-in-stata)_ <br>

## Code in Stata
This code was written for the purpose of harmonizing data from MoBa with data from the Danish National Birth Cohort (DNBC) for the MOBAND CP study. The harmonization was regarded as partial due to use of different type of data sources in MoBa and the DNBC, but no information from MoBa was considered lost when harmonizing the data.

##### MoBa is not responsible for any errors in the study results that are caused by errors in code or documentation at the MoBa Wiki page.

```stata
/***************************************************************************************************
* Variable: Inflammatory bowel disease
* Purpose: Generate a variable for inflammatory bowel disease by use of data from Q1
* Author: Ingeborg Forthun 
* Date: 22.09.2014 (version 8 of MoBa)
* Questionnaires used: First questionnaire (Q1)
* Variables used: AA1795-AA1814 AA617-AA625 
****************************************************************************************************/


/*
Do you have or have you had any of the following illnesses or health problems? If you have
taken medication in conjunction with the illness or health problem give the name(s) of the medication(s) 
and when you took them.  

Version A
Chron's disease
AA1795: Before pregnnacy
AA1796: During pregnancy

Use of medication
AA1799: Last 6 months before pregnnacy
AA1800: Pregnancy week 0-4
AA1801: Pregnancy week 5-8
AA1802: Pregnnacy week 9-12
AA1803: Pregnancy week 13+

Values (for all the variables listed above):
1=Yes

AA1804: Number of days used

Ulcerative colitis
AA1805: Before pregnnacy
AA1806: During pregnancy

Use of medication
AA1809: Last 6 months before pregnnacy
AA1810: Pregnancy week 0-4
AA1811: Pregnancy week 5-8
AA1812: Pregnnacy week 9-12
AA1813: Pregnancy week 13+

Values (for all the variables listed above):
1=Yes

AA1814: Number of days used

Version B, C, E
Chron's disease and ulcerative colitis
AA617: Before pregnancy
AA618: During pregnancy

Use of medication
AA620: Last 6 months before pregnnacy
AA621: Pregnancy week 0-4
AA622: Pregnancy week 5-8
AA623: Pregnnacy week 9-12
AA624: Pregnancy week 13+

Values (for all the variables listed above):
1=Yes

AA625: Number of days used 
*/

use "Q1.dta", clear

*Generate the variable

gen inflam_bowel=0

*inflam_bowel is coded 1 (Yes) if the respondent has answered "Yes" for chron's disease or ulcerative colitis before or during pregnancy
foreach v of var AA617-AA624 AA1795-AA1803 AA1805-AA1813 {
replace inflam_bowel=1 if `v'==1
}

*inflam_bowel is coded 1 (Yes) if the respondent has filled in the number of days she has used medication for chron's disease or ulcerative colitis
replace inflam_bowel=1 if (AA625>0 & AA625!=.)|(AA1804>0 & AA1804!=.)|(AA1814>0 & AA1814!=.) 

label define yesno 0 "No" 1 "Yes"
label values inflam_bowel yesno
label variable inflam_bowel "Inflammatory bowel disease before and/or during pregnancy" 

tab inflam_bowel
```


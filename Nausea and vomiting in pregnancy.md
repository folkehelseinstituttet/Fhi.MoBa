# Nausea and vomiting in pregnancy

#### Table of Contents
- _[1. Code in Stata](#code-in-stata)_ <br>

## Code in Stata

Hyperemesis is not included here.

This code was written for the purpose of harmonizing data from MoBa with data from the Danish National Birth Cohort (DNBC) for the MOBAND CP study. The harmonization was regarded as partial due to difference in format of questions in MoBa and the DNBC, but no information from MoBa was considered lost when harmonizing the data. However, information on use of medication during episodes of nausea and vomiting in the third questionnaire in MoBa was not used.
In the code below, variables for response time for the first questionnaire (Q1) and the third questionnaire (Q3) are used.

##### MoBa is not responsible for any errors in the study results that are caused by errors in code or documentation at the MoBa Wiki page.

```stata
/***************************************************************************************************
* Variable: Nausea and vomiting
* Purpose: Generate a variable for nausea by use of data from Q1, Q3 and MBR
* Author: Ingeborg Forthun 
* Date: 23.09.2014 (version 8 of MoBa)
* Questionnaires used: First questionnaire (Q1), third questionnaire (Q3), Medical Birth Registry (MBR)
* Variables used: ALDERUTFYLT_S1 ALDERRETUR_S1 ALDERUTSENDT_S1 AA216-AA218 AA226-AA228 
*                 ALDERUTFYLT_S3 ALDERRETUR_S3 ALDERUTSENDT_S3 CC376-CC379 CC388-CC391
*                 SVLEN_DG
****************************************************************************************************/


/*
First questionnaire

Have you experienced any of the following illnesses or problems during this pregnancy?

Nausea
AA216: Week of pregnnacy 0-4
AA217: Week of pregnancy 5-8
AA218: Week of pregnnacy 9-12

Nausea with vomiting
AA226: Week of pregnnacy 0-4
AA227: Week of pregnancy 5-8
AA228: Week of pregnnacy 9-12

Do you have or had any of the following illnesses or problems after the 13th week of pregnancy?

Nausea
CC376: Week of pregnnacy 13-16	
CC377: Week of pregnancy 17-20
CC378: Week of pregnnacy 21-24
CC379: Week of pregnnacy 25-28

Long-term nausea and vomiting
CC388: Week of pregnnacy 13-16	
CC389: Week of pregnancy 17-20
CC390: Week of pregnnacy 21-24
CC391: Week of pregnnacy 25-28

Valuse (for all variables listed):
1=Yes
*/

*run do-file for response time for Q1

*Pregnancy week 0-12

generate nausea_0_12=.

/*nausea_0_12 is coded 1 (Nausea) if the respondent has checked that she has had nausea some time
  during pregnancy week 0-12. nausea_0_12 is coded 2 (Vomiting) if the respondent has checked that
  she has had vomiting some time during pregnancy week 0-12. 
*/

foreach v of var AA216 AA217 AA218 {
replace nausea_0_12 = 1 if `v'==1
}

foreach v of var AA226 AA227 AA228 {
replace nausea_0_12 = 2 if `v'==1
}

/*nausea_0_12 is coded 0 (No) if the respondent has not checked for nausea or vomiting during pregnancy week 0-12
  and she has answered the first questionnaire after 11+6 days (83 days). 
*/

replace nausea_0_12=0 if (nausea_0_12==. & (respons_time_1>83 & respons_time_1!=.))

*Add label
label define nausea 0 "No" 1 "Nausea" 2 "Vomiting"

label values nausea_0_12 nausea
label variable nausea_0_12 "Nausea during pregnancy week 0-12"

tab nausea_0_12, mis

*run do-file for response time for Q3

*Pregnancy week 13-20

generate nausea_13_20=. 

/*nausea_13_20 is coded 1 (Nausea) if the respondent has checked that she has had nausea some time
  during pregnancy week 13-20. nausea_13_20 is coded 2 (Vomiting) if the respondent has checked that
  she has had vomiting some time during pregnancy week 13-20. 
*/

foreach v of var CC376 CC377 {
replace nausea_13_20 = 1 if `v'==1
}

foreach v of var CC388 CC389 {
replace nausea_13_20 = 2 if `v'==1
}

/*nausea_13_20 is coded 0 (No) if the respondent has not checked for nausea or vomiting during pregnancy week 13-20
  and she has answered the third questionnaire after 19+6 days (139 days).
*/

replace nausea_13_20=0 if (nausea_13_20==. & (respons_time_2>139 & respons_time_2!=.)) 

*Add label
label define nausea 0 "No" 1 "Nausea" 2 "Vomiting"
label values nausea_13_20 nausea
label variable nausea_13_20 "Nausea during pregnancy week 13-20"

tab nausea_13_20, mis

*Pregnancy week 21-28

generate nausea_21_28=.

/*nausea_21_28 is coded 1 (Nausea) if the respondent has checked that she has had nausea some time
  during pregnancy week 21-28. nausea_21_28 is coded 2 (Vomiting) if the respondent has checked that
  she has had vomiting some time during pregnancy week 21-28. 
*/

foreach v of var CC378 CC379 {
replace nausea_21_28 = 1 if `v'==1
}

foreach v of var CC390 CC391 {
replace nausea_21_28 = 2 if `v'==1
}

/*nausea_21_28 is coded 0 (No) if the respondent has not checked for nausea or vomiting during pregnancy week 21-28
  and she has answered the third questionnaire after 27+6 days (195 days).
*/

replace nausea_21_28=0 if (nausea_21_28==. & (respons_time_2>195 & respons_time_2!=.)) 

*Add label
label values nausea_21_28 nausea
label variable nausea_21_28 "Nausea during pregnancy week 21-28"

tab nausea_21_28, mis


*Merge Q1 and Q3

*Nausea during pregnancy upp till the second interview/third questionnaire

gen nausea=.

/*nausea is coded 1 (Nausea) if the respondent has checked that she has had nausea in at least 
  one of the pregnancy weeks. nausea is coded 2 (Vomiting) if the respondent has checked that she
  has had vomiting in at least one of the pregnancy week or if she has been hospitalised because of 
  prolonged nausea and vomiting during pregnancy. 
*/

foreach v of var AA216 AA217 AA218 CC376 CC377 CC378 CC379 {
replace nausea=1 if `v'==1
}

foreach v of var AA226 AA227 AA228 CC388 CC389 CC390 CC391 {
replace nausea=2 if `v'==1
}

/*nausea is coded 0 (No) if the respondent has not checked that she had nausea or vomiting during pregnancy
  and she has filled out both questionnaires. 
*/

replace nausea=0 if ///
nausea==. ///
& _merge==3 

label define nausea 0 "No" 1 "Nausea" 2 "Vomiting"
label values nausea nausea

label variable nausea "Nausea during pregnancy upp till questionnaire 3"

tab nausea, missing
```


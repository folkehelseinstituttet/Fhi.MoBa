# Fever in pregnancy

#### Table of Contents
- _[1. Code in Stata](#code-in-stata)_ <br>
  - _[1.1 Description of code](#description-of-code)_ <br> 

## Code in Stata
This code was written for the purpose of harmonizing data from MoBa with data from the Danish National Birth Cohort (DNBC) for the MOBAND CP study. The harmonization was regarded as partial due to difference in format of questions in MoBa and the DNBC, but no information from MoBa was considered lost when harmonizing the data.
Information about fever in the last part of pregnancy (after pregnancy week 30) is available in the fourth qustionnaire in MoBa. Information about use of medication is also available in the first and third questionnaire in MoBa.

## Description of code
Respondents who have indicated a fever episode either in the first questionnaire (Q1) or in the third questionnaire (Q3) are coded 1 (Yes) for the variable fever. Respondents who have not reported a fever episode and have answered the first and third questionnaire are coded 0 (No). The substantial number of missing for the variable fever (based on the code below) is mostly due to respondents that have only answered the first questionnaire and not the third questionnaire.

For the variables indicating fever in specific pregnancy weeks the response time for each questionnaire have been taken into account when coding the variables.

##### MoBa is not responsible for any errors in the study results that are caused by errors in code or documentation at the MoBa Wiki page.
```stata
/***************************************************************************************************
* Variable: Fever
* Purpose: Generate a variable for fever by use of data from Q1, Q3 and MBR
* Author: Ingeborg Forthun 
* Date: 23.09.2014 (version 8 of MoBa)
* Questionnaires used: First questionnaire (Q1), third questionnaire (Q3), Medical Birth Registry (MBR)
* Variables used: ALDERUTFYLT_S1 ALDERRETUR_S1 ALDERUTSENDT_S1 AA326-AA329 AA336-AA339 
*                 ALDERUTFYLT_S3 ALDERRETUR_S3 ALDERUTSENDT_S3 CC712-CC716 CC720-CC724 CC728-CC732 CC736
*                 SVLEN_DG
****************************************************************************************************/

/*
First questionnaire
Have you experienced any of the following illnesses or problems during this pregnancy?

Fever with rash
AA326: Week of pregnancy 0-4
AA327: Week of pregnancy 5-8
AA328: Week of pregnancy 9-12
AA329: Week of pregnancy 13+

Fever over 38.5 C 
AA336: Week of pregnancy 0-4
AA337: Week of pregnancy 5-8
AA338: Week of pregnancy 9-12
AA339: Week of pregnancy 13+

Values (for all the above):
1=Yes

Third questionnaire

If you have had a fever once or more since the 13th week of pregnancy, indicate in which 
week of pregnancy. 
Which week of pregnancy did you have a fever?
First time
CC712: Week of pregnancy 13-16
CC713: Week of pregnancy 17-20
CC714: Week of pregnancy 21-24
CC715: Week of pregnancy 25-28
CC716: Week of pregnancy 29+

Second time
CC720: Week of pregnancy 13-16
CC721: Week of pregnancy 17-20
CC722: Week of pregnancy 21-24
CC723: Week of pregnancy 25-28
CC724: Week of pregnancy 29+

Third time
CC728: Week of pregnancy 13-16
CC729: Week of pregnancy 17-20
CC730: Week of pregnancy 21-24
CC731: Week of pregnancy 25-28
CC732: Week of pregnancy 29+

CC736: Fever more than 3 times

Values (for all the above):
1=Yes
*/

*run do-file for response time for Q1 and Q3
*Merge the two files, for a description on how to do this see "help" - "code" - "Norwegian Mother and Child Cohort Study".

*Fever during pregnancy upp till the second interview/third questionnaire

gen fever=. 

*fever is coded 1 (Yes) if the respondent has checked that she has had fever in at least one of the pregnancy weeks.  

foreach v of var AA326 AA327 AA328 AA329 AA336 AA337 AA338 AA339 CC712 CC713 CC714 CC715 CC716 ///
CC720 CC721 CC722 CC723 CC724 CC728 CC729 CC730 CC731 CC732 CC736 {
replace fever=1 if `v'==1
}

/*fever is coded 0 (No) if the respondent has not checked of for fever and she has answered both
  questionnaires.
*/

replace fever=0 if fever==. & _merge==3

label define yesno 0 "No" 1 "Yes"
label values fever yesno

label variable fever "Fever during pregnancy upp till interview 2/questionnaire 3"

tab fever, missing


*Pregnancy week 0-12

generate fever_0_12=.

*fever_0_12 is coded 1 (Yes) if the respondent has reported fever some time during pregnancy week 0-12. 

foreach v of var AA326 AA327 AA328 AA336 AA337 AA338 {
replace fever_0_12=1 if `v'==1
}

*fever_0_12 is coded 0 (No) if fever_0_12=. and the respondent has answered the first questionnaire after 11+6 days (83 days). 


replace fever_0_12=0 if fever_0_12==. & respons_time_1>83 & respons_time_1!=. 

label define yesno 1 "Yes" 0 "No"
label values fever_0_12 yesno

label variable fever_0_12 "Fever in pregnancy week 0-12"

tab fever_0_12, missing


*Pregnancy week 13-20

generate fever_13_20=.

*fever_13_20 is coded 1 (Yes) if the respondent has reported fever some time during pregnancy week 13-20. 

foreach v of var CC712 CC713 CC720 CC721 CC728 CC729 {
replace fever_13_20=1 if `v'==1
} 

*fever_13_20 is coded 0 (No) if fever_13_20=. and the respondent has answered the third questionnaire after 19+6 days (139 days).

replace fever_13_20=0 if fever_13_20==. & respons_time_2>139 & respons_time_2!=.

label values fever_13_20 yesno

label variable fever_13_20 "Fever in pregnancy week 13-20"

tab fever_13_20, missing


*Pregnancy week 21-28

generate fever_21_28=.

*fever_21_28 is coded 1 (Yes) if the respondent has reported fever some time during pregnancy week 21-28. 

foreach v of var CC714 CC715 CC722 CC723 CC730 CC731 {
replace fever_21_28=1 if `v'==1
}

*fever_21_28 is coded 0 (No) if fever_21_28=. and the respondent has answered the third questionnaire after 27+6 days (195 days).

replace fever_21_28=0 if fever_21_28==. & respons_time_2>195 & respons_time_2!=. 

label values fever_21_28 yesno

label variable fever_21_28 "Fever in pregnancy week 21-28"

tab fever_21_28, missing
```

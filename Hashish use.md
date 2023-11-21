# Hashish use

#### Table of Contents
- _[1. Code in Stata](#code-in-stata)_ <br>

## Code in Stata
This syntax was written for the purpose of harmonizing data from MoBa with data from the Danish National Birth Cohort (DNBC) for the MOBAND CP study. The harmonization was regarded as complete and no information was lost from MoBa when harmonizing the data.

##### MoBa is not responsible for any errors in the study results that are caused by errors in code or documentation at the MoBa MediaWiki page.

```stata
/***************************************************************************************************
* Variable: Hashish use
* Purpose: Generate a variable for hashish use by use of data from Q1
* Author: Ingeborg Forthun 
* Date: 22.09.2014 (version 8 of MoBa)
* Questionnaires used: Q1, Q3
* Variables used: Q1, AA1435 
*                 Q3, CC1067
****************************************************************************************************/

*HASHISH USE DURING PREGNANCY 

/*
First questionnaire

Have you used any of the following substances?
AA1435: Hash, During pregnancy
Values:
1=Yes

Third questionnaire

Have you used any of the following substances after the 13th week of pregnancy?
CC1067: Hash
Values:
1=No
2=Yes
0=More than 1 check box filled in
*/

*Merge Q1 and Q3. 

generate hash=1 if (CC1067==2|CC1067==0|AA1435==1)
replace hash=0 if (AA1435==.|CC1067==1) & hash==.

label define hash 0 "No" 1 "Yes" 
label values hash hash

label variable hash "Hashish use during pregnancy up till the time of the third questionnaire"

tab hash, missing
```

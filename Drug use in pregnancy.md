# Drug use in pregnancy

#### Table of Contents
- _[1. Illicit drug use](#illicit-drug-use)_ <br>
 	- _[1.1 Code in Stata](#code-in-stata)_ <br>

## Illicit drug use
### Code in Stata
This code was written for the purpose of harmonizing data from MoBa with data from the Danish National Birth Cohort (DNBC) for the MOBAND CP study. The harmonization was regarded as complete and no information was lost from MoBa when harmonizing the data.

##### MoBa is not responsible for any errors in the study results that are caused by errors in code or documentation at the MoBa Wiki page.

```stata
/***************************************************************************************************
* Variable: Illicit drug use
* Purpose: Generate a variable for illicit durig use by use of data from Q1 and Q3
* Author: Ingeborg Forthun 
* Date: 23.09.2014 (version 8 of MoBa)
* Questionnaires used: First questionnaire (Q1) and third questionnaire (Q3)
* Variables used: AA1435 AA1439 AA1443 AA1447 AA1451
*                 CC1067 CC1068 CC1069 CC1070 CC1071 
****************************************************************************************************/

/*
First questionnaire

Have you used any of the following substances?
Hash AA1435: During pregnancy
Amphetamine AA1439: During pregnancy
Ecstasy AA1443: During pregnancy
Cocaine AA1447: During pregnancy
Heroin AA1451: During pregnancy

Values:
1=Yes


Third questionnaire

Have you used any of the following substances after the 13th week of pregnancy?
CC1067: Hash
CC1068: Amphetamine 
CC1069: Ecstasy 
CC1070: Cocaine
CC1071: Heroin

Values:
1=No
2=Yes
0=More than 1 check box filled in
*/

*Merge Q1 and Q3. 

generate illicit_drug=.
replace illicit_drug=1 if (AA1435==1|AA1439==1|AA1443==1|AA1447==1|AA1451==1 ///
|CC1067==2|CC1068==2|CC1069==2|CC1070==2|CC1071==2|CC1067==0|CC1068==0 ///
|CC1069==0|CC1070==0|CC1071==0)

*illicit_drug is coded 0 (No) if illicit_drug=. 
replace illicit_drug=0 if illicit_drug==.

label define drug 0 "No" 1 "Yes" 
label values illicit_drug drug

label variable illicit_drug "Drug use during pregnancy at time of the third questionnaire"

tab illicit_drug, missing
```

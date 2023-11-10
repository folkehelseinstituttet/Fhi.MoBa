# Response time

#### Table of Contents
- _[1. Description of syntax](#description-of-syntax)_ <br>
- _[2. Syntax](#syntax)_ <br>
 	- _[2.1 Stata syntax](#stata-syntax)_ <br>
  
## Description of syntax
Each file with questionnaire data from MoBa includes variables for age of child (in days) when the questionnaire was sent out, filled out and returned. These variables are called ALDERUTSENDT_Sx, ALDERUTFYLT_Sx and ALDERRETUR_Sx, and can be used to calculate response time. For the questionnaires filled out before birth these variables corresponds to number of days before the child is born. When generating variables for response time for these questionnaires, the questionnaire has to be merged with the file from the Medical Birth Registry of Norway (MBRN) in order to get information on gestational age in days.
## Syntax
### Stata syntax
##### MoBa is not responsible for any errors in the study results that are caused by errors in code or documentation at the MoBa Wiki page.

```stata
/***************************************************************************************************
* Variable: Response time
* Purpose: Generate a variable for response time by use of data from Q1, Q3, Q4 and MFR
* Author: Ingeborg Forthun 
* Date: 15.09.2014
* Questionnaires used: First questionnaire (Q1), Third questionnaire (Q3), Fourth questionnaire (Q4), Medical Birth Registry (MBR)
* Variables used: MBR, SVLEN_DG 
*                 Q1, ALDERUTFYLT_S1 ALDERRETUR_S1
*                 Q3, ALDERUTFYLT_S3 ALDERRETUR_S3
*                 Q4, ALDERUTFYLT_S4 ALDERRETUR_S4
* Valid for MoBa version: 8 
****************************************************************************************************/

/*
MFR
SVLEN_DG: Gestational age in days

MoBa
Q1
ALDERUTSENDT_S1: Childs age when questionnaire was sent
ALDERUTFYLT_S1: Childs age when questionnaire was completed 
ALDERRETUR_S1: Childs age when questionnaire was returned
 
Q3
ALDERUTSENDT_S3: Childs age when questionnaire was sent
ALDERUTFYLT_S3: Childs age when questionnaire was completed 
ALDERRETUR_S3: Childs age when questionnaire was returned
 
Q4
ALDERUTSENDT_S4: Childs age when questionnaire was sent
ALDERUTFYLT_S4: Childs age when questionnaire was completed 
ALDERRETUR_S4: Childs age when questionnaire was returned
*/
 
 
*First questionnaire 
 
*1) Merge Q1 and MFR. For a description on how to do this see "technical" - "merge file" at the MoBa wiki web-page.
 
/*If you only want to keep pregnancies that are in the Q1, use the following code:
*Drop observations that are only in MFR.
drop if _merge==2 //Valid for _merge will depende on how the merge has been conducted!!

*Drop duplicates (in multiple pregnancies)
duplicates report PREG_ID_XX
sort PREG_ID_XX BARN_NR
duplicates drop PREG_ID_XX, force
*/
 
*2) Check that ALDERUTFYLT_S1 is between ALDERUTSENDT_S1 and ALDERRETUR_S1.
 
generate x=0
 
replace x=1 if (ALDERUTFYLT_S1>=ALDERUTSENDT_S1 & ALDERRETUR_S1>=ALDERUTFYLT_S1) & ///
(ALDERUTSENDT_S1!=. & ALDERRETUR_S1!=. & ALDERUTFYLT_S1!=.)
 
*x=1 if ALERUTFYLT_S1 is between ALDERUTSENDT_S1 and ALDERRETUR_S1
 
/*3) Generate a variable for response time that equals the difference between gestational age (SVLEN_DG) and the childs age when 
     completing the questionnaire (ALDERUTFYLT_S1). For most of the respondents the childs age will be negative, in these cases 
     |ALDERUTFYLT_S1| should not exceed SVLEN_DG.
*/
 
generate respons_time_1=SVLEN_DG+(ALDERUTFYLT_S1) if x==1
 
*4) ALDERRETUR_S1-5 days is used as a measure of the childs age when filling out the questionnaire if x=0. 
 
generate alderutfylt_temp=ALDERRETUR_S1-5 if x==0
generate respons_time_temp=SVLEN_DG+(alderutfylt_temp)
replace respons_time_1=respons_time_temp if respons_time_1==. & respons_time_temp>0
  
*5) If the respondent has filled out the questionnaire after birth response_time_1 is coded .a.  
 
replace respons_time_1=.a if (ALDERUTFYLT_S1>0 & ALDERUTFYLT_S1!=. & x==1|ALDERRETUR_S1>0 & ALDERRETUR_S1!=. & x==0)
 
label define responslab 0 "After birth"
 
*6) respons_time_1 is coded missing if respons_time_1 is negative (most probably due to gestational age in days being wrong (too low)).
 
replace respons_time_1=. if respons_time_1<0
 
label values respons_time_1 responslab
 
drop alderutfylt_temp respons_time_temp x
 
label variable respons_time_1 "Response time of questionnaire 1"
 
sum respons_time_1
 
 
*Third questionnaire
 
*1) Merge Q3 and MFR. For a description on how to do this see "technical" - "merge file" at the MoBa wiki web-page.
 
/*If you only want to keep pregnancies that are in the Q1, use the following code:
*Drop observations that are only in MFR.
drop if _merge==2 //Valid for _merge will depende on how the merge has been conducted!!

*Drop duplicates (in multiple pregnancies)
duplicates report PREG_ID_XX
sort PREG_ID_XX BARN_NR
duplicates drop PREG_ID_XX, force
*/
 
*2) Check that ALDERUTFYLT_S3 is between ALDERUTSENDT_S3 and ALDERRETUR_S3.
 
generate x=0
replace x=1 if ALDERUTFYLT_S3>=ALDERUTSENDT_S3 & ALDERRETUR_S3>=ALDERUTFYLT_S3 & ///
ALDERUTSENDT_S3!=. & ALDERRETUR_S3!=. & ALDERUTFYLT_S3!=.
 
*x=1 if ALERUTFYLT_S3 is between ALDERUTSENDT_S3 and ALDERRETUR_S3
 
/*3) Generate a variable for response time that equals the difference between gestational age (SVLEN_DG) and the childs age when 
     completing the questionnaire (ALDERUTFYLT_S3). For most of the respondents the childs age will be negative, in these cases 
     |ALDERUTFYLT_S3| should not exceed SVLEN_DG.
*/
 
generate respons_time_2=SVLEN_DG+(ALDERUTFYLT_S3) if x==1
 
*4) ALDERRETUR_S3-5 days is used as a measure of the childs age when filling out the questionnaire if x=0.
 
generate alderutfylt_temp=ALDERRETUR_S3 if respons_time_2==.
generate respons_time_temp=SVLEN_DG+(alderutfylt_temp)
replace respons_time_2=respons_time_temp if respons_time_2==. & respons_time_temp>0
 
*5) If the respondent has filled out the questionnaire after birth response_time_1 is coded .a.
 
replace respons_time_2=.a if (ALDERUTFYLT_S3>0 & ALDERUTFYLT_S3!=. & x==1|ALDERRETUR_S3 >0 & ALDERRETUR_S3!=. & x==0)
 
label define responslab .a "After birth"
 
drop alderutfylt_temp respons_time_temp x
 
label variable respons_time_2 "Response time of questionnaire 3"
 
sum respons_time_2


*Fourth questionnaire
  
*1) Check that ALDERUTFYLT_S4 is between ALDERUTSENDT_S4 and ALDERRETUR_S4.
 
generate x=0
replace x=1 if ALDERUTFYLT_S4>=ALDERUTSENDT_S4 & ALDERRETUR_S4>=ALDERUTFYLT_S4 & ///
(ALDERUTSENDT_S4 !=. & ALDERRETUR_S4!=. & ALDERUTFYLT_S4!=.)
 
*x=1 if ALERUTFYLT_S4 is between ALDERUTSENDT_S4 and ALDERRETUR_S4
 
*2) Generate a variable for response time.
 
generate respons_time_3=ALDERUTFYLT_S4 if (x==1|ALDERRETUR_S4==.|ALDERRETUR_S4<0)
 
*3) ALDERRETUR_S4-5 days is used as a measure of the childs age when filling out the questionnaire if x=0.
 
replace respons_time_3=ALDERRETUR_S4-5 if respons_time_3==. 
 
label variable respons_time_3 "Respons time of questionnaire 4"
 
sum respons_time_3
drop x
```

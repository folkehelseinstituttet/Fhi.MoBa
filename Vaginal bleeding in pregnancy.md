# Vaginal bleeding in pregnancy

#### Table of Contents
- _[1. Code in Stata](#code-in-stata)_ <br>

## Code in Stata
This code was written for the purpose of harmonizing data from MoBa with data from the Danish National Birth Cohort (DNBC) for the MOBAND CP study. The harmonization was regarded as partial. Information about pregnancy week of vaginal bleeding and assumed reason for bleeding can be extracted from MoBa, but is not included in the code below (information on pregnancy week can be extracted by using information about the date of the last menstrual bleeding and the date of vaginal bleeding in Q1, and pregnancy weeks for vaginal bleeding given in Q3). Note that assumed reason for vaginal bleeding is missing for a substantial number of mothers in MoBa.

Information on vagnial bleeding in pregnancy is also available in the Medical Birth Registry (MBR).

##### MoBa is not responsible for any errors in the study results that are caused by errors in code or documentation at the MoBa Wiki page.

```stata
/***************************************************************************************************
* Variable: Vaginal bleeding during pregnancy
* Purpose: Generate a variable for vaginal bleeding by use of data from Q1 and Q3
* Author: Ingeborg Forthun 
* Date: 23.09.2014 (version 8 of MoBa)
* Questionnaires used: First questionnaire (Q1) and third questionnaire (Q3)
* Variables used: AA164 AA168 AA169 AA173 AA174 AA175
*                 CC315-CC338
****************************************************************************************************/

 
/*
First questionnaire

AA164: Have you had bleeding from the vagina once or more during this pregnancy?
Values: 
0=Checked more than one box
1=No
2=Yes

If yes, describe the first and last bleeding. Give the date the bleeding started, how many days the bleeding lasted and how much you bled.
First bleeding
AA168: No. of days variation 

AA169: Amount 
Values: 
1=Trace of blood
2=More than just a trace
3=Clots
4=Trace of blood + More than just a trace
5=Trace of blood + Clots
6=More than just a trace + Clots
7=Trace of blood + More than just a trace + Clots

Last bleeding
AA173: No. of days variation

AA174: Amount 
Values: 
1=Trace of blood
2=More than just a trace
3=Clots
4=Trace of blood + More than just a trace
5=Trace of blood + Clots
6=More than just a trace + Clots
7=Trace of blood + More than just a trace + Clots
 
If more than two episodes of bleeding write in the number of times.
AA175: Number of times

Third questionnaire

CC315: Have you had one or more episodes of vaginal bleeding after the 13th week of pregnancy?
0=Checked more than one box
1=No
2=Yes

If yes, how much did you bleed, in which week(s) of pregnancy and how many days did the bleeding last?
(If you have had more than 2 episodes of bleeding, describe the last 2 only.)

1. episode

CC316: The amount of blood
1=Spotting
2=More than spotting
3=Large amounts
4=Spotting + More than spotting
5=Spotting + Large amounts
6=More than spotting + Large amounts
7=Spotting + More than spotting + Large amounts

In which week of pregnancy did the bleeding occur?
CC317=Pregnancy week 13-16
CC318=Pregnancy week 17-20
CC319=Pregancy week 21-24
CC320=Pregnancy week 25-28
CC321=Pregnancy week 29+

CC322: No. of days bleeding lasted

2. episode

CC323: The amount of blood
1=Spotting
2=More than spotting
3=Large amounts
4=Spotting + More than spotting
5=Spotting + Large amounts
6=More than spotting + Large amounts
7=Spotting + More than spotting + Large amounts

In which week of pregnancy did the bleeding occur?
CC324: Pregnancy week 13-16
CC325: Pregnancy week 17-20
CC326: Pregancy week 21-24
CC327: Pregnancy week 25-28
CC328: Pregnancy week 29+

Values (for all the above):
1=Yes

CC329: No. of days bleeding lasted

CC330: Number of episodes of bleeding if more than 2

CC331: No. of days bleeding lasted

CC332: Do you know why you bled?
Values: 
0=More than one check box filled in 
1=No
2=Yes

If yes what was the reason?
CC333: The placenta is too low/is in a difficult position/placenta previa
CC334: Premature separation 
CC335: Threatening miscarrigae/premature birth
CC336: Cervical ulcer, bleeding of the mucous membrane in the vagina
CC337: Following intercourse 
CC338: Other reason 

Values (for all the above):
1=Yes
*/

*Vaginal bleeding in pregnancy at time of first questionnaire

use "Q1.dta", clear

gen vag_bleed_1=.

*Generate temperary variables for number of days/episodes given.
tempvar episode_1_1 episode_1_2 episode_more

*The number 99 is not included, indicates unspecified in MoBa.
egen `episode_1_1'=anymatch(AA168), values(1/90)
egen `episode_1_2'=anymatch(AA173), values(1/90)
egen `episode_more'=anymatch(AA175), values(1/90)

/*If the respondent has answered "Yes" when asked if she has had bleeding from the vagina during pregnancy
or if she has reported number of days or number of episodes of bleeding vag_bleed_1 is coded 1 (Yes). 
*/
replace vag_bleed_1=1 if ((AA164==2)|(`episode_1_1'==1)|(`episode_1_2'==1)|(`episode_more'==1)|(AA169!=.)|(AA174!=.))

*vag_bleed is 0 (No) if vag_bleed is coded missing and the respondent has answered "No" in AA164
replace vag_bleed_1=0 if vag_bleed_1==. & AA164==1

/*vag_bleed_1 is coded 0 (No) if vag_bleed is coded missing and the respondent has answered version A of the questionnaire
  (in this version AA164 is not included). */

replace vag_bleed_1=0 if vag_bleed_1==. & VERSJON_SKJEMA1_TBL1=="SKJEMA1A"

label define yesno 0 "No" 1 "Yes"
label values vag_bleed_1 yesno

label variable vag_bleed_1 "Vaginal bleeding in pregnancy at time of questionnaire 1"

tab vag_bleed_1, missing

*Amount of blood
gen vag_bleed_amount_1=.

replace vag_bleed_amount_1=1 if (AA169==1|AA174==1)
replace vag_bleed_amount_1=2 if (inlist(AA169, 2,3,4,5,6,7)|inlist(AA174, 2,3,4,5,6,7)) 

replace vag_bleed_amount_1=0 if vag_bleed_amount_1==. & vag_bleed_1==0

label define spots 0 "No" 1 "Spots" 2 "More than spots" 
label values vag_bleed_amount_1 spots

label variable vag_bleed_amount_1 "Amount of bleeding up till the time of questionnaire 1"

tab vag_bleed_amount_1, missing

*Vaginal bleeding in pregnancy from week 13 up till the time of the third questionnaire

use "Q3.dta", clear

gen vag_bleed_2=.

*Generate temperary variables for number of days/episodes given.
tempvar episode_2_1 episode_2_2 episode_2_3

*The number 99 is not included, indicates unspecified in MoBa.
egen `episode_2_1'=anymatch(CC322), values(1/90)
egen `episode_2_2'=anymatch(CC329), values(1/90)
egen `episode_2_3'=anymatch(CC331), values(1/90)

/*If the respondent has answered "Yes" when asked if she has had bleeding from the vagina during pregnancy
or if she has reported number of days or number of episodes of bleeding vag_bleed_2 is coded 1 (Yes). 
*/
replace vag_bleed_2=1 if ((CC315==2)|(CC316!=.)|(CC317==1)|(CC318==1)|(CC319==1)|(CC320==1)|(CC321==1)|(`episode_2_1'==1) ///
|(CC323!=.)|(CC324==1)|(CC325==1)|(CC326==1)|(CC327==1)|(CC328==1)|(`episode_2_2'==1)|(CC330!=.)|(`episode_2_3'==1) ///
|(CC332==2)|(CC333!=.)|(CC334!=.)|(CC335!=.)|(CC336!=.)|(CC337!=.)|(CC338!=.))

*vag_bleed_2 is 0 (No) if vag_bleed_2 is coded missing and the respondent has answered "No" in CC315.

replace vag_bleed_2=0 if vag_bleed_2==. & CC315==1

label define yesno 0 "No" 1 "Yes"
label values vag_bleed_2 yesno

label variable vag_bleed_2 "Vaginal bleeding from pregnancy 13 up till questionnaire 3"

tab vag_bleed_2, missing

*Check of variable
tab CC315 vag_bleed_2, mis //The substantial amount of missing for vag_bleed_2 is due to the fact that many respondents have missing for CC351

*Amount of blood
gen vag_bleed_amount_2=.

replace vag_bleed_amount_2=1 if (CC316==1|CC323==1)
replace vag_bleed_amount_2=2 if ((inlist(CC316, 2,3,4,5,6,7))|(inlist(CC323, 2,3,4,5,6,7)))

*vag_bleed_amount is 0 (No) if vag_bleed=0 (No) and vag_bleed_amount is coded missing.
replace vag_bleed_amount_2=0 if vag_bleed_amount_2==. & vag_bleed_2==0

label define spots 0 "No" 1 "Spots" 2 "More than spots" 
label values vag_bleed_amount_2 spots

label variable vag_bleed_amount_2 "Amount of bleeding from pregnancy 13 up questionnaire 3"

tab vag_bleed_amount_2, missing
```


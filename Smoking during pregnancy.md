# Smoking during pregnancy

#### Table of Contents
- _[1. Smoking](#smoking)_
    - _[1.1 Code in Stata](#1---code-in-stata)_
- _[2. Number of cigarettes](#number-of-cigarettes)_
    - _[2.1 Code in Stata](#2.1-code-in-stata)_
- _[3. Use of other type of nicotine](#use-of-other-type-of-nicotine)_
    - _[3.1 Code in Stata](#3.1-code-in-stata)_
- _[4. Passive smoking](#passive-smoking)_
    - _[4.1 Code in Stata](#4.1-code-in-stata)_

## Smoking

### 1 - Code in Stata
This code was written for the purpose of harmonizing data from MoBa with data from the Danish National Birth Cohort (DNBC) for the MOBAND CP study. The harmonization was regarded as complete and no information from MoBa was considered lost when harmonizing the data.

##### MoBa is not responsible for any errors in the study results that are caused by errors in code or documentation at the MoBa Wiki page.

```stata
/***************************************************************************************************
* Variable: Smoking
* Purpose: Generates variables for smoking in pregnancy
* Author: Ingeborg Forthun, Ingeborg.Forthun@fhi.no
* Published: 08.09.2014 (version 8 of MoBa)
* Questionnaires used: First questionnaire (Q1), third questionnaire (Q3), fourth questionnaire (Q4)
* Variables used: Q1, AA1979 AA1355 AA1356 AA1357 AA1358 AA1365 
*                 Q3, CC1037 CC1038 CC1039 CC1047 CC1048 CC1049 CC1050 CC1051 
*                     CC1052 CC1053 CC1054
*                 Q4, DD732 DD1114 DD738
****************************************************************************************************/
 
/*MoBa

Source: The Norwegian Mother and Child Cohort Study (MoBa)
File: Q1 (first questionnaire), Q3 (third questionnaire) and Q4 (fourth questionnaire)
 
First questionnaire
Version A: AA1979: Do you smoke now or have you ever smoked?
Values:
1=Never
2=Sometimes
3=Daily
4=Stopped smoking
 
Version B/C/E: AA1355: Have you ever smoked?
Values:
1=No
2=Yes
 
Version A/B/C/E: AA1356: Do you smoke now (after you became pregnant)?
Values:
1=No
2=Sometimes
3=Daily
4=No+Sometimes
5=No+Daily
6=Sometimes+Daily
 
AA1357: Number of cigaretter per week
 
AA1358: Number of cigaretter per day
 
AA1365: If you stopped smoking after you became pregnant, in which week of pregnancy did you stop?
 
Third questionnaire
 
CC1037: Do you smoke at present?
Values:
1=No
2=Sometimes
3=Daily
4=No + Sometimes
5=Sometimes + Daily
 
CC1038: Cigarettes per week
 
CC1039: Cigarettes per day
 
If you or the baby's father have smoked during the pregnancy, 
were there periodes during which you or the baby's father did not smoke?
CC1047: Pregnancy week 0-4
CC1048: Pregnancy week 5-8
CC1049: Pregnancy week 9-12
CC1050: Pregnancy week 13-16
CC1051: Pregnancy week 17-20
CC1052: Pregnancy week 21-24
CC1053: Pregnancy week 25-28
CC1054: Pregnancy week 29+
 
Fourth questionnaire
 
DD732: What were your and your partner/husbands smoking habits during the last 3 months of your pregnancy and in the period
after the birth?
Last 3 months during pregnancy
Values:
1=Didn't smoke
2=Smoked sometimes
3=Smoked every day
4=Didn't smoke + Smoked sometimes
5=Didn't smoke + Smoked daily
6=Smoked sometimes + Smoked daily
7=Didn't smoke + Smoked sometimes + Smoked daily
 
DD1114: Cigarettes per week
 
DD738: Cigarettes per day
*/ 
 
*SMOKING IN PREGNANCY UP TILL FIRST QUESTIONNAIRE
 
use "Q1.dta", clear
 
generate smoke_1=.
 
*smoke_1 is coded 1 (No) if AA1979=1 (No), AA1355=1 (No) or AA1356=1 (No). 
replace smoke_1=1 if (AA1979==1|AA1355==1|AA1356==1)
 
/*smoke_1 is coded 2 (Yes) if the respondent has answered "Sometimes", "Daily", "No+Sometimes", "No+Daily" or "Sometimes+Daily" in AA1356
  or has reported a positive number of cigarettes per week or per day. */ 
replace smoke_1=2 if inlist(AA1356, 2, 3, 4, 5, 6)|(AA1357>0 & AA1357!=.)|(AA1358>0 & AA1358!=.)
 
/*smoke_1 is coded 3 (Stopped) if the respondent has answered "No" in AA1356 (do not smoke now) and she has 
  reported a pregnancy week in AA1365 and is not previously coded smoke_1=2. */ 
replace smoke_1=3 if AA1356==1 & AA1365!=. & smoke_1!=2
 
label define smoke 1 "No" 2 "Yes" 3 "Stopped"
label values smoke_1 smoke
 
label variable smoke_1 "Smoking in pregnancy up till questionnaire 1"
 
tab smoke_1, missing
 
*Check of variable
brow smoke_1 AA1979 AA1355 AA1356 AA1357 AA1358 AA1365 if smoke_1==. 
 
*SMOKING AT TIME OF QUESTIONNAIRE 3
 
use "Q3.dta", clear
 
generate smoke_2=.
 
*smoke_2 is coded 1 (No) if CC1037=1 (No)
replace smoke_2=1 if CC1037==1
 
/*smoke_2 is coded 2 (Yes) if the respondent has answered "Sometimes", "Daily", "No+Sometimes" or "Sometimes+Daily" in CC1037
  or the respondent has reported a positive number of cigarettes per day or per week. */
replace smoke_2=2 if inlist(CC1037, 2, 3, 4, 5)|(CC1038>0 & CC1038!=.)|(CC1039>0 & CC1039!=.)
 
/*smoke_2 is coded 3 (Stopped) if the respondent has answered "No" in CC1037 (do not smoke at present) and
  the respondent is not previously coded 2 (Yes) for smoke_1 and she has checked one of the boxes for 
  pregnancy week in CC1047-CC1054. */
foreach v of var CC1047-CC1054 {
replace smoke_2=3 if `v'==1 & CC1037==1 & smoke_2!=2 
}
 
label define smoke 1 "No" 2 "Yes" 3 "Stopped"
label values smoke_2 smoke
 
label variable smoke_2 "Smoking at time of questionnaire 3"
 
tab smoke_2, missing
 
*Check of variable 
order smoke_2 CC1037 CC1038 CC1039 CC1047-CC1054 
brow if smoke_2==3 
 
*SMOKING DURING THE LAST PART OF PREGNANCY
 
use "Q4.dta", clear
 
/*smoke_3 is coded 0 (No) if the respondent has answered "Didn't smoke in DD732, has missing in DD732 and DD738 and has reported 
  no cigarettes weekly, or has missing in DD732 and DD1114 and has reported no cigarettes daily.*/
generate smoke_3=0 if (DD732==1|(DD732==. & DD1114==0 & DD738==.)| ///
                      (DD732==. & DD1114==. & DD738==0)| ///
                      (DD732==. & DD1114==0 & DD738==0))
 
/*smoke_3 is coded 1 (Yes) if the respondent has reported to have smoked in the last three months of pregnancy,
  or has reported a positive number of cigarettes daily or weekly. */
 
replace smoke_3=1 if ((DD732==2|DD732==3|DD732==4)|(DD1114>0 & DD1114!=.) ///
|(DD732==5|DD732==6|DD732==7)|(DD738>0 & DD738!=.))

*Check of variable
brow DD732 DD738 DD1114 smoke_3 if smoke_3==0 & DD732!=1 
brow DD732 DD738 DD1114 smoke_3 if smoke_3!=0 & DD732==1 

label define yesno 0 "No" 1 "Yes" 
label values smoke_3 yesno
 
label variable smoke_3 "Smoking in the last part of pregnancy"
 
tab smoke_3, missing
```

## Number of cigarettes

### Code in Stata
This syntax was written for the purpose of harmonizing data from MoBa with data from the Danish National Birth Cohort (DNBC) for the MOBAND CP study. The harmonization was regarded as complete and no information from MoBa was considered lost.

##### MoBa is not responsible for any errors in the study results that are caused by errors in code or documentation at the MoBa Wiki page.

```stata
/***************************************************************************************************
* Variable: Number of cigarettes per day during pregnancy
* Programmer: Ingeborg Forthun, Ingeborg.Forthun@fhi.no
* Published: 15.09.2014 (version 8 of MoBa)
* Questionnaires used: First questionnaire (Q1), third questionnaire (Q3), fourth questionnaire (Q4)
* Variables used: Q1, AA1357 AA1358 AA1979 AA1355 AA1356
*                 Q3, CC1038 CC1039 CC1037
*                 Q4, DD738 DD1114 DD732    
****************************************************************************************************/

/*NUMBER OF CIGARETTES PER DAY AT TIME OF QUESTIONNAIRE 1

First questionnaire

Version A: AA1979: Do you smoke or have you ever smoked?
Values:
1=Never
2=Sometimes
3=Daily
4=Stopped smoking
 
Version B/C/E: AA1355: Have you ever smoked?
Values:
1=No
2=Yes
 
Version A/B/C/E: AA1356: Do you smoke now (after you became pregnant)?
Values:
1=No
2=Sometimes
3=Daily
4=No+Sometimes
5=No+Daily
6=Sometimes+Daily
 
AA1357: Number of cigaretter per week
 
AA1358: Number of cigaretter per day
*/ 

use "Q1.dta", clear

generate cig_daily_1=AA1358
 
*Cigarettes/week
generate cig_weekly=AA1357/7

/*cig_daily_1=cig_weekly if: 
1. the respondents has not reported cigarettes per day in (AA1358) and cig_weekly is not missing, or
2. the respondent has indicated a larger amount of cigarettes per day in AA1357 than in AA1358. 
*/
replace cig_daily_1=cig_weekly if ((cig_daily_1==.|cig_daily_1==0) & cig_weekly!=.) | (cig_weekly>cig_daily_1 & cig_weekly!=.)  
 
/*cig_daily_1 is coded 0 if cig_daily_1 is missing and the respondent has answered "No"
for "Have you ever smoked?" or "No" for "Are you smoking now?" */
replace cig_daily_1=0 if cig_daily_1==. & (AA1979==1|AA1355==1|AA1356==1)

/*cig_daily_1 is coded missing if cig_daily_1=0 and the respodent has answered
"Sometimes", "Daily", "No + Sometimes", "No + Daily" or "Sometimes + Daily" 
when asked "Do you smoke now?*/ 
replace cig_daily_1=. if cig_daily_1==0 & inlist(AA1356, 2,3,4,5,6)

format cig_daily_1 %9.1f
 
drop cig_weekly

label variable cig_daily_1 "Number of cigarettes per day at time of questionnaire 1"

sum cig_daily_1


/*NUMBER OF CIGARETTES PER DAY AT TIME OF QUESTIONNAIRE 3

Third questionnaire

CC1037: Do you smoke at present?
Values:
1=No
2=Sometimes
3=Daily
4=No + Sometimes
5=Sometimes + Daily
 
CC1038: Cigarettes per week
 
CC1039: Cigarettes per day
*/

use "Q3.dta", clear

*Cigarettes/day
generate cig_daily_2=CC1039
 
*Cigarettes/week
generate cig_weekly=CC1038/7
 
replace cig_daily_2=cig_weekly if ((cig_daily_2==.|cig_daily_2==0) & cig_weekly!=.) | (cig_weekly>cig_daily_2 & cig_weekly!=.)  
 
format cig_daily_2 %9.1f
 
/*cig_daily_2 is coded 0 if cig_daily_2 is missing and respondent has answered "No"
for "Do you smoke at present?" */
replace cig_daily_2=0 if cig_daily_2==. & CC1037==1

/*cig_daily_2 is coded missing if cig_daily_2=0 and respondent has answered "Sometimes", 
"Daily", "No + Sometimes" or "Sometimes + Daily" when asked "Do you smoke at present?"*/
replace cig_daily_2=. if cig_daily_2==0 & inlist(CC1037, 2,3,4,5)

drop cig_weekly
label variable cig_daily_2 "cig_daily_2"

label variable cig_daily_2 "Number of cigarettes per day at time of questionnaire 3"

sum cig_daily_2


/*NUMBER OF CIGARETTES PER DAY IN THE LAST THREE MONTHS OF PREGNANCY 

Fourth questionnaire

DD732: What were your and your partner/husbands smoking habits during the last 3 months of your pregnancy and in the period
after the birth? 
Values:
1=Didn't smoke
2=Smoked sometimes
3=Smoked every day
4=Didn't smoke + Smoked sometimes
5=Didn't smoke + Smoked daily
6=Smoked sometimes + Smoked daily
7=Didn't smoke + Smoked sometimes + Smoked daily
 
DD1114: Cigarettes per week
 
DD738: Cigarettes per day
*/

use "Q4.dta", clear

/*Q4 includes one observation for each child. In multiple births information about number of cigarettes will in most cases 
  only be present for one child as the mother answered the quetions regarding herself only in one of the questionnaires. 
  To get information on all children in the same birth, the information needs to be duplicated. */  

*Cigarettes daily
generate cig_daily_3=DD738
 
*Cigarettes weekly
generate cig_weekly=DD1114/7

replace cig_daily_3=cig_weekly if ((cig_daily_3==.|cig_daily_3==0) & cig_weekly!=.) | (cig_weekly>cig_daily_3 & cig_weekly!=.)  
 
format cig_daily_3 %9.1f
 
*cig_daily_3 is coded 0 if the respondent has answered "Didn't smoke" for DD732
replace cig_daily_3=0 if cig_daily_3==. & DD732==1

/*cig_daily_3 is coded missing if cig_daily_3=0 and the respondent has answered "Smoked sometimes"
"Smoked every day", "Didn't smoke + Smoked sometimes", "Didn't smoke + Smoked daily", "Smoked sometimes + Smoked daily"
or "Didn't smoke + Smoked sometimes + Smoked daily" when asked for DD732*/
replace cig_daily_3=. if cig_daily_3==0 & inlist(DD732, 2,3,4,5,6,7)

drop cig_weekly

label variable cig_daily_3 "cig_daily_3"

label variable cig_daily_3 "Number of cigarettes per day in the last part of pregnancy"

sum cig_daily_3
```

##### MoBa is not responsible for any errors in the study results that are caused by errors in code or documentation at the MoBa Wiki page.

```stata
```

##### MoBa is not responsible for any errors in the study results that are caused by errors in code or documentation at the MoBa Wiki page.

```stata
```

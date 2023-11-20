# Binge drinking in pregnancy

### Code in Stata
This syntax was written for the purpose of harmonizing data from MoBa with data from the Danish National Birth Cohort (DNBC) for the MOBAND CP study. The harmonization was regarded as partial and information was lost from MoBa when harmonizing. For more information, see comment in section [Alcohol consumption in pregnancy.md](alcohol%consumption%in%pregnancy.md)

[Coding of occupation](Coding%20of%20occupation%20and%20industry.md))

##### MoBa is not responsible for any errors in the study results that are caused by errors in code or documentation at the MoBa MediaWiki page.

```stata
/***************************************************************************************************
* Variable: Binge drinking
* Purpose: Generate a variable for binge drinking by use of data from Q1 and Q3
* Author: Ingeborg Forthun 
* Date: 08.09.2014 (version 8 of MoBa)
* Questionnaires used: Q1, Q3
* Variables used:
*      - Q1: AA2019 AA2021 AA2022 AA1452 AA1453 AA1454AA1462 AA1463
*      - Q3: CC1156 CC1157 CC1158 CC1159 CC1160 CC1161 CC1162 CC1163
****************************************************************************************************/
 
/*
BINGE-DRINKING IN THE FIRST PART OF PREGNANCY (UP TIL QUESTIONNAIRE 1)
 
First questionnaire
 
AA2019: How often did you drink alcohol before you got pregnant?
Last 3 months before pregnancy
AA2020: During pregnancy
 
Values:
0=More than one box checked
1=Never
2=Less than once a month
3=Approximately 1-3 times a month
4=Approximately time a week
5=Approximately 2-3 times a week
6=Approximately 4-5 times a week
7=Approximately 6-7 times a week
 
AA2021: If you think back to the period you have been pregnant, also the start of the pregnancy, have 
you been drinking 5 unit or more of alcohol at least at one occasion?
Values:
0=More than one box checked
1=No
2=Yes
3=Do not know
 
AA2022: Number of times
 
AA1452: Have you ever consumed alcohol? 
Values:
1=No
2=Yes
 
AA1453: How often did you consume alcohol in the 3 months before you
became pregnant?
AA1454: How often do you consume alcohol during the
pregnancy?
 
Values (for both of the above):
1=Approximately 6-7 times a week
2=Approximately 4-5 times a week
3=Approximately 2-3 times a week
4=Approximately once a week
5=Approximately 1-3 times a month
6=Less than once a month
7=Never
 
AA1462: Did you drink 5 units or more at least once during the last three months before pregnancy?
AA1463: Did you drink 5 units or more at least once during pregnancy?
 
Values (for both of the above):
0=More than 1 check box filled in
1=Several times per week
2=Once a week
3=1-3 times a month
4=Less than once a month
5=Never
*/
 
use "Q1.dta", clear
 
generate binge_1=0 if (AA2021==1|AA2022==0|AA1463==5)
replace binge_1=1 if (AA2022==1|AA2022==2|AA1463==4)
replace binge_1=2 if (AA2022>=3 & AA2022!=.|AA1463==3|AA1463==2|AA1463==1)
 
/*binge_1 is coded 0 if binge_1=. and the respondent has answered: 
1) "No" when asked "Have you ever consumed alcohol?" (AA1452=1 or AA2020=1) 
2) "Never" when asked "How often do you consume alcohol during the pregnancy?" (AA1454=7)
3) "Never" when asked "How often did you consume alcohol in the 3 months before you
    became pregnant?" (AA1453=7 or AA2019=1)
4) "Never" when asked "Did you drink 5 units or more at least once during the last three months before pregnancy?" (AA1462=5)  
*/
 
replace binge_1=0 if ((AA1452==1|AA2020==1) & binge_1==.)|(AA1454==7 & binge_1==.)
replace binge_1=0 if ((AA1453==7|AA2019==1) & binge_1==.)|(AA1462==5 & binge_1==.)
 
label define binge_1 0 "0" 1 "1-2" 2 ">2"
label values binge_1 binge_1
 
label variable binge_1 "Number of episodes of binge-drinking up till questionnaire 1"
 
tab binge_1, missing
 
/*
BINGE-DRINKING IN THE FIRST PART OF PREGNANCY (UP TIL QUESTIONNAIRE 3)
 
Third questionnaire
 
CC1156: How often did you consume alcohol before and how
often do you consume it now? Last three months before last period
CC1157: Pregnancy week 0-12
CC1158: Pregnancy week 13-24
CC1159: Pregnancy week 25+
 
Values (for all the above):
1=Roughly 6-7 times a week 
2=Roughly 4-5 times a week 
3=Roughly 2-3 times a week 
4=Roughly 1 time a week 
5=Roughly 1-3 times a month
6=Less then once a month
7=Never
 
CC1160:In the period just before you became pregnant and during this
pregnancy, how many times have you consumed 5 units or
more of alcohol?
CC1161: Pregnancy week 0-12
CC1162: Pregnancy week 13-24
CC1163: Pregnancy week 25+
 
Values (for all the above):
0=More than 1 check box filled in
1=Several times a week
2=Once a week
3=1-3 times a month
4=Less than once a month
5=Never
*/
 
use "Q3.dta", clear
 
egen var1=anycount (CC1161 CC1162 CC1163), value (1)
egen var2=anycount (CC1161 CC1162 CC1163), value (2)
egen var3=anycount (CC1161 CC1162 CC1163), value (3)
egen var4=anycount (CC1161 CC1162 CC1163), value (4)
egen var5=anycount (CC1161 CC1162 CC1163), value (5)
 
generate binge_2=0 if (CC1161==5 & CC1162==5 & CC1163==5)
replace binge_2=1 if (var3==1|var4==1|var4==2)
replace binge_2=2 if (var1>0|var2>0|var3>1|var4>2)
 
/*binge_2 is coded 0 if the respondent has answered "Never" when asked "In the period just before you became pregnant 
  and during this pregnancy, how many times have you consumed 5 units or more of alcohol?" 
  in 0-12, 13-24 and 25+ weeks (CC1161=CC1162=CC1163=5).
  As some respondents might have answered "Never" for only one or two of the periods and 
  has missing on the remaining period(s) we have to construct a variable x that indicates if CC1161, CC1162 or CC1163 is 5 or missing.
*/
 
*x=1 if CC1161=CC1162=CC1163=. or CC1161=CC1162=CC1163=5 independent of the value of CC1160
generate x=1 if (CC1161==5|CC1161==.) & (CC1162==5|CC1162==.) & (CC1163==5|CC1163==.)
replace x=. if x==1 & CC1161==. & CC1162==. & CC1163==. & CC1160!=5 //x=1 only if at least one of CC1160, CC1161, CC1162 or CC1163 is 5
 
replace binge_2=0 if x==1 & binge_2==. 
 
/*binge_2 is coded 0 if the respondent has answered "Never" when asked "How often did you consume alcohol before and how
  often do you consume it now?" (CC1156=CC1157=CC1158=CC1159=7). As some respondents might have answered "Never" for only one or two of the period 
  and has missing on the remaining period(s) we construct a variable y that indicates if CC1156, CC1157, CC1158 or CC1159 is 7 or missing.
*/
 
*y=1 if CC1157=CC1158=CC1159=. or CC1157=CC1158=CC1159=7 independent of the value of CC1156
generate y=1 if (CC1157==7|CC1157==.) & (CC1158==7|CC1158==.) & (CC1159==7|CC1159==.)
replace y=. if y==1 & CC1156!=7 & CC1157==. & CC1158==. & CC1159==. //y=1 only if at least one of CC1156, CC1157, CC1158 or CC1159 is 7
 
replace binge_2=0 if y==1 & binge_2==.
 
label define binge_2 0 "0" 1 "1-2" 2 ">2"
label values binge_2 binge_2
 
drop var1-var5 x y 
 
label variable binge_2 "Number of episodes of binge-drinking up till questionnaire 3"
 
tab binge_2, missing
```

# Binge drinking in pregnancy

### Code in Stata
This syntax was written for the purpose of harmonizing data from MoBa with data from the Danish National Birth Cohort (DNBC) for the MOBAND CP study. The harmonization was regarded as partial and information was lost from MoBa when harmonizing. For more information, see comment above.

##### MoBa is not responsible for any errors in the study results that are caused by errors in code or documentation at the MoBa MediaWiki page.

/***************************************************************************************************

* Variable: Binge drinking
* Purpose: Generate a variable for binge drinking by use of data from Q1 and Q3
* Author: Ingeborg Forthun 
* Date: 08.09.2014 (version 8 of MoBa)
* Questionnaires used: Q1, Q3
* Variables used:
  ###### Q1: AA2019 AA2021 AA2022 AA1452 AA1453 AA1454AA1462 AA1463
  ###### Q3: CC1156 CC1157 CC1158 CC1159 CC1160 CC1161 CC1162 CC1163

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

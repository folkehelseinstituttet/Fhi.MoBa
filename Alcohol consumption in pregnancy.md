# Alcohol consumption in pregnancy

#### Table of Contents
- _[1. Alcohol consumption before and during pregnancy](#alcohol-consumption-before-and-during-pregnancy)_
    - _[1.1 Code in Stata](#s1---code-in-stata)_
    - _[1.2 Description of code](#description-of-code)_
 - _[2. Binge drinking in pregnancy](#binge-drinking-in-pregnancy)_
   - _[2.1 Code in Stata](#s2---code-in-stata)_

## Alcohol consumption before and during pregnancy
### S1 - Code in Stata
The code below was written for the purpose of harmonizing data from MoBa with data from the Danish National Birth Cohort (DNBC) for the MOBAND CP study. The harmonization was regarded as partial as information was lost from MoBa when harmonizing, and due to the substantial amount of missing for the generated variables in MoBa. The questionnaires in MoBa includes information about how often the respondents consume alcohol and how many units of alcohol they usually consume each time. This information was recoded to construct a mesaure of total alcohol consumption per week as this harmonized better with the questions in the DNBC. Therefore, not all information was kept from MoBa when harmonizing the data. Some women might have reported either how often they drink alcohol during pregnancy or how much they drink each time. These women were coded missing as we did not have information on both and therefore could not construct a measure of consumption per week.
The suggested code for variables for alcohol consumption during pregnancy (alcohol_1, alcohol_2 and alcohol_3) gives weekly consumption of alcohol at time of filling out the questionnaire. Below is suggested code for binge-drinking. The code for binge-drinking generate variables that give number of episodes of binge-drinking so far in pregnancy. A number of women have had one or more binge-drinking episodes during pregnancy, but report that they do not drink alcohol at present when they respond to the interview/questionnaire. These women will be coded 0 for alcohol consumption, but "1-2 episodes" or "More than 2 episodes" for binge-drinking. Because of this, one may consider to combine the variables for alcohol consumption and binge-drinking if one wants to include alcohol consumption during pregnancy as a variable(s) in the analyses.

### Description of code
All respondents who have reported that they "Never" drink alcohol (during pregnancy) in AA1454 or AA2020 are coded "No" for alcohol_1 (alcohol consumption per week at time of questionnaire 1) even though a few of them have reported number of units usually consumed during pregnancy in AA1465 or AA2024. Of these, more then 99 percent have answered "Less than 1". We interpret this inconsistency as no alcohol consumption. This as "Less than 1" is the lowest category for this question and "0" is not an answer category. We trust the respondents' first answer in AA1454/AA2020 ("Never") the most. <br>

There is a substantial amount of missing on the generated variable (alcohol_1). This is mainly due to the following:
1. Some women have reported how often they drink in AA1454/AA2020 or how much they drink at each occasion in AA1465/AA2024. But as they have not reported both it is not possible to estimate consumption per week and therefore they are coded missing for alcohol_1.
2. As there is no direct questions asking the respondents whether they drink in pregnancy, and the question regarding alcohol consumption asks both about consumption before and in pregnancy, many women have only checked the boxes for alcohol consumption before prenancy. We believe that most of these women do not drink in pregnancy, and that this is the reason they have not answered. However, since they have not answered "Never" in AA1454/AA2020 we can not know for sure whether they do not drink in pregnancy. This problem is also present for alcohol_2 (alcohol consumption per week at time of questionnaire 3) and alcohol_3 (alcohol consumption per week in last part of pregnancy).
##### MoBa is not responsible for any errors in the study results that are caused by errors in code or documentation at the MoBa Wiki page.

```stata
/***************************************************************************************************
* Variable: Alcohol consumption
* Purpose: Generate a variable for alcohol consumption by use of data from Q1, Q3 and Q4
* Author: Ingeborg Forthun 
* Date: 02.09.2014 (version 8 of MoBa)
* Questionnaires used: First questionnaire (Q1), Third questionnaire (Q3), Fourth questionnaire (Q4)
* Variables used: Q1, AA2019 AA2020 AA2023 AA2024 AA1452 AA1453 AA1464 AA1465
*                 Q3, CC1156-CC1159 CC1164-CC1167
*                 Q4, DD774 DD777
****************************************************************************************************/

/*
ALCOHOL CONSUMPTION BEFORE PREGNANCY
 
First questionnaire
 
AA2019: How often did you drink alcohol before you got pregnant?
 
Last 3 months before pregnancy
Values:
0=More than one box checked
1=Never
2=Less than once a month
3=Approximately 1-3 times a month
4=Approximately time a week
5=Approximately 2-3 times a week
6=Approximately 4-5 times a week
7=Approximately 6-7 times a week
 
AA2023: How many units of alcohol do you usually drink when you
consume alcohol?
Last 3 months before pregnancy
Values:
0=More than one box checked
1=Less than 1
2=1-2
3=3-4
4=5-6
5=7-9
6=10 or more
 
AA1452: Have you ever consumed alcohol?
Values:
1=No
2=Yes
 
AA1453: How often did you consume alcohol in the 3 months before you
became pregnant?
Values:
1=Approximately 6-7 times a week
2=Approximately 4-5 times a week
3=Approximately 2-3 times a week
4=Approximately once a week
5=Approximately 1-3 times a month
6=Less than once a month
7=Never
 
AA1464: How many units of alcohol do you usually drink when you
consume alcohol?
Last 3 months before pregnancy
Values:
1=10 or more
2=7-9
3=5-6
4=3-4
5=1-2
6=Less than 1
*/
 
use "Q1.dta", clear
 
tempvar number_times_0 number_units_0
 
*Number of times per week the respondent has consumed alcohol before pregnancy
generate `number_times_0'=0 if AA1453==7|AA2019==1 
replace `number_times_0'=6.5 if AA1453==1|AA2019==7 //6-7 times a week is coded 6.5
replace `number_times_0'=4.5 if AA1453==2|AA2019==6 //4-5 times a week is coded 4.5
replace `number_times_0'=2.5 if AA1453==3|AA2019==5 // 2-3 times a week is coded 2.5
replace `number_times_0'=1 if AA1453==4|AA2019==4
replace `number_times_0'=0.5 if AA1453==5|AA2019==3 //1-3 times a month is coded 0.5 
replace `number_times_0'=0.25 if AA1453==6|AA2019==2 //Less than once a month is coded 0.25
 
*Number of units the respondent usually drink when consuming alcohol
generate `number_units_0'=0.5 if AA1464==6|AA2023==1 //Less than 1 is coded 0.5
replace `number_units_0'=1.5 if AA1464==5|AA2023==2 //1-2 is coded 1.5
replace `number_units_0'=3.5 if AA1464==4|AA2023==3 //3-4 is coded 3.5
replace `number_units_0'=5.5 if AA1464==3|AA2023==4 //5-6 is coded 5.5
replace `number_units_0'=8 if AA1464==2|AA2023==5 //7-9 is coded 8
replace `number_units_0'=10 if AA1464==1|AA2023==6 //10 or more is coded 10
 
*Weekly consumption
generate alcohol_0=`number_times_0'*`number_units_0'
 
/*alcohol_0 is coded 0 if alcohol_0=. and:
1. The respondent has never consumed alcohol in the last three months before pregnancy (AA2019==1) and 
has missing on usual consumption before pregnancy (AA2023=.) (Version A). 
2. The respondent has answered "Never" when asked "Have you ever consumed alcohol?" (AA1452=1), and 
has missing on number of times (AA1453=.) and usual consumption in the last three months before 
pregnancy (AA1464=.) (Version B/C/D/E).
3. The respondent has answered "Never" when asked "How often did you consume alcohol in the 3 months 
before you became pregnant?" (AA1453=7) and has missing on usual consumption (AA1464=.) 
(Version B/C/D/E).   
*/
 
replace alcohol_0=0 if alcohol_0==. & (AA2019==1 & AA2023==.)|(AA1452==1 & AA1453==. & AA1464==.) ///
|(AA1453==7 & AA1464==.)
 
label define alcohollab 65 ">=65" //maximum alcohol consumption in MoBa is 65 units/week
label values alcohol_0 alcohollab 
 
label variable alcohol_0 "Alcohol consumption per week before pregnancy"
 
sum alcohol_0
 
/*
ALCOHOL CONSUMPTION AT TIME OF INTERVIEW 1/QUESTIONNAIRE 1
 
First questionnaire
 
AA2020: Hvor ofte drikker du alkohol i svangerskapet?
Values:
0=Mer enn et kryss
1=Aldri
2=Sjeldnere enn 1 gang pr måned
3=Omtrent 1-3 ganger pr måned
4=Omtrent 1 gang pr uke
5=Omtrent 2-3 ganger pr uke
6=Omtrent 4-5 ganger pr uke
7=Omtrent 6-7 ganger pr uke
 
AA1452: Have you ever consumed alcohol?
Values:
1=No
2=Yes
 
AA1454: How often do you consume alcohol during the
pregnancy?
Values:
1=Approximately 6-7 times a week
2=Approximately 4-5 times a week
3=Approximately 2-3 times a week
4=Approximately once a week
5=Approximately 1-3 times a month
6=Less than once a month
7=Never
 
AA2024: Hvor mange enheter drikker du vanligvis når du nyter alkohol?
Values:
0=Mer enn et kryss
1=Færre enn 1
2=1-2
3=3-4
4=5-6
5=7-9
6=10 eller flere
 
AA1465: How many units of alcohol do you usually drink when you
consume alcohol?
Values:
1=10 or more
2=7-9
3=5-6
4=3-4
5=1-2
6=Less than 1
*/
 
use "Q1.dta", clear
 
tempvar number_times_1 number_units_1
 
*Number of times per week the respondent has consumed alcohol during pregnancy
generate `number_times_1'=0 if AA1454==7|AA2020==1 
replace `number_times_1'=6.5 if AA1454==1|AA2020==7 //6-7 times a week is coded 6.5
replace `number_times_1'=4.5 if AA1454==2|AA2020==6 //4-5 times a week is coded 4.5
replace `number_times_1'=2.5 if AA1454==3|AA2020==5 // 2-3 times a week is coded 2.5
replace `number_times_1'=1 if AA1454==4|AA2020==4
replace `number_times_1'=0.5 if AA1454==5|AA2020==3 //1-3 times a month is coded 0.5 
replace `number_times_1'=0.25 if AA1454==6|AA2020==2 //Less than once a month is coded 0.25
 
*Number of units the respondent usually drink when consuming alcohol
generate `number_units_1'=0.5 if AA1465==6|AA2024==1 //Less than 1 is coded 0.5
replace `number_units_1'=1.5 if AA1465==5|AA2024==2 //1-2 is coded 1.5
replace `number_units_1'=3.5 if AA1465==4|AA2024==3 //3-4 is coded 3.5
replace `number_units_1'=5.5 if AA1465==3|AA2024==4 //5-6 is coded 5.5
replace `number_units_1'=8 if AA1465==2|AA2024==5 //7-9 is coded 8
replace `number_units_1'=10 if AA1465==1|AA2024==6 //10 or more is coded 10
 
*Weekly consumption
generate alcohol_1=`number_times_1'*`number_units_1'
 
/*alcohol_1 is coded 0 if alcohol_1=. and:
1. The respondent has never consumed alcohol in this pregnancy (AA2020=1) and 
has missing on usual consumption in pregnancy (AA2024=.) (Version A). 
2. The respondent has answered "Never" when asked "Have you ever consumed alcohol?" (AA1452=1), 
and has missing on number of times (AA1454=.) and usual consumption in pregnancy (AA1465=.) 
(Version B/C/D/E).
3. The respondent has answered "Never" when asked "How often do you consume alcohol during 
the pregnancy?" (AA1454=7) and has missing on usual consumption (AA1465=.) (Version B/C/D/E).
4. The respondent has ansered "Never" when asked "How often did you consume alcohol in the 3 months 
before you became pregnant?" (AA1453=7 or AA2019=1), and has missing on usual consumption in 
pregnancy (AA1465=. or AA2024=.).   
*/
 
replace alcohol_1=0 if alcohol_1==. & (AA2020==1 & AA2024==.)|(AA1452==1 & AA1454==. & AA1465==.) ///
|(AA1454==7 & AA1465==.)
replace alcohol_1=0 if (AA1453==7 & AA1465==. | AA2019==1 & AA2024==.) 
 
label values alcohol_1 alcohollab 
 
label variable alcohol_1 "Alcohol consumption per week at time of questionnaire 1"
 
sum alcohol_1
 
/*
ALCOHOL CONSUMPTION AT TIME OF INTERVIEW 2/QUESTIONNAIRE 3
 
Third questionnaire
 
CC1156: How often did you consume alcohol before and how
often do you consume it now? Last 3 months before last period
CC1157: Week of pregnancy 0-12
CC1158: Week of pregnancy 13-24
CC1159: Week of pregnancy 25+
 
Values (for all the above):
0=More than 1 check box filled in
1=Roughly 6-7 times a week 
2=Roughly 4-5 times a week 
3=Roughly 2-3 times a week 
4=Roughly 1 time a week 
5=Roughly 1-3 times a month
6=Less then once a month
7=Never
 
CC1164: How many units do you usually drink when you consume
alcohol? Last 3 months before last period
CC1165: Week of pregnancy 0-12
CC1166: Week of pregnancy 13-24
CC1167: Week of pregnancy 25+
 
Values (for all the above):
1=10 or more
2=7-9
3=5-6
4=3-4
5=1-2
6=Less than 1
*/
 
use "Q3.dta", clear
 
tempvar number_times_2 number_units_2
 
*Number of times per week the respondent has consumed alcohol during pregnancy
generate `number_times_2'=0 if CC1159==7 
replace `number_times_2'=6.5 if CC1159==1 //6-7 times a week is coded 6.5
replace `number_times_2'=4.5 if CC1159==2 //4-5 times a week is coded 4.5
replace `number_times_2'=2.5 if CC1159==3 // 2-3 times a week is coded 2.5
replace `number_times_2'=1 if CC1159==4
replace `number_times_2'=0.5 if CC1159==5 //1-3 times a month is coded 0.5 
replace `number_times_2'=0.25 if CC1159==6 //Less than once a month is coded 0.25
 
*Number of units the respondent usually drink when consuming alcohol
generate `number_units_2'=0.5 if CC1167==6 //Less than 1 is coded 0.5
replace `number_units_2'=1.5 if CC1167==5 //1-2 is coded 1.5
replace `number_units_2'=3.5 if CC1167==4 //3-4 is coded 3.5
replace `number_units_2'=5.5 if CC1167==3 //5-6 is coded 5.5
replace `number_units_2'=8 if CC1167==2 //7-9 is coded 8
replace `number_units_2'=10 if CC1167==1 //10 or more is coded 10
 
*Weekly consumption
generate alcohol_2=`number_times_2'*`number_units_2'
 
/*alcohol_2 is coded 0 if the respondent answers "Never" when asked "How often do you
consume alcohol now?" (CC1159=7) and if they have not reported any number of units of alcohol 
in week 25+ (CC1167=.)*/
replace alcohol_2=0 if (CC1159==7 & CC1167==.)
 
/*alcohol_2 is coded 0 if the respondent answers "Never" when asked "How often did you
consume alcohol before?" 3 months before pregnancy, in week 0-12 and in week 13-24 
(CC1156=CC1157=CC1158=7), and has not reported any number of units of alcohol in week 25+ (CC1167=.).
As some respondents might have answered "Never" for only one or two of the periods and has missing on 
the remaining period(s) we have to construct a variable x that indicates if CC1156, CC1157 or CC1158 
is 7 or missing.
*/
 
generate x=1 if (CC1156==7|CC1156==.) & (CC1157==7|CC1157==.) & (CC1158==7|CC1158==.) & alcohol_2==. & CC1167==.
 
*x=1 only if at least one of CC1157, CC1158 or CC1159 is 7
replace x=. if x==1 & CC1156==. & CC1157==. & CC1158==.
replace alcohol_2=0 if x==1 
 
drop x
 
label define alcohollab 65 ">=65" //maximum alcohol consumption in MoBa is 65 units/week
label values alcohol_2 alcohollab 
 
label variable alcohol_2 "Alcohol consumption per week at time of questionnaire 3"
 
sum alcohol_2

/*
ALCOHOL CONSUMPTION IN THE LAST PART OF PREGNANCY (FROM WEEK 30)
 
Fourth questionnaire
 
DD774: How often did you drink alcohol during the last 3 months of your pregnancy?
Values:
1=Roughly 6-7 times a week
2=Roughly 4-5 times a week 
3=Roughly 2-3 times a week 
4=Roughly once a week 
5=Roughly 1-3 times a month 
6=Less often than once a month
7=Never
 
DD777: How many units of alcohol do you usually drink when you
consume alcohol? Values:
0=More than one answer
1=10 or more
2=7-9
3=5-6
4=3-4
5=1-2
6=Less than 1
*/
 
use "Q4.dta", clear
 
tempvar number_times_3 number_units_3
 
*Number of times per week the respondent has consumed alcohol during pregnancy
generate `number_times_3'=0 if DD774==7 
replace `number_times_3'=6.5 if DD774==1 //6-7 times a week is coded 6.5
replace `number_times_3'=4.5 if DD774==2 //4-5 times a week is coded 4.5
replace `number_times_3'=2.5 if DD774==3 // 2-3 times a week is coded 2.5
replace `number_times_3'=1 if DD774==4
replace `number_times_3'=0.5 if DD774==5 //1-3 times a month is coded 0.5 
replace `number_times_3'=0.25 if DD774==6 //Less than once a month is coded 0.25
 
*Number of units they usually drink when consuming alcohol
generate `number_units_3'=0.5 if DD777==6 //Less than 1 is coded 0.5
replace `number_units_3'=1.5 if DD777==5 //1-2 is coded 1.5
replace `number_units_3'=3.5 if DD777==4 //3-4 is coded 3.5
replace `number_units_3'=5.5 if DD777==3 //5-6 is coded 5.5
replace `number_units_3'=8 if DD777==2 //7-9 is coded 8
replace `number_units_3'=10 if DD777==1 //10 or more is coded 10
 
*Weekly consumption
generate alcohol_3=`number_times_3'*`number_units_3'
 
/*alcohol_3 is coded 0 if the respondent anwers "Never" when asked "How often did you 
drink alcohol during the last 3 months of your pregnancy?" (DD774=7) and if the respondent has not 
reported any number of units of alcohol (DD777=.).*/
 
replace alcohol_3=0 if alcohol_3==. & (DD774==7 & DD777==.)
 
label define alcohollab 65 ">=65" //maximum alcohol consumption in MoBa is 65 units/week
label values alcohol_3 alcohollab 
 
label variable alcohol_3 "Alcohol consumption per week in the last part of pregnancy"
 
sum alcohol_3
```

## Binge drinking in pregnancy
### S2 - Code in Stata
This syntax was written for the purpose of harmonizing data from MoBa with data from the Danish National Birth Cohort (DNBC) for the MOBAND CP study. The harmonization was regarded as partial and information was lost from MoBa when harmonizing. For more information, see comment above.

##### MoBa is not responsible for any errors in the study results that are caused by errors in code or documentation at the MoBa Wiki page.
```stata
/***************************************************************************************************
* Variable: Binge drinking
* Purpose: Generate a variable for binge drinking by use of data from Q1 and Q3
* Author: Ingeborg Forthun 
* Date: 08.09.2014 (version 8 of MoBa)
* Questionnaires used: Q1, Q3
* Variables used: Q1, AA2019 AA2021 AA2022 AA1452 AA1453 AA1454AA1462 AA1463
*                 Q3, CC1156 CC1157 CC1158 CC1159 CC1160 CC1161 CC1162 CC1163
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

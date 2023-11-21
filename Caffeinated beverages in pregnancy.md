# Caffeinated beverages in pregnancy

#### Table of Contents
- _[1. Number of cups and caffeine in mg](#number-of-cups-and-caffeine-in-mg)_ <br>
  - _[1.1. Code in Stata](#code-in-stata)_ <br>

## Number of cups and caffeine in mg
### Code in Stata
This code was written for the purpose of harmonizing data from MoBa with data from the Danish National Birth Cohort (DNBC) for the MOBAND CP study. The harmonization was regarded as complete for cups of coffee and cups of tea as all information from MoBa was kept when harmonizing the data. The harmonization was regarded as partial for weekly consumption of Coca Cola, Pepsi etc. as more detailed information about number of cups of Coca Cola, Pepsi etc. per day in MoBa was lost when harmonizing the data. In the DNBC, only information on weekly, and not daily, consumption of Coca Cola, Pepsi etc. was available and a variable for weekly consuption of coke was therefore generated. As 1 cup a day of Coca Cola, Pepsi etc. is the smallest amount the respondents can give in the questionnaires in MoBa, weekly consumption of coke given by coke_1 and coke_2 and intake of caffeine in mg from coke might be underestimated when using the code below.

The calculation of caffeine consumption in mg was based on references given in Sengpiel et al. (2013), BMC Med. In this study, they use the Food Calc and the [Norwegian Food Composition Table](https://www.matvaretabellen.no/) to calculate mg of caffeine.

##### MoBa is not responsible for any errors in the study results that are caused by errors in code or documentation at the MoBa Wiki page.
```stata
/***************************************************************************************************
* Variable: Caffeine consumption during pregnancy 
* Purpose: Generate variables for consumption of caffeinated beverages
* Author: Ingeborg Forthun, Ingeborg.Forthun@fhi.no 
* Published: 09.09.2014 (version 8 of MoBa)
* Data used: First questionnaire (Q1) and third questionnaire (Q3)
* Variables used: Q1, AA1378 AA1379 AA1381 AA1382 AA1384 AA1385 AA1387 AA1388 AA1393 AA1394 AA1399 AA1400
*                 Q3, CC1119 CC1120 CC1121 CC1122 CC1123 CC1124 CC1125 CC1126 CC1127 CC1128 CC1129 CC1130 
*                     CC1133 CC1134 
****************************************************************************************************/

 
/*
CAFFEINE CONSUMPTION AT THE TIME OF THE FIRST QUESTIONNAIRE/INTERVIEW
 
First questionnaire

What was your fluid consumption (number of cup/glasses) per day during pregnancy? 
 
AA1378: Filter coffee
AA1379: Decaffeinated
AA1381: Instant coffee
AA1382: Decaffeinated
AA1384: Boiled coffee
AA1385: Decaffeinated
AA1387: Tea
AA1388: Decaffeinated
AA1393: Coca Cola/Pepsi etc.
AA1394: Decaffeinated
AA1399: Diet Coca Cola/Pepsi
AA1400: Decaffeinated
*/
 
use "Q1.dta", clear
 
*Decaff is not included here (the questionnaire includes information about consumption of decaffeinated coffee). 
*The respondent marks the boxes AA1379, AA1382 etc if the drink is decaffeinated. 
generate cups_filter_1=AA1378 if AA1379==.
generate cups_instant_1=AA1381 if AA1382==.
generate cups_boiled_1=AA1384 if AA1385==.
 
egen cups_coffee_1=rowtotal(cups_filter_1 cups_instant_1 cups_boiled_1)
 
/*If consumption is reported by a mark but no amount is given (variable is coded 99), cups_coffee_1, 
cups_tea_1 and coke_1 is coded missing.*/
 
replace cups_coffee_1=. if (AA1378==99|AA1381==99|AA1384==99)
 
label variable cups_coffee_1 "Cups of coffee per day at time of questionnaire 1"
 
sum cups_coffee_1
 
*Cups of tea/daily
generate cups_tea_1 = AA1387 if AA1388==. & AA1387!=99
 
replace cups_tea_1=0 if cups_tea_1==. & AA1387!=99
 
label variable cups_tea_1 "Cups of tea per day at time of questionnaire 1"
 
sum cups_tea_1

*Cups of coke/daily
generate cups_coke_regular_1=AA1393 if AA1394==.
generate cups_coke_diet_1=AA1399 if AA1400==.
 
egen cups_coke_1=rowtotal(cups_coke_regular_1 cups_coke_diet_1) //total daily consumption
 
replace cups_coke_1=. if (AA1393==99|AA1399==99)
  
label variable cups_coke_1 "Cups of coke per week at time of questionnaire 1"
 
sum cups_coke_1
drop cups_coke_regular_1 cups_coke_diet_1 
 
*Caffeine in mg/1 dl
*1 cup is defined as 1,25 dl
 
*Coffee filtered: 57 mg/1 dl
*Instant coffee: 40 mg/1 dl
*Black tea: 16 mg/1 dl
*Herbal tea: 0 mg/1 dl
*Coke: 12 mg/1 dl
 
*Caffeine in mg from coffee
generate coffee_filter_mg_1=cups_filter_1*57*1.25   //Number of cups*57 mg (per 1 dl)*1,25 dl
generate coffee_boiled_mg_1=cups_boiled_1*57*1.25 
generate coffee_instant_mg_1=cups_instant_1*40*1.25
 
drop cups_filter_1 cups_boiled_1 cups_instant_1
 
*Caffeine in mg from tea
generate tea_mg_1=cups_tea_1*16*1.25 
 
/*Caffeine in mg from coffee and tea
egen mg_caffeine_1=rowtotal(coffee_filter_mg_1 coffee_boiled_mg_1 coffee_instant_mg_1 tea_mg_1)
replace mg_caffeine_1=. if mg_caffeine==0 & (cups_coffee_1==.|cups_tea_1==.)
*/

*Caffeine in mg from coke
generate coke_mg_1=cups_coke_1*12*1.25
  
*Caffeine in mg from coffee, tea and coke
egen mg_caffeine_1=rowtotal(coffee_filter_mg_1 coffee_boiled_mg_1 coffee_instant_mg_1 tea_mg_1 coke_mg_1)
 
replace mg_caffeine_1=. if mg_caffeine==0 & (cups_coffee_1==.|cups_tea_1==.|cups_coke_1==.)
 
label variable mg_caffeine_1 "Daily intake of caffeine at time of questionnaire 1"
 
sum mg_caffeine_1
 
/*
CAFFEINE CONSUMPTION AT TIME OF THE THIRD QUESTIONNAIRE
 
Third questionnaire
 
What was your fluid consumption (number of cup/glasses) per day after the 13th week of pregnancy? 
 
CC1119: Filter coffee
CC1120: Decaffeinated
CC1121: Instant coffee
CC1122: Decaffeinated
CC1123: Boiled coffee
CC1124: Decaffeinated
CC1125: Other coffee
CC1126: Decaffeinated
CC1127: Tea
CC1128: Decaffeinated
CC1129: Coca Cola/Pepsi etc.
CC1130: Decaffeinated
CC1133: Diet Coca Cola/Pepsi
CC1134: Decaffeinated
*/
 
use "Q3.dta", clear
 
*cups of coffee/daily
generate cups_filter_2=CC1119 if CC1120==.
generate cups_boiled_2=CC1123 if CC1124==.
generate cups_instant_2=CC1121 if CC1122==.
generate cups_other_2=CC1125 if CC1126==. 
 
egen cups_coffee_2=rowtotal(cups_filter_2 cups_instant_2 cups_boiled_2 cups_other_2)
 
/*If consumption is reported by a mark but no amount is given (variable is coded 99), cups_coffee_2, 
cups_tea_2 and coke_2 is coded missing.*/
 
replace cups_coffee_2=. if (CC1119==99|CC1123==99|CC1121==99|CC1125==99)
 
label variable cups_coffee_2 "Cups of coffee per day at time of questionnaire 3"
 
sum cups_coffee_2
 
*cups of tea/daily
generate cups_tea_2 = CC1127 if CC1128==. & CC1127!=99
 
replace cups_tea_2=0 if cups_tea_2==. & CC1127!=99
 
label variable cups_tea_2 "Cups of tea per day at time of questionnaire 3"
 
sum cups_tea_2
 
*cups of coke/daily 
generate cups_coke_regular_2=CC1129 if CC1130==.
generate cups_coke_diet_2=CC1133 if CC1134==.
 
egen cups_coke_2=rowtotal(cups_coke_regular_2 cups_coke_diet_2) //total daily consumption
 
replace cups_coke_2=. if (CC1129==99|CC1133==99)
 
 
label variable cups_coke_2 "Weekly consumption of coke at time of questionnaire 3"

sum cups_coke_2

drop cups_coke_regular_2 cups_coke_diet_2
 
*Caffeine in mg
 
*Caffeine in mg from coffee
generate coffee_filter_mg_2=cups_filter_2*57*1.25
generate coffee_boiled_mg_2=cups_boiled_2*57*1.25 
generate coffee_instant_mg_2=cups_instant_2*40*1.25
generate coffee_other_mg_2=cups_other_2*57*1.25 
 
*Caffeine in mg from tea
generate tea_mg_2=cups_tea_2*16*1.25 
 
drop cups_filter_2 cups_boiled_2 cups_instant_2 cups_other_2
 
/*Caffeine in mg from coffee and tea
egen mg_caffeine_2=rowtotal(coffee_filter_mg_2 coffee_boiled_mg_2 coffee_instant_mg_2 /// 
coffee_other_mg_2 tea_mg_2)
 
replace mg_caffeine_2=. if mg_caffeine_2==0 & (cups_coffee_2==.|cups_tea_2==.)
*/
 
*Caffeine in mg from coke 
 
generate coke_mg_2=cups_coke_2*12*1.25
  
*Caffeine in mg from coffee, tea and coke
egen mg_caffeine_2=rowtotal(coffee_filter_mg_2 coffee_boiled_mg_2 coffee_instant_mg_2 ///
coffee_other_mg_2 tea_mg_2 coke_mg_2)
 
replace mg_caffeine_2=. if mg_caffeine_2==0 & (cups_coffee_2==.|cups_tea_2==.|cups_coke_2==.)
 
label variable mg_caffeine_2 "Daily intake of caffeine at time of questionnaire 3"
 
sum mg_caffeine_2

drop coke_mg_2 tea_mg_2 coffee_other_mg_2 coffee_instant_mg_2 coffee_boiled_mg_2 ///
coffee_filter_mg_2 cups_coke_2
```

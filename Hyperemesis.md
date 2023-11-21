# Hyperemesis

#### Table of Contents
- _[1. Code in Stata](#code-in-stata)_ <br>

## Code in Stata

##### MoBa is not responsible for any errors in the study results that are caused by errors in code or documentation at the MoBa Wiki page.

```stata
/***************************************************************************************************
* Variable: Hyperemesis
* Purpose: Generate a variable for hyperemesis by use of data from Q3
* Author: Ingeborg Forthun 
* Date: 22.09.2014 (version 8 of MoBa)
* Questionnaires used: Third questionnaire (Q3)
* Variables used: CC137-CC145 
****************************************************************************************************/


/*
Third questionnaire
If yes, why and when were you hospitalised?
Prolonged nausea and vomiting
CC137: Yes
CC138: Week of pregnnacy 0-4
CC139: Week of pregnancy 5-8
CC140: Week of pregnnacy 9-12
CC141: Week of pregnancy week 13-16
CC142: Week of pregnancy week 17-20
CC143: Week of pregnancy 21-24
CC144: Week of pregnancy 25-28
CC145: Week of pregnancy 29+
*/

use "Q3.dta", clear

gen hyperemesis=0

*hyperemeis is coded 1 if at least one of CC137-CC145 is 1. 
foreach v of var CC137-CC145 {
replace hyperemesis=1 if `v'==1
}

label define yesno 0 "No" 1 "Yes"
label values hyperemesis yesno 
label variable hyperemesis "Hyperemesis" 

tab hyperemesis
```


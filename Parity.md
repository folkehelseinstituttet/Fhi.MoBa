# Parity

#### Table of Contents
- _[1. Code in SPSS](#code-in-spss)_ <br>
- _[2. Code in Stata](#code-in-stata)_ <br>

## Code in SPSS 

##### MoBa is not responsible for any errors in the study results that are caused by errors in code or documentation at the MoBa Wiki page.
```stata
/***************************************************************************************************
* Variable: Parity
* Purpose: Generate a variable for parity by use of data from Q1
* Author: Elin Alsaker, Elin.Alsaker@fhi.no
* Date: (version 8 of MoBa)
* Questionnaires used: First questionnaire (Q1)
****************************************************************************************************/

* "OUT" stands for "Outcome". Pregnancies with the following outcomes are included:
* a) live born or live born+spontaneous abortion/still birth.
* b) if gestational age>12: spontaneous abortion/still birth, terminated pregnancy, ectopic pregnancy, spontaneous abortion/still birth+ectopic pregnancy,
*    spontaneous abortion/still biirth+terminated pregnancy.
 
* E.g. If you only want to include pregnancies with gestional length>21 you can write "LEN>21" instead of "LEN>21" in the code below.

COMPUTE PARI_MB=0.

DO REPEAT 	    OUT = AA95 AA101 AA107 AA113 AA119 AA125 AA131 AA137 AA143 AA149
		 /  LEN = AA96 AA102 AA108 AA114 AA120 AA126 AA132 AA138 AA144 AA150.
	IF (OUT=1 OR OUT=5 OR ((OUT=2 OR OUT=3 OR OUT=4 OR OUT=6 OR OUT=7) AND LEN>12) ) PARI_MB=PARI_MB+1.
END REPEAT.

EXECUTE.
	
FREQUENCIES
  VARIABLES=PARI_MB
  /ORDER=  ANALYSIS .
```

## Code in Stata 
```stata
/***************************************************************************************************
* Variable: Parity
* Purpose: Generate a variable for parity by use of data from Q1
* Author: Ingeborg Forthun, Ingeborg.Forthun@fhi.no 
* Date: 02.09.2014 (version 8 of MoBa)
* Questionnaires used: First questionnaire (Q1)
* Variables used: AA93 AA95, AA101, AA107, AA113, AA119, AA125, AA131, AA137, AA143, AA149
*                 AA96, AA102, AA108, AA114, AA120, AA126, AA132, AA138, AA144, AA150 
****************************************************************************************************/



/*
AA93: Have you been pregnant before?
1=No
2=Yes

If yes, fill in for all earlier pregnancies. Include all pregnancies that ended in abortion, miscarriage
or stillbirth as well as ectopic pregnancies.
Outcome of pregnancy: AA95, AA101, AA107, AA113, AA119, AA125, AA131, AA137, AA143, AA149
Values:
0=More than 1 box checked
1=Live infant born
2=Spontaneous abortion/stillbirth 
3=Termination of pregnancy
4=Ectopic pregnancy
5=Liveborn+spontaneous abortion/stillbirth 
6=Spontaneous abortion/stillbirth+ectopic pregnancy 
7=Spontaneous abortion/stillbirth+termination of pregnancy
 
Week of pregnancy for abortion/stillbirth: AA96, AA102, AA108, AA114, AA120, AA126, AA132, AA138, AA144, AA150
Values:
1=Yes
*/

/*Definition:

Parity is defined as number of:
a) Liveborn or Liveborn+spontaneous abortion/stillbirth
b) if pregnancy week>12: Spontaneous abortion/stillbirth, Termination of pregnancy, Ectopic pregnancy, 
                         Spontaneous abortion/stillbirth+ectopic pregnancy and Spontaneous abortion/stillbirth+termination of pregnancy.  
*/

gen parity = 0

/*The loop below checks whether one of the variables AA95, AA101, AA107, AA113, AA119, AA125, AA131, AA137, AA143, AA149 (denoted by "j")
  has a value corresponding to the included outcomes AND the corresponding variable  
  AA96, AA102, AA108, AA114, AA120, AA126, AA132, AA138, AA144, AA150 (denoted by "J") has a value of 22 or above. 
  Every time the criterias are fulfilled, the loop will add 1 to the variable parity. 
*/

forval j = 95(6)149 {
        local J = `j' + 1 
replace parity = parity + (inlist(AA`j', 1, 5) | (inlist(AA`j', 2, 3, 4, 6, 7) ///
& inrange(AA`J', 22, .)))
} // inrange gives number of gestational weeks. If the chosen range is gestational weeks>=24 replace 22 with 24.  

/*Observations are coded missing if (this step is not part of the syntax in SPSS above): 
a) missing for AA93 and pregnancy outcome and 
b) "Yes" for "Have you been pregnant before?" (AA93=2) and missing for pregnancy outcome. 
*/

replace parity=. if (AA93==2|AA93==.) & AA95==. & AA101==. & AA107==. & AA113==. ///
& AA119==. & AA125==. & AA131==. & AA137==. & AA143==. & AA149==.

tab parity, missing

/*It is possible to use information on parity from the Medical Birth Registry of Norway (MBR) to check whether some of
  the missing cases are registered with parity=0 in MBR. If so, you might consider recoding them to parity=0 here as well. 
*/
```

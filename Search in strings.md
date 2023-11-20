# Search in strings

#### Table of Contents
- _[1 Code in SPSS](#code-in-spss)_ <br>
- _[2 Code in Stata](#code-in-stata)_ <br>

## Code in SPSS
##### MoBa is not responsible for any errors in the study results that are caused by errors in code or documentation at the MoBa Wiki page.
```stata
/***************************************************************************************************
* Variable: Cerclage
* Purpose: Identify pregnancies where the mother has been diagnosed with cerclage
* Author: Elin Alsaker, Elin.Alsaker@fhi.no 
* Date: (Version 8 of MoBa)
* Variables used: MORS_HELSE_UNDER
****************************************************************************************************/

/*Generate a variable called "cerclage" that is coded 1 if the diagnosis "O343" is part of the string variable MORS_HELSE_FOER 
  or MORS_HELSE_UNDER. The function INDEX in SPSS returns 0 if the text is not part of any of the string variables, and returns the 
  position if the specific text is part of the variables.  */

COMPUTE cerclage=( ( INDEX(MORS_HELSE_FOER,'o343') + INDEX(MORS_HELSE_UNDER,'o343') ) >0 ).
FREQ cerclage.

*Note that the function INDEX is case sensitiv. Therefore, the following will not give the same result:
COMPUTE cerclage2=( ( INDEX(MORS_HELSE_FOER,'O343') + INDEX(MORS_HELSE_UNDER,'O343') ) >0 ).
FREQ cerclage2.

*If you are uncertain whether to use upper or lower case letters you can use the function UPCASE. 
COMPUTE cerclage3=( ( INDEX(UPCASE(MORS_HELSE_FOER),'O343') + INDEX(UPCASE(MORS_HELSE_UNDER),'O343') ) >0 ).
FREQ cerclage3.
```

## Code in Stata
##### MoBa is not responsible for any errors in the study results that are caused by errors in code or documentation at the MoBa Wiki page.
```stata
/***************************************************************************************************
* Variable: Fever during labor
* Purpose: Identify pregnancies where the mother has been diagnosed with fever during labor
* Author: Ingeborg Forthun, Ingeborg.Forthun@fhi.no 
* Date: 08.09.2014 (version 8 of MoBa)
* Variables used: MORS_HELSE_UNDER
****************************************************************************************************/

* The function regexm in Stata returns 1 if the expressions in brackets is satisfied, otherwise it returns 0.

gen fever_labor=0 

*"DO752" is the ICD-10 code, while "XC77" is a check box on the notification of birth. 
replace fever_labor=1 if ((regexm(KOMPLIKASJONER, "DO752")|regexm(MOR_HELSE_UNDER, "7888") ///
|regexm(KOMPLIKASJONER, "XC77")|rexexm(MORS_HELSE_UNDER, "XC77"))
```

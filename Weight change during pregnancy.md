# Weight change during pregnancy

#### Table of Contents
- _[1. Weight gain in pregnancy](#weight-gain-in-pregnancy)_
    - _[1.1 Code in Stata](#1---code-in-stata)_
 - _[2. Weight loss in pregnancy](#weight-loss-in-pregnancy)_
   - _[2.1 Code in Stata](#s---code-in-stata)_

## Weight gain in pregnancy

### 1 - Code in Stata

##### MoBa is not responsible for any errors in the study results that are caused by errors in code or documentation at the MoBa Wiki page.
```stata
/***************************************************************************************************
* Variable: Weight gain during pregnancy
* Programmer: Ingeborg Forthun, Ingeborg.Forthun@fhi.no
* Published: 29.09.2014 (version 8 of MoBa)
* Questionnaires used: First questionnaire (Q1) and fourth questionnaire (Q4)
* Variables used: Q1, AA85 Q4, DD672
****************************************************************************************************/


/*
Q1
AA85 "What did you weigh at the time you became pregnant (in kilograms)?"

Q4
DD672 "How much did you weigh at the end of your pregnancy?"
*/

*Merge Q1 and Q4 to get weight before and after pregnancy so that weight gain can be measured. 

codebook DD672
generate weight_after=DD672

generate weight_before=AA85

*Weight in kilos lower than 35 and higher than 200 is coded missing
replace weight_after=. if (weight_after<35|weight_after>200) & weight_after!=.
replace weight_before=. if (weight_before<35|weight_before>200) & weight_before!=.

*Generate a variable for weight gain
generate weight_gain=weight_after-weight_before
replace weight_gain=. if weight_gain<0

*Rounded to nearest .1 decimal. 
replace weight_gain=round(weight_gain, 0.1)

drop weight_after weight_before 

sum weight_gain

*Add label 
label variable weight_gain "Weight gain during pregnancy"

*The substantial amount of missing is due to lower particpation rate for the fourth questionnaire compared to the first.
codebook weight_gain
```

## Weight loss in pregnancy

### 2 - Code in Stata

##### MoBa is not responsible for any errors in the study results that are caused by errors in code or documentation at the MoBa Wiki page.
```stata

```

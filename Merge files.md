# Merge files

#### Table of Contents
- _[1 Description of code](#description-of-code)_ <br>
	- _[1.1 Code in SPSS](#code-in-spss)_ <br>
	- _[1.2 Code in Stata](#code-in-stata)_ <br>

## Description of code
Considering the large amount of data, each questionnaire is delivered as separate data files. When merging the files for analysis, it is important to be aware of the following:
1.	The unit of observation in the files with data from the first (Q1, pregnancy week 17), second (Q2, FFQ) and third questionnaire (Q3, pregnancy week 30) is the pregnancy. The unit of observation in the files with data from the questionnaires after birth and the Medical Birth Registry of Norway (MBRN) is the child.
2.	All files include a unique identifier for the pregnancy. The variable is called ‘PREG_ID_XX’. This variable uniquely identifies observations in the files with data from the questionnaires before birth. In the files with data from questionnaires after birth and MBRN, the variables 'PREG_ID_XX' and 'BARN_NR' together uniquely identifies each observation. BARN_NR is the child's order at birth.
3.	When merging files with data before pregnancy (where the pregnancy is the unit of observation), use the variable 'PREG_ID_XX' as the key variable when merging. When merging a file with data from a questionnaire after birth or MBRN, use both 'PREG_ID_XX' and 'BARN_NR' as key variables to ensure that the data is linked correctly for twins/triplets.

In the code below syntax/code for merging MBRN, Q1 and Q3 is shown.

### Code in SPSS 
##### MoBa is not responsible for any errors in the study results that are caused by errors in code or documentation at the MoBa Wiki page.
```stata
/***************************************************************************************************
* Purpose: Merge the first (Q1) and the third (Q3) questionnaire with MFR 
* Author: Elin Alsaker, Elin.Alsaker@fhi.no 
* Last updated: 09.09.2014 (version 8 of MoBa)
****************************************************************************************************/

***************************************************************************
*Replace PREG_ID_XX with project specific identifier in the syntax below.
***************************************************************************

***************************************************************************
*Merge Q1 and MFR
***************************************************************************

GET FILE = "J:\MorbarnData\Versjon_v8\Data\MFR_370_v7_standard.sav" .
SORT CASES BY PREG_ID_XX BARN_NR.
DATASET NAME MFR.

GET FILE = "J:\MorbarnData\Versjon_v7\Data\Skjema1_v7_standard.sav" .
SORT CASES BY PREG_ID_XX.
DATASET NAME Skj1.

*****************************************************************************************************
*This is a one to many merge. The first questionnaire includes one row per pregnancy,
*while MFR includes more than one row for some pregnancies. In SPSS you use /TABLE= instead of /FILE=
*for the file including one row per pregnancy.
*****************************************************************************************************

MATCH FILES  /FILE=MFR  /IN = IMFR  
	      /TABLE=Skj1  /IN = ISkj1
	      /BY PREG_ID_XX .
EXECUTE.

CROSSTABS IMFR BY Iskj1.

DATASET NAME Kobl1.

***************************************************************************
*Merge with Q3.
***************************************************************************

GET FILE = "J:\MorbarnData\Versjon_v7\Data\Skjema3_v7_standard.sav" .
SORT CASES BY PREG_ID_XX.
DATASET NAME Skj3.

MATCH FILES   /FILE=Kobl1   /IN = IKobl1 
	       /TABLE=Skj3  /IN = ISkj3 
	       /BY PREG_ID_XX.
EXECUTE.

CROSSTABS IKobl1 BY Iskj3.

SAVE OUTFILE = "Z:\TMP\MB13MFR.SAV" /DROP IKobl1 ISkj1 ISkj3.

*************************************************************************************************************
*Generate a variable that indicated whether the respondent has responded to the specific questionnaire and is
*part of MBR.  
*************************************************************************************************************

GET FILE = "Z:\TMP\MB13MFR.SAV".
*Variabel for response for Q1.
RECODE VERSJON_SKJEMA1_TBL1 ('SKJEMA1A'=1) ('SKJEMA1B'=1) ('SKJEMA1C'=1)  INTO ISKJ1.

*Variabel for response for Q3.
RECODE VERSJON_SKJEMA3_TBL1 ('SKJEMA3A'=1) ('SKJEMA3B'=1) ('SKJEMA3C'=1)  INTO ISKJ3.

RECODE ISKJ1 ISKJ3 IMFR (SYSMIS=0).
EXECUTE.

VARIABLE LABELS ISKJ1 "Data from Q1".
VARIABLE LABELS ISKJ3 "Data from Q3".
VARIABLE LABELS IMFR "Data from MBR".

  CROSSTABS
  /TABLES=ISKJ1  BY ISKJ3  BY IMFR
  /FORMAT= AVALUE TABLES
  /CELLS= COUNT
  /COUNT ROUND CELL .

***************************************
*Save the file with the new variables.
***************************************

SAVE OUTFILE = "Z:\TMP\MB13MFR.SAV".

*************************************************************************************************************
* If you want to only include those who have responded to Q1 and who are included in MFR, 
* you can use the following command:
*************************************************************************************************************

SELECT IF (ISKJ1=1 AND IMFR=1).
EXECUTE.

******************************************************************************
*Save a file where only those with data in Q1 and MFR are included.
******************************************************************************

SAVE OUTFILE = "Z:\TMP\MB13MFR.SAV".

*************************************************************************************************************
*If you want to include only those who have responded to Q1, Q3 and MFR, you can use the following command:
*************************************************************************************************************

SELECT IF (ISKJ1=1 AND IMFR=1 AND ISKJ3=1).
EXECUTE.

******************************************************************************
*Save a file where only those with data in Q1, Q3 and MFR are included.
******************************************************************************

SAVE OUTFILE = "Z:\TMP\MB13MFR.SAV".
```

### Code in stata
##### MoBa is not responsible for any errors in the study results that are caused by errors in code or documentation at the MoBa Wiki page.
```stata
/***************************************************************************************************
* Purpose: Merge the first (Q1) and the third (Q3) questionnaire with MFR 
* Author: Ingeborg Forthun, Ingeborg.Forthun@fhi.no
* Last updated: 09.09.2014 (version 8 of MoBa)
****************************************************************************************************/

*Replace PREG_ID_XX with project specific identifier in the code below.
clear

*Directory/folder used -- Replace with the name of the folder where you have saved the files. 
cd "Z:\MoBa"

*Merge Q1 and MFR

use "MFR.dta", clear
 
sort PREG_ID_XX 

*Generate a variable that indicates whether there is data in MFR.
gen inMBR=1
 
save "MFR_merge.dta"
 
use "Q1.dta", clear
 
sort PREG_ID_XX 

*Generate a variable that indicates whether the respondent has responded to the first questionnaire. 
gen inQ1=1

/*This is a one to many merge (1:m) as PREG_ID_XX uniquely identifies the observations in Q1, 
  but not in MFR (a pregnancy can result in more than one child). */
merge 1:m PREG_ID_XX using "MFR_merge.dta"

tab _merge //_merge indicates whether the pregnancy is only in Q1, only in MFR, or in both. 

drop _merge

*Save the file

save "Q1MFR.dta", replace
 
*Merge Q1 and MFR with Q3
  
use "Q3.dta", clear
 
sort PREG_ID_XX

*Generate a variable that indicates whether the respondent has answered Q3.
gen inQ3=1
 
merge 1:m PREG_ID_XX using "Q1MFR.dta"

drop _merge

*Save file

save "MFRQ1Q3.dta", replace

*If you want to only include those who have responded to Q1 and who are included in MFR, you can use the following command:

keep if inQ1==1 & inMBR==1

*If you want to only include those who have responded to Q1, Q3 and are included in MFR, you can use the following command:

keep if inQ1==1 & inQ3==1 & MFR==1
```

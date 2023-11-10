# Response to questionnaires

- Select the first pregnancy for each woman
    - [S1: Code in SPSS](###S1-Code-in-SPSS)
    - [S2: Code in Stata](#S2-Code-in-Stata)
- Select those who have responded to all questionnaires
    - [S3: Code in SPSS](###S3-Code-in-SPSS)
  
## Select the first pregnancy for each woman

### S1 Code in SPSS 
```
/***************************************************************************************************
* Purpose: Select the first pregnancy for each woman 
* Author: Elin Alsaker, Elin.Alsaker@fhi.no 
* Last updated: 09.09.2014 (version 8 of MoBa)
****************************************************************************************************/

**********************************************************************************************
* Replace PREG_ID_XX and M_ID_XX with the project specific identifiers in the syntax below.
**********************************************************************************************

* This syntax selects the pregnancy with the smallest PREG_ID_XX for each woman. 
* This is not necessarily the first pregnancy. If you want to make sure of this,
* you have to sort by birth date instead of by PREG_ID_XX (assuming that you have a file including birth date for the child),
* or you will have to sort on date of filling out questionnaire (either on date or year depending on information available). 

* In order to identify women with more than one pregnancy you have the merge the file in question with the SV_INFO file.

* Start by sorting the specific file (active file).
SORT CASES BY PREG_ID_XX.

*The file SV_INFO should already be sorted by MoBa.

*Merge the open/active file with SV_INFO.

MATCH FILES /FILE=*  /IN=IN_1_3
 /FILE='J:\MorbarnData\Versjon_v3\Data\Norsk\SPSS\SV_INFO_v3.sav'
 /IN=INsv_info
 /BY PREG_ID_XX.
VARIABLE LABELS INsv_info
 'Case source is J:\MorbarnData\Versjon_v2\Data\SPSS\SV_INFO.sav'.
EXECUTE.

*Include only observations that are in both files.
SELECT IF (IN_1_3=1 & INsv_info=1).
EXECUTE.

*Sort by identifier of the mother (M_ID_XX) and the pregnancy (PREG_ID_XX=).
SORT CASES BY
  M_ID_XX (A) PREG_ID_XX (A) .

*Identify duplicates (twins and triplets).
MATCH FILES  /FILE=* 
	/BY M_ID_XX
	/FIRST = FORSTE  /LAST = SISTE.
COMPUTE DUPLIKAT = 0.
IF (FORSTE ~= SISTE OR FORSTE =0 ) DUPLIKAT =1.
EXECUTE.

*Select only the first record.
SELECT IF FORSTE=1.
EXECUTE.

*Frequency count on duplicates (1=duplicate - more than one pregnancy 0=only one pregnancy).
FREQ DUPLIKAT .
```

# Code in Stata

## Select those who have responded to all questionnaires
### S3 Code in SPSS

# Merge files
## Description of syntax
Considering the large amount of data, each questionnaire is delivered as separate data files. When merging the files for analysis, it is important to be aware of the following:
1.	The unit of observation in the files with data from the first (Q1, pregnancy week 17), second (Q2, FFQ) and third questionnaire (Q3, pregnancy week 30) is the pregnancy. The unit of observation in the files with data from the questionnaires after birth and the Medical Birth Registry of Norway (MBRN) is the child.
2.	All files include a unique identifier for the pregnancy. The variable is called ‘PREG_ID_XX’. This variable uniquely identifies observations in the files with data from the questionnaires before birth. In the files with data from questionnaires after birth and MBRN, the variables 'PREG_ID_XX' and 'BARN_NR' together uniquely identifies each observation. BARN_NR is the child's order at birth.
3.	When merging files with data before pregnancy (where the pregnancy is the unit of observation), use the variable 'PREG_ID_XX' as the key variable when merging. When merging a file with data from a questionnaire after birth or MBRN, use both 'PREG_ID_XX' and 'BARN_NR' as key variables to ensure that the data is linked correctly for twins/triplets.

In the code below syntax/code for merging MBRN, Q1 and Q3 is shown.

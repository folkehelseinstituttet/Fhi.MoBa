# Read this before analyses
#### Please read the following important information regarding data from the Norwegian Mother and Child Cohort Study (MoBa) before analyzing the data.
## Description
Most of the documentation at the MoBa Wiki web site is translated into English, although some documentation is only available in Norwegian.
#### Versions of questionnaires
There are more than one version of each questionnaire (A, B, C, ..). Many questions are identical across versions and the data is stored in the same variable. Where the questions differs significally between versions data is stored in separate variables. A few questions are version specific. In addition, there are web versions (W) for the dietary (Q2), 3 year (Q6) and 7 year (Q7) questionnaire. An overview of all questionnaires can be found on the wiki page Questionnaires and on fhi.no (English versions, Norwegian versions)
#### Variable names
The variable names consist of two letters and a number. In questionnaire 1 (Q1) all variables (corresponding to questions) are named ‘AA’ followed by a number. In questionnaire 3 (Q3) all variables (corresponding to questions) are named ‘CC’ followed by a number.
#### Variable labels
The variable labels include the following information:
1. The questionnaire version(s) in which the question is found
2. The question number(s)
3. The question text
Note that the question number may differ in different versions of the questionnaire. For the majority of the questionnaires, labels have been translated into English. The english versions of MoBa questionnaires can be found on the wiki page Questionnaires and on fhi.no.
#### Coding of variables
It is essential to know the basic rules applied for coding of data before analyzing the data. A categorical variable is coded ‘1’ for the first category, ‘2’ for the second category, and so on. Normally, the answer category ‘No’ is coded ‘1’ and ‘Yes’ is coded ‘2’. However, in some cases it might be opposite, where ‘Yes’ is coded ‘1’ and ‘No’ is coded ‘2’. In general, one should always check the questionnaires for the correct sequence of categories. Variables corresponding to questions where only one answer is intended (index variables) are coded ‘0’ if more than one answer is given. However, some index variables are given valid codes for combinations of two or more ticks. This recoding work is ongoing. Dichotomous variables are coded ‘1’ for a tick and a blank (missing) value when there is no tick.
Coded variables based on text fields for medications are available for all questionnaires except for the second Fathers questionnaire - See wiki page Coding of medication. Coded variables for mother and father’s occupation and industry are available for questionnaire 1 (Q1) for about 71 700 pregnancies - See wiki page Coding of occupation. Data for diseases and other text fields are not coded and are not included in the data sets.
#### Data from the Medical Birth Registry of Norway (MBRN)
Data from the Medical Birth Registry of Norway (MBRN) is linked to MoBa, and records related to participating MoBa pregnancies are included in the MoBa MBRN file. For updated documentation for MBRN see wikipage Medical Birth Registry and the home page of the Medical Birth Registry of Norway. Note that documentation for MBRN is only partly translated into English and we encourage you to consult your Norwegian collaborators when working with these data.
### Data quality
MoBa data have been through extensive quality control procedures. These are divided in two levels.
Level 1: During scanning and verification, all scanned and digitalized numbers are evaluated to check correspondence with what is written in the questionnaires. Some check boxes will also be validated, i.e. when more than one check box is marked for a group of mutually exclusive check boxes.
Level 2: Registered data are evaluated again. Rules have been defined for each variable to check values that are (slightly) extreme or not consistent with other answers. Note that limits for valid values set in the quality control procedure do not necessarily comply with plausible limits. Specifically, numbers are checked by comparing the digitalized number with the written response in the questionnaire. Where there are discrepancies, corrections are made. Otherwise data are left unchanged, even if implausible or illogical. Hence, for cut-offs, researchers must use their own considerations. Rules applied in level 2 of the quality control can be found on wiki page Quality control. Quality control is an ongoing process. Attached documentation explains what has already been done to ensure good data quality. However, if errors are discovered by use of MoBa data, please let us know. If necessary, new rules for quality control will be implemented. Data quality is hence better in each subsequent MoBa version.
### Use of data
A guideline for use of MoBa data can be found on wiki page User guides (only available in Norwegian). Below, some important points are listed.
#### Generated variables
Some questions are excluded from the data files for confidential and practical reasons. The information given in these questions is incorporated in generated variables. The most important generated variables are described in the sections below.
##### Dates and age
The generated variable ‘ALDERUTFYLT_Sx’ contains the date when the questionnaire was filled out by the respondent. The date is given as “Time in days from the date of birth of the child until filling out the questionnaire”, and is calculated from the variable ”Date for filling out the questionnaire”. This will correspond to the child’s age in days for the following questionnaires: 6 months (Q4), 18 months (Q5), 3 years (Q6), 5 years (Q5y), 7 years (Q7y), 8 years (Q8y) and father’s questionnaire no. 2 (QF2). For the first (Q1), second (Q2) and third (Q3) questionnaire as well as the father’s questionnaire (QF) it corresponds to the number of days before the child is born. Note that we have observed some evident errors in the variable ”Date for filling out the questionnaire”. Some respondents have written their own date of birth, their child’s date of birth or other dates that are not valid. This results in errors in the generated variable ‘ALDERUTFYLT_Sx’. To control for these errors, we have added two generated variables to the dataset; ‘ALDERUTSENDT_Sx’ and ‘ALDERRETUR_Sx’. These variables are calculated in the same way as ‘ALDERUTFYLT_Sx’, but utilize the date the questionnaire was sent out and the date we received it. These variables may be used for estimation of the child’s age when ‘ALDERUTFYLT_Sx’ is missing or implausible. In the same way as for variable “Date of filling out the questionnaire”, all date variables in the questionnaires are replaced by variables that correspond with the child’s age in days or months.
##### Number of responses
A category of variables has been generated, providing the total number of responses filled in on each page of the questionnaire by the respondent. The variable names consist of the questionnaire number and page number; Q1P1, Q1P2 etc. Note that a question may be placed on different pages in different versions of the same questionnaire. These variables are meant to simplify quality control of the data for users and the interpretation of, for instance, missing values (e.g. without noticing, some women have left two pages blank because two sheets have been stuck together).
##### Generated variables from the dietary questionnaire during pregnancy (Q2)
38 variables are generated from the dietary questionnaire and are included in the file “Q2_calculation”. These variables are calculated by use of the Norwegian Food Composition Table 2001. Note that dietary supplements are not included in the intake calculations of vitamins and minerals. The variable ‘VERSJON_KOST_TBL1’ specify which version (KOST_A=A/B or KOST_B=C/D/W) of the dietary questionnaire the calculations are based on. Questionnaires A and B are very different from questionnaires C, D and W. In version A and B the women are asked what they have been eating the last 12 months before they became pregnant, while in version C, D and W they are asked what they have been eating since they became pregnant and until the day they filled out the questionnaire (about week 17-22).
Note that data from the first two versions (A and B) of the diet questionnaire, are given in separate files only by request, since they are not comparable to the questionnaires C, D and W.
More information about data from the dietary questionnaire.
### Merging data files
Considering the large amount of data, each questionnaire is delivered as separate data files. When merging the files for analysis, it is important to be aware of the following
* For Q1, Q2, Q3, Q4, Q5, Q6, Q5Y, Q7Y, Q8Y and MBRN: All files include a unique identifier for the pregnancy. The variable is called ‘PREG_ID_XX’. This variable must be used as the key variable when merging files. The data files based on data after birth (data for the questionnaire at 6 months (Q4), 18 months (Q5), 3 years (Q6), 5 years (Q5Y), 7 years (Q7Y), 8 years (Q8Y) and MBRN) include the variable ‘BARN_NR’. This variable must also be used as a key variable in addition to ‘PREG_ID_XX’ to ensure that data is linked correctly for twins/triplets.
* For the second Father questionnaire (QF2): Se wiki page about Father's questionnaire no. 2.

Syntax for merging files can be found on the wiki page Merge files.
Some participants will have data from all 11 questionnaires and MBRN, while others will only have data from one source (one questionnaire or MBRN). It is crucial to be aware of this when interpreting missing values. The data set “SV_INFO” includes a unique identifier for each woman called ‘M_ID_XX’. This variable can be used to identify women who participate with more than one pregnancy and hence identify siblings.
### Important considerations
Not all questions are present in every version of a questionnaire. Hence, a variable may have a missing value because the respondent has answered a version of the questionnaire that does not include the specific question. The variable label indicates in which version(s) a specific question was asked, while the variable ‘VERSJON_SKJEMAX_TBL1’ indicates which version of the questionnaire each respondent has answered. If you want to use a variable not included in e.g. version A of a questionnaire, you can use ‘VERSJON_SKJEMAX_TBL1’ to remove version A-respondents from the data set.
We urge you to consider how the specific question may be interpreted by the respondent. If a respondent has answered “No” to a question, he/she may not have answered the follow-up questions and the corresponding variable(s) will have a missing value. Some respondents may also have answered inconsistently, e.g. answered “No” when asked “Have you ever smoked?” and answered “Daily” when asked “Do you smoke now?”. Therefore, it is important to check for inconsistency.
For twins and triplets, the mother fills out one questionnaire for each child after birth (questionnaire 6 months, 18 months, 3 years, 5 years, 7 years and 8 years). The mothers are told to fill in the section “About yourself” for the first child only. MoBa have not duplicated this information to the other sibling(s). These variables are usually missing for one of the twins or two of the triplets.
### Formats in SAS
A format file should be included in data deliveries for SAS users. To open and read SAS datasets you need to include ‘OPTIONS FMTSEARCH=(libname);’ (libname is the path where the format file is stored). Editor files to add value and variable labels are included with the data files.
## Major changes in version 12
Questionnaire 2 (Q2)
In the English data file for Questionnaire 2 (Q2CDW_V12), the labels for question T_10_2 (BB163-BB165) and T_10_3 (BB166-BB168) have been exchanged. The correct labels are:
T_10_2 Sweetened muesli with dried fruit, nuts etc.; 10. How often have you eaten breakfast cereals or porridge on average since you became pregnant?
T_10_3 Porridge, cream of wheat, rice etc.; 10. How often have you eaten breakfast cereals or porridge on average since you became pregnant?
This is corrected in data files made available after 09.04.2021.
## Major changes in version 11
#### MoBa’s 18 year olds
MoBa is currently working with an invitation for participating MoBa children that have turned 18 years of age. Data for children turning 18 years of age by the end of 2018 is not included in MoBa Version 11. A new version of MoBa will be published when this work is completed.
#### 8 years questionnaire (Q8Y)
The 8 years questionnaire (Q8Y) is considered to be complete in Version 11 (See wiki page Response rate).
#### Father's questionnaire no. 2 (QF2)
Data for Father's questionnaire no. 2 is included in Version 11. Se wiki page about Father's questionnaire no. 2 for more information.

Medications listed as "other relief medicines" in question 13 are not yet coded for this questionnaire.
#### Year for filling in questionnaire
If the year given for filling in questionnaire is before 1999 the year is set to 9999. The belonging age for filling in questionnaire is set to missing.
#### Age calculations
Previous versions have had a minor error for a few subjects in calculated date differences. The date differences calculated have had a round off error giving 1 day wrong in difference for some participants.
#### 5 years questionnaire (Q5Y)
As a pilot of the 5 years questionnaire a subgroup was sent the 5 years questionnaire (version A) together with a separate questionnaire to the mother with "The International Personality Item Pool (IPIP) Big-Five factor markers". In version B of the 5 years questionnaire IPIP was included as the second last question (question 57). About 840 answered the pilot version of IPIP. In MoBa version 11, data from these has been put into the same variables as for IPIP in the B version (LL535-LL584, for BARN_NR = 1). Link to extra questionnaire sent with Q5y version A for a subgroup.
FFQ calculation based on Questionnaire 2
Calculations have been rerun with a few minor changes.

## Major changes in version 10
Changes in the coding of variables
#### 3 years questionnaire (Q6)
In previous MoBa versions dichotomous variables in the web version of the 3 years questionnaire was coded ‘0’ when there was no tick, instead of a blank (missing) value. In MoBa version 10 this has been corrected and these variables are now coded with a blank (missing) value when there is no tick.
#### 7 years questionnaire (Q7y)
In previous MoBa versions there has been an error in the response categories for question 21_2 and 21_3, regarding wood-burning heating in the home for the web version of the 7 years questionnaire. The response category “No” was coded “Yes” and vice versa. For MoBa version 10 this has been corrected.

## Questions
If you have questions or comments about the data files or suspect that something can be incorrect in labels or other documentation, please contact us at MorBarnData@fhi.no.
 

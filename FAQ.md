# FAQ

## I have just received files from MoBa, where do I start?
By reading this very important information.
## How do I merge files?
Information and syntax on how to merge files from MoBa can be found here.
## Why are there duplicates in my files?
There should not be any duplicates on any of the variables in any of the files. But the data files based on data after birth (data from questionnaire at 6 months, 18 months, 3 years, 5 years, 7 years, 8 years and MBRN) will include duplicates on pregnancy as one pregnancy may result in more than one child. Therefore, it is extremely important to remember to use both the unique identifier for the pregnancy, PREG_ID_XXX, and the variable BARN_NR to merge these files. PREG_ID_XXX and BARN_NR together should give a unique identifier for each observation in all files after birth.
## Why is there such a high percentage of missing values for some of the variables in my files?
There are several possible reasons for this:
1) All questions are not included in every version of a questionnaire. Hence, some respondents may be counted as missing on a variable because they have answered a questionnaire version that does not include that specific question. The variable label indicates in which version(s) the question is asked. The variable ‘Versjon_SKJEMAx_TBL1’ will indicate which version of the questionnaire each respondent has answered.
E.g. Q_27_1:SKJEMA7A;Høyde;m;27.Hva er barnets høyde og vekt?
This question was only included in version A of the questionnaire (see text in bold). Only a few of the respondents answered this version of the questionnaire. Consequently, there will be a substantial number of missing values for this variable. A question regarding the child's height is part of another question in the other versions of this questionnaire. In order to get information on all children's height you have to combine the different variables regarding height.
2) One should always consider how the specific question may be interpreted by the respondent. If a respondent has answered “No” to a question he/she will probably not answer the follow-up questions and therefore count as missing on the corresponding variables. If so, missing can probably be interpreted as “No”. Some respondents may also have answered inconsistently, e.g. answering “No” when asked “Have you ever smoked?” and answering “Daily” when asked “Do you smoke now?”. Therefore, one should always control for inconsistency.
3) For twins and triplets, the mother fills out one questionnaire for each child after birth (questionnaire 6 months, 18 months, 3 years, 5 years, 7 years and 8 years). The mothers are told to fill in the section “About yourself” for the first child only. MoBa have not duplicated this information to the other sibling(s). Therefore these variables are usually missing for one of the twins or two of the triplets.
## How do I calculate response time in MoBa?
Each file includes variables for age of child (in days) when the questionnaire was sent out, filled out and returned. These variables are called ALDERUTSENDT_Sx, ALDERUTFYLT_Sx and ALDERRETUR_Sx, and can be used to calculate response time. For the questionnaires filled out before birth these variables corresponds to number of days before the child is born. When generating variables for response time for these questionnaires, the questionnaire has to be merged with the file from the Medical Birth Registry of Norway (MBRN) in order to get information on gestational age in days.
Code for response time
 

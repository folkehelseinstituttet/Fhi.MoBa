# Response time
## Description of syntax
Each file with questionnaire data from MoBa includes variables for age of child (in days) when the questionnaire was sent out, filled out and returned. These variables are called ALDERUTSENDT_Sx, ALDERUTFYLT_Sx and ALDERRETUR_Sx, and can be used to calculate response time. For the questionnaires filled out before birth these variables corresponds to number of days before the child is born. When generating variables for response time for these questionnaires, the questionnaire has to be merged with the file from the Medical Birth Registry of Norway (MBRN) in order to get information on gestational age in days.
## Syntax
### Stata

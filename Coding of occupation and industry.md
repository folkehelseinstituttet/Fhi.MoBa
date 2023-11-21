# Coding of occupation and industry

#### Table of Contents
- _[1. Coding of mother's and father's industry](#coding-of-mother's-and-father's-industry)_ <br>
- _[2. Coding of mother's and father's occupation](#coding-of-mother's-and-father's-occupation)_ <br>

Mother's and father's occupation and industry given in questionnaire 1 is coded for approximately 70% of the participants.

## Coding of mother's and father's industry
The Norwegian version of SIC-2002 - Standard Industrial Classification is used to code industries. SIC is based on EU’s and FN’s standards. Statistics Norway is responsible for the Norwegian 
version (see https://www.ssb.no/en/klass/klassifikasjoner/6/versjon/31/koder).

## Coding of mother's and father's occupation
The Norwegian version (STYRK98) of ISCO-88 -International Standard Classification of Occupations compiled by International Labour Office (ILO) 
http://www2.warwick.ac.uk/fac/soc/ier/research/isco88 is used to code occupation. The standard have been adjusted to comply with Norwegian conditions. Statistics Norway is responsible for the 
Norwegian version (see http://www.ssb.no/emner/06/01/nos_c521/nos_c521.pdf and http://www.ssb.no/emner/06/yrke). Two variables have been generated based on occupation code: M_OCCU and F_OCCU. 
These consist of the first digit of the occupation code for the mother and the father.

**The variables M_OCCU and F_OCCU are coded as follows:**
 | Code | Occupation |
 | -- | -- |
 | 1 | Legislators, senior officials and managers | 
 | 2 | Professionals | 
 | 3 | Technicians and associate professionals | 
 | 4 | Clerks | 
 | 5 | Service workers and shop and market sales workers | 
 | 6 | Skill agricultural and fishery workers | 
 | 7 | Craft and related workers | 
 | 8 | Plant and machine operators and assemblers | 
 | 9 | Elementary occupations | 
 | 0 | Armed forces and unspecified | 

<br> Note that some have given more than one occupation. Codes for each occupation are then listed, separated by a hyphen (-). For instance, some have the value "4-6" which means that two occupations are given, one of them is categorized as "Clerks" and the other is categorized as "Skill agricultural and fishery workers". 

### Other codes used
Some extra codes have been used in addition to the ones included in STYRK. These are given in the table below. Note that all these will end up in the category "Armed forces and unspecified" for the variables (M_OCCU and F_OCCU).

 | Code | Occupation | 
 | -- | -- |
 | 200 | Student (including high school, other courses, etc) | 
 | 201 | At home | 
 | 202 | Unemployed / work seeking | 
 | 203 | On social security (including disability benefits) | 
 | 300 | Sick leave | 
 | 301 | On leave (paid maternity leave, paid father's leave, unpaid leave etc.) | 
 | 302 | Flexible leave in connection with returning to work after maternity leave | 
 | 303 | Temporary employment | 
 | 304 | Summer job / seasonal work | 
 | 305 | Rehabilitation | 
 | 306 | Laid off | 

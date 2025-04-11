# OMOP-onc

This readme file contains additional text describing the results.

# files
- **regimen** shows example of drug regiments in GENIE data
- **source_drug_terms** shows examples of drug terms that aggregate drugs into ingredient class

# materials
https://www.aacr.org/professionals/research/aacr-project-genie/  
https://www.aacr.org/professionals/research/aacr-project-genie/bpc/nsclc  
https://www.aacr.org/wp-content/uploads/2022/05/GENIE-BPC-NSCLC-v2.0-public-Analytic-Data-Guide-1.pdf   

Data uses relative dates (days since birth) (no exact dates).

GENIE main registry has very limited set of data elements.
Specific projects (e.g., NSCLC study/cohort) have much richer set of data elements.


# Evaluation
Results of evaluation are below

# by source table

## `clinical patient` 
one row per patient.
target OMOP tables are `person`, `death`.

## `clinical sample`
one row per cancer sample
target OMOP tables are `condition_occurrence` (patient may have multiple cancers)

## `regimen`
target OMOP tables are `drug_exposure`


# by OMOP table

## person
July 1st could be used as default month and date for date of birth. Only year granularity is provided. OMOP requires absolute date and can not accomodate research data de-identified with this approach without introducing some assumptions. 

## condition_ocurrence
OncoTree registry code for cancer needs to be mapped to SNOMED CT standard terms. Mapping exist in OMOP Athena terminology layer. 

## death
GENIE registry element for death (boolan indicator) and relative time of death


# Terminologies

## OncoTree
JSON representation is obtainable at http://oncotree.mskcc.org/api/tumorTypes/tree


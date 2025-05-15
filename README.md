# OMOP-onc

# March 2025 abstract


OMOP model evaluation for representing GENIE oncology research project data

## Background
Confirming validity of research results of healthcare observational research on external datasets is good practice. Observational Medical Outcomes Partnership (OMOP) Common Data Model (CDM) facilitates analysis portability and conducting external validations. Our objective was to evaluate whether a cancer research dataset can be represented using OMOP model and what lessons learned can be communicated to the OHDSI CDM workgroup.

### Materials:
American Association for Cancer Research established in 2015 a research project in oncology called GENIE (Genomics Evidence Neoplasia Information Exchange). This project made available several datasets. We used GENIE non-small cell lung cancer (NSCLC) dataset.1 This dataset relies on some features of GENIE registry.2 

## Methods
If GENIE data were to be represented in OMOP CDM, it would be possible to repeat analysis of GENIE data on other OMOP datasets (for example using data from the ‘All of Us’ research program). While full conversion of GENIE data into OMOP format was out of scope for our study, as a first step in this ambition, we evaluated whether OMOP model can fully capture GENIE lung cancer data. We used de-identified patient level GENIE NSCLC data and also retrieved publicly posted corresponding data dictionary of GENIE data elements.3 For OMOP model specification, we used version 5.4.4
## Results
Many publications demonstrate the ability of the OMOP model to capture Electronic Health Record (EHR) data and healthcare claims data. There is also growing experience with using OMOP model for registry data and clinical trials data. GENIE represents a combination of EHR data and Case Report Form (CRF) data captured via an Electronic Data Capture (EDC) system. Some data elements in GENIE contain results of manual chart review. For example, GENIE categorical data element of ‘Medical Oncologist Assessment of Change in Cancer Status’ (element_id: md_ca_status) has 5 permissible values of: Improving/Responding; Stable/No change; Mixed; Progressing/Worsening/Enlarging; Not stated/Indeterminate). In OMOP model, the OBSERVATION table can be used to capture this data. An identical representation approach was chosen by the All of Us NIH study.5 Tumor staging and grading data is best represented using conventions defined by the OHDSI Oncology workgroup (oncology extension). This convention uses OBSERVATION table rows that reference CONDITION_OCCURRENCE rows that are being elaborated by those OBSERVATION rows.

To represent dates, GENIE uses relative paradigm as ‘days since birth’ for all events.6 Using year of birth, OMOP representation would introduce some assumptions (e.g., July 1st as default day and month element of birth date) and impute necessary absolute days. This is necessary because the OMOP model requires absolute days. 

GENIE medication history data only capture active ingredients administered and not full details about brand name, strength, dose form, or quantity. For ingredients, GENIE uses aggregated ingredient drug terms. Two examples would be: example 1 ‘Fluorouracil (5FU, 5Fluorouracil, 5Fluracil, AccuSite, Adrucil, Carac, Fluouracil, Flurablastin, Fluracedyl, Fluracil, Fluril, Fluroblastin, Ribofluor, Ro29757); example 2 ‘Oxaliplatin(1OHP, Ai Heng, DACPLAT, Dacotin, ELOXATIN, Eloxatine, JM83)’. If GENIE terms would be mapped and converted to OMOP drug ingredient concepts, it would be possible to formulate a medication analysis and such an analysis would be portable across OMOP datasets. As one special drug concept, GENIE allows capture of administration of an investigational drugs in the context of a clinical trial. The drug recorded would be ‘investigational drug’ and it would not specify which drug exactly, just the category of investigational). In OMOP, such data rows would have to be represented using ‘concept 0 approach’ or using a custom local concept for investigational drug (“two billion” concept approach). GENIE medication data also include an inferred construct of drug regimen. EPISODE table in OMOP oncology extension could capture these inferred regiments.7 

GENIE diagnostic data are organized as cancer diagnosis history and as CRF data elements indicating comorbidities. GENIE registry uses OncoTree terminology for cancer diagnosis. For example, data would use OncoTree code of ‘NSCLCPD’ (in data element OCOTREE_CODE). Additional two data elements provide additional information: data element CANCER_TYPE has value ‘Non-Small Cell Lung Cancer’ and data element CANCER_TYPE_DETAILED has value ‘Poorly Differentiated Non-Small Cell Lung Cancer’. To achieve comparable analyses on non-oncology specified OMOP data (e.g., All Of Us), diagnosis history data would be captured in the CONDITION_OCCURRENCE OMOP table relying on mapping of OncoTree concepts to standard diagnostic SNOMED CT concept. To describe the mapping using an example, concept with concept_id of 777920 (representing NSCLPDP code in OncoTree) is mapped to standard concept of ‘Non-small cell lung cancer’ (concept_id of 4115276; vocabulary: SNOMED CT). 

Full overview by GENIE source data category and by OMOP target table will be included in the poster and at our project repository at github.com/informaticsrepo/omop-onc
Conclusion
GENIE data can be captured in OMOP model; however, to achieve comparable analyses, careful attention to conventions must be paid. We specifically considered the case of All of Us OMOP dataset to guide our evaluation and see if comparable data would be present and whether necessary mappings currently exist. Mappings of ICD10CM cancer diagnoses (in All Of Us) and OncoTree diagnoses (in GENIE) do not map to identical standard concepts and proper traversing the terminology hierarchy would be essential for any analysis.

For medication analyses, inferring drug regimen episodes from medication history data is an important unsolved challenge because differences in the inference methodology will lead to non-comparable OMOP cancer-related datasets. For some data rows, OMOP lacks suitable convention to capture investigational drugs that would allow across datasets analyses of what cancer types and what types of patients receive investigational drugs.

## Discussion 
Current oncology workgroup OMOP extension is not formally included in official OMOP v5.4 specification.8 As a result, All of Us data may be captured on misaligned level of granularity and not capture detailed cancer data in comparable way. Our evaluation revealed some OMOP model limitations that we hope to submit to the OHDSI CDM workgroup, THEMIS workgroup and/or other workgroups. Current standard terminology for diagnoses, SNOMED CT, does not allow full capture of detailed cancer diagnosis and the oncology OMOP extension is the best approach for this data. However, other disease domains may encounter a similar challenge as oncology did. The best compromise may be development of expected expanded target concepts (in measurement and/or observation domain) for each such domain. OHDSI community and CDM workgroup may need to design a formal format specification that would work for number of disease domains. This specification would communicate what expected expanded concepts are preferred target concept to unite the community around. The challenge is analogous to the clinical research informatics challenge of formulating research common data elements9 but in OMOP model context it can be firmly grounded to currently present concepts in the OMOP Athena terminology layer. 

### Limitations
We did not include GENIE genomic data in this phase 1 of our evaluation. As future work, we hope to assess to what extend OMOP can capture such data. This is especially important because GENIE project’s strength lies in providing extensive genomic data in addition to clinical data for cancer patients. We expect further input to OHDSI workgroups stemming from this future phase 2.
## References

1. 	Non-small cell lung cancer GENIE dataset [Internet]. Available from: https://www.aacr.org/professionals/research/aacr-project-genie/bpc/nsclc
2. 	Acebedo A, Bedard PL, Brown S, Ceca E, Fiandalo M, Fuchs H, et al. Collaborating across sectors in service of open science, precision oncology, and patients: an overview of the AACR Project GENIE (Genomics Evidence Neoplasia Information Exchange) Biopharma Collaborative (BPC). ESMO Real World Data Digit Oncol. 2025 Mar 1;7:100097. 
3. 	GENIE NSLC Dataset Documentation [Internet]. Available from: https://www.aacr.org/wp-content/uploads/2022/05/GENIE-BPC-NSCLC-v2.0-public-Analytic-Data-Guide-1.pdf
4. 	OMOP model specification [Internet]. Available from: https://ohdsi.github.io/CommonDataModel
5. 	Mayer CS, Huser V. Learning important common data elements from shared study data: The All of Us program analysis. Pry JM, editor. PLOS ONE. 2023 Jul 7;18(7):e0283601. 
6. 	Choudhury NJ, Lavery JA, Brown S, de Bruijn I, Jee J, Tran TN, et al. The GENIE BPC NSCLC Cohort: A Real-World Repository Integrating Standardized Clinical and Genomic Data for 1,846 Patients with Non–Small Cell Lung Cancer. Clin Cancer Res. 2023 Sep 1;29(17):3418–28. 
7. 	Warner JL, Dymshyts D, Reich CG, Gurley MJ, Hochheiser H, Moldwin ZH, et al. HemOnc: A new standard vocabulary for chemotherapy regimen representation in the OMOP common data model. J Biomed Inform. 2019 Aug 1;96:103239. 
8. 	OMOP extensions [Internet]. Available from: https://ohdsi.github.io/CommonDataModel/typesOfAdditions.html#ExpansionExtension
9. 	Kush RD, Warzel D, Kush MA, Sherman A, Navarro EA, Fitzmartin R, et al. FAIR data sharing: The roles of common data elements and harmonization. J Biomed Inform. 2020 Jul 1;107:103421. 





# additional information
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


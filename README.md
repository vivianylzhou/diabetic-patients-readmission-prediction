### Predicting 30-Day Readmissions in Diabetic Patients

## Description
Early identification of at-risk patients prevent excess readmissions and benefit patient health. We try to find which diabetic patients are at highest risk of being readmitted to the hospital within 30 days of discharge.

## Data
* Source: [Diabetes 130-US Hospitals for Years 1999-2008] (https://archive.ics.uci.edu/dataset/296/diabetes-130-us-hospitals-for-years-1999-2008) (UCI Machine Learning Repository)
* Download diabetic_data.csv and IDS_mapping.csv from the link and place them in /data

## Method
1. Data cleaning (notebooks/data_cleaning.ipynb)
* Dropped weight (97% missing)
* Filled missing lab results (max_glu_serum, A1Cresult) with "Not tested" rather than dropping those rows
* Excluded patients with expired discharge id since they cannot be readmitted
* Grouped diag_1 into 8 clinical categories using ICD-9 codes
* Defined target variable: readmitted_30d (1 if readmitted within 30 days, else 0)
2. Feature encoding
* Ordinal encoding for age, max_glue_serum, A1Cresult and one-hot encoding for race, gender, diag_1_group, discharge_disposition_id
* 47 features on demographics, healthcare history, admission severity, and discharge disposition

## Modeling
Logistic regression with standardized features, class_weight = "balance" for class imbalance (~11% positive class)

## Results
AUC: 0.66
Precision: 0.18
Recall: 0.52

The model ranks patients meaningfully and catches about half of all actual 30-day readmissions. Low precision suggests that most high-risk candidates are false positives. From a clinical perspective, this recall-precision tradeoff ensures that genuinely at-risk patients are not missed. 

## Key risk drivers
* Prior inpatient admissions (number_inpatient) is the strongest predictor
* Discharge disposition, where patients are discharged (rehab, another hospital, hospice), raises or lowers risk (Note: code 18 maps to "NULL")
* Diagnosis complexity (number_diagnoses) and number_emergency increase risk

## Next Steps
Expand feature set with secondary diagnoses (diag_2, diag_3) and medication columns

## Tools
Python (pandas, scikit-learn), SQL, Jupyter
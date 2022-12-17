# Appendix

## Data

### Data access

Data was obtained from the Sentinel Stroke National Audit (SSNAP [1]), managed through the Healthcare Quality Improvement Partnership (HQIP [2]). SSNAP has near-complete coverage of all acute stroke admissions in the UK (outside Scotland). All hospitals admitting acute stroke participate in the audit, and year-on-year comparison with Hospital Episode Statistics [3] confirms estimated case ascertainment of 95% of coded cases of acute stroke.

The NHS Health Research Authority decision tool [4] was used to confirm that ethical approval was not required to access the data. Data access was authorised by HQIP (reference HQIP303). 

Data was retrieved for 246,676 emergency stroke admissions to acute stroke teams in England and Wales between 2016 and 2018 (three full years).

[1] https://www.strokeaudit.org/

[2] https://www.hqip.org.uk/

[3] https://digital.nhs.uk/data-and-information/data-tools-and-services/data-services/hospital-episode-statistics

[4] http://www.hra-decisiontools.org.uk/research/

### Data Fields

#### Stroke Team
* StrokeTeam: Pseudonymised SSNAP ‘routinely admitting team‘ unique identifier. For emergency care it is expected that each hospital has one stroke team (though post-72 hour care may be reported under a different team at that hospital).PatientUID: Pseudonymised patient unique identifier

#### Patient – general
* Pathway: Total number of team transfers, excluding community teams
* S1AgeOnArrival: Age on arrival aggregated to 5 year bands
* MoreEqual80y: Whether the patient is >= 80 years old at the moment of the stroke
* S1Gender: Gender
* S1Ethnicity: Patient Ethnicity. Aggregated to White, Black, Mixed, Asian and Other

#### Patient – pathway information
* S1OnsetInHospital: Whether the patient was already an inpatient at the time of stroke
* S1OnsetToArrival_min: Time from symptom onset to arrival at hospital in minutes, where known and if out of hospital stroke
* S1OnsetDateType: Whether the date of onset given is precise, best estimate or if the stroke occurred while sleep
* S1OnsetTimeType: Whether the time of symptom onset given is precise, best estimate, not known
* S1ArriveByAmbulance: Whether the patient arrived by ambulance
* S1AdmissionHour: Hour of arrival, aggregates to 3 hour epochs
* S1AdmissionDay: Day of week at the moment of admission
* S1AdmissionQuarter: Year quarter (Q1: Jan-Mar; Q2:April-Jun; Q3: Jul-Sept; Q4: Oct-Dec)
* S1AdmissionYear: Year of admission
* S2BrainImagingTime_min: Time from Clock Start to brain scan. In minutes. “Clock Start” is used throughout SSNAP reporting to refer to the date and time of arrival at first hospital for newly arrived patients, or to the date and time of symptom onset if patient already in hospital at the time of their stroke.
* S2ThrombolysisTime_min: Time from Clock Start to thrombolysis. In minutes. “Clock Start” is used throughout SSNAP reporting to refer to the date and time of arrival at first hospital for newly arrived patients, or to the date and time of symptom onset if patient already in hospital at the time of their stroke.

#### Patient – comorbidities
* CongestiveHeartFailure: Pre-Stroke Congestive Heart Failure
* Hypertension: Pre-Stroke Systemic Hypertension
* AtrialFibrillation: Pre-Stroke Atrial Fibrillation (persistent, permanent, or paroxysmal)
* Diabetes: Comorbidities: Pre-Stroke Diabetes Mellitus
* StrokeTIA: Pre-Stroke history of stroke or Transient Ischaemic Attack (TIA)
* AFAntiplatelet: Only available if “Yes” to Atrial Fibrillation comorbidity. Whether the patient was on antiplatelet medication prior to admission
* AFAnticoagulent: Prior to 01-Dec-2017: Only available if “Yes” to Atrial Fibrillation comorbidity; From 01-Dec-2017: available even if patient is not in Atrial Fibrillation prior to admission. Whether the patient was on anticoagulant medication prior to admission
* AFAnticoagulentVitK: If the patient was receiving anticoagulant medication, was it vitamin K antagonists
* AFAnticoagulentDOAC: If the patient was receiving anticoagulant medication, was it direct oral anticoagulants (DOACs)
* AFAnticoagulentHeparin: If the patient was receiving anticoagulant medication, was it Heparin

#### Patient – NIH Stroke Scale
* S2NihssArrival: National Institutes of Health Stroke Scale score on arrival at hospital
* BestGaze: National Institutes of Health Stroke Scale Item 2 Best Gaze (higher values indicate more severe deficit)
* BestLanguage: National Institutes of Health Stroke Scale Item 9 Best Language (higher values indicate more severe deficit)
* Dysarthria: National Institutes of Health Stroke Scale Item 10 Dysarthria (higher values indicate more severe deficit)
* ExtinctionInattention: National Institutes of Health Stroke Scale Item 11 Extinction and Inattention (higher values indicate more severe deficit)
* FacialPalsy: National Institutes of Health Stroke Scale Item 4 Facial Paresis (higher values indicate more severe deficit)
* LimbAtaxia: National Institutes of Health Stroke Scale Item 7 Limb Ataxia (higher values indicate more severe deficit)
* Loc: National Institutes of Health Stroke Scale Item 1a Level of Consciousness (higher values indicate more severe deficit)
* LocCommands: National Institutes of Health Stroke Scale Item 1c Level of Consciousness Commands (higher values indicate more severe deficit)
* LocQuestions: National Institutes of Health Stroke Scale Item 1b Level of Consciousness Questions (higher values indicate more severe deficit)
* MotorArmLeft: National Institutes of Health Stroke Scale Item 5a Motor Arm - Left (higher values indicate more severe deficit)
* MotorArmRight: National Institutes of Health Stroke Scale Item 5b Motor Arm - Right (higher values indicate more severe deficit)
* MotorLegLeft: National Institutes of Health Stroke Scale Item 6a Motor Leg - Left (higher values indicate more severe deficit)
* MotorLegRight: National Institutes of Health Stroke Scale Item 6b Motor Leg - Right (higher values indicate more severe deficit)
* Sensory: National Institutes of Health Stroke Scale Item 8 Sensory (higher values indicate more severe deficit)
* Visual: National Institutes of Health Stroke Scale Item 3 Visual Fields (higher values indicate more severe deficit)

#### Patient – other clinical features
* S2INR: Patient’s International Normalised ratio (INR) on arrival at hospital (available since 01-Dec-2017)
* S2INRHigh: INR was greater than 10 on arrival at hospital (available since 01-Dec-2017)
* S2INRNK: INR not checked (available since 01-Dec-2017)
* S2NewAFDiagnosis: Whether a new diagnosis of Atrial Fibrillation was made on admission
* S2RankinBeforeStroke: Patient’s modified Rankin Scale score before this stroke (Higher values indicate more disability)
* S2StrokeType: Whether the stroke type was infarction or primary intracerebral haemorrhage
* S2TIAInLastMonth: Whether the patient had a Transient Ischaemic Attack during the last month. Item from the SSNAP comprehensive dataset questions (not mandatory)

#### Patient – thrombolysis given
* S2Thrombolysis: Whether the patient was given thrombolysis (clot busting medication)

#### Patient – reason stated for not giving thrombolysis
* Age: If the answer to thrombolysis given was “no but”, the reason was Age
* Comorbidity: If the answer to thrombolysis given was “no but”, the reason was Co-morbidity
* Haemorrhagic: If the answer to thrombolysis given was “no but”, the reason was Haemorrhagic stroke
* Improving: If the answer to thrombolysis given was “no but”, the reason was Symptoms Improving
* Medication: If the answer to thrombolysis given was “no but”, the reason was Medication
* OtherMedical: If the answer to thrombolysis given was “no but”, the reason was Other medical reason
* Refusal: If the answer to thrombolysis given was “no but”, the reason was Refusal
* TimeUnknownWakeUp: If the answer to thrombolysis given was “no but”, the reason was Symptom onset time unknown/wake-up stroke
* TimeWindow: If the answer to thrombolysis given was “no but”, the reason was Age
* TooMildSevere: If the answer to thrombolysis given was “no but”, the reason was Stroke too mild or too severe

## Feature selection

25 features were selected by identifying one feature at a time that led to greatest improvement in Receiver Operating Characteristic (ROC) Area Under Curve (AUC). ROC AUC was measured using stratified 5-fold cross validation.

The best model with 1, 2, 5, 10, 25 & all features (60 features before one-hot encoding of fields) had ROC AUCs of 0.715, 0.792, 0.891, 0.919, 0.923 & 0.922. We selected 10 features for all subsequent work, which were:

* Arrival-to-scan time: Time from arrival at hospital to scan (mins)
* Infarction: Stroke type (1 = infarction, 0 = haemorrhage)
* Stroke severity: Stroke severity (NIHSS) on arrival
* Precise onset time: Onset time type (1 = precise, 0 = best estimate)
* Prior disability level: Disability level (modified Rankin Scale) before stroke
* Stroke team: Stroke team attended
* Use of AF anticoagulants: Use of atrial fibrillation anticoagulant (1 = Yes, 0 = No)
* Onset-to-arrival time: Time from onset of stroke to arrival at hospital (mins)
* Onset during sleep: Did stroke occur in sleep?
* Age: Age (as middle of 5 year age bands)

The improvement in ROC AUC with increasing features is shown in the figure below.

<img src="./images/01_feature_selection.jpg" width="800"/>

### Correlations within the 10 features

Correlations between the 10 features were measured using coefficients of determination (r-squared). All r-squared were less than 0.15, and all r-squared were less than 0.05 except 1) age and prior disability level (r-squared 0.146), and 2) onset during sleep and precise onset time (r-squared 0.078).

|               variable 1 |               Variable 2 | r-squared |
|-------------------------:|-------------------------:|----------:|
|                      Age |   Prior disability level |    0.1462 |
|       Onset during sleep |       Precise onset time |    0.0784 |
|          Stroke severity |   Prior disability level |    0.0454 |
|          Stroke severity |               Infarction |    0.0386 |
|       Precise onset time |    Onset-to-arrival time |    0.0344 |
|          Stroke severity |                      Age |    0.0268 |
|                      Age | Use of AF anticoagulants |    0.0207 |
|          Stroke severity |    Onset-to-arrival time |    0.0186 |
|       Precise onset time |   Prior disability level |    0.0131 |
|                      Age |       Precise onset time |    0.0090 |
|   Prior disability level | Use of AF anticoagulants |    0.0070 |
|       Onset during sleep |    Onset-to-arrival time |    0.0043 |
|    Onset-to-arrival time |                      Age |    0.0038 |
| Use of AF anticoagulants |               Infarction |    0.0033 |
|   Prior disability level |    Onset-to-arrival time |    0.0022 |
|       Precise onset time |     Arrival-to-scan time |    0.0021 |
| Use of AF anticoagulants |          Stroke severity |    0.0019 |
|     Arrival-to-scan time |          Stroke severity |    0.0019 |
|       Precise onset time | Use of AF anticoagulants |    0.0016 |
|          Stroke severity |       Onset during sleep |    0.0011 |
|               Infarction |    Onset-to-arrival time |    0.0007 |
|               Infarction |       Onset during sleep |    0.0007 |
|               Infarction |       Precise onset time |    0.0006 |
|    Onset-to-arrival time |     Arrival-to-scan time |    0.0004 |
|     Arrival-to-scan time |   Prior disability level |    0.0001 |
|    Onset-to-arrival time | Use of AF anticoagulants |    0.0001 |
|          Stroke severity |       Precise onset time |    0.0000 |
|     Arrival-to-scan time |                      Age |    0.0000 |
| Use of AF anticoagulants |       Onset during sleep |    0.0000 |
|   Prior disability level |       Onset during sleep |    0.0000 |
|               Infarction |                      Age |    0.0000 |
| Use of AF anticoagulants |     Arrival-to-scan time |    0.0000 |
|       Onset during sleep |     Arrival-to-scan time |    0.0000 |
|     Arrival-to-scan time |               Infarction |    0.0000 |
|                      Age |       Onset during sleep |    0.0000 |
|   Prior disability level |               Infarction |    0.0000 |



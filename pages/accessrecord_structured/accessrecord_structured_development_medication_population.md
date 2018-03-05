---
title: Medication
keywords: getcarerecord
tags: [getcarerecord]
sidebar: accessrecord_structured_sidebar
permalink: accessrecord_structured_development_medication_population.html
summary: "Guidance for populating and reading the Medication resource"
---

# Populating the elements in the medication FHIR profiles 

## Medication Profile ##

Element | Usage | Datatype | Optionality | Guidance 
--------|-------|----------|-------------|------------------------------------------------
code | Medication code| CodeableConcept | M | The code that identifies the medication. A dm+d code should always be supplied if possible. If not then a code from another codesystem or the SNOMED degrade code (196421000000109, Transfer-degraded medication entry) must be supplied. 
status | If a medication is active or inactive | code | O | Not necessary for this version of GP Connect
isBrand | True if a brand | boolean | O | Not necessary for this version of GP Connect
isOverTheCounter | True if medication does not require a prescription | boolean | O | Not necessary for this version of GP Connect 
manufacturer | Manufacturer of the item |Reference(Organization) | O | Not necessary for this version of GP Connect 
form |The form of the medication eg. powder, tablets,capsule, etc. | CodeableConcept | O |Not necessary for this version of GP Connect 
ingredient | Active or inactive ingredient | BackboneElement | O | Not necessary for this version of GP Connect
package |Details about packaged medications | BackboneElement | O | If the supplier has data relating to the Batch Number or expiry date they may be populated here
image | Picture of the medication | Attachment | O | Not necessary for this version of GP Connect 

## Medication Statement ##

Element | Usage | Datatype | Optionality | Guidance 
--------|-------|----------|-------------|------------------------------------------------
identifier | Logical identifier for resource | Identifier | O | Refer to generic guidance
basedOn | Link to the MedicationRequest that this MedicationStatement is 'basedOn' | Reference(MedicationRequest) | M | Every MedicationStatement must be baseOn a MedicationRequest with intent set to 'plan'
partOf  | DO NOT USE | Reference(MedicationAdministration, MedicationDispense, MedicationStatement, Procedure, Observation) | O |
context | The encounter within which the medication was authorised | Reference(Encounter) | R | As per base profile guidance
status | The status of the authorisation | code | M | Use one of active, completed or stopped. 'active' represents an active authorisation - used for active repeat medications, 'stopped' a repeat authorisation which has been discontinued/stopped. 'complete' should be used for all acute authorisations. 
category  | DO NOT USE | CodeableConcept | O | 
medication | The medication the authorisation is for|Reference(Medication) | M | The Medication resource provides the coded representation of the medication
effective  | Start and end dates of this medication record | dateTime or Period | R | Start date is mandatory. Where there is a defined expiry or end date the end date should be supplied. For acute prescriptions no specific end date should be supplied  
dateAsserted | When this medication Statement was believed true | dateTime | M | Unless there is a distinct user modifiable availabilityTime for the authorisation, this is the audit trail datetime for when the authorisation was entered. 
informationSource | DO NOT USE | Reference(Patient, Practitioner, RelatedPerson, Organization) | O |
subject | Who the medication is for i.e. who it will be administered to | Reference(Patient) | M | Reference to patient 
derivedFrom | DO NOT USE| Reference(Any) | O |
taken | Whether a medication was taken | code | M| Providers MUST use a default value of unk - unknown
reasonNotTaken | DO NOT USE | CodeableConcept | O |
reasonCode | The coded reason for authorising the medication | CodeableConcept | O | 
reasonReference | References the condition or observation that was the reason for this authorisation | Reference(Condition, Observation) | O | Unless there is a specific linkage in the context of medication, indirect linkages to be handled via Problem list
note  | All notes that are associated with this medication record | Annotation | R | All patient notes and prescriber notes at authorisation(plan) and issue(order) level should be included in this field. They should be concatonated and indicate the level the notes come from eg. 1st Issue and also be prefixed with either 'Patient Notes:' or 'Prescriber Notes:' as appropriate.
dosage/text | Complete dosage instructions as text | String | M | 
dosage/patientInstruction | Additional instructions for patient i.e. RHS of prescription label | String | R |
lastIssueDate |When the medication was last issued | dateTime | R | This will be the authored on field for the most recent MedicationRequest with an intent of 'order'. For repeat dispense this will be the latest ValidityPeriod/start date that is not in the future
medicationEpisodeChangeSummary | DO NOT USE | Complex Extension | O | Guidance


## Principles for populating the medication request profiles ##

When populating the MedicationRequest profile it may appear that a number of fields will be duplicated in other associated resources. In the interests of minimising redundancy the 2 following principles should be applied when populating the MedicationRequest profiles,
1. All mandatory fields must be populated
2. Required fields should always be populated where the data exists in the system with the exception of where a lexically identical value exists for an equivalent data item in one of the parent profiles. For a Medication Request with intent 'plan' the associated MedicationStatement would be the parent profile. For a MedicationRequest with intent 'order' the associated MedicationStatement and MedicationRequest with intent 'plan' are both considered parent profiles.


## Medication request

Element|Usage|Datatype|Optionality|Guidance 
---|---|---|---|---
identifier|Logical identifier for resource |Identifier|O|Refer to generic guidance
definition|DO NOT USE|Reference(ActivityDefinition, PlanDefinition)|O|
basedOn|DO NOT USE|Reference(CarePlan, MedicationRequest, ProcedureRequest, ReferralRequest)   |R| Do not use for authorisations (intent of 'plan') - it is valid for issue which would always be basedOn the MedicationRequest authorisation resource
groupIdentifier|	Composite request this is part of|Identifier|R|All repeat prescribed and repeat dispensed medications should have a group identifier that is populated for the 'plan' and all 'orders' relating to them.
status|The status of the authorisation|code|M|Use one of active, completed or stopped. 'active' represents an active authorisation - used for active repeat medications, 'stopped' a repeat authorisation which has been discontinued/stopped. 'complete' should be used for all acute authorisations. ** Justification - if we were to use active for acute authorisations when would it transition to complete without introducing complicated date based statuses, perhaps OK for a 'current' medication list but not necessarily OK here
intent|Distinguished between authorisations and medication issues|code|M|'plan' represents an authorisation, 'order' represents a prescription issue
category|DO NOT USE|CodeableConcept|O|
priority|DO NOT USE|code|O|
medication|The medication the authorisation is for|Reference(Medication)|M|The Medication resource provides the coded representation of the medication|
subject|Who the medication is for i.e. who it will be administered to| Reference(Patient)|M|Reference to patient 
context|The encounter within which the medication was authorised|Reference(Encounter)|R|As per base profile guidance
supportingInformation|DO NOT USE|Reference(Any)|O|D
authoredOn|Authorisation date, when the medication was authorised.|dateTime|M|Unless there is a distinct user modifiable availabilityTime for the authorisation, this is the audit trail datetime for when the authorisation was entered. ** is optional in DDM but if it's audit trail why not have it?
requester|Person and their organization requesting authorisation for prescription |BackboneElement|R|To be used if the medication was prescribed at another practice and has been imported via GP2GP. In that case the onBehalfOf should be completed with a reference to the other organisation
recorder|The responsible practitioner who authorised the medication| Reference(Practitioner)|M|May not always be the user who entered the record on the system but where a system supports attribution to a responsible clinician, the attributed clinician should be referenced here.
reasonCode|The coded reason for authorising the medication|CodeableConcept|O|
reasonReference|References the condition or observation that was the reason for this authorisation| Reference(Condition, Observation)|O|Unless there is a specific linkage in the context of medication, indirect linkages to be handled via Problem list
note|Notes for dispenser|Annotation|R|Sometimes labelled Pharmacy text or instructions for pharmacy
dosageInstruction/text|Complete dosage instructions as text|String|M| 
dosageInstruction/patientInstruction|Additional instructions for patient i.e. RHS of prescription label|String|R|
dispenseRequest/validityPeriod|Prescription start and end dates|Period|M|Start date is mandatory. Where there is a defined expiry or end date the end date should be supplied. For acute prescriptions no specific end date should be supplied
dispenseRequest/numberOfRepeatsAllowed|The number of repeat issues allowed  for repeat and repeat dispensed medications where specified|positiveInt|O|DO NOT USE - use the extension repeatInformation/numberOfRepeatPrescriptionsAllowed
dispenseRequest/quantity|The quantity to dispense|SimpleQuantity|R|If the units are text then the extension dispenseRequest/quantityText should be used
dispenseRequest/quantityText|textual representation of quantity|string|R|Only to be used if there is no numerical value
dispenseRequest/expectedSupplyDuration|Number of days supply per dispense|Duration|R|
dispenseRequest/performer|Nominated pharmacy for dispense |Reference(Organization)|R|
substitution|DO NOT USE|BackboneElement|O|
priorPrescription|References prior prescription authorisation|Reference(MedicationRequest)|O|May be used for example to reference prior authorisation where prescription is re-authorised or where amendments have been made, may reference the previous authorisation before the amendment
detectedIssue|Where a medication has been stopped (status == 'stopped'), the reason is provided via as reference to the contained DetectedIssue resource |Reference(DetectedIssue)|R|Mandatory for authorisations with stopped status
detectedIssue/status|Fixed value of 'final'|code|M
detectedIssue/date|The datetime the medication was stopped/discontinued|dateTime|M|Mandatory for stopped/discontinued medications as the date will always be known
detectedIssue/detail|The textual reason either freetext or the term of a code for stopping/discontinuing the medication|string|M|
eventHistory|DO NOT USE|Reference(Provenance)|O|
repeatInformation|Extension elements to hold details of repeat authorisation|Extension|M|
repeatInformation.numberOfRepeatPrescriptionsAllowed|The number of repeat issues authorised if specified|PositiveInt|R|Must be present where a repeat is authorised for a defined number of issues. Must not be specified for acute medications or where the number of repeat issues has not been defined. There is no concept of an initial dispense in GP Connect usage, therefore the numberOfRepeats allowed is the total number of allowed issues
repeatInformation.numberOfRepeatPrescriptionsIssued|Running total of number of issues made against a repeat authorisation|PositiveInt|R|
repeatInformation.authorisationExpiryDate|The date a repeat prescription authorisation will expire|dateTime|R|
repeatInformation.repeatOrAcute|If a medication is an acute, repeat or repeat dispense|Code|M|Explicit repeat or acute flag rather than deriving it from presence of extension elements or repeatNumber
supplyType|Restricted vocabulary of prescription type related to reimbursement|Code|M|Private, ACBS, NHS Prescription etc.
prescriptionType|TO DO - vocabulary of wider prescription types|Code|Wider prescription type - hospital. dental etc ...

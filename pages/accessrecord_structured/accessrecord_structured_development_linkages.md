---
title: Linkages between data items
keywords: getcarerecord, structured
tags: [getcarerecord, structured]
sidebar: accessrecord_structured_sidebar
permalink: accessrecord_structured_development_linkages.html
summary: "Introduction to how linkages between data items in GP Connect"
---
## Linkages ##
The purpose in developing the FHIR profiles is to ensure that clinical data is, as much as possible, presented the same way regardless of the provider system. This ensures the consuming system (and clinician) will always know where to look for each type of information.


However, information about the patient is not just held within the profiles but in how those profiles are linked together. 


For example, linking a medication to a problem means that as well as the record showing the what the patient is taking, it also explains why they are taking it.


For a consuming system to be able to interpret this linkage information correct it needs to be presented in the same way regardless of the providing system.

### GP Connect FHIR Model ###
To support this, GP Connect has developed a FHIR model that identifies all the GP Connect FHIR profiles and how they are related together to store the patient record.


The model currently covers consultations, problems, medications, allergies, immunisations and uncategorised data. Other clinical areas will be added as they are developed.

<img src="images/access_structured/GP_Connect_FHIR_Model.png" alt="GP Connect FHIR Model" style="max-width:100%;max-height:100%;">

The relationships between two FHIR resources are defined in only one of the linked FHIR resources (similar to in a RDBMS). This is shown by the direction of the arrow in the FHIR model. 


For example, the MedicationStatement resource contains a field that can be used to lookup the linked medication. There is no field in the Medication resource that can be used to lookup the linked MedicationStatement.

## FHIR Profiles Returned on Query ##
There are three main considerations when determining which data is returned by a query on each clinical area.
* Include all the FHIR profiles required to fully describe the requested clinical area
* Include the FHIR profiles required to define all the linkages from the requested clinical area
* Include the FHIR profiles from linked clinical areas where they are key to understanding the requested clinical area. 

### Consultations ###
When GP Connect returns a Consultation it will supply the metadata of the consultation and all the clinical data that was recorded during the Consultation.


Include the following FHIR profiles:
* The Encounter profile of the Consultation
*	The List profiles of the Consultation
*	The ProblemHeader profile of any directly linked Problems
*	The MedicationRequest, MedicationStatement and Medication profiles of any linked Medications or Medical Devices.
    * Always include the MedicationStatement, MedicationRequest (intent = plan) and Medication profiles.
    * Only include MedicationRequest (intent = order) for directly linked issues.
    *	Include the ProblemHeader profile of any Problems linked to the returned MedicationRequests
*	The AllergyIntolerance profile of any linked Allergies
    *	Include the ProblemHeader profile of any Problems linked to the returned Allergies
*	The Immunization profile of any linked Immunisations
    *	Include the ProblemHeader profile of any Problems linked to the returned Immunisations
*	The Observation profile of any linked Uncategorised Data
    *	Include the ProblemHeader profile of any Problems linked to the returned Uncategorised Data

<img src="images/access_structured/Consultation bundle.png" alt="Consultation FHIR profiles" style="max-width:100%;max-height:100%;">

### Problems ###
When GP Connect returns a Problem it will supply the metadata and description of the problem. If asked for by the consumer, GP Connect will also return all the clinical data that has been linked to the Problem.


Include the following FHIR profiles:
*	The ProblemHeader profile of the Problem
*	The ProblemHeader profiles of any directly linked Problems
*	Where requested, The MedicationRequest, MedicationStatement and Medication profiles of any linked Medications or Medical Devices.
    *	Always include the MedicationStatement, MedicationRequest (intent = plan) and Medication profiles.
    *	Only include MedicationRequest (intent = order) for directly linked issues.
*	Where requested, The AllergyIntolerance profile of any linked Allergies
    *	Include the ProblemHeader profile of any Problems linked to the returned Allergies
*	Where requested, The Immunization profile of any linked Immunisations
    *	Include the ProblemHeader profile of any Problems linked to the returned Immunisations
*	Where requested, The Observation profile of any linked Uncategorised Data
    *	Include the ProblemHeader profile of any Problems linked to the returned Uncategorised Data

### Medications and Medical Devices ###
When GP Connect returns a Medication or Medical Device it will supply the prescription plan information. If asked for by the consumer, GP Connect will also return all the prescription issues made under the plan.


Include the following FHIR profiles:
*	The MedicationRequest (intent = plan), MedicationStatement and Medication profiles of the Medication and Medical Device
*	The ProblemHeader profiles of any directly linked Problems
*	Where requested, the MedicationRequest (intent = order) profile for every issue.


### Allergies ###
When GP Connect returns an Allergy it will supply all the Allergy data.


Include the following FHIR profiles:
*	The AllergyIntolerance profile of the Allergy
*	The ProblemHeader profiles of any directly linked Problems

### Immunisation ###
When GP Connect returns an Immunisation it will supply all the Immunisation data.


Include the following FHIR profiles:
*	The Immunization profile of the Immunisation
*	The ProblemHeader profiles of any directly linked Problems

### Uncategorised Data ###
When GP Connect returns Uncategorised Data it will supply all the data about the Uncategorised Data.


Include the following FHIR profiles:
*	The Observation profile of the Uncategorised Data
*	The ProblemHeader profiles of any directly linked Problems

## Following a Linkage ##
For the majority of scenarios, the information required by the consumer can be retrieved through a single query. The consumer system identifies what clinical areas of the patient record they require and what search criteria should be applied then call the GP Connect API. The provider system then returns all the requested information in a single bundle.


There are however scenarios where the information to the first query identifies additional information that is required.


For example:
* A retrieved medication is linked to a problem
* A retrieved problem is linked to ten consultations
* A retrieved immunisation is linked to a consultation


Where this happens the provider system will include FHIR identifiers for all the linked items.


There may be a decision by the consuming system (or by a user of the consuming system) that they need additional information about the linked clinical item. To get these the consumer system can call the GP Connect API a second time using the FHIR identifiers as search criteria. 

If required, this can be repeated a third, fourth or any number of times to trace through a chain of linked data in a patient record.

For example:
* Retrieve all active medications of a patient.
   * Spot one of the medications as being of potential relevance to the patient’s current issue. See that the medication is linked to a problem record.
* Retrieve the problem linked to the medication
   * See that the problem is linked to multiple consultations and was last discussed with the patient in a consultation two months ago.
* Retrieve the latest consultation linked to the problem.
   * Review the consultation notes to understand the latest information about the problem.


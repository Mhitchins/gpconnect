---
title: Additional parameters guidance
keywords: getstructuredrecord, view
tags: [use_case,getstructuredrecord]
sidebar: accessrecord_structured_sidebar
permalink: accessrecord_structured_operation_definition_parameters.html
summary: "Access record structured operation definition parameter details"
---

## Operation Definition Parameters ##

The following parameters are available to be requested as part of the operation definition `GPConnect-GetStructuredRecord-Operation-1`:

- `patientNHSnumber`
- `includeAllergies`
- `includeMedication`

{% include important.html content="At least one of the `include[x]` parameters listed above **MUST** be supplied" %}

### patientNHSnumber ###

The `patientNHSnumber` parameter refers to the NHS number of the patient whose record is being extracted, which must be traced or verified against the national demographics index.

> `patientNHSnumber` **MUST** be included in the request.

| Name                  |  Type | Value | Optionality | Comments |
|-----------------------|-------|-------|:-----------:|----------|
| `patientNHSnumber.Id` | `integer` | [patientNHSNumber](https://fhir.nhs.uk/Id/nhs-number) | Mandatory | Patient's NHS Number |

### includeAllergies ###

Including the `includeAllergies` parameter will request resources representing a patient's allergies and intolerances in the response bundle. By default, resolved allergies and intolerances are not included.

`includeAllergies` consists of the following FHIR resources:

- [AllergyIntolerance](http://www.hl7.org/fhir/STU3/allergyintolerance.html "AllergyIntolerance")
- [Patient](https://www.hl7.org/fhir/patient.html "Patient")
- [Practitioner](https://www.hl7.org/fhir/practitioner.html "Practitioner")

{: .center-image }
![AlleryIntolerance Resource Group diagram](images/access_structured/AllergyIntoleranceResourceGroup.png)


#### Part Parameters ####

The following describes the part parameters available for filtering on this parameter:

| Name                  | Include Parameter | Type | Value | Optionality | Comments |
|-----------------------|-------------------|------|-------|:-----------:|----------|
| `includeResolvedAllergies.valueBoolean` | `Allergies` | `boolean` | `true` or `false` | Optional | Include resolved allergies and intolerances in the response bundle |

#### Request Example ####

On the wire a JSON serialised `$gpc.getstructuredrecord-1` request for **Allergies only** would look something like the following:

```json
{
  "meta": {
    "profile": {
      "value": "https://fhir.nhs.uk/STU3/OperationDefinition/GPConnect-GetStructuredRecord-Operation-1"
    }
  },
  "parameter": [
    {
      "name": "patientNHSNumber",
      "valueIdentifier": {
            "system": "https://fhir.nhs.uk/Id/nhs-number",
            "value": "9999999999"
      }
    },
    {
      "name": "includeAllergies"
    }
  ]
}
```

### includeMedication ###

Includes the 'includeMedication' parameter will request resources representing a patient's medication record in the response bundle.

`includeMedication` consists of the following FHIR resources:

- [MedicationStatement](https://www.hl7.org/fhir/medicationstatement.html "MedicationStatement")
- [MedicationRequest](https://www.hl7.org/fhir/medicationrequest.html "MedicationRequest")
- [Medication](http://www.hl7.org/fhir/STU3/medication.html "Medication")
- [Patient](https://www.hl7.org/fhir/patient.html "Patient")
- [Practitioner](https://www.hl7.org/fhir/practitioner.html "Practitioner")
- [Organization](https://www.hl7.org/fhir/organization.html "Organization")

{: .center-image }
![Medication Resource Group diagram](images/access_structured/MedicationResourceGroup.png)


#### Part Parameters ####

The following describes the part parameters available for filtering on this parameter:

| Name                  | Include Parameter | Type | Value | Optionality | Comments |
|-----------------------|-------------------|------|-------|:-----------:|----------|
| `timePeriod.start` | `Medication` | `date` | `yyyy-mm-dd` | Optional |Restrict the patient's medication record to a specific time period (start date) [Date display](http://systems.digital.nhs.uk/data/cui/uig/datedisplay.pdf) |
| `timePeriod.end` | `Medication` | `date` | `yyyy-mm-dd` | Optional | Restrict the patient's medication record to a specific time period (end date ) [Date display](http://systems.digital.nhs.uk/data/cui/uig/datedisplay.pdf) |
| `includePrescriptionIssues.valueBoolean` | `Medication` | `boolean` | `true` or `false` | Optional | Include individual prescription issues in the response bundle |

#### Provider details - `timePeriod` ####

The folowing list describes the expected provider system behaviours based on `timePeriod` parameter:

> - The provider system **MUST** return the medication summary data items for all medications with a `lastIssueDate` which falls within the `timePeriod.start` and `timePeriod.end` date range (inclusive). 
> - Where a medication does not have a `lastIssueDate`, the provider system **MUST** return the medication summary data items for all medications whose `effective[x]` date range overlaps the `timePeriod.start` and `timePeriod.end` date range (inclusive).
> - Where a medication does not have a `lastIssueDate` or `effective[x]` date range, the provider system **MUST** return the medication summary data items for all medications whose `effective[x]` start date falls within the `timePeriod.start` and `timePeriod.end` date range (inclusive).
> - Where a medication does not have a `lastIssueDate`, `effective[x]` date range or `effective[x]` start date, the provider system **MUST** return the medication summary data items for all medications whose `dateAsserted` date falls within the `timePeriod.start` and `timePeriod.end` date range (inclusive).

#### Request Example ####

On the wire a JSON serialised `$gpc.getstructuredrecord-1` request for **Medication only** would look something like the following:

```json
{
  "meta": {
    "profile": {
      "value": "https://fhir.nhs.uk/STU3/OperationDefinition/GPConnect-GetStructuredRecord-Operation-1"
    }
  },
  "parameter": [
    {
      "name": "patientNHSNumber",
      "valueIdentifier": {
            "system": "https://fhir.nhs.uk/Id/nhs-number",
            "value": "9999999999"
      }
    },
    {
      "name": "includeMedication"
    }
  ]
}
```

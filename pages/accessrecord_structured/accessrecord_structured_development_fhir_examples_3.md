---
title: FHIR Examples
keywords: structured design
tags: [design,structured]
sidebar: accessrecord_structured_sidebar
permalink: accessrecord_structured_development_fhir_examples_3.html
summary: "Acess Record Structured FHIR examples"
---

## Allergies ##

- The GP Connect API is not a full FHIR RESTful interface - therefore not all resources returned are directly accessible
- The message conventions reflect this e.g. fullURLs for individual resources are not provided
- It is assumed that the querying system already has patient details so no patient resource is returned, instead the patient is referenced by identifier (NHS #)

### Example 3 ###

Example of response to getstructuredrecord request with includeAllergies and includeResolvedAllergies missing or false.

Assumes - 'No Known Allergies' has been positively asserted on record and is not contradicted by presence of active allergies on record.

```json
{
  "resourceType": "Bundle",
  "entry": [
    {
      "resource": {
        "date": "2018-03-01T10:57:34+00:00",
        "resourceType": "List",
        "status": "current",
        "code": {
          "coding": [
            {
              "system": "http://snomed.info/sct",
              "code": "TBD",
              "display": "Active Allergies"
            }
          ]
        },
        "meta": {
          "profile": [
            "https://fhir.nhs.uk/STU3/StructureDefinition/GPConnect-Allergy-List-1"
          ]
        },
        "mode": "snapshot",
        "emptyReason": {
          "coding": [
            {
              "system": "http://hl7.org/fhir/special-values",
              "code": "nil-known",
              "display": "Nil Known"
            }
          ],
          "text": "No Known Allergies"
        },
        "title": "Active Allergies",
        "subject": {
          "identifier": {
            "system": "https://fhir.nhs.uk/Id/nhs-number",
            "value": "1234567890"
          }
        }
      }
    }
  ],
  "type": "searchset",
  "meta": {
    "lastUpdated": "2018-03-01T10:57:34+00:00"
  }
}
```

---
title: AccessRecord Resources
keywords: development
sidebar: accessrecord_sidebar
toc: false
permalink: datalibraryaccessRecord.html
summary: "The fhir prifles and interactions required for the AccessRecord capability"
---

The following profiled FHIR resources are used in the current version of the AccessRecord capability, see "API Use Cases" in the menu on the left. Full details of profiled FHIR resources and worked examples are available on the [Access Record HTML DMS Bundle](http://data.developer.nhs.uk/fhir/candidaterelease-170816-getrecord/index.html).

---
## AccessRecord HTML ##
### Request ###
* [gpconnect-carerecord-operation-1](https://data.developer.nhs.uk/fhir/candidaterelease-170816-getrecord/Profile.GetRecordQueryRequest/gpconnect-carerecord-operation-1.html) (based on [FHIR Parameters](https://www.hl7.org/fhir/DSTU2/parameters.html))

### Response ###
* [gpconnect-searchset-bundle-1](https://data.developer.nhs.uk/fhir/candidaterelease-170816-getrecord/Profile.GetRecordQueryResponse-HTMLView/gpconnect-searchset-bundle-1.html) (based on [FHIR Bundle](https://www.hl7.org/fhir/DSTU2/bundle.html))
  * [gpconnect-carerecord-composition-1](https://data.developer.nhs.uk/fhir/candidaterelease-170816-getrecord/Profile.GetRecordQueryResponse-HTMLView/gpconnect-carerecord-composition-1.html) (based on [FHIR Composition](https://www.hl7.org/fhir/DSTU2/composition.html))
  * [gpconnect-patient-1](https://data.developer.nhs.uk/fhir/candidaterelease-170816-getrecord/Profile.GetRecordQueryResponse-HTMLView/gpconnect-patient-1.html) (based on [FHIR Patient](https://www.hl7.org/fhir/DSTU2/patient.html))
  * [gpconnect-practitioner-1](https://data.developer.nhs.uk/fhir/candidaterelease-170816-getrecord/Profile.GetRecordQueryResponse-HTMLView/gpconnect-practitioner-1.html) (based on [FHIR Practitioner](https://www.hl7.org/fhir/DSTU2/practitioner.html))
  * [gpconnect-organization-1](https://data.developer.nhs.uk/fhir/candidaterelease-170816-getrecord/Profile.GetRecordQueryResponse-HTMLView/gpconnect-organization-1.html) (based on [FHIR Organization](https://www.hl7.org/fhir/DSTU2/organization.html))
  * [gpconnect-location-1](https://data.developer.nhs.uk/fhir/candidaterelease-170816-getrecord/Profile.GetRecordQueryResponse-HTMLView/gpconnect-location-1.html) (based on [FHIR Location](https://www.hl7.org/fhir/DSTU2/location.html))
  * [gpconnect-device-1](https://data.developer.nhs.uk/fhir/candidaterelease-170816-getrecord/Profile.GetRecordQueryResponse-HTMLView/gpconnect-device-1.html) (based on [FHIR Device](https://www.hl7.org/fhir/DSTU2/device.html))

---
## Errors ##
### Response ###
* [gpconnect-operationoutcome-1](https://data.developer.nhs.uk/fhir/candidaterelease-170816-getrecord/Profile.GetRecordQueryResponse-HTMLView/gpconnect-operationoutcome-1.html) (based on [FHIR OperationOutcome](https://www.hl7.org/fhir/DSTU2/operationoutcome.html))
---
title: Interaction IDs
keywords: fhir development
tags: [fhir,development]
sidebar: overview_sidebar
permalink: integration_interaction_ids.html
summary: "A list of GP Connect API interaction IDs"
---

## GP Connect interaction ID naming policy ##

All interaction IDs are expected to follow the following format `urn:nhs:names:services:[program]:[standard]:[mechanism]:[operation]:[subject]`

- Program = `gpconnect`
- Standard = `fhir`
- Mechanism = [ `rest`, `operation` ]
	- `rest` for RESTful API interactions
	- `operation` for custom Operation API interactions
- Operation
	- RESTful style = [ `create`, `read`, `update`, `delete`, `search` ] + any more specific actions (for example, `cancel`)
	- Extended operation = [ `gpc.getcarerecord`]
- Subject = [ `resourceType`, `operationName` ]
	- Resource Type is the name of a FHIR resource, such as `Patient`, `Appointment`, `Organization`
	- Operation Name is the name of an extended operation, such as `gpc.getcarerecord`

## List of interaction IDs ##

### Foundations interactions ###

| Operation                 | InteractionID             | 
|---------------------------|---------------------------| 
| [Read metadata](foundations_use_case_get_the_fhir_conformance_profile.html) | `urn:nhs:names:services:gpconnect:fhir:rest:read:metadata` |


### Access Record HTML interactions ###

| Operation                 | InteractionID             | 
|---------------------------|---------------------------| 
| [Get Care Record](accessrecord_use_case_retrieve_a_care_record_section.html) | `urn:nhs:names:services:gpconnect:fhir:operation:gpc.getcarerecord` |


### Access Record Structured and Appointment Management interactions ###

Access Record Structured and Appointment Management interactions are not available at this specification version. Please refer to the [GP Connect specifications page](https://developer.nhs.uk/gp-connect-specification-versions/) for more information.

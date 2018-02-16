---
title: Foundations
keywords: foundations
tags: [foundations]
sidebar: foundations_sidebar
permalink: foundations.html
summary: "Introduction to the GP Connect Foundations capability"
---

{% include todo.html content="This capability pack is published as a **work in progress** version and as such is subject to change. It has been published to show the direction of travel and to serve as a discussion document for parties involved with the implementation and consumption of GP Connect FHIR based APIs." %}

## Purpose ##

The foundations capability cover the basic API requirements and prerequisites to utilise the GP Connect APIs.

{% include important.html content="In order to successfully make use of the GP Connect APIs, initially a level of pre-existing accredited Spine connectivity will be required along with some FHIR foundation API capabilities." %}

{% include roadmap.html content="Over time the necessity to have access to pre-existing spine services (i.e. PDS and SDS integration) is likely to be replaced by GP Connect/FHIR based equivalents." %}

## Prerequisites ##

### PDS ###

You'll need to be able to provide a verified NHS Number to use an API. This can be achieved using a spine accredited system, a DBS batch-traced record (CSV), or using a Spine Mini Services Provider (HL7v3).

### SDS / ODS ###

In order to resolve a given GP Practice organisation to their URI you'll need to be able to do a spine SDS query (LDAP) using the practice's ODS Code to perform an [SDS End Point Lookup](integration_spine_directory_service.html).

### FHIR ###

In order to be a compliant FHIR server, provider systems need to expose a valid FHIR [Conformance](https://www.hl7.org/fhir/DSTU2/conformance.html) profile.

Please also refer to [Development Guidance - FHIR API Guidance - Common API Guidance](development_fhir_api_guidance.html) for full details on the common FHIR API patterns used throughout all the GP Connect APIs.

### Use Cases ###

- [Get the FHIR conformance profile](foundations_use_case_get_the_fhir_conformance_profile.html)

## Spine Interactions

The Foundation capability message set includes the following set of spine interactions:

| Operation                 | InteractionID             | 
|---------------------------|---------------------------| 
| [Read Metadata](foundations_use_case_get_the_fhir_conformance_profile.html) | `urn:nhs:names:services:gpconnect:fhir:rest:read:metadata` |


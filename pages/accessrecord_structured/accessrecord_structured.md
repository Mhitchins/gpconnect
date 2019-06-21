---
title: Introduction
keywords: getcarerecord, structured
tags: [getcarerecord, structured]
sidebar: accessrecord_structured_sidebar
permalink: accessrecord_structured.html
summary: "Introduction to the Access Record Structured capability"
---

## Purpose ##

The Access Record Structured capability enables an NHS system to request and consume a patient’s GP record in a structured and coded format that is machine readable. The data will be made available via a standard API. Structured data allows the consuming system to import and process the patient data in whatever way it requires to best support patients and the healthcare professionals treating them. GP Connect does not place any specific restrictions on how the data is processed so long as the data is only used for the direct care of the patient and the system meets the specified GP Connect consumer requirements (including information governance and clinical safety standards).

## Scope ##

The Access Record Structured capability will expose data for a number of clinical areas. This release supports:

1. Medications
2. Allergies
3. Immunizatons
4. Uncategorised items
5. Consultations
6. Problems

{% include roadmap.html content="Subsequent releases are to be scoped" %}

## Development ##

Please see the [development introduction](accessrecord_structured_development.html) for information on the API use cases supported in this capability.
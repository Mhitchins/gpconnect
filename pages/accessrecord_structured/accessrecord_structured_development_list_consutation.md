---
title: List Consultation
keywords: getcarerecord
tags: [getcarerecord]
sidebar: accessrecord_structured_sidebar
permalink: accessrecord_structured_development_list_consultation.html
summary: "Guidance for populating the list resource at the topic level resource"
---

## Overview

A two or three level List structure is used to represent structured consultations.

1.  **List (Consultation)**

Coded as **325851000000107 |Consultation encounter type (record artifact)|** this top level resource represents the structured Consultation as a whole.

2.  **List (Topic)**

Coded as **25851000000105 |Topic (EHR) (record artifact)|** this level represents the Topic/Problem groupings within Consultations.

3.  **List (Category)**

Coded as **24781000000107 |Category (EHR) (record artifact)|** this level represnts the headings (SOAP heading) levels of the Consultation structure
that contain record entries.

In the case of Consultation which has a 'flat' structure i.e. contains record entries without a surrounding Topic/Category structure
Producer systems gneerate a List(Topic) level which links directly to record entries without the List(Category) level.

Empty Consultations and empty subsections (Topics and headings) are suppressed at source and this is reflected in the cardinalities specified.

### Common List Attributes

The population of List attributes that are common to all of the Consultation List types is described here.

### id

Data type: Id	Optionality: Mandatory	Cardinality: 1..1


The logical identifier of the List resource

### meta.profile

Data type: uri	Optionality: Mandatory	Cardinality: 1..1


The List profile URL.

### identifier

Data type: Identifier	Optionality: Mandatroy	Cardinality: 1..*


This is for business identifiers.

This is sliced to include a cross care setting identifier which MUST be populated. The codeSystem for this identifier is `https://fhir.nhs.uk/Id/cross-care-setting-identifier`.

### status

Data type: code	Optionality: Mandatroy	Cardinality: 1..1


Fixed value of **current**

### mode

Data type: code	Optionality: Mandatory	Cardinality: 1..1


Fixed value of **snapshot**

### subject

Data type: Reference(Patient)	Optionality: Mandatory	Cardinality: 1..1


Reference to the Patient resource for the patient whose patient record contains a consultation represented by this List resource.

The Patient reference is provided by all Lists in the structure rather than the top level List(Consultation) only.

### encounter

Data type: Reference(Encounter)	Optionality: Mandatory	Cardinality: 1..1


Mandatory reference to the Encounter resource providing the context for the Consultation (Date/Doctor/Place ....)

The Encounter reference is provided by all Lists in the structure rather than the top level List(Consultation) only.

### date

Data type: dateTime	Optionality: Mandatory	Cardinality: 1..1


The system rather than clinical date time for when the consultation was last edited i.e. the date time the consultation was last modified on the source system.

If no separate date time is recorded for Consultation sub sections, the overall audit date of the Consultation is replicated at all levels.

The clinically significant or effective Consultation date is provided by the associated Encounter resource.

### source

Data type: Reference(Practitioner)	Optionality: Mandatory	Cardinality: 1..1


The system (audit trail) user attributed to the authoring or modification the consultation.

The source Practitioner reference should be replaicate at all levels of the Consultation List structure.

### orderedBy

Data type: CodeableConcept	Optionality: Mandatory	Cardinality: 1..1


Fixed value of **system** from  vocabulary.

By convention the order entries should appear is the default order subsections or entries are displayed by the native system at source.

### note

Not used withn the Lists representing the Consultation structure

## List (Consultation)

The List level representing the Consultation as a whole.

### title

Data type: string	Optionality: Required	Cardinality: 0..1


The type or name of the consultation as it appears to users of the source system. 
This duplicates the Consultation type/name provided at **Encounter.type**

### code

Data type: CodeableConcept	Optionality: Mandatory	Cardinality: 1..1


Fixed value of **325851000000107 |Consultation encounter type (record artifact)|**

### entry

Data type: BackboneElement	Optionality: Mandatory	Cardinality: 1..*


Will contain at least one reference to a List resource providing the Topic level of the Consultation structure.

**flag**, **deleted** and **date** are not used.

## List (Topic)

The List level representing the Topic level of the Consultation structure.

### title

Data type: string	Optionality: Required	Cardinality: 0..1



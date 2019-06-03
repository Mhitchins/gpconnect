---
title: Observation - uncategorised
keywords: getcarerecord
tags: [getcarerecord]
sidebar: accessrecord_structured_sidebar
permalink: accessrecord_structured_development_guidance_observation_uncategorisedData.html
summary: "Guidance for the representation and consumption of observation resource representing uncategorised data items from the GP record"
---

## Introduction ##

There are many instances of uncoded freetext narrative notes within patient records.
* Paragraphs of freetext consultation notes perhaps associated with codes via the wider consultation context but not otherwise explicitly coded.
* Text which may or may not be associated with codes but where the association is inexact and it is considered better to express the notes text as a standalone item rather than bind the text (possibly incorrectly) to an associated coded resource. See Consultation guidance for further information.
* Representing uncoded or structural elements in patient records for which no suitable resource or is currently available to represent the information and for which falling back to a coded Observation resource as the 'uncategorised' representation is inappropriate e.g. no appropriate codes are available to represent the source record entry as an Observation.

Because FHIR does not provide an underlying resource suitable for representing this information a profile of Observation is utilised to represent narrative text as a Observation Narrative.

## Approach ##

The approach for all of these cases is to use an appropriate coded Observation resource to represent the freetext.

Instances of Observation narrative are identified by Observation.code of **900000000000540000 |Plain text (foundation metadata concept)|**

** TO DO - confirm code **

## Observation Narrative ##

1. All mandatory fields **MUST** be populated.

2. Required fields **MUST** always be populated where the data exists in the system apart from where a lexically identical value exists for an equivalent data item in one of the parent profiles. 

3. Any attributes of the underlying Observeation profile that are not listed below are not used.


### id ###

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Id</code></td>
    <td><b>Optionality:</b> Mandatory</td>
    <td><b>Cardinality:</b> 1..1</td>
  </tr>
</table>

The logical identifier of the Observation narrative resource.

### meta.profile ###

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>uri</code></td>
    <td><b>Optionality:</b> Mandatory</td>
    <td><b>Cardinality:</b> 1..1</td>
  </tr>
</table>

The Observation narrative profile URL.

### identifier ###

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Identifier</code></td>
    <td><b>Optionality:</b> Mandatory</td>
    <td><b>Cardinality:</b> 1..*</td>
  </tr>
</table>

This is for business identifiers.

This is sliced to include a cross care setting identifier which MUST be populated. The codeSystem for this identifier is `https://fhir.nhs.uk/Id/cross-care-setting-identifier`.


### status ###

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>status</code></td>
    <td><b>Optionality:</b> Mandatory</td>
    <td><b>Cardinality:</b> 1..1</td>
  </tr>
</table>

Fixed value of **finished**. 



### code ###

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>CodeableConcept</code></td>
    <td><b>Optionality:</b> Mandatory</td>
    <td><b>Cardinality:</b> 1..1</td>
  </tr>
</table>

Fixed value of **900000000000540000 |Plain text (foundation metadata concept)|**

### subject ###

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Reference(Patient)</code></td>
    <td><b>Optionality:</b> Mandatory</td>
    <td><b>Cardinality:</b> 1..1</td>
  </tr>
</table>

Reference to Patient resource representing the Patient against whom the narrative text was recorded.

### context ###

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Reference(Encounter)</code></td>
    <td><b>Optionality:</b> Optional</td>
    <td><b>Cardinality:</b> 0..1</td>
  </tr>
</table>

Optional reference to the Encounter resource representing the consultation context in which the narrataive freetext was recorded. Will not be populated where the freetext was recorded ouside of a consultation context.

### effectiveDateTime ###

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>DateTime</code></td>
    <td><b>Optionality:</b> Optional</td>
    <td><b>Cardinality:</b> 0..1</td>
  </tr>
</table>

The clinically relevent effective data or datetime for the narrative record entry.

Where the effective date is unknown or not recorded will be absent, otherwise it should be populated.

### issued ###

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Instance</code></td>
    <td><b>Optionality:</b> Mandatory</td>
    <td><b>Cardinality:</b> 1..1</td>
  </tr>
</table>

The audit trail timestamp representing when the narrative was last modified.

### performer ###

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Reference(Practitioner)</code></td>
    <td><b>Optionality:</b> Mandatory</td>
    <td><b>Cardinality:</b> 1..1</td>
  </tr>
</table>

The Practitioner resource representing the person responsible for recording the narrative

### comment ###

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>String</code></td>
    <td><b>Optionality:</b> Mandatory</td>
    <td><b>Cardinality:</b> 1..1</td>
  </tr>
</table>

The freetext narrative as plain text.

### related ###

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>BackboneElement</code></td>
    <td><b>Optionality:</b> Mandatory</td>
    <td><b>Cardinality:</b> 0..*</td>
  </tr>
</table>

This attribute is used to specify the sequence of the narrative in relation to other resources e.g. when an ordered set of resource is being used to express the structure of a consultation displayed at source. See Consultation guidance for further information.

The **related.type** and **related.target** are used to represent the sequence. Only the **related.type** value of **'sequel-to'** is utilised.


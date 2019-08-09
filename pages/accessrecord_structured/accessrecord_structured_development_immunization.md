---
title: Immunization resource
keywords: structured design
tags: [design,structured]
sidebar: accessrecord_structured_sidebar
permalink: accessrecord_structured_development_immunization.html
summary: "FHIR resource for population guidance for immunization"
div: resource-page
---
## Introduction

The headings below list the elements of the Immunization resource and describe how to populate and consume them.

{% include important.html content="Any element not specifically listed below **MUST NOT** be populated or consumed. A full list of elements not used is available [here](accessrecord_structured_development_immunization.html#elements-not-in-use)." %}

{% include tip.html content="You'll find it helpful to read it in conjunction with the underlying [Immunization profile definition](https://fhir.nhs.uk/STU3/StructureDefinition/CareConnect-GPC-Immunization-1)." %}

## Immunization elements

### id

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Id</code></td>
    <td><b>Optionality:</b> Mandatory</td>
    <td><b>Cardinality:</b> 1..1</td>
  </tr>
</table>

The logical identifier of the Immunization resource.

### meta.profile

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>uri</code></td>
    <td><b>Optionality:</b> Mandatory</td>
    <td><b>Cardinality:</b> 1..1</td>
  </tr>
</table>

The Immunization profile URL.

Fixed value [https://fhir.nhs.uk/STU3/StructureDefinition/CareConnect-GPC-Immunization-1](https://fhir.nhs.uk/STU3/StructureDefinition/CareConnect-GPC-Immunization-1)

### extension[parentPresent]

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>boolean</code></td>
    <td><b>Optionality:</b> Required</td>
    <td><b>Cardinality:</b> 0..1</td>
  </tr>
</table>

Indicates whether a parent was present at the immunisation.

### extension[recordedDate]

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>dateTime</code></td>
    <td><b>Optionality:</b> Mandatory</td>
    <td><b>Cardinality:</b> 1..1</td>
  </tr>
</table>

When the record of the immunization was created on the clinical system.

### extension[vaccinationProcedure]

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>CodeableConcept</code></td>
    <td><b>Optionality:</b> Mandatory</td>
    <td><b>Cardinality:</b> 1..1</td>
  </tr>
</table>

The procedure code describing the vaccine that was administered.

### identifier

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Identifier</code></td>
    <td><b>Optionality:</b> Mandatory</td>
    <td><b>Cardinality:</b> 1..*</td>
  </tr>
</table>

This is for business identifiers.

This is sliced to include a cross-care setting identifier which **MUST** be populated. The codeSystem for this identifier is  `https://fhir.nhs.uk/Id/cross-care-setting-identifier`.

This **MUST** be a GUID.

_Providing_  systems **MUST** ensure this GUID is globally unique and a persistent identifier (that is, it doesn’t change between requests and, therefore, is stored with the source data).

Where  _consuming_  systems are integrating data from this resource to their local system, they **MUST** also persist this GUID at the same time.

### status

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Code</code></td>
    <td><b>Optionality:</b> Mandatory</td>
    <td><b>Cardinality:</b> 1..1</td>
  </tr>
</table>

Fixed to the value <code>completed</code> for all CareConnect profiles.

### notGiven

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Boolean</code></td>
    <td><b>Optionality:</b> Mandatory</td>
    <td><b>Cardinality:</b> 1..1</td>
  </tr>
</table>

Fixed value of <code>false</code>.

### vaccineCode

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>CodeableConcept</code></td>
    <td><b>Optionality:</b> Mandatory</td>
    <td><b>Cardinality:</b> 1..1</td>
  </tr>
</table>

Vaccine product administered.

Where the vaccine product that was administered is not known then one of the null flavour values defined in the profile **MUST** be populated.

### patient

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Reference(Patient)</code></td>
    <td><b>Optionality:</b> Mandatory</td>
    <td><b>Cardinality:</b> 1..1</td>
  </tr>
</table>

A reference to the patient who had the immunisation specified.

### encounter

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Reference(Encounter)</code></td>
    <td><b>Optionality:</b> Required</td>
    <td><b>Cardinality:</b> 0..1</td>
  </tr>
</table>

The consultation within which the immunisation was recorded.
This may be when the vaccination was administered or when the registered practice was informed of an immunisation administered elsewhere.

As per base profile guidance.

### date

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>dateTime</code></td>
    <td><b>Optionality:</b> Required</td>
    <td><b>Cardinality:</b> 0..1</td>
  </tr>
</table>

The date (and time if applicable) when the immunization was administered.
If the immunisation was administered elsewhere, this may be an estimated or partial date.

### primarySource

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Boolean</code></td>
    <td><b>Optionality:</b> Mandatory</td>
    <td><b>Cardinality:</b> 1..1</td>
  </tr>
</table>

This indicates whether the record is based on information from the person who administered the vaccine.

This **MUST** be <code>true</code> where the immunisation record was recorded by the person who administered the vaccine or directly on behalf of the administrator of the vaccine (this includes recording the immunisation based on a complete, original, verifiable document from the administration of the vaccine).
This **MUST** be <code>false</code> where it a secondary report of a vaccination for example the recollection of the patient, the patient's parent, carer or guardian or a secondary document. 

As this relates to the context of the original source of the immunisation record, a record from a GP2GP transfer is still a primary record if it was originally recorded as primary.
If it is not known whether the record of the vaccination was made from a primary or secondary source, then return the default value of <code>true</code>.

### reportOrigin

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>CodeableConcept</code></td>
    <td><b>Optionality:</b> Required</td>
    <td><b>Cardinality:</b> 0..1</td>
  </tr>
</table>

This indicates the source of a secondary reported record.

This provides additional context to the source of the immunisation record where it is not based on information from the person who administered the vaccine.
This can be absent if the record is known not to be the primary source, but the origin of the record is otherwise not recorded / unknown.

This **MUST NOT** be included where <code>primarySource</code> is <code>true</code>.

### location

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Reference(Location)</code></td>
    <td><b>Optionality:</b> Required</td>
    <td><b>Cardinality:</b> 0..1</td>
  </tr>
</table>

The GP practice, branch surgery or other location where the vaccination occurred, if known.

If the immunisation record in the GP Clinical System does not record a location for the vaccination, but is linked to a consultation during which the vaccine was administered, then the <code>location</code> **MUST** be populated with the location of the consultation.
If the immunisation was not administered during the linked consultation (the vaccine was administered elsewhere but reported and recorded in that consultation), then a <code>location</code> **MUST NOT** be returned.

### manufacturer

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Reference(Organization)</code></td>
    <td><b>Optionality:</b> Required</td>
    <td><b>Cardinality:</b> 0..1</td>
  </tr>
</table>

The manufacturer of the vaccine.

### lotNumber

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>string</code></td>
    <td><b>Optionality:</b> Required</td>
    <td><b>Cardinality:</b> 0..1</td>
  </tr>
</table>

The batch number of the vaccine.

### expirationDate

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>date</code></td>
    <td><b>Optionality:</b> Required</td>
    <td><b>Cardinality:</b> 0..1</td>
  </tr>
</table>

The expiry date of the batch the vaccine is from.

### site

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>CodeableConcept</code></td>
    <td><b>Optionality:</b> Required</td>
    <td><b>Cardinality:</b> 0..1</td>
  </tr>
</table>

The site on the body where the vaccine was administered.

### route

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>CodeableConcept</code></td>
    <td><b>Optionality:</b> Required</td>
    <td><b>Cardinality:</b> 0..1</td>
  </tr>
</table>

The route through which the vaccine entered the body.

### doseQuantity

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>SimpleQuantity</code></td>
    <td><b>Optionality:</b> Optional</td>
    <td><b>Cardinality:</b> 0..1</td>
  </tr>
</table>

The amount of the vaccine administered.

### practitioner

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>BackboneElement</code></td>
    <td><b>Optionality:</b> Required</td>
    <td><b>Cardinality:</b> 0..*</td>
  </tr>
</table>

The person who recorded the vaccine as having been administered **MUST** be included.
This **SHOULD** also include the practitioner who adminisitered the vaccine if that is known.

### practitioner.role

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Code</code></td>
    <td><b>Optionality:</b>Required</td>
    <td><b>Cardinality:</b> 0..1</td>
  </tr>
</table>

The role of the referenced practitioner.

The code <code>EP</code> (Entering Provider) **MUST** be used to designate the practitioner as having recorded the vaccination.

The code <code>AP</code> (Administering Provider) **MUST** be used to designate the practitioner as having administered the vaccination.

### practitioner.actor

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Reference(Practitioner)</code></td>
    <td><b>Optionality:</b> Mandatory</td>
    <td><b>Cardinality:</b> 1..1</td>
  </tr>
</table>

A reference to the <code>practitioner</code> resource for who administered and / or recorded the vaccine. 

If the GP Clinical System only captures who recorded the vaccination, and it cannot determine whether this was the practitioner who administered the vaccine, then return a single <code>actor</code> reference with a <code>role</code> of <code>EP</code>.

This is mandatory where the <code>practitioner.role</code> is populated.

### note

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Annotation</code></td>
    <td><b>Optionality:</b> Required</td>
    <td><b>Cardinality:</b> 0..*</td>
  </tr>
</table>

Notes about the immunization.

### explanation.reason

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>CodeableConcept</code></td>
    <td><b>Optionality:</b> Required</td>
    <td><b>Cardinality:</b> 0..*</td>
  </tr>
</table>

The reason why the immunization was given, for example, travel, occupation, and so on.

### vaccinationProtocol

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>BackboneElement</code></td>
    <td><b>Optionality:</b> Required</td>
    <td><b>Cardinality:</b> 0..*</td>
  </tr>
</table>

The protocol for the vaccination.

### vaccinationProtocol.doseSequence

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>positiveInt</code></td>
    <td><b>Optionality:</b> Mandatory</td>
    <td><b>Cardinality:</b> 1..1</td>
  </tr>
</table>

If the immunisation is achieved via a series of vaccinations, this is the position of the vaccine procedure in the series.

### vaccinationProtocol.description

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>string</code></td>
    <td><b>Optionality:</b> Required</td>
    <td><b>Cardinality:</b> 0..1</td>
  </tr>
</table>

A description for the vaccination protocol this vaccination is administered under.

### vaccinationProtocol.seriesDoses

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>positiveInt</code></td>
    <td><b>Optionality:</b> Required</td>
    <td><b>Cardinality:</b> 0..1</td>
  </tr>
</table>

The number of doses in the series which are required for vaccination given (at the time it was given).

### vaccinationProtocol.targetDisease

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>CodeableConcept</code></td>
    <td><b>Optionality:</b> Mandatory</td>
    <td><b>Cardinality:</b> 1..*</td>
  </tr>
</table>

The disease or diseases the patient is being immunised against.

### vaccinationProtocol.doseStatus

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>CodeableConcept</code></td>
    <td><b>Optionality</b> Mandatory</td>
    <td><b>Cardinality</b> 1..1</td>
  </tr>
</table>

Fixed value <code>count</code>

<h2 style="color:#ED1951;"> Immunization elements <b>not in use</b> </h2>

The following elements **MUST NOT** be populated:

<h3 style="color:#ED1951;"> explanation.reasonNotGiven </h3>

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>CodeableConcept</code></td>
  </tr>
</table>

Only Immunizations where <code>notGiven</code> is set to <code>false</code> are to be sent using the Immunization profile.
This means that there will never be cause to use <code>reasonNotGiven</code>.

<h3 style="color:#ED1951;"> reaction </h3>

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>BackboneElement</code></td>
  </tr>
</table>

Any reaction to an immunization **MUST** be sent separately in an <code>AllergyIntolerance</code> resource.

<h3 style="color:#ED1951;"> vaccinationProtocol.authority </h3>

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Reference(Organization)</code></td>
  </tr>
</table>

<h3 style="color:#ED1951;"> vaccinationProtocol.series </h3>

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>string</code></td>
  </tr>
</table>

<h3 style="color:#ED1951;"> vaccinationProtocol.doseStatusReason </h3>

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>CodeableConcept</code></td>
  </tr>
</table>

<br>

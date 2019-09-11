---
title: ReferralRequest resource
keywords: structured design
tags: [design,structured]
sidebar: accessrecord_structured_sidebar
permalink: accessrecord_structured_development_referralrequest.html
summary: "FHIR resource for population guidance for ReferralRequest"
div: resource-page
---
## Introduction

The headings below list the elements of the ReferralRequest resource and describe how to populate and consume them.

{% include important.html content="Any element not specifically listed below **MUST NOT** be populated or consumed. A full list of elements not used is available [here](accessrecord_structured_development_referralrequest.html#elements-not-in-use)." %}

{% include tip.html content="You'll find it helpful to read it in conjunction with the underlying [ReferralRequest profile definition](https://fhir.nhs.uk/STU3/StructureDefinition/CareConnect-GPC-ReferralRequest-1)." %}

## ReferralRequest elements

### id

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Id</code></td>
    <td><b>Optionality:</b> Mandatory</td>
    <td><b>Cardinality:</b> 1..1</td>
  </tr>
</table>

The logical identifier of the ReferralRequest resource.

### meta.profile

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>uri</code></td>
    <td><b>Optionality:</b> Mandatory</td>
    <td><b>Cardinality:</b> 1..1</td>
  </tr>
</table>

The ReferralRequest profile URL.

Fixed value [https://fhir.nhs.uk/STU3/StructureDefinition/CareConnect-GPC-ReferralRequest-1](https://fhir.nhs.uk/STU3/StructureDefinition/CareConnect-GPC-ReferralRequest-1)

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

If the referral was made via the e-Referral Service and a UBRN exists for the referral, then it **MUST** be included as an identifier.
The codeSystem for this is XXXXXXXXXX

### basedOn

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Reference(CarePlan, ProcedureRequest, ReferralRequest)</code></td>
    <td><b>Optionality:</b> Optional</td>
    <td><b>Cardinality:</b> 0..*</td>
  </tr>
</table>

Indicates any plans or prior referrals that this referral is intended to fulfill.

### status

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Code</code></td>
    <td><b>Optionality:</b> Mandatory</td>
    <td><b>Cardinality:</b> 1..1</td>
  </tr>
</table>

Fixed value of <code>unknown</code>.
Referrals 'entered in error' must not be included.

### intent

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Code</code></td>
    <td><b>Optionality:</b> Mandatory</td>
    <td><b>Cardinality:</b> 1..1</td>
  </tr>
</table>

Fixed value of <code>order</code>.

### priority

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Code</code></td>
    <td><b>Optionality:</b> Required</td>
    <td><b>Cardinality:</b> 0..1</td>
  </tr>
</table>

A mapping is applied to the priority codes to align it to the e-Referral Service priority types.
This **MUST** be populated where the source system has a referral priority which matches the e-Referral Service or can be reasonably mapped to those priority codes.
If there is a priority code for the referral but it is incompatible with the e-Referral Service priorities, this element **MUST** be excluded and the priority **MUST** be supplied in the <code>note</code> element. 

### serviceRequested

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>CodeableConcept</code></td>
    <td><b>Optionality:</b> Optional</td>
    <td><b>Cardinality:</b> 0..*</td>
  </tr>
</table>

This **MUST NOT** be populated with the main SNOMED CT coded value for the referral, which **MUST** be returned in the <code>reasonCode</code> element.
This **MAY** be populated if the GP Clinical System holds a distinct entry for the type of service requested. 

### subject

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Reference(Patient)</code></td>
    <td><b>Optionality:</b> Mandatory</td>
    <td><b>Cardinality:</b> 1..1</td>
  </tr>
</table>

A reference to the patient who is the subject of the referral.

### context

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Reference(Encounter)</code></td>
    <td><b>Optionality:</b> Required</td>
    <td><b>Cardinality:</b> 0..1</td>
  </tr>
</table>

The `Consultation` within which the referral was recorded.

### authoredOn

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>dateTime</code></td>
    <td><b>Optionality:</b> Required</td>
    <td><b>Cardinality:</b> 0..1</td>
  </tr>
</table>

The user entered date for the referral.

### requester

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>BackboneElement</code></td>
    <td><b>Optionality:</b> Required</td>
    <td><b>Cardinality:</b> 0..1</td>
  </tr>
</table>

Who has administered the referral request.

### requester.agent

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Reference(Device, Organization, Patient, RelatedPerson, Practitioner)</code></td>
    <td><b>Optionality:</b> Mandatory</td>
    <td><b>Cardinality:</b> 1..1</td>
  </tr>
</table>

Where a practitioner is recorded as the referrer, this **MUST** be the referenced <code>Practitioner</code>.
If the referrer is not known, then this **SHOULD** be the practitioner who recorded the referral.
If <code>Practitioner</code> cannot be populated with either of these, then any of the other resource options **MAY** be used as most appropriate.

### requester.onBehalfOf

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Reference(Organization)</code></td>
    <td><b>Optionality:</b> Required</td>
    <td><b>Cardinality:</b> 0..1</td>
  </tr>
</table>

This **MUST** be populated if the <code>requester.agent</code> is a practitioner and the <code>Organization</code> associated to the <code>Practitioner</code> is not the GP Practice responsible for the referral.  

### specialty

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>CodeableConcept</code></td>
    <td><b>Optionality:</b> Optional</td>
    <td><b>Cardinality:</b> 0..1</td>
  </tr>
</table>

This **MUST NOT** be populated with the main SNOMED CT coded value for the referral, which **MUST** be returned in the <code>reasonCode</code> element.
This **MAY** be populated if the GP Clinical System holds a distinct entry for clinical / practitioner specialty being requested by the referral. 

### recipient

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Reference(HealthcareService, Organization, Practitioner)</code></td>
    <td><b>Optionality:</b> Required</td>
    <td><b>Cardinality:</b> 0..*</td>
  </tr>
</table>

This **MUST** be populated with the practitioner and / or organisation the patient has been referred to, if that is recorded in a suitable coded format.
If there are recipient details of who the referral is to are captured as text entries and cannot be returned as a referenced resource, the details **MUST** be populated to the <code>note</code> as key value pairs.

### reasonCode

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> CodeableConcept</td>
    <td><b>Optionality:</b> Required</td>
    <td><b>Cardinality:</b> 0..*</td>
  </tr>
</table>

The primary SNOMED CT coded term for the type of referral.
Additionally, SNOMED CT or locally coded entries **MAY** be included as reasons for the referral provided the term is distinctly a reason for referral, otherwise populate the information in <code>note</code> as key value pairs. 

### description

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>string</code></td>
    <td><b>Optionality:</b> Required</td>
    <td><b>Cardinality:</b> 0..1</td>
  </tr>
</table>

The free text description for the referral.

### supportingInfo

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Reference(Any)</code></td>
    <td><b>Optionality:</b> Required</td>
    <td><b>Cardinality:</b> 0..*</td>
  </tr>
</table>

Reference to the referral letter(s) and any other supporting documents. 
This can also include reference to any other resources which are relevant to the referral but are not specified to use any other <code>referralRequest</code> element, for instance, this could include reference to linked observations or test results.

### note

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Annotation</code></td>
    <td><b>Optionality:</b> Required</td>
    <td><b>Cardinality:</b> 0..*</td>
  </tr>
</table>

Any additional information recorded against the referral.
This could include additional categorisation of the referral or notes recorded against the referral after it has been made such as details of progress or outcomes.


<h2 style="color:#ED1951;"> Elements <b>not in use</b> </h2>

The following elements **MUST NOT** be populated:

<h3 style="color:#ED1951;"> definition </h3>

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Reference(ActivityDefinition, PlanDefinition)</code></td>
  </tr>
</table>

This is not required by GP Connect.

<h3 style="color:#ED1951;"> replaces </h3>

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Reference(ReferralRequest)</code></td>
  </tr>
</table>

Terminated referrals are not in scope so cannot be referenced.
Any association to a prior, completed referral can be made via the basedOn element.

<h3 style="color:#ED1951;"> groupIdentifier </h3>

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Identifier</code></td>
  </tr>
</table>

This is not required by GP Connect.

<h3 style="color:#ED1951;"> type </h3>

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>CodeableConcept</code></td>
  </tr>
</table>

This element has not been defined for GP Connect use.
The <code>reasonCode</code> element **MUST** be used for the SNOMED CT coded referral code.

<h3 style="color:#ED1951;"> occurrence </h3>

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>dateTime, Period</code></td>
  </tr>
</table>

<h3 style="color:#ED1951;"> reasonReference </h3>

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Reference(ProblemHeader-Condition)</code></td>
  </tr>
</table>

A referral may have a linked problem which represents the reason for the referral.
This is required information where it exists, but the problem **MUST** be included and reference to the <code>referralRequest</code>.
The <code>referralRequest</code> **MUST NOT** reference to the problem.

<h3 style="color:#ED1951;"> relevantHistory </h3>

<table class='resource-attributes'>
  <tr>
    <td><b>Data type:</b> <code>Reference(Provenance)</code></td>
  </tr>
</table>

<br>

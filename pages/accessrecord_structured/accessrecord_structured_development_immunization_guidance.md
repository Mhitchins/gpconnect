---
title: Immunisation guidance
keywords: structured design
tags: [design,structured]
sidebar: accessrecord_structured_sidebar
permalink: accessrecord_structured_development_immunization_guidance.html
summary: "FHIR&reg; resource Immunization"
---

## What is an immunisation?

An immunisation is the event of a patient being administered a vaccination. This may be a contemporaneous record by the clinician administering the vaccination (or by another member of the practice staff recording the event directly on behalf of the clinician) or it may be a record of an immunisation administered elsewhere as reported to the registered GP practice by the patient, a carer, guardian or other representative of the patient or another healthcare provider.

Immunisation may be given as part of a scheduled programme of immunisations such as childhood immunisations, seasonal influenza vaccination or in response to specific circumstances (for example, prior to travel, disease outbreak or occupational risk).

## Using the procedure code

GP Clinical Systems do not record the full vaccine product (dm+d code) as their main identifier for immunisation.
GP Clinical Systems often record the type of vaccine administered as opposed to the vaccine product.
This may be as a procedure code or a local code which maps to a procedure code. 
The immunisation procedure code will be used as the main vaccine identifier. 
The vaccine product MUST be included if it is available.

## Vaccinations that were not given

This version of the specification only supports immunisation which have been given to the patient.
GP Clinical Systems may capture details of circumstances where an immunisation has not been given but there was an intention to.
Planned immunisations may not be given for a variety of reasons, such as:

* an explicit refusal for a specific vaccine or for any form of immunisation
* lack of consent for a vaccine to be administered
* the vaccine is temporarily declined
* a contraindication with another medication
* the patient does not attend an appointment or does not attend an open clinic (for example, seasonal flu vaccination clinic)
* the patient is unwell or does not meet criteria for the vaccination

All circumstances of an immunisation not given are out of scope of this version.
Options are to be evaluated for each not given use case and whether to include details in another resource or a future version of this profile.

## Ineffective vaccination

In the event a vaccination is suspected or found to be ineffective, for example as a result of a product recall or cold chain break, the Immunization FHIR profile contains a field indicating that it does not count towards immunity.
GP clinical systems do not have a standard means to identify an ineffective vaccination, hence immunisaton records will always be returned as having counted towards immunity.

## Reactions to a vaccine

Allergic or adverse reactions to an immunisation may be captured in the GP Clinical System but these are not generally directly associated to the immunisation event.
It has not been considered reliable to link any allergic or adverse reaction to the immunisation record therefore information and reactions will not be included.
For details of allergies or adverse reaction, the Allergies resource **MUST** be requested.

## Immunisation schedules and recalls

The resources required to describe planned immunisation schedules or diarised recalls to complete an immunisation series are out of scope for this guidance.

## Immunisation notes

GP Systems that support a note entry against the immunisation **MUST** populate the text to the <code>note</code> element.
Additionally, any other information relevant to the immunisation which does not have a suitable element within the <code>immunization</code> profile **MUST** be populated to the <code>note</code> as a key value pair.
This includes where there is an element in the profile for the type of information, but the data type is not compatible with the way  the data is recorded in the GP System.
For example, the vaccine manufacturer is recorded in a free text field so is not suited as a reference to an <code>oragnization</code> resource for the <code>manufacturer</code> element and is therefore populated to the <code>note</code> element as 'Manaufacturer: Acme Pharmaceuticals'.

## Using the `List` resource for immunisation queries

The results of a query for immunisation details **MUST** return a `List` containing references to all `Immunisation` resources that are returned.

The `List` **MUST** be populated in line with the guidance on `List` resources.

If the `List` is empty, then an empty `List` **MUST** be returned with an `emptyReason` with the value `noContent`.


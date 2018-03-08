---
title: Population
keywords: getcarerecord
tags: [getcarerecord]
sidebar: accessrecord_structured_sidebar
permalink: accessrecord_structured_development_allergies_population.html
summary: "Guidance for populating and reading the AllergyIntolerance resource"
---

The table below shows you how to use each element of the AllergyIntolerance resource. You'll find it helpful to read it in conjunction with the underlying [AllergyIntolerance resource definition](https://fhir.nhs.uk/STU3/StructureDefinition/CareConnect-GPC-AllergyIntolerance-1).

## AllergyIntolerance element usage ##

Element | Usage | Datatype | Optionality | Guidance 
-------|--------|----------|:-----------:|------------------------------------------------
`EndDate`  | The date the allergy or intolerance was recorded as resolved. | `Extension (valueDateTime)` | R | Must be populated if the status is set to 'resolved'.
`EndReason`| The reason why the allergy or intolerance has been resolved. | `Extension (valueString)` | R |
`Evidence` | Reference to confirmatory diagnostic report, e.g. pathology RAST test result. | `Extension (valueReference)` | O |
`clinicalStatus` | 'active' for all active allergies. 'resolved' for resolved allergies. | `Code` | M | Producers which support the concept of resolved/ended allergies SHOULD set the clinicalStatus of resolved allergies to 'resolved'.
`verificationStatus` | Fixed value of 'unconfirmed' | `Code` | M | DO NOT USE - this value is mandatory in base FHIR so cannot be removed. It is not a concept in GP systems and as such no meaning SHOULD be attributed to this field in consuming systems.
`type` | Set to 'allergy' for reactions which are allergenic in nature (immunological), a value of 'intolerance may be used to indicate Adverse Reactions (not immunologic in nature). Where the type is unknown the type element may be omitted. | `Code` | O |Some systems allow explicit identification of Adverse Reactions and Intolerances and the type SHOULD be used to make this distinction where it exists.
`category` | Use 'medication' for all drug allergy types, 'environmental' for all non-drug allergies. The other values in the ValueSet (food and biologic) SHOULD NOT be used by convention. | `Code` | M |See note on 'AllergyIntolerance Category'.
`criticality` | Distinguishes between life-threatening (high) and non-life-threatening (low) potential as well as unable-to-assess. May be used in addition to severity within the reaction element to express severity, e.g. systems that support a severity of life-threatening may set criticality to high. | `Code` | O | May be used in conjunction with reaction/severity by systems which support a severity of 'Life Threatening' or equivalent.
`code` | The causative agent such as food, drug or substances that has caused or may cause an allergy, intolerance or adverse reaction in this patient. | `CodeableConcept` | M | Systems will evolve to use the specified vocabulary of SNOMED CT concepts from the specified subset. The subset includes products and concepts from the substance and product hierarchies and allows medication concepts from the dm+d SNOMED CT extension. In the interim this coded element will hold the primary code for the AllergyIntolerance which may in the case of drug allergies be a medication code or a pre-coordinated code which triggers decision support on the system. Where the AllergyIntolerance has no coded representation in the source system, but is identified as such in the source record then the appropriate degrade code may be used and the text of the AllergyIntolerance placed in the text of the code.
`onset[dateTime]` | Date when allergy/intolerance first manifested. Currently restricted to values of dateTime for GP Connect provider systems. | `dateTime` | R | Present and populated when the provider system records an explicit onset date for an allergy.
`assertedDate` | The datetime the record was recorded or believed to be true. | `dateTime` | M | The asserted date is when the allergy related to the patient was asserted. In many cases, this will be when the allergy is entered onto the system, although some systems may allow this date to be modified.
`lastOccurrence` | Represents the date and/or time of the last known occurrence of a reaction event. | `dateTime` | O | May not currently be available from participating systems and may be omitted. Ommission SHOULD NOT prejudice the ability of providers and consumers to process this element if and when it is available.
`note` | All text associated with the AllergyIntolerance including user-entered notes and qualifiers is grouped together and expressed in this field. Ensures unmapped coded values or qualifiers are not lost. | `Annotation` | R | Must be used to contain any textual data relevant to the allergy.
`reaction.note` | NOT TO BE USED | `Annotation` | O | AllergyIntolerance.note SHOULD contain all of the consolidated text from the allergy/intolerance.
`reaction.manifestation` | Conveys the reaction resulting from the allergy/intolerance as a code. Where no code is available, but a textual description of the reaction is available then the nullFlavor UNC may be used and the textual description conveyed via reaction/description. If no reaction has explicitly been recorded, but the reaction element is present to convey severity, then reaction/manifestation SHOULD be coded as the nullFlavor NI. If the patient has been asked, but is unable to specify a reaction the nullFlavor, 'ASKU' SHOULD be used. | `CodeableConcept` | O |
`reaction.description` | Conveys the textual description of the manifestation where no code is available. | `String` | O | A consuming system may concatenate the contents (appropriately labelled) with text in AllergyIntolerance.note if a textual description of the manifestation is not supported in the receiving system record structure.
`reaction.severity` | Severities of mild, moderate, severe are mapped directly to the FHIR ValueSet. Map life threatening to severe and populate criticality with high. | `Code' | O |Unmapped or converted severity codes in original system SHOULD be expressed in AllergyIntolerance.note.
`reaction.exposureRoute` | The route by which exposure to the substance causing the reaction occurred. Utilise the dm+d route codes. | `CodeableConcept` | O | 
`reaction.onset[x]` | NOT TO BE USED | `Choice` | O | Onset explicitly supplied via AllergyIntolerance.onset[dateTime].
`reaction.substance` | NOT TO BE USED | `CodeableConcept` | O | The causative is explicitly and specifically coded via AllergyIntolerance.code.


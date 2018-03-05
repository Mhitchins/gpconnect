---
title: Allergy Intolerance
keywords: getcarerecord
tags: [getcarerecord]
sidebar: accessrecord_structured_sidebar
permalink: accessrecord_structured_development_allergies_guidance.html
summary: "Guidance for populating and reading the AllergyIntolerance resource"
---

## Guidance

### Entries of allergy concepts as 'non-allergies' in source systems
Participating systems have a variety of record types/structures that explicitly represent Allergy and Intolerance structures within the patient record. These structures are explicitly selected by users to record Allergy and Intolerance information in the record or may be automatically triggered by attempts to enter allergy codes/concepts into the patient record. As these structures readily identify the presence of Allergy/Intolerance concepts in the record they are readily identifiable and mappable to the GP Connect AllergyIntolerance request when processing FHIR requests. 

It is also possible in some cases to bypass these data entry features and enter allergy codes/concepts as ordinary coded record entries in the patient record. Such entries may appear in the source system as ordinary journal entries and may not appear as Allergies/Intolerances in patient summaries or Allergy/Intolerance lists in provider systems. 

*If it is possible on the provider system to record allergy concepts as non-allergies in the patient record then the system **must** express these record entries as FHIR AllergyIntolerance resources when queried.*

### Allergy/Intolerance interoperability and clinical safety
It is recognised that Allergy/Intolerance information may not be fully interoperable between participating systems. Where Allergy/Intolerance information is not fully understood by a receiving system then it is the responsibility of the receiver to mitigate any risks arising and make related workflows as safe as possible. 

Where AllergyIntolerance information is integrated into the consuming system patient record then AllergyIntolerance information should be degraded where the coded AllergyIntolerance information is not fully understood by the consumer. In the context of drug allergies, an AllergyIntolerance is only understood if the coded causative agent triggers equivalent prescribing decision support to that triggered on the producing system.

Degradation can be handled by coding the record entries resulting from the processed allergy as 196461000000101,Transfer-degraded drug allergy and preserving the original term for the received code as text.

Where the processing of drug allergy AllergyIntolerance resources results in degradation or the FHIR resource being processed is already coded as a degrade drug allergy code (196461000000101, Transfer-degraded drug allergy) then the consuming system should prevent prescribing while degraded drug allergies are present in the patient record and inform users attempting prescribing actions that prescribing is prevented due to the presence of degraded drug allergies.

In other contexts, task based workflows have been utilised to assist users on consuming systems to process degraded drug allergies by re-coding them to concepts understood by the consuming system.

*When considering the implementation of use cases involving AllergyIntolerance interoperability suppliers should perform an appropriate clinical safety assessment and obtain the necessary clinical safety approvals for the processing performed by their system*

### Orthogonality to problem orientation
On some participating systems it is possible to manage Allergy/Intolerance information via problem orientation. This means, for example, making an Allergy/Intolerance record entry a problem heading with associated episodicities, priorities, linkage (to other record entries) and even start and end dates for the problem, independent of the Allergy/Intolerance record entry.

*The dual representation of an Allergy/Intolerance as a problem should not affect the representation of the Allergy/Intolerance via the FHIR resource*

### Unsupported qualifiers
It is possible on some participating systems to attach system specific qualifiers to Allergy/Intolerance record entries, for example a priority for the Allergy/Intolerance record entry or an episodicity. Similarly, some systems support richer sets of values for severity than are catered for via the criticality and reaction/severity elements. The certainty qualifiers that exist in participating systems are not supported. In all cases, the full set of qualifiers and values that are associated with an Allergy/Intolerance in the source system should be rendered as text (suitable formatted name/value pairs) and placed in the AllergyIntolerance/note element.

### Adoption of single notes field
Rather than split descriptive and user entered text across a number of notes fields the AllergyIntolerance/note element is used as the single notes field to convey all qualifiers and user-entered text associated with the Allergy/Intolerance in a single place. Qualifiers and values expressed as text should be appropriately labelled and formatted and where user notes have been entered against explicit fields such as Certainty then appropriate labels should be used.

### 'Resolved' allergies and intolerances
On some systems it is possible to explicitly mark an Allergy/Intolerance as resolved or ended such that it still appears in the patient record but is no longer active but no longer interacts with prescribing decision support. This inactivation may be achieved by explicit entry of an end date or a user action that alters the status of the Allergy and Intolerance.

There is currently no support for expressing the end date of a 'resolved' allergy. 

Allergies and Intolerances which have been explicitly resolved should only be returned in response to resource queries which have the *includeResolvedAllergies* parameter set to true (see GP Connect API).

When the provider is sending resolved allergies it must send them in a separate list to the active allergies as contained resources in that list. The list should have the title 'Resolved Allergies' and resolved allergy resources should be assigned a clinicalStaus of 'resolved'. A title of 'Active Allergies' should be used for the list containing the active allergy resources.

Consuming systems must ensure that resolved allergies are not treated as active i.e. must not interact with prescribing decision support or be misinterpreted by users as being active.

Where the consuming system does not natively support a resolved allergy concept, suppliers should seek appropriate clinical safety advice on the handling of the resolved allergy concept.

### Relationship to yellow card 
Allergy and Intolerance information may co-exist alongside system support for the capture of medication adverse reactions via the MHRA Yellow Card scheme. Suppliers should ensure that only information directly associated with the Allergy and Intolerance entry in the system is included in the AllergyIntolerance resource and not data only associated with the MHRA yellow card dataset. 

### AllergyIntolerance category
It is expected that it will always be possible to assign a category of 'medication' for drug allergies or 'environmental' for all other types of Allergy/Intolerance. Generally, the choice in a given system is explicit. In some cases, the type of Allergy/Intolerance may be more general - for example, a system designated type of 'Other' or equivalent. In such cases, if the Allergy/Intolerance entry interacts with prescribing decision support it should be assigned a category of 'medication', otherwise the category of 'environmental' should be used.

### Date usage
The FHIR AllergyIntolerance resource supports a rich set of date concepts, however there is currently limited support for additional date concepts via the allergy record structures in producing systems. 

The asserted date is when the allergy related to the patient was asserted. In many cases this will be when the allergy is entered on to the system although systems may allow this date to be modified by the user.

If a producing system, currently or at a future point supports allergy date concepts that explicitly capture onset dates and last occurrence dates then the onset[dateTime] and lastOccurrence elements of AllergyIntolerance may be populated.

### Unsupported codes
In some of the participating systems, an additional coded concept may be entered that represents the Allergy/Intolerance in addition to coding of the causative agent and reaction. These additional codes are not explicitly supported by the AllergyIntolerance resource. The unsupported allergy code should be encoded as a textual qualifier in the note element.

*The reasoning being that as systems converge on interoperable coding of Allergies and Intolerances via AllergyIntolerance/code the need for another code to represent the Allergy/Intolerance concept diminishes*

### Negation - Handling 'no known allergies'
Where there is an explicit assertion of the 'No Known Allergies' concept in the record (equivalent to SNOMED CT concept 716186003 and children) and there are otherwise no Allergy/Intolerance entries in the patient record, then systems may respond to queries for all AllergyIntolerance resources for the patient with an empty List containing an emptyReason code of 'nilKnown' with the term of the 'No Known Allergies' present expressed as text.

Where there are no Allergy/Intolerance entries in the patient record, but no explicit recording of the ‘No Known Allergies’ concept and equivalents, then systems should return an empty list with no emptyReason code and a List/note with the text. “There are no allergies in the patient record but it has not been confirmed with the patient that they have no allergies (ie..a ‘no known allergies’ code has not been recorded).”

Because resolved allergies are queried via the includeResolvedAllergies query parameter and resolved allergies are conveyed via a separate list, assertions of 'No known allergies' are true only in the context of a given list. Therefore an empty 'Active Allergies' list only asserts that there are no active allergies and nilKnown in the context of the active allergies list asserts that there are no active allergies to contradict the recorded 'No Known Allergies' code. Similarly where the includeResolvedAllergies flag is present an empty 'Resolved Allergies' list asserts the absence of resolved allergies and nilKnown asserts that the coding of 'No Known Allergies' is not contradicted by the presence of resolved allergies. Only the inclusion of the includedResolvedAllergies flag and emptiness of both lists can fully assert that there are no allergies (active or resolved) in the source record.

In most use cases consuming systems are only interested in active allergies but the solution supports the retrieval of resolved allergies where this is required.

### Degradation actions to be performed by consumers not providers
Provider systems should not in principle limit potential interoperability by pre-emptively degrading coded information in export. It is a consumer responsibility to determine the understandability/processability of received resources and degrade if appropriate. In the case of drug allergies understandibility is determined by the consuming system being able to trigger equivalent prescribing decision support as the source system in response to the causative agent code.

It is expected that initial GP Connect provider implementations will reflect the current state of provider systems prior to full convergence on the specified causative agent subset and at this point causative agents may be coded using legacy terminologies/code systems or contain codes in supported terminologies that lie outside the hierarchies specified by the causative agent subset.

### Reaction cardinality
The AllergyIntolerance/reaction element is optional, but where a severity is available in the source system it will be included to convey severity even if no other reaction details are explicitly available. If this is the case the reaction/manifestation should be coded as the nullFlavor NI.

By convention, only one reaction should be expressed per allergy with only one manifestation per reaction.




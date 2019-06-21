---
title: Consultation
keywords: getcarerecord
tags: [getcarerecord]
sidebar: accessrecord_structured_sidebar
permalink: accessrecord_structured_development_consultation_guidance.html
summary: "Guidance for the representation and consumption of Consultations"
---

## Introduction ##
Consultations are one of the most important forms in which structured patient information is recorded in the patient records within GP systems.

However, there is no standard structure or format for recording consultations that is common across all systems. This presents not only a challenge in developing a common set of FHIR&reg; profiles that are capable of representing the consultation structures that exist within participating systems, but also a challenge for producing systems in expressing native consultations in a standardised form. It is also a challenge for consumers to understand those structures.

It is also important to define what we mean by 'consultation'. Not all clinical information is recorded consistently across systems within structures that can be classed as consultations. Therefore, it is important to note that a consultation-oriented query (precise API TBD) may not return all of the clinical information within a patient record.

This guidance is designed to address these challenges:
* it provides a clear definition of what is meant by consultation in the context of the current set of participating systems
* it identifies the limitations of consultation-based queries in isolation as a method for retrieving all elements of patient records
* it describes the set of consultation structures available in producer systems and guidance for populating the FHIR resources required to represent these structures
* it provides guidance for consumer systems processing the FHIR resources represnting consultation structures on source systems as to the variances between representations and the correct handling of these structures

## What is a consultation ? ##
A consultation is the structure within which source systems group one or more clinical record entries which occurred at the same time and for the same or similar purpose attributed or asserted by the same actor.

* consultations do not exclusively represent Clinician-Patient encounters although they are commonly used for that purpose
* consultations may record purely administrative or communications triggered events on source systems - for example, repeat medication administration or a pathology report filed into the patient record via messaging
* consultations are generally assigned and attributed to what can loosely be termed a 'Date/Doctor/Place/Type' (Encounter), although these attributes may be overridden or refined in the context of individual record entries within the consultation
* consultations may or may not have associated structure - for example, the ability to decompose a Consultation into multiple topics/subjects/problems and, within each 'topic', further group clinical content under headings (SOAP headings) in line with long standing clinical note-taking practice [1]
* consultations may also be recorded in non-structured form - that is, a grouping of clinical record entries under the same 'Date/Dr/Place/Type' but without additional structure (topics or SOAP headings)
* consultations may incorporate a range of different record entries - for example, measurements, test results, textual narrative, structured narrative, coded observations, medications

## Logical structure ##

Consultations follow a common logical structure.

* Context

  Each consultation has a set of context information that describes when and where the consultation took place, the patient it covered and who else was involved (such as a doctor).

* Topics

  A consultation will be split into one of more topics. Each topic describes an area of discussion (or activity) that took place in the consultation.

  A topic may (but not always) be related to a specific problem in the patient’s record. In these cases, all the discussions and activities recorded under that topic are in regards to that problem.

* Headings

  A topic may be split into one of more headings (sometimes referred to as SOAP headings). Each heading covers a specific part of the consultation process about that topic.

  There is no national standard for headings so they will reflect the headings available in the provider clinical system.

* Clinical items

  Within each heading there may be one or more clinical items. Grouped together and in order, these clinical items describe in full all the details recorded under that heading of the consultation.

  Any type of clinical item may form part of the consultation (medications, allergies, immunisations, uncategorised data, and so on). 

* Topics without headings

  A topic can be created without headings. In these cases, the clinical items are recorded directly under the topic the same way they are recorded under a heading.

## Example ##

<img src="images/access_structured/Consultation_Example_v4.png" alt="Sequence diagram for retrieving a patient record" style="max-width:100%;max-height:100%;">

## Approach ##

![Consultation FHIR Resource Model ](images/access_structured/Consultation_FHIR_Resource_Model.png)
![Consultation FHIR Resource Model ](images/access_structured/Consultatiion_FHIR_Resource_Model.png)

{: .center-image }
![Consultation Structure ](images/access_structured/Consultation_Structure.png)
![Consultation Structure ](images/access_structured/Consultation_Stucture.png)

* The Encounter resource and related resources like Location are adopted to provide the consultation context (Date/Doctor/Place).
* The Composition resource is adopted to provide consultation structure. The top level Composition.section is utilised to represent the topic/problem level of the source consultation. Within each Composition.section child sections are used to represent headers (SOAP headings) and references to resources within each heading section reference resources representing each record entry within a section.
* Some systems allow the recording of Consultation content outside of the Topic/Problem and Heading hierarchy - that is, a flat structure of record entries organised with the same 'Date/Dr/Place' context. These structures are handled by the creation of an untitled Composition.section to contain the record entries and entries are referenced directly from that section with no additional Heading subsections.

## Consultation ntes ##

Consultation notes are the human readable version of the clinical information recorded under each heading. They may or may not be accompanied by a clinically coded version of the information.

There are two primary ways that consultation notes are recorded on native GP systems:

* consultation notes for a heading are recorded as a single piece of free text. Any clinical coded information under the same heading is associated to that text as a whole

<img src="images/access_structured/Consultation_text_1a.png" alt="Free text with multiple clinical codes" style="max-width:100%;max-height:100%;">

* consultation notes for a heading are recorded as a collection of observations, each with a clinical code and or text. When read together in order they produce the consultation notes

  Note – this may be entering free text format dynamically identifying codes or through forms where there are specific fields for codes and free text.

<center>
<img src="images/access_structured/Consultation_text_1b.png" alt="Clinical code and text" style="max-width:40%;max-height:40%;">
</center>

&nbsp;

When reflecting these in FHIR it is important they these two methods are represented in a way that retains the structural information they contain, does not create any unintended clinical meaning and can be viewed/imported. This is done by taking any free text in model one and representing it as unstructured data and positioning it as the first clinical item under the heading. 

<img src="images/access_structured/Consultation_text_2.png" alt="Consultation text in FHIR" style="max-width:100%;max-height:100%;">

&nbsp;

While there are difference between the two outputs, the consultation notes can be derived from both by reading through each clinical item in order and merging the Term Text, Clinical Code, Values and Comment into a single narrative.

<img src="images/access_structured/Consultation_text_3.png" alt="Reconstituted Consultation Text" style="max-width:100%;max-height:100%;">

## Consultations containing unsupported clinical items ##
Depending on the GP Connect version supported by the provider system it can be possible for the consultation to link to a clinical item that the provider system is not yet able to export with GP Connect. For example, if the consultation contains a link to a referral record but the provider system does not yet support exporting referrals.

Where a provider system is not able to export a linked clinical item it will create a section.section.entry (or section.entry) entry with the:
*	Reference.Identifier set to null; and
*	Reference.Display set to “Clinical item not supported by the provider system.”

## Suppression of empty consultations ##
On some systems all or almost all record entry is captured in the context of a consultation. This coupled with default behaviours such as starting a consultation on opening a patient record, leads to the phenomenon of 'empty consultations' where the Data/Dr/Place/Type has been created as a default but there is no subsequent entry of any clinical information within the consultation.

Producer systems which allow empty consultations should not return empty consultations in response to consultation queries.

## What information may be recorded outside of consultations ##
* some systems do not restrict entry of clinical data to a consultation context - that is, it is possible to record clinical information about a patient without starting or initiating a consultation
* such 'Non Consultation' behaviour does not imply any loss of information or structure by the source system, that is, the record items are still fully recorded and attributed
* but because they are recorded outside of a consultation context they will not be returned by an API query directed at returning consultation resorces (see [Design decisions](#Design decisions))

## Consumer guidance ##
* Although the expression of full consultation structure is provided by the specified protocols, it is recognised that many consumers simply want access to the resource referenced by the consultations rather than the Topic or SOAP heading structures. Consumers in the category may simply flatten the consultation structure or otherwise extract the resources referenced within the composition carrying the consultation structure.

## Consumer cautions ##

* a consumer making consultation oriented queries only **MUST NOT** expect to obtain all items in the patient record
* other GP Connect APIs will provide full access to items in the patient record including all medications, all allergies, all problems up to an including get whole record queries

## Design decisions ##

* Systems which allow direct recording of data outside of consultation contexts should not fabricate consultations to return such data when consultation queries are received, as to do so would be generating information and structure which does not exist on the source system, and which would obscure the genuine consultation content that does exist. Systems in this category have clear distinctions between consultations and other types of record content - for example, last X consultations displayed in patient summaries and to synthesise consultations would distort this native behaviour.

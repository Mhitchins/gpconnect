---
title: Documents
keywords: getcarerecord
tags: [getcarerecord]
sidebar: accessrecord_structured_sidebar
permalink: accessrecord_structured_development_documents_guidance.html
summary: "Guidance for the representation and consumption of documents"
---

## What is a Document ##

A clinical document (CD) is a written, printed or electronic record that provides evidence of medical care. Clinical documents must be accurate, timely and reflect specific services provided to a patient. 
Clinical documentation is used to facilitate inter-provider communication, allow evidence-based healthcare systems to automate decisions, provide evidence for legal records and create patient registry functions so public health agencies can manage and research large patient populations more efficiently.
HL7 characterizes a document by the following properties:
 - Persistence – Documents are persistent over time. The content of the document does not change from one moment to another. A document represents information stored at a single instance in time.  
 - Wholeness - A document is a whole unit of information. Parts of the document may be created or edited separately, or may also be authenticated or legally authenticated, but the entire document is still to be treated as a whole unit.  
 - Stewardship –A document is maintained over its lifetime by a custodian, either an organization or a person entrusted with its care.
 - Context - A clinical document establishes the default context for its contents  
 - Potential for authentication - A clinical document is an assemblage of information that is intended to be legally authenticated.  

## Problem Statement ##

A Patient's GP Practice is custodian of Patient's clinical documents received from various health and care settings.
Other Practices and health care settings are not able to electronically search for a Patient's document in the Patient's GP practice and retrieve those documents.
This is usually a manual process whereby other GP Practices and Health care settings ask a Patient's GP Practice for documents of the Patient. The Patient's GP Practice then emails or faxes a Patient's documents. This can lead to loss of valuable time and effort of GP Practice's staff.

## Objective ##

Enable health and care organisations to electronically search for and retrieve Patient's clinical documents from their GP Practice.
Standardise the search and retrieval of clinical documents from the GP Practices using GPConnect API Standards.

## As Is ##

<IMG src="images/access_structured/As-Is.jpg" alt="As-Is process for search and retrieval of documents from a GP Practice"  style="max-width:80%;max-height:80%;">
 ### Process Steps ###
 
 <table width="60%" height="60%">
    <thead>
        <tr>
            <th>Id</th>
            <th>Title</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1</td>
            <td>Send document</td>
            <td>Hospital sends document to the GP Practice</td>
        </tr>
      <tr>
            <td>2</td>
            <td>Document upload to clinical system</td>
            <td>The document is manually or automatically uploaded to the Clinical System of the GP Practice.</td>
        </tr>
      <tr>
            <td>3</td>
            <td>Document matched to patient record</td>
            <td>The clinical System matches the document to the patient record.</td>
        </tr>
      <tr>
            <td>4</td>
            <td>File document</td>
            <td>Clinician views the document in their clinical system</td>
        </tr>
      <tr>
            <td>5</td>
            <td>File document</td>
            <td>Clinician reads or meta data information to the document</td>
        </tr>
      <tr>
            <td>6</td>
            <td>File document</td>
            <td>Clinician extracts read codes, add read code or annotates the document.</td>
        </tr>
      <tr>
            <td>7</td>
            <td>File document</td>
            <td>Clinician add the document to a workflow</td>
        </tr>
      <tr>
            <td>8</td>
            <td>Request document</td>
            <td>Hospital request for a document of a Patient from it’s registered GP Practice</td>
        </tr>
      <tr>
            <td>9</td>
            <td>Search document</td>
            <td>Clinician in a GP Practice searches for the document</td>
        </tr>
     <tr>
            <td>10</td>
            <td>Send document</td>
            <td>Clinician emails/faxes the document to the Hospital </td>
        </tr>
     <tr>
            <td>11</td>
            <td>View document</td>
            <td>Hospital receives and views the requested document</td>
        </tr>
    </tbody>
</table>
 

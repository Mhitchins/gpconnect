---
title: Medications
keywords: getcarerecord, view, section, medications
tags: [view,getcarerecord]
sidebar: accessrecord_sidebar
permalink: accessrecord_view_medications.html
summary: "Medications HTML view"
---

| Section code | Section name | TPP | EMIS | Vision | Microtest |
| ------------ | ------------ |-----|------|------|-----------|
| MED | Medications | Yes | Yes | Yes | Yes |


## Clinical narrative ##

A drug or other form of medicine that is used to treat or prevent disease. 

## Purpose ##

The purpose of this section is to provide a chronological history of medication prescribing as recorded.

## Sections and subsections ##

Contains one main section, and three subsections:

 - [Recent Acute Medication](accessrecord_view_medications.html#recent-acute-medication)
 - [Current Repeat Medication](accessrecord_view_medications.html#current-repeat-medication)
 - [All Medication (Summary)](accessrecord_view_medications.html#all-medication-summary)
 - [All Medication Issues](accessrecord_view_medications.html#all-medication-issues)

## Section title ##

The section title **SHALL** be "Medications".
 
## Section content banner ##

Provider's message describing at a summary level how they have populated this section. Also includes the warning message where medications prescribed elsewhere have been excluded.

  
### Discontinued/cancelled/naturally ended medications ###

{% include custominfocallout.html content="**Note:** It is not currently possible for all GP system suppliers to distinguish between “Discontinued”, “Cancelled” and “Naturally ended” medications." type="info" %}

The definitions for each of these are supplied below:

- **Naturally ended:** Medication naturally came to an end - that is, no manual intervention via the user (auto system transition)
- **Cancelled:** Actively stopped acute medication by user
- **Discontinued:** Actively stopped repeat medication by user

Not all suppliers can supply the details (date and reason) for the ending of a medication (either through discontinuation or cancellation).

It is extremely important for the details regarding the ending of a medication to be available to a clinician, as this will highlight any clinical issues (for example, stopping a medication due to an allergy) and allow the clinician to make an efficient and informed clinical decision.

Refer to decision log item (HDL-212) which contains significant detail around this issue.


## Recent Acute Medication ##

### Clinical narrative ###

A list of acute medicines that are currently being, or have recently been, used to treat or prevent disease for the patient.


### Purpose ###

The purpose of this section is to provide a view of acute medications that the patient has recently been taking which informs the clinical decision-making process.


### Subsection title ###

The subsection title **SHALL** be "Recent Acute Medication".


### Date filter ###

The provider **SHALL** include all acute medication whose `Start Date` (the date the prescription is expected to start) is greater than the current date minus 365 days.
All relevant records **SHALL** be returned.


### Subsection content banner ###

Provider's message describing at a summary level how they have populated this section.


### Table columns ###

Providers **SHALL** return all the columns as described in the table below, sorted by `Start Date` descending:

<div>
<table>
<thead>
  <tr class="header">
	<th width="5%">Order</th>
	<th width="23%">Name</th>
	<th width="58%">Description</th>
	<th width="14%">Value details</th>
  </tr>
 </thead>
  <tr>
    <td align="center">1</td>
    <td><code>Start Date</code> <i class="fa fa-sort-desc" aria-hidden="true"></i></td>
    <td>The date of issue of the acute medications, except where:
		<ul>
			<li>the medication is post-dated it <b>SHALL</b> be the post date</li>
			<li>the medication is prescribed elsewhere it <b>SHALL</b> be the expected start date if known, else the entered date </li>
		</ul>
	</td>
    <td><code>dd-Mmm-yyyy</code></td>
  </tr>
  <tr>
    <td align="center">2</td>
    <td><code>Type</code></td>
    <td>Type of medication issued (for example, <code>Acute, Acute Post-Dated, Prescribed Elsewhere – [Agency]<sup><b>1</b></sup></code>).</td>
    <td><code>free-text</code></td>
  </tr> 
  <tr>
    <td align="center">3</td>
    <td><code>Medication Item</code></td>
    <td>Descriptive name of medication item (including dosage)  for example, <code>Ibuprofen 400mg tablets</code>.</td>
    <td><code>free-text</code></td>
  </tr>
  <tr>
    <td align="center">4</td>
    <td><code>Dosage Instruction</code></td>
    <td>Dosage instructions for the medication item. As a minimum, this <b>SHALL</b> include:
		<ul>
			<li>Dosage Rate</li>
			<li>Schedule (when / how often)</li>
		</ul>
	<p>for example, <code>two to be taken daily</code></p>
	</td>
	<td><code>free-text</code></td>
  </tr>
  <tr>
    <td align="center">5</td>
    <td><code>Quantity</code></td>
    <td>Quantity details for the medication item. As a minimum, this <b>SHALL</b> include:
		<ul>
			<li>Quantity</li>
			<li>Quantity unit</li>
		</ul>
	<p>for example, <code>14 capsule</code></p>
	</td>
	<td><code>free-text</code></td>
  </tr>
  <tr>
    <td align="center">6</td>
    <td><code>Scheduled End Date</code></td>
    <td>The date the prescription is expected to finish</td>
    <td><code>dd-Mmm-yyyy</code></td>
  </tr>  
  <tr>
    <td align="center">7</td>
    <td><code>Days Duration</code></td>
    <td>Duration of medication issued in days</td>
    <td><code>integer</code></td>
  </tr>
  <tr>
    <td align="center">8</td>
    <td><code>Additional Information</code></td>
    <td>If the medication record includes the information, the following details <b>SHALL</b> be included:
		<ul>
			<li>Reason for the medication</li>
			<li>Linked problems / diagnoses</li>
			<li>Other supporting information</li>
			<li><code>CANCELLED: </code> label with cancellation date and reason</li>
		</ul>
	<p>The provider <b>MAY</b> include labels in addition to the ones specified to support additional text (for example, <code>Linked Problem : Ear Infection</code>).</p>
	</td>
    <td><code>free-text</code></td>
  </tr>  
</table>
</div>

<sup><b>1</b></sup> Where the medication type is Prescribed Elsewhere the prescribing agency (type of organisation responsible for authorising and issuing the medication) **SHALL** be included with the type of Prescribed Elsewhere - for example, ‘Prescribed Elsewhere – Dentist’.





## Current Repeat Medication##

### Clinical narrative ###

A list of repeat drugs or other forms of medicines that are currently being used to treat or prevent disease for the patient. This may also include PRN occasional use medication for example, EpiPen, antihistamines, monitoring or continence products

### Purpose ###

The purpose of this section is to provide a view of all repeat medications that the patient is currently prescribed, which informs the clinical decision-making process.

### Subsection title ###

The subsection title **SHALL** be "Current Repeat Medication".

### Date filter ###

All relevant records **SHALL** be returned (that is, no time limit/filtering is to be applied).

### Subsection content banner ###

Provider's message describing at a summary level how they have populated this section.

### Table columns ###

Providers **SHALL** return all the columns as described in the table below, sorted by `Start Date` descending:

<div>
<table>
<thead>
  <tr class="header">
	<th width="5%">Order</th>
	<th width="23%">Name</th>
	<th width="58%">Description</th>
	<th width="14%">Value details</th>
  </tr>
 </thead>
  <tr>
    <td align="center">1</td>
    <td><code>Start Date</code> <i class="fa fa-sort-desc" aria-hidden="true"></i></td>
    <td>The original date of authorisation of the repeat medication (if a provider system allows re-authorisation of a repeat medication the start date <b>SHALL</b> be the first authorisation). If this is not known (for example, for prescribed elsewhere) then date of entry of the repeat medication record <b>SHALL</b> be returned.</td>
    <td><code>dd-Mmm-yyyy</code></td>
  </tr>
  <tr>
    <td align="center">2</td>
    <td><code>Type</code></td>
    <td>Type of medication issued (for example, <code>Repeat, Repeat Dispense, Repeat – [Prescribing Agency]</code>).</td>
    <td><code>free-text</code></td>
  </tr>
  <tr>
    <td align="center">3</td>
    <td><code>Medication Item</code></td>
    <td>Descriptive name of medication item (including dosage)  for example, <code>Ibuprofen 400mg tablets</code>.</td>
    <td><code>free-text</code></td>
  </tr>  
  <tr>
    <td align="center">4</td>
    <td><code>Dosage Instruction</code></td>
    <td>Dosage instructions for the medication item. As a minimum, this <b>SHALL</b> include:
		<ul>
			<li>Dosage Rate</li>
			<li>Schedule (when / how often)</li>
		</ul>
	<p>for example, <code>two to be taken daily</code></p>
	</td>
	<td><code>free-text</code></td>
  </tr>
  <tr>
    <td align="center">5</td>
    <td><code>Quantity</code></td>
    <td>Quantity details for the medication item. As a minimum, this <b>SHALL</b> include:
		<ul>
			<li>Quantity</li>
			<li>Quantity unit</li>
		</ul>
	<p>for example, <code>14 capsule</code></p>
	</td>
	<td><code>free-text</code></td>
  </tr>  
  <tr>
    <td align="center">6</td>
    <td><code>Last Issued Date</code></td>
    <td>The last issue date. If the medication is repeat dispense or prescribed elsewhere this <b>SHALL</b> be null.</td>
    <td><code>dd-Mmm-yyyy</code></td>
  </tr>  
  <tr>
    <td align="center">7</td>
    <td><code>Number of Prescriptions Issued</code></td>
    <td>This <b>SHALL</b> be the number issued up to the current date inclusive. For a repeat dispense this will be null. This <b>SHALL</b> be null where no issues of the repeat have been made at the current date. If the medication is repeat dispensed or prescribed elsewhere this <b>SHALL</b> be null.</td>
    <td><code>integer</code></td>
  </tr>
  <tr>
    <td align="center">8</td>
    <td><code>Max Issues</code></td>
    <td>The maximum number of issues the repeat prescription has authorised.</td>
    <td><code>integer</code></td>
  </tr>
  <tr>
    <td align="center">9</td>
    <td><code>Review Date</code></td>
    <td>The date the repeat medication is due for review.</td>
    <td><code>dd-Mmm-yyyy</code></td>
  </tr>  
  <tr>
    <td align="center">10</td>
    <td><code>Additional Information</code></td>
    <td>If the medication record includes the information, the following details <b>SHALL</b> be included:
			<ul>
			<li>Reason for the medication</li>
			<li>Linked problems / diagnoses</li>
			<li>Other supporting information</li>
		</ul>
	<p>The provider <b>MAY</b> include labels in addition to the ones specified to support additional text (for example, <code>Linked Problem : Heart Failure</code>).</p>
	</td>
    <td><code>free-text</code></td>
  </tr>  
</table>
</div>

{% include custominfocallout.html content="**Note:** If the provider system allows the repeat medication (master / template) details to be altered pending or as new repeat issues are generated, then the latest details **SHALL** be returned by the provider in the detail column (for example, TPP current include the guidance text ‘The medication above is taken from a list of Repeat Medication Templates in the patient record which may have been amended since they were last issued. You should look at the Current Medication Issues section to see what the patient has been given.’)" type="info" %}






## All Medication (Summary) ##

### Clinical narrative ###

A history view of drugs or other forms of medicines that have been used to treat or prevent disease for the patient.

### Purpose ###

The purpose of this section is to provide a distinct list of all the medications recorded for the patient. 
 
Where the medication was cancelled (Acute) or Discontinued (Repeat), this should be included in the Details column as Cancelled followed by Date of Cancellation or Discontinued, followed by Date when discontinued.

### Subsection title ###

The subsection title **SHALL** be "All Medication (Summary)".

### Date filter ###

A date filter is applicable for the Past Medications subsection:

- The date filter **SHALL** be applied to the `First Prescribed` field
- All relevant records **SHALL** be returned according to the consumer-supplied date range
- If a date is not supplied all records **SHALL** be returned


### Subsection content banner ###

Provider's message describing at a summary level how they have populated this section.


### Table columns ###

Providers **SHALL** return all the columns as described in the table below. It **MUST** be grouped by `Medication Item` and `Type` and sorted alphabetically:

<div>
<table>
<thead>
  <tr class="header">
	<th width="5%">Order</th>
	<th width="23%">Name</th>
	<th width="58%">Description</th>
	<th width="14%">Value details</th>
  </tr>
 </thead>
  <tr>
    <td align="center">1</td>
    <td><code>First Prescribed Date</code></td>
    <td>The date the medication was first prescribed. If this is not known (for example, for prescribed elsewhere) then date of entry <b>SHALL</b> be used.</td>
    <td><code>dd-Mmm-yyyy</code></td>
  </tr>
  <tr>
    <td align="center">2</td>
    <td><code>Type</code> <i class="fa fa-object-group" aria-hidden="true"></i></td>
    <td>Type of medication issued (for example, <code>Acute, Repeat, Repeat Dispense, Acute - [prescribing agency], Repeat - [prescribing agency]</code>).</td>
    <td><code>free-text</code></td>
  </tr>
  <tr>
    <td align="center">3</td>
    <td><code>Medication Item</code> <i class="fa fa-object-group" aria-hidden="true"></i> <i class="fa fa-sort-asc" aria-hidden="true"></i></td>
    <td>Descriptive name of medication item (including dosage)  for example, <code>Ibuprofen 400mg tablets</code>.</td>
    <td><code>free-text</code></td>
  </tr>  
  <tr>
    <td align="center">4</td>
    <td><code>Dosage Instruction</code></td>
    <td>Dosage instructions for the medication item. As a minimum, this <b>SHALL</b> include:
		<ul>
			<li>Dosage Rate</li>
			<li>Schedule (when / how often)</li>
		</ul>
	<p>for example, <code>two to be taken daily</code></p>
	</td>
	<td><code>free-text</code></td>
  </tr>
  <tr>
    <td align="center">5</td>
    <td><code>Quantity</code></td>
    <td>Quantity details for the medication item. As a minimum, this <b>SHALL</b> include:
		<ul>
			<li>Quantity</li>
			<li>Quantity unit</li>
		</ul>
	<p>for example, <code>14 capsule</code></p>
	</td>
	<td><code>free-text</code></td>
  </tr>   
  <tr>
    <td align="center">6</td>
    <td><code>Last Issued Date</code></td>
    <td>The last issue of the medication item (acute or repeat). If the medication is repeat dispense or prescribed elsewhere this <b>SHALL</b> be null.</td>
    <td><code>dd-Mmm-yyyy</code></td>
  </tr>  
  <tr>
    <td align="center">7</td>
    <td><code>Number of Prescriptions Issued</code></td>
    <td>The sum of the number of issues of the medication – actual issues not the max issues. If this is not known (for example, for medication prescribed elsewhere or repeat dispense, this <b>SHALL</b> be null).</td>
    <td><code>integer</code></td>
  </tr>
  <tr>
    <td align="center">8</td>
    <td><code>Additional Information</code></td>
    <td>If the medication record includes the information, the following details <b>SHALL</b> be included:
			<ul>
			<li>Reason for the medication</li>
			<li>Linked problems / diagnoses</li>
			<li>Other supporting information</li>
			<li><code>CANCELLED: </code> label with cancellation date and reason (acute) or <code>DISCONTINUED: </code> label with discontinued date and reason (repeat)</li>
		</ul>
	<p>The provider <b>MAY</b> include labels in addition to the ones specified to support additional text (for example, <code>Linked Problem : Heart Failure</code>).</p>
	</td>
    <td><code>free-text</code></td>
  </tr>  
</table>
</div>






## All Medication Issues ##

### Clinical narrative ###

A history view of drugs or other forms of medicines that have been used to treat or prevent disease for the patient.

### Purpose ###

The purpose of this section is to provide a historical view of all issues (prescribed elsewhere and repeat dispense are not included as their issues are not recorded on the GP system).

### Subsection title ###

The subsection title **SHALL** be "All Medication Issues".

### Date filter ###

A date filter is applicable for the Repeat Medication Issues subsection:

- The date filter **SHALL** be applied to the `Issued Date` field
- All relevant records **SHALL** be returned according to the consumer-supplied date range
- If a date is not supplied all records **SHALL** be returned

### Subsection content banner ###

Providers message describing at a summary level how they have populated this section.

### Table columns ###

Providers **SHALL** return all the columns as described in the table below, sorted by `Issue Date` descending:

<div>
<table>
<thead>
  <tr class="header">
	<th width="5%">Order</th>
	<th width="23%">Name</th>
	<th width="58%">Description</th>
	<th width="14%">Value details</th>
  </tr>
 </thead>
  <tr>
    <td align="center">1</td>
    <td><code>Issue Date</code> <i class="fa fa-sort-desc" aria-hidden="true"></i></td>
    <td>The date the medication item was issued.</td>
    <td><code>dd-Mmm-yyyy</code></td>
  </tr>
  <tr>
    <td align="center">2</td>
    <td><code>Type</code></td>
    <td>Type of medication issued (for example, <code>Acute, Repeat, Repeat Dispense, Acute - [prescribing agency], Repeat - [prescribing agency]</code>).</td>
    <td><code>free-text</code></td>
  </tr>
  <tr>
    <td align="center">3</td>
    <td><code>Medication Item</code></td>
    <td>Descriptive name of medication item (including dosage)  for example, <code>Ibuprofen 400mg tablets</code>.</td>
    <td><code>free-text</code></td>
  </tr>  

  <tr>
    <td align="center">4</td>
    <td><code>Dosage Instruction</code></td>
    <td>Dosage instructions for the medication item. As a minimum, this <b>SHALL</b> include:
		<ul>
			<li>Dosage Rate</li>
			<li>Schedule (when / how often)</li>
		</ul>
	<p>for example, <code>two to be taken daily</code></p>
	</td>
	<td><code>free-text</code></td>
  </tr>
  <tr>
    <td align="center">5</td>
    <td><code>Quantity</code></td>
    <td>Quantity details for the medication item. As a minimum, this <b>SHALL</b> include:
		<ul>
			<li>Quantity</li>
			<li>Quantity unit</li>
		</ul>
	<p>for example, <code>14 capsule</code></p>
	</td>
	<td><code>free-text</code></td>
  </tr>   
  <tr>
    <td align="center">6</td>
    <td><code>Scheduled End Date</code></td>
    <td>The date the prescription is expected to finish.</td>
    <td><code>dd-Mmm-yyyy</code></td>
  </tr>
  <tr>
    <td align="center">7</td>
    <td><code>Days Duration</code></td>
    <td>Duration of the medication issued in days.</td>
    <td><code>integer</code></td>
  </tr>
  <tr>
    <td align="center">8</td>
    <td><code>Additional Information</code></td>
    <td>If the medication record includes the information, the following details <b>SHALL</b> be included:
			<ul>
			<li>Reason for the medication</li>
			<li>Linked problems / diagnoses</li>
			<li>Other supporting information</li>
			<li><code>CANCELLED: </code> label with cancellation date and reason (acute) or <code>DISCONTINUED: </code> label with discontinued date and reason (repeat)</li>
		</ul>
	<p>The provider <b>MAY</b> include labels in addition to the ones specified to support additional text (for example, <code>Linked Problem : Ear Infection</code>).</p>
	</td>
    <td><code>free-text</code></td>
  </tr>  
</table>
</div>


## HTML view ##

The following content highlights the expected HTML tags and format providers **SHALL** use when generating the HTML content:

{% include accessrecord/medications.html %}

## Example view ##

<p data-height="2000" data-theme-id="light" data-slug-hash="EeQJyW" data-default-tab="result" data-user="tford70" data-embed-version="2" data-pen-title="Medications" class="codepen">See the Pen <a href="https://codepen.io/tford70/pen/EeQJyW/">Medications</a> by gp_connect (<a href="https://codepen.io/tford70">@tford70</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

{% include tip.html content="Please see [CodePen](https://codepen.io/gpconnect/pen/EeQJyW) for example of using AngularJS to generate table content." %}

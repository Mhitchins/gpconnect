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

Contains one main section, and five subsections:

 - [Recent Acute Medication](accessrecord_view_medications.html#recent-acute-medication)
 - [Current Repeat Medication](accessrecord_view_medications.html#current-repeat-medication)
 - [Discontinued Repeat Medication](accessrecord_view_medications.html#discontinued-repeat-medication)
 - [All Medication (Summary)](accessrecord_view_medications.html#all-medication-summary)
 - [All Medication Issues](accessrecord_view_medications.html#all-medication-issues)

 
## Section title ##

The section title **MUST** be "Medications".

## Date filter ##

A date filter is applicable to this section. The date filter **MUST** be applied to the All Medication (Summary) and All Medication Issues subsections only. All other subsections **MUST** return all records as defined in the subsection descriptions below, subject to exclusions.
 
## Section content banner ##

Provider message describing at a summary level how they have populated this section.




## Recent Acute Medication ##

### Clinical narrative ###

A list of acute medicines that are currently being, or have recently been, used to treat or prevent disease for the patient. The provider **MUST** include all acute medication whose `Start Date` (the date the prescription is expected to start) is greater than the current date minus 365 days.

This is aligned to the Acute Medications section in Summary Care Records (SCR). 

### Purpose ###

The purpose of this section is to provide a view of acute medications that the patient has recently been taking which informs the clinical decision-making process.

### Subsection title ###

The subsection title **MUST** be "Recent Acute Medication".

### Date filter ###

All relevant records **MUST** be returned.

### Subsection content banner ###

Provider message describing at a summary level how they have populated this subsection.

Providers **MUST** return the following message, if applicable:

```html
<div>
	<p>Scheduled End Date is not always captured in the source, where it was not recorded the displayed date is calculated from start date and days duration</p>
</div>
```

### Table columns ###

Providers **MUST** return all the columns as described in the table below, sorted by `Start Date` descending:

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
    <td><code>Type</code></td>
    <td>Type of medication issued (for example, <code>Acute, Acute Post-Dated, Acute - [Prescribing Agency Type]<sup><b>1</b></sup></code>).</td>
    <td><code>free-text</code></td>
  </tr> 
  <tr>
    <td align="center">2</td>
    <td><code>Start Date</code> <i class="fa fa-sort-desc" aria-hidden="true"></i></td>
    <td>The date of issue of the acute medications, except where:
		<ul>
			<li>the medication is post-dated it <b>MUST</b> be the post date</li>
			<li>the medication is prescribed elsewhere it <b>MUST</b> be the expected start date if known, else the entered date </li>
		</ul>
	</td>
    <td><code>dd-Mmm-yyyy</code></td>
  </tr>
  <tr>
    <td align="center">3</td>
    <td><code>Medication Item</code></td>
    <td>Descriptive name of medication item (including dosage) (for example, <code>Ibuprofen 400mg tablets</code>).</td>
    <td><code>free-text</code></td>
  </tr>
  <tr>
    <td align="center">4</td>
    <td><code>Dosage Instruction</code></td>
    <td>Dosage instructions for the medication item. As a minimum, this <b>MUST</b> include:
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
    <td>Quantity details for the medication item. As a minimum, this <b>MUST</b> include:
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
    <td>If the medication record includes the information, the following details <b>MUST</b> be included:
		<ul>
			<li><code>CANCELLED: </code> label with cancellation date and reason</li>
			<li>Reason for the medication</li>
			<li>Linked problems / diagnoses</li>
			<li>Other supporting information</li>
		</ul>
	<p>The provider <b>MAY</b> include labels in addition to the ones specified to support additional text (for example, <code>Linked Problem : Ear Infection</code>).</p>
	</td>
    <td><code>free-text</code></td>
  </tr>  
</table>
</div>

<sup><b>1</b></sup> Where the medication was Prescribed Elsewhere the prescribing agency (type of organisation responsible for authorising and issuing the medication) **MUST** be included with the Type, for example, ‘Acute – Dentist’. If the medication item is identifiable as prescribed elsewhere but the type of organisation who prescribed it is not recorded, then the Type **MUST** be returned as ‘Acute – Unknown Prescriber’.



## Current Repeat Medication##

### Clinical narrative ###

A list of repeat drugs or other forms of medicines that are currently being used to treat or prevent disease for the patient. This may also include PRN occasional use medication - for example, EpiPen, antihistamines, monitoring or continence products.

The provider **MUST** include all repeat and repeat dispensed medications (templates/plans/courses **NOT** individual issues) which have not been discontinued or otherwise ended. Repeat medications (including repeat dispense) which have not been discontinued are considered current where the Effective End Date (the date the cycle of prescriptions is expected to end) is either greater than the current date or is null. This **MUST** include those which has been authorised but not yet issued (Last Issued and Number Issued will be null).

### Purpose ###

The purpose of this section is to provide a view of all repeat medications that the patient is currently prescribed, which informs the clinical decision-making process.

### Subsection title ###

The subsection title **MUST** be "Current Repeat Medication".

### Date filter ###

All relevant records **MUST** be returned (that is, no time limit/filtering is to be applied).

### Subsection content banner ###

Provider message describing at a summary level how they have populated this subsection.

Providers **MUST** return the following message, if applicable:

```html
<div>
	<p>The Review Date is that set for each Repeat Course. Reviews may be conducted according to a diary event which differs from the dates shown</p>
</div>
```

### Table columns ###

Providers **MUST** return all the columns as described in the table below, sorted by `Start Date` descending:

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
    <td><code>Type</code></td>
    <td>Type of medication issued (for example, <code>Repeat, Repeat Dispense, Repeat – [Prescribing Agency Type]</code>).</td>
    <td><code>free-text</code></td>
  </tr>
  <tr>
    <td align="center">2</td>
    <td><code>Start Date</code> <i class="fa fa-sort-desc" aria-hidden="true"></i></td>
    <td>The original date of authorisation of the repeat medication (if a provider system allows re-authorisation of a repeat medication the start date <b>MUST</b> be the first authorisation). If this is not known (for example, for prescribed elsewhere) then date of entry of the repeat medication record <b>MUST</b> be returned.</td>
    <td><code>dd-Mmm-yyyy</code></td>
  </tr>
  <tr>
    <td align="center">3</td>
    <td><code>Medication Item</code></td>
    <td>Descriptive name of medication item (including dosage) (for example, <code>Ibuprofen 400mg tablets</code>).</td>
    <td><code>free-text</code></td>
  </tr>  
  <tr>
    <td align="center">4</td>
    <td><code>Dosage Instruction</code></td>
    <td>Dosage instructions for the medication item. As a minimum, this <b>MUST</b> include:
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
    <td>Quantity details for the medication item. As a minimum, this <b>MUST</b> include:
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
    <td>The last issue date. If the medication is repeat dispense or prescribed elsewhere this <b>MUST</b> be null.</td>
    <td><code>dd-Mmm-yyyy</code></td>
  </tr>  
  <tr>
    <td align="center">7</td>
    <td><code>Number of Prescriptions Issued</code></td>
    <td>This <b>MUST</b> be the number issued up to the current date inclusive. For a repeat dispense this will be null. This <b>MUST</b> be null where no issues of the repeat have been made at the current date. If the medication is repeat dispensed or prescribed elsewhere this <b>MUST</b> be null.</td>
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
    <td>If the medication record includes the information, the following details <b>MUST</b> be included:
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

{% include custominfocallout.html content="**Note:** If the provider system allows the repeat medication (master/template) details to be altered pending or as new repeat issues are generated, then the latest details **MUST** be returned by the provider in the Additional Information column. A subsection banner message should be added if applicable (for example ‘The medication above is taken from a list of Repeat Medication Templates in the patient record which may have been amended since they were last issued. You should look at the All Medication Issues section to see what the patient has been given)." type="info" %}



## Discontinued Repeat Medication##

### Clinical narrative ###

A list of discontinued repeat drugs or other forms of medicines. This may also include PRN occasional use medication - for example, EpiPen, antihistamines, monitoring or continence products.

### Purpose ###

The purpose of this section is to provide a view of all discontinued repeat medications that the patient has been prescribed, which informs the clinical decision-making process.

### Subsection title ###

The subsection title **MUST** be "Discontinued Repeat Medication".

### Date filter ###

All relevant records **MUST** be returned (that is, no time limit/filtering is to be applied).

### Subsection content banner ###

Provider message describing at a summary level how they have populated this subsection.

Providers **MUST** return the following message:

```html
<div>
	<p>All repeat medication ended by a clinician action</p>
</div>
```

### Table columns ###

Providers **MUST** return all the columns as described in the table below, sorted by `Last Issued Date` descending:

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
    <td><code>Type</code></td>
    <td>Type of medication issued (for example, <code>Repeat, Repeat Dispense, Repeat – [Prescribing Agency]</code>).</td>
    <td><code>free-text</code></td>
  </tr>
  <tr>
    <td align="center">2</td>
    <td><code>Last Issued Date</code> <i class="fa fa-sort-desc" aria-hidden="true"></i></td>
    <td>The last issue date. If the medication is repeat dispense this <b>MUST</b> be null.</td>
    <td><code>dd-Mmm-yyyy</code></td>
  </tr>
  <tr>
    <td align="center">3</td>
    <td><code>Medication Item</code></td>
    <td>Descriptive name of medication item (including dosage) (for example, <code>Ibuprofen 400mg tablets</code>).</td>
    <td><code>free-text</code></td>
  </tr>  
  <tr>
    <td align="center">4</td>
    <td><code>Dosage Instruction</code></td>
    <td>Dosage instructions for the medication item. As a minimum, this <b>MUST</b> include:
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
    <td>Quantity details for the medication item. As a minimum, this <b>MUST</b> include:
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
    <td><code>Discontinued Date</code></td>
    <td>The date the medication item was discontinued.</td>
    <td><code>dd-Mmm-yyyy</code></td>
  </tr>  
  <tr>
    <td align="center">7</td>
    <td><code>Discontinued Reason</code></td>
    <td></td>
    <td><code>free-text</code></td>
  </tr>
  <tr>
    <td align="center">8</td>
    <td><code>Additional Information</code></td>
    <td>If the medication record includes the information, the following details <b>MUST</b> be included:
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




## All Medication (Summary) ##

### Clinical narrative ###

A history view of drugs or other forms of medicines that have been used to treat or prevent disease for the patient.

### Purpose ###

The purpose of this section is to provide a distinct list of all the medications recorded for the patient. 
 
Where the medication was cancelled (Acute) or Discontinued (Repeat), this should be included in the Details column as Cancelled followed by Date of Cancellation or Discontinued, followed by Date when discontinued.

### Subsection title ###

The subsection title **MUST** be "All Medication (Summary)".

### Date filter ###

If a consumer submits a date filter for this section the dates will be applied as follows:

1. The `Start Date` **MUST** be the date of the earliest prescription within the period of the date filter (including prescriptions issued on the date to the filter start date, or all prescription if no start date is specified)
2. The `Last Issued Date` **MUST** be the date of the last prescription within the period of the date filter (including prescriptions issued on the date to the filter end date, or all prescription if no end date is specified)
3. The definitions in points 1 and 2 **MUST** be applied to the date recorded for medications prescribed elsewhere and date authorised for repeat dispense
4. The `Number of Prescriptions Issued` **MUST** be the count of prescription issued between the `Start Date` and the `Last Issued Date` inclusive (as dates are defined in points 1 and 2) or null for medication prescribed elsewhere and repeat dispense
5. Additional Information **MUST** include discontinued or cancelled details (where identifiable) for any medications which fall within the date filter definitions in points 1 to 3 regardless of whether the date discontinued/cancelled falls within the date filter range
6. Additional Information **MUST NOT** include ended date and reason if the date falls outside of the date filter range


### Subsection content banner ###

Provider message describing at a summary level how they have populated this section.


### Table columns ###

Providers **MUST** return all the columns as described in the table below. It **MUST** be grouped by `Medication Item`, with `Medication Item` repeated as a group title, then sorted alphabetically. `Medication Items` within a group **MUST** be sorted by `Start Date` descending:

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
    <td><code>Type</code></td>
    <td>Type of medication issued (for example, <code>Acute, Repeat, Repeat Dispense, Acute - [prescribing agency], Repeat - [prescribing agency]</code>).</td>
    <td><code>free-text</code></td>
  </tr>
  <tr>
    <td align="center">2</td>
    <td><code>Start Date</code> <i class="fa fa-sort-desc" aria-hidden="true"></i></td>
    <td>The date the medication was first prescribed. If this is not known (for example, for prescribed elsewhere) then date of entry <b>MUST</b> be used.</td>
    <td><code>dd-Mmm-yyyy</code></td>
  </tr>
  <tr>
    <td align="center">3</td>
    <td><code>Medication Item</code> <i class="fa fa-object-group" aria-hidden="true"></i> <i class="fa fa-sort-asc" aria-hidden="true"></i></td>
    <td>Descriptive name of medication item (including dosage) (for example, <code>Ibuprofen 400mg tablets</code>).</td>
    <td><code>free-text</code></td>
  </tr>  
  <tr>
    <td align="center">4</td>
    <td><code>Dosage Instruction</code></td>
    <td>Dosage instructions for the medication item. As a minimum, this <b>MUST</b> include:
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
    <td>Quantity details for the medication item. As a minimum, this <b>MUST</b> include:
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
    <td>The last issue of the medication item (acute or repeat). If the medication is repeat dispense or prescribed elsewhere this <b>MUST</b> be null.</td>
    <td><code>dd-Mmm-yyyy</code></td>
  </tr>  
  <tr>
    <td align="center">7</td>
    <td><code>Number of Prescriptions Issued</code></td>
    <td>The sum of the number of issues of the medication – actual issues not the max issues. If this is not known (for example, for medication prescribed elsewhere or repeat dispense, this <b>MUST</b> be null).</td>
    <td><code>integer</code></td>
  </tr>
  <tr>
    <td align="center">8</td>
    <td><code>Additional Information</code></td>
    <td>If the medication record includes the information, the following details <b>MUST</b> be included:
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

The subsection title **MUST** be "All Medication Issues".

### Date filter ###

If a consumer submits a date filter for this section the dates will be applied as follows:

1. The `Start Date` **MUST** be the date of the earliest prescription within the period of the date filter (including prescriptions issued on the date to the filter start date, or all prescription if no start date is specified)
2. The `Last Issued Date` **MUST** be the date of the last prescription within the period of the date filter (including prescriptions issued on the date to the filter end date, or all prescription if no end date is specified)
3. The definitions in points 1 and 2 **MUST** be applied to the date recorded for medications prescribed elsewhere and date authorised for repeat dispense
4. The `Number of Prescriptions Issued` **MUST** be the count of prescription issued between the `Start Date` and the `Last Issued Date` inclusive (as dates are defined in points 1 and 2) or null for medication prescribed elsewhere and repeat dispense
5. Additional Information **MUST** include discontinued or cancelled details (where identifiable) for any medications which fall within the date filter definitions in points 1 to 3 regardless of whether the date discontinued/cancelled falls within the date filter range
6. Additional Information **MUST NOT** include ended date and reason if the date falls outside of the date filter range

### Subsection content banner ###

Providers message describing at a summary level how they have populated this section.

### Table columns ###

Providers **MUST** return all the columns as described in the table below. It **MUST** be grouped by `Medication Item`, with `Medication Item` repeated as a group title, then sorted alphabetically. `Medication Items` within a group **MUST** be sorted by `Issue Date` descending:

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
    <td><code>Type</code></td>
    <td>Type of medication issued (for example, <code>Acute, Repeat, Repeat Dispense, Acute - [prescribing agency], Repeat - [prescribing agency]</code>).</td>
    <td><code>free-text</code></td>
  </tr>
  <tr>
    <td align="center">2</td>
    <td><code>Issue Date</code> <i class="fa fa-sort-desc" aria-hidden="true"> <sub>2</sub></i></td>
    <td>The date the medication item was issued.</td>
    <td><code>dd-Mmm-yyyy</code></td>
  </tr>
  <tr>
    <td align="center">3</td>
    <td><code>Medication Item</code> <i class="fa fa-object-group" aria-hidden="true"></i> <i class="fa fa-sort-asc" aria-hidden="true"></i> <sup>1</sup></td>
    <td>Descriptive name of medication item (including dosage) (for example, <code>Ibuprofen 400mg tablets</code>).</td>
    <td><code>free-text</code></td>
  </tr>  

  <tr>
    <td align="center">4</td>
    <td><code>Dosage Instruction</code></td>
    <td>Dosage instructions for the medication item. As a minimum, this <b>MUST</b> include:
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
    <td>Quantity details for the medication item. As a minimum, this <b>MUST</b> include:
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
    <td>If the medication record includes the information, the following details <b>MUST</b> be included:
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


## Additional medication information ##

### Ended medications ###

It is extremely important for the details regarding the ending of a medication to be available to a clinician, as this will highlight any clinical issues (for example, stopping a medication due to an allergy) and allow the clinician to make an efficient and informed clinical decision.

Any information relevant to the circumstances of ending a medication **MUST** be provided in the `Additional Information` column. The following definitions are provided to support the labelling of the information within Additional Information:

- **Naturally ended**: Medication naturally came to an end, that is, no manual intervention via the user (auto system transition)
- **Cancelled**: Actively stopped acute medication by user
- **Discontinued**: Actively stopped repeat medication by user

For Cancelled and Discontinued the reason **MUST** be included as human readable text where coded or free text information if captured as part of the cancellation or discontinuation action. The date **MUST** also be included. The date **MUST** be the date recorded as the date cancelled or discontinued if a user entered a date, or the transaction date if not.

For Naturally Ended medications the end date and reason **MUST NOT** be included in additional information.

If a system provider cannot differentiate between naturally ended, discontinued and cancelled, or cannot provide clear details for cancelled and discontinued in the form of an accurate date, or clearly display reason text which will differentiate a clinical discontinuation (for example, stopping a medication due to an allergy), from any other reason for ending the medication (for example, change to an alternative, equivalent medication without clinical issues having occurred) a section/subsection banner message(s) as appropriate **MUST** be included to describe the extent or limitation of compliance.

### Prescribed elsewhere ###

All subsections (except All Medication Issues) **MUST** include items which are recorded on the system as a prescription but prescribed elsewhere (for example, hospitals or special clinics) or ‘Over The Counter’ drugs taken by the patient and recorded on the system as a prescription.

### GP2GP transfer ###

Repeat medications transferred as part of GP2GP **MUST** only be included in the Current Repeat Medications subsection if authorised by a clinician at the new practice, otherwise they are treated as not current, therefore they appear in All Medication (Summary) and All Medication Issues, but not in Current Repeat Medication.
 


## HTML view ##

The following content highlights the expected HTML tags and format providers **MUST** use when generating the HTML content:

{% include accessrecord/medications.html %}

## Example view ##

<p data-height="2000" data-theme-id="light" data-slug-hash="bxOYYZ" data-default-tab="result" data-user="tford70" data-embed-version="2" data-pen-title="Medications" class="codepen">See the Pen <a href="https://codepen.io/tford70/pen/bxOYYZ/">Medications</a> by gp_connect (<a href="https://codepen.io/tford70">@tford70</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

{% include tip.html content="Please see [CodePen](https://codepen.io/gpconnect/pen/bxOYYZ) for example of using AngularJS to generate table content." %}

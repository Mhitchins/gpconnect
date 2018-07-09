---
title: Allergies and Adverse Reactions
keywords: getcarerecord, view, section, allergies
tags: [view,getcarerecord]
sidebar: accessrecord_sidebar
permalink: accessrecord_view_allergies.html
summary: "Allergies HTML View."
---

## Allergies and Adverse Reactions ##

| Section Code | Section Name | TPP | EMIS | INPS | Microtest |
| ------------ | ------------ |-----|------|------|-----------|
| ALL | Allergies and Adverse Reactions| Yes | Yes<sup>1</sup> | Yes<sup>2</sup> | Yes |

<sup>1</sup> EMIS have indicated that they are including all allergies & sensitivities in the 'Current Allergies .....' section.

<sup>2</sup> INPS have indicated they don't record end dates for allergies & sensitivities. Hence, no History view is possible.


### Clinical Narrative ###

A reaction to a substance not intended to occur. Allergies are caused by hypersensitivity of the immune system to an external agent

### Purpose ###

The purpose of this section is to provide the clinician with a list of patient allergies to enable safe prescribing and treatment recommendations for a patient.

### Sections and Subsections ###

Contains two sections:

 - [Current Allergies and Adverse Reactions](accessrecord_view_allergies.html#current-allergies-and-adverse-reactions)
 - [Historical Allergies and Adverse Reactions](accessrecord_view_allergies.html#historical-allergies-and-adverse-reactions)

### Date Filter ###

Date filters are not supported for this section all relevant records shall be returned for the Current and Historic Allergies and Adverse Reactions.
 
 
### Section Banner Content Message ###

Providers message describing at a summary level how they have populated this section:

<div class="panel panel-default">
  <div class="panel-heading">
    <p class="panel-title"><span class="icon">+</span> EMIS banner content (click here to expand/collapse) </p>
  </div>
  <div class="panel-body">
		<p><b>Always displays this text:</b></p>
			<ul>
				<li>All relevant items.</li>
				<li>Historical Allergies and Adverse Reactions data is not supported by this system.</li>
			</ul>
  </div>
  <div class="panel-heading">
    <p class="panel-title"><span class="icon">+</span> TPP banner content (click here to expand/collapse)</p>
  </div>
  <div class="panel-body">
	No section banner text displayed.
  </div>
  <div class="panel-heading">
    <p class="panel-title"><span class="icon">+</span> INPS banner content (click here to expand/collapse) </p>
  </div>
  <div class="panel-body">
    No section banner text displayed.
  </div>
  <div class="panel-heading">
    <p class="panel-title"><span class="icon">+</span> MicroTest banner content (click here to expand/collapse) </p>
  </div>
  <div class="panel-body">
    No section banner text displayed.
  </div>
</div>

### Section Business Rules ###

| Supplier | Business Rules |
|----------|----------------|
| EMIS | All allergies and adverse reactions will be recorded in the Current Allergies and Adverse Reactions subsection.  Historical allergies and adverse reactions data is not supported by EMIS. |
| TPP | TBC |
| INPS | All allergy and adverse reactions data will be recorded in the Current Allergies and Adverse Reactions subsection Vision do not record end dates for allergies & adverse reactions, therefore a history view is not possible. |
| MicroTest | N/A |


## Current Allergies and Adverse Reactions ##

### Clinical Narrative ###

A reaction to a substance not intended to occur. Allergies are caused by hypersensitivity of the immune system to an external agent

### Purpose ###

The purpose of this section is to provide the clinician with a list of current allergies to enable safe prescribing and treatment recommendations for a patient.

### Subsection Banner Content Message ###

Providers message describing at a summary level how they have populated this section:

<div class="panel panel-default">
  <div class="panel-heading">
    <p class="panel-title"><span class="icon">+</span> EMIS banner content (click here to expand/collapse) </p>
  </div>
  <div class="panel-body">
    No section banner text displayed
  </div>
  <div class="panel-heading">
    <p class="panel-title"><span class="icon">+</span> TPP banner content (click here to expand/collapse)</p>
  </div>
  <div class="panel-body">
		<p><b>Displayed dependent on date range:</b></p>
			<ul>
				<li>All relevant items.</li>
			</ul>
		<p><b>If GP2GP in progress:</b></p>
			<ul>
				<li>Record is in transit and may be incomplete.</li>
			</ul> 
  </div>
  <div class="panel-heading">
    <p class="panel-title"><span class="icon">+</span> INPS banner content (click here to expand/collapse) </p>
  </div>
  <div class="panel-body">
		<p><b>Always displays this text:</b></p>
			<ul>
				<li>All relevant items subject to patient preferences and/or RCGP exclusions.</li>
				<li>All Allergies Listed in Current Section.</li>
			</ul>
  </div>
  <div class="panel-heading">
    <p class="panel-title"><span class="icon">+</span> MicroTest banner content (click here to expand/collapse) </p>
  </div>
  <div class="panel-body">
    No section banner text displayed.
  </div>
</div>

### Table Construction Requirements ###

Providers must adhere to the table construction requirements listed below:

- Table header **SHALL** be "Current Allergies and Adverse Reactions".
- Table columns **SHALL** be ordered left-to-right (1..N).
- Table content **SHALL NOT** be truncated.
- Table rows **SHALL** be ordered by date descending (i.e. most recent date/time first).


### Table Columns ###

Providers must return all the columns as described in the table below:

| Order | Name | Description | Value Details &nbsp;&nbsp;&nbsp; |
| ------------ | ------------ | ------------ |
| <center>1</center> | `Start Date` | The start date of the allergy or adverse reaction | `dd-Mmm-yyyy` |
| <center>2</center> | `Details` | Longer human readable details for the allergy or adverse reaction | `free-text` |


### HTML View ###

{% raw %}
```html
<div ng-controller="ctrl">
	<h3>Current Allergies and Adverse Reactions</h3>
	<table class="table">
		<thead>
			<tr>
				<th class="col-sm-2">Start Date</th>
				<th class="col-sm-2">Details</th>
			</tr>
		</thead>
			<tr ng-repeat="x in records" class="table">
				<td class="col-sm-2">{{x.start}}</td>
				<td class="col-sm-2">{{x.details}}</td>

			</tr>
	</table>
</div>
```
{% endraw %}

## Historical Allergies and Adverse Reactions ##

### Clinical Narrative ###

A reaction to a substance not intended to occur. Allergies are caused by hypersensitivity of the immune system to an external agent.

### Purpose ###

The purpose of this section is to provide the clinician with a list of historical allergies to enable safe prescribing and treatment recommendations for a patient.


### Subsection Banner Content Message ###

Providers message describing at a summary level how they have populated this section:

<div class="panel panel-default">
  <div class="panel-heading">
    <p class="panel-title"><span class="icon">+</span> EMIS banner content (click here to expand/collapse) </p>
  </div>
  <div class="panel-body">
		<p><b>Always displays this text:</b></p>
			<ul>
				<li>Historical Allergies and Adverse Reactions data is not supported by this system.</li>
			</ul>
  </div>
  <div class="panel-heading">
    <p class="panel-title"><span class="icon">+</span> TPP banner content (click here to expand/collapse)</p>
  </div>
  <div class="panel-body">
		<p><b>Displayed dependent on date range:</b></p>
			<ul>
				<li>All relevant items.</li>
			</ul>
		<p><b>If GP2GP in progress:</b></p>
			<ul>
				<li>Record is in transit and may be incomplete.</li>
			</ul> 
  </div>
  <div class="panel-heading">
    <p class="panel-title"><span class="icon">+</span> INPS banner content (click here to expand/collapse) </p>
  </div>
  <div class="panel-body">
    No section banner text displayed.
  </div>
  <div class="panel-heading">
    <p class="panel-title"><span class="icon">+</span> MicroTest banner content (click here to expand/collapse) </p>
  </div>
  <div class="panel-body">
    No section banner text displayed.
  </div>
</div>


### Table Construction Requirements ###

Providers must adhere to the table construction requirements listed below:

- Table header **SHALL** be "Historical Allergies and Adverse Reactions".
- Table columns **SHALL** be ordered left-to-right (1..N).
- Table content **SHALL NOT** be truncated.
- Table rows **SHALL** be ordered by date descending (i.e. most recent date/time first).

### Table Columns ###

Providers must return all the columns as described in the table below:

| Order | Name | Description | Value Details &nbsp;&nbsp;&nbsp; |
| ------------ | ------------ | ------------ |
| <center>1</center> | `Start Date` | The start date of the allergy or adverse reaction | `dd-Mmm-yyyy` |
| <center>2</center> | `End Date` | The end date of the allergy or adverse reaction | `dd-Mmm-yyyy` |
| <center>3</center> | `Details` | Longer human readable details for the allergy or adverse reaction | `free-text` |


### HTML View ###

{% raw %}
```html
<div ng-controller="ctrl">
	<h3>Historical Allergies and Adverse Reactions</h3>
	<table class="table">
		<thead>
			<tr>
				<th class="col-sm-2">Start Date</th>
				<th class="col-sm-2">End Date</th>
				<th class="col-sm-2">Details</th>
			</tr>
		</thead>
			<tr ng-repeat="x in records1" class="table">
				<td class="col-sm-2">{{x.start}}</td>
				<td class="col-sm-2">{{x.end}}</td>
				<td class="col-sm-2">{{x.details}}</td>
			</tr>
	</table>
</div>
```
{% endraw %}

{% include important.html content="AngularJS tags (e.g ng-repeat) are present merely to indicate to a developer the structure of the table content. Presence of these tags are not intended to imply use of any specific technology." %} 

## Example View ##

<p data-height="500" data-theme-id="light" data-slug-hash="NXqqZW" data-default-tab="result" data-user="tford70" data-embed-version="2" data-pen-title="Allergies" class="codepen">See the Pen <a href="https://codepen.io/tford70/pen/NXqqZW/">Allergies</a> by gp_connect (<a href="https://codepen.io/tford70">@tford70</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

{% include tip.html content="Please see [CodePen](https://codepen.io/gpconnect/pen/NXqqZW) for example of using AngularJS to generate table content" %}

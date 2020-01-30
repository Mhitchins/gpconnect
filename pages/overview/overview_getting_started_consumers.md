---
title: Getting started for consumer suppliers
keywords: engage, about
tags: [getting_started]
sidebar: overview_sidebar
toc: false
permalink: overview_getting_started_consumers.html
summary: "Overview of development process and first steps"
---

You may be working with an end user organisation to solve their business needs or developing an application to secure a place in the healthcare market. Whatever your reason for developing, this specification shows you how to connect to the GP Connect APIs and get your product ready for use.

As an approximate guide you can expect to complete the following tasks on your GP Connect development journey:

<p><strong>Get started:</strong></p>
- Read about the GP Connect capabilities
- Study the prerequisites
<p><strong>Explore:</strong></p>
- Try out the GP Connect demonstrator
- Choose capabilities
<p><strong>Engagement:</strong></p>
- Contact GP Connect team
- Describe use case (purpose of your product)
- Obtain use case approval
- Contact GP Connect Consumer Assurance team
- Receive Supplier Conformance Assessment List (SCAL)
<p><strong>Develop:</strong></p>
- Develop your product
- Communication – ongoing communication with GP Connect
<p><strong>Assurance:</strong></p>
- Test against internet-facing demonstrator
- Test against OpenTest demonstrator
- Test against environment demonstrator in Integration (INT)
- Fill in and submit SCAL, incuding evidence of clinical safety
- Test against providers in INT
- Consumer receives technical conformance certificate
- Receive connection agreement
<p><strong>End-user engagement: (if relevant)</strong></p>
- Start of end-user group engagement with GP Connect (end-user group consists of, for example, practices that are sharing with a hospital)
- Data sharing agreement
- End-user group reviews SCAL
- End-user declaration signed
<p><strong>Deploy:</strong></p>
- Approval to Go Live
- Commissioner and Supplier Deployment Approach
- Go Live

<p></p>

## Introductory topics ##

- read about the GP Connect [capabilities](overview_priority_capabilities.html)
- look through the design decisions made so far in relation to each capability ([Foundations](foundations_design.html), [Access Record Structured](accessrecord_structured_design.html) and [Appointment Management](appointments_design.html)) and get involved:
	- <span class="label label-success">SELECTED</span> / <span class="label label-info">DECISION</span> a decision has been made for first release.
	- <span class="label label-warning">ASSUMPTION</span> an assumption has been made which is under review/needs validated
- read the GP Connect [FHIR&reg; API guidance](development_fhir_api_guidance.html) common to all APIs

## Developer ecosystem ##

Our developer ecosystem diagram gives you an overview of a consumer supplier journey for a typical GP Connect API project (including 'Getting started'). Click on subsequent stages to find out more.

{% include developer_journey.html %}

## Integration with Spine ##

<p>Consumer suppliers can develop applications that use GP Connect {% include tooltip.html type="FHIR&reg;" %} {% include tooltip.html type="API" %} specifications to consume GP data. To retrieve data from an end-user organisation, your application will need to integrate with the {% include tooltip.html type="Spine" %} as follows:</p>

<p>1. Make a request to the {% include tooltip.html type="PDS" %} to retrieve patient's registered practice.</p>
<p>2. Make a call to {% include tooltip.html type="SDS" %} to retrieve provider endpoint information.</p>
<p>3. Using the endpoint information retrieved in step 2, make a request via {% include tooltip.html type="SSP" %} to the provider.</p>
<p>4. {% include tooltip.html type="SSP" %} passes request to provider system.</p>
<p>5. Provider returns data to {% include tooltip.html type="SSP" %}.</p>
<p>6. {% include tooltip.html type="SSP" %} passes data to consumer.</p>

![Img](images/overview/gp_connect_apis.png)

For more details on consumer request interactions, see the [SSP implementation guide](https://developer.nhs.uk/apis/spine-core-1-0/ssp_implementation_guide.html).

## Next step ##
[Explore](/overview_explore.html)

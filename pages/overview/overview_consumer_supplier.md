---
title: Getting started for consumer suppliers
keywords: consumer
tags: [consumer_supplier]
sidebar: overview_sidebar
permalink: overview_consumer_supplier.html
toc: false
pathways: [consumer]
summary: Step-by-step guide to developing consumer applications
---

<p>Consumer suppliers can develop applications that use GP Connect {% include tooltip.html type="FHIR&reg;" %} {% include tooltip.html type="API" %} specifications to consume GP data. To retrieve data from an end-user organisation, your application will need to integrate with the {% include tooltip.html type="Spine" %} as follows:</p>

![Img](images/overview/gp_connect_apis.png)

**Step by step:**
<p>1. Make a request to the {% include tooltip.html type="PDS" %} to retrieve patient's registered practice.</p>
<p>2. Make a call to {% include tooltip.html type="SDS" %} to retrieve provider endpoint information.</p>
<p>3. Using the endpoint information retrieved in step 2, make a request via {% include tooltip.html type="SSP" %} to the provider.</p>
<p>4. {% include tooltip.html type="SSP" %} passes request to provider system.</p>
<p>5. Provider returns data to {% include tooltip.html type="SSP" %}.</p>
<p>6. {% include tooltip.html type="SSP" %} passes data to consumer.</p>

For more details on consumer request interactions, see the [SSP implementation guide](https://developer.nhs.uk/apis/spine-core-1-0/ssp_implementation_guide.html).

## Things to consider

- read our GP Connect [FHIR&reg; API guidance](development_fhir_api_guidance.html) common to all APIs
- read about the GP Connect [priority capabilities](overview_priority_capabilities.html)
- look through the design decisions made so far in relation to each capability pack ([Foundations](foundations_design.html), [Access Record HTML](accessrecord.html), [Access Record Structured](accessrecord_structured_design.html) and [Appointment Management](appointments_design.html)) and get involved:
	- <span class="label label-success">SELECTED</span> / <span class="label label-info">DECISION</span> A decision has been made for first release.
	- <span class="label label-warning">ASSUMPTION</span> An assumption has been made which is under review/needs validated.

## Next step ##
[Explore](/overview_explore.html)

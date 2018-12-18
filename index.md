---
title: Introduction - what is GP Connect?
keywords: homepage
tags: [introduction]
sidebar: home_sidebar
permalink: index.html
toc: false
summary: An introduction to the GP Connect FHIR® APIs
---

{% comment %}
[![Semver](http://img.shields.io/badge/semver-2.0.0-yellow.svg)](http://semver.org/spec/v2.0.0.html){:target="_blank" class="no_icon"} [![License](http://img.shields.io/:license-apache2-blue.svg)](http://www.apache.org/licenses/LICENSE-2.0.html){:target="_blank" class="no_icon"} 
{% endcomment %}

GP Connect is a service that will allow GP practices and authorised clinical staff to share and view GP practice clinical information and data between IT systems, quickly and efficiently. This will make sure patient medical information is available to clinicians when and where they need it, improving patient care.

The GP Connect programme is working to develop products which will enable different systems to communicate so that clinicians in different care settings can:

* view a patient’s GP practice record
*	manage GP appointments
*	import or download data on a patient’s medicines and allergies

This will save time for clinicians, and provide better, more convenient care for patients. It will also help meet targets under [NHS England’s improving access to general practice programme](https://www.england.nhs.uk/gp/gpfv/redesign/improving-access/).

## GP Connect overview ##

![Img](images/overview/GP Connect Model.png)

## GP Connect APIs ##
GP Connect has worked with GP clinical system suppliers to develop [Application Programming Interfaces (APIs)](overview_glossary.html#API®). These APIs make data from clinical systems available in a standard form that can be used across different systems and be made available to clinicians who need access to the data for direct patient care. 

The APIs currently available are:
*	Access Record HTML – enables clinicians to view a patient’s medical record
*	Appointment Management – enables clinical staff to book, amend or cancel an appointment for a patient
*	Access Record Structured – enables an export of a patient’s medications and allergies 

[Find out more about the APIs](https://gpconnect-specrestructure.netlify.com/overview_priority_capabilities.html)

## Pilot using the GP Connect APIs ##
The GP Connect programme is now supporting the development of products that connect with the APIs to use data from patient records and appointment schedulers for patient care by integrating it or importing it into local clinical systems.

We’re working with selected partnerships of health and care organisations and software suppliers. Partnerships will use the APIs to develop technical solutions to local issues with sharing patient records and appointment booking across system boundaries.
We’ve developed a process called ‘First of Type’ to support this development and provide assurance for these early products.

[Getting involved with GP Connect](https://digital.nhs.uk/services/gp-connect)

## Development pathways ##
GP Connect has identified three main customer groups:
 
* consumer supplier
*	clinical system supplier (also known as a 'provider')
*	end-user organisation

This GitHub repository contains all the technical resources that a potential supplier needs to connect between clinical systems and GP data using our [FHIR®](overview_glossary.html#fhir®) APIs. To continue on your development journey, follow the relevant pathway below.

If you are an end-user organisation and you want to find out more about working with GP Connect, email us at [gpconnect@nhs.net](mailto: gpconnect@nhs.net).

<div class="row">
         <div class="col-lg-12">
                                                           </div>
                        <div class="col-6 col-md-4">
             <div class="panel panel-default text-center">
                 <div class="panel-heading">
                     <span class="fa-stack fa-5x">
                           <i class="fa fa-circle fa-stack-2x text-primary" style="color:#005EB8"></i>
                           <i class="fa fa-desktop fa-stack-1x fa-inverse"></i>
                     </span>
                 </div>
                 <div class="panel-body" align="left">
                      <h4>Consumer supplier</h4>
                         <p align="left">You're a software development company already working with the NHS or would like to work with the NHS. You have checked that you comply with IG requirements.</p>
                         <p align="left">You want to use GP Connect to develop an API to consume GP data.  You intend to work with a suitable end-user organisation, which you may or not have already identified.</p>
                     <a href="overview_consumer_pathway.html" class="btn btn-primary">Learn more</a>
                 </div>
             </div>
         </div>
            <div class="col-6 col-md-4">
             <div class="panel panel-default text-center">
                 <div class="panel-heading">
                     <span class="fa-stack fa-5x">
                           <i class="fa fa-circle fa-stack-2x text-primary" style="color:#005EB8"></i>
                           <i class="fa fa-globe fa-stack-1x fa-inverse"></i>
                     </span>
                 </div>
                 <div class="panel-body" align="left">
                     <h4>Clinical system supplier</h4>
                         <p align="left">You're a GP clinical data supplier/provider.</p>
                     <p align="left">You want to use GP Connect to develop a way of allowing other systems to access GP data on your system for direct patient care.</p>
                     <a href="overview_provider_pathway.html" class="btn btn-primary">Learn more</a>
                 </div>
             </div>
         </div>
  <div class="col-6 col-md-4">
             <div class="panel panel-default text-center">
                 <div class="panel-heading">
                     <span class="fa-stack fa-5x">
                           <i class="fa fa-circle fa-stack-2x text-primary" style="color:#005EB8"></i>
                           <i class="fa fa-user-md fa-stack-1x fa-inverse"></i>
                     </span>
                 </div>
                 <div class="panel-body" align="left">
                     <h4>End-user organisation</h4>
                     <p align="left">You're a CCG with GP practices organised in a federation or hub; you're a hospital or provider of emergency care.</p>
                              <p align="left">You want to use an existing GP Connect API - or commission someone to create a new one - to access GP data from more than one GP clinical data provider to improve direct patient care.</p>
                      <a href="https://digital.nhs.uk/services/gp-connect" class="btn btn-primary">Learn more</a>
                                     </div>
         </div>
         </div> 
</div>
              
{% include important.html content="This site is under active development by the GP Connect team and is intended to provide all the technical resources you need to successfully develop GP Connect provider APIs or consuming applications. Some areas are being formulated and iterative updates to content will be added on a regular basis. See our GitHub [releases page](https://github.com/nhsconnect/gpconnect/releases) for more information." %}

{% include warning.html content="This site is provided for information only and is intended for those engaged with NHS Digital in First of Type activities. Other parties are advised not to develop against these specifications until a formal announcement has been made." %}

{% include twitterfollow.html %}

{% include gitterbadge.html %}




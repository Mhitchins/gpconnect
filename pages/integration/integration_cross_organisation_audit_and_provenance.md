---
title: Cross Organisation Audit & Provenance
keywords: spine, ssp, integration, audit, provenance
tags: [integration]
sidebar: overview_sidebar
permalink: integration_cross_organisation_audit_and_provenance.html
summary: "Overview of how audit and provenance data is expected to be transported over the GP Connect FHIR interfaces."
---

## Cross Organisation Audit & Provenance ##

### Governance ###

Provider systems SHALL ensure that access to confidential data, including patient or clinical data, through the API must meet, as a minimum, the same requirements for information governance, authentication and authorisation, and auditing as that of the host system the API exposes.

### Audit Trail ###

{% include important.html content="As the GP Connect APIs are commissioned under the GPSoC framework, Provider and Consumer systems are expected to follow the standard 'IG Requirements for GP Systems V4' and 'GP Systems Interface Mechanism' requirements." %}

For implementers that don't have access to the GP SoC Framework / 'IG Requirements for GP Systems V4' requirements then the following extract of requirements covers the main audit trail requirements:

Provider systems SHALL record in an audit trail all access and data changes within the system as a result of API activity in the same way that internal access and changes are required to be recorded.

Provider systems SHALL ensure that all API transactions are recorded in an audit trail, and that audit trails must be subject to the standard IG audit requirements as defined in “IG Requirements for GP Systems V4” or as subsequently amended.

Provider systems SHALL ensure failed or rejected API transactions are recorded with the same detail as for successful API requests, with error codes as per the [error handling guidance](development_fhir_error_handling_guidance.html).

Audit Trail records shall include the following minimum information:

- a record of the user identity. This is the User ID, Name, Role profile (including Role and Organisation, URP id when Smartcard authenticated) attribute values, obtained from the user’s Session structure;
- a record of the identify of the authority – the person authorising the entry of, or access to data (if different from the user);
- the date and time on which the event occurred;
- details of the nature of the audited event and the identity of the associated data (e.g. patient ID, message ID) of the audited event;
- a sequence number to protect against malicious attempts to subvert the audit trail by, for example, altering the system date.
- Audit trail records should include details of the end-user device (or system) involved in the recorded activity.

Audit Trails shall be enabled at all times and there shall be no means for users, or any other individuals, to disable any Audit Trail.

{% include note.html content="Whilst some details (such as name, role) associated with individual users are likely to change over time, the display of user information must reflect the state of such information as it was at the time of the associated event (such as data entry)." %}

### Provenance ###

Provider systems SHALL ensure that all additions, amendments or logical deletions to administrative and clinical data made via an API is clearly identified with information regarding the provenance of the data (e.g. timestamp, details of Consumer system, details of user (including role), so it is clear which information has been generated through an API rather than through the Provider system itself.

Provider systems SHALL record the following provenance details of all API personal and sensitive personal data recorded within the system:

- Author details (identified through unique ID), including name and role
- Data entered by (if different from author)
- Date & time (to the second) entered
- Originating organisation
- API interaction

### Legal Processing ###

Provider systems SHALL ensure that data provided to Consumer systems only include data for which the GP practice acts as Data Controller.

### Patient Dissent ###

Provider systems SHALL ensure that Patient Consent is respected (i.e. where express dissent is recorded then data is not shared).

## Cross Organisation Audit & Provenance Transport ##

### Bearer Token ###

Consumer systems SHALL provide audit and provenance details in the HTTP authorization header as an oAuth Bearer Token (as outlined in [RFC 6749](https://tools.ietf.org/html/rfc6749){:target="_blank"}) in the form of a JSON Web Token (JWT) as defined in [RFC 7519](https://tools.ietf.org/html/rfc7519){:target="_blank"}.

An example such an HTTP header is given below:

```
     Authorization: Bearer jwt_token_string
```

Provider systems SHALL respond to oAuth Bearer Token errors inline with [RFC 6750 - section 3.1](https://tools.ietf.org/html/rfc6750#section-3.1).

It is highly recommended that standard libraries are used for creating the JWT as constructing and encoding the token manually may lead to issues with parsing the token. A good source of information about JWT and libraries to use can be found on the [JWT.io site](https://jwt.io/)

### JSON Web Tokens (JWT) ###

Consumer system SHALL generate a new JWT for each API request. The Payload section of the JWT (see "JWT Generation" below for futher details) shall be populated as follows:

| Claim | Priority | Description | Fixed Value | Dynamic Value |
|-------|----------|-------------|-------------|------------------|
| iss | R | Requesting systems issuer URI | No | Yes |
| sub | R | ID for the user on whose behalf this request is being made. Matches `requesting_practitioner.id` | No | Yes |
| aud | R | Authorization server's `token_URL` | `https://authorize.fhir.nhs.net/token` | No |
| exp | R | Expiration time integer after which this authorization MUST be considered invalid. | `exp` | (now + 5 minutes) UTC time in seconds |
| iat | R | The UTC time the JWT was created by the requesting system | `iat` | now UTC time in seconds |
| reason_for_request | R | Purpose for which access is being requested | `directcare` | No |
| requested_record | R | A Minimal FHIR resource which describes the resource being requested or searched for. | No | FHIR Patient<sup>1</sup> <br/>OR <br/>FHIR Organization<sup>2</sup> |
| requested_scope | R | Data being requested<sup>2</sup> | `patient/*.[read|write]` <br/>OR <br/>`organization/*.[read|write]` | No |
| requesting_device | R | FHIR device resource making the request | No | FHIR Device<sup>1</sup> |
| requesting_organization | R | FHIR organisation resource making the request | No | FHIR Organization<sup>1+4</sup> | 
| requesting_practitioner | R | FHIR practitioner resource making the request | No | FHIR Practitioner<sup>1+3</sup> |

<sup>1</sup> Minimal FHIR resource to include any relevant business identifier(s), conforming to the base fhir resources definition (the resource does not need to conform to the GP Connect fhir profile).

<sup>2</sup> Patient scope for patient centric APIs (i.e. Get Care Record and Patient's Appointments, Patient Task) or scope for organisation centric APIs (i.e. Get Organization Schedule)

<sup>3</sup> To contain the practitioners local system identifier(s) (i.e. login details / username). Where the user has both a local system 'role' as well as a nationally-recognised role, then the latter SHALL be provided. Default usernames (e.g. referring to systems or groups) SHALL NOT be used in this field.

<sup>4</sup> The requesting organisation resource SHALL refer to the care organisation from where the request originates rather than any other organisation which may host hardware or software, route requests to Spine, and/or hold the endpoint registration. 

{% include important.html content="In topologies where GP Connect consumer applications are provisioned via a portal or middleware hosted by another organisation (see [Topologies](integration_system_topologies.html)) it is important for audit purposes that the practitioner and organisation populated in the JWT reflect the originating organisation rather than the hosting organisation." %}

{% include important.html content="Providers SHALL implement loose validation for identifier system and identifier value used within the JWT to allow for backwards compatibility as a consumer may use an identifier system from an older version of the spec when making a request." %}

#### Population of requesting_organization ####

The `requesting_organization` claim within the JWT SHALL be a FHIR `Organization` resource representing the organization making the request.

The organization resource SHALL include the elements:

| Element | Description |
| --- | --- |
| name | A textual representation of the name of the organisation. |
| identifier | An identifier should be included contain a fixed `system` of `"https://fhir.nhs.uk/Id/ods-organization-code"` and a identifier `value` containing the ODS code of requesting organsiation. |


#### Population of requested_record ####

The `requested_record` claim within the JWT should be a minimal FHIR resource which describes the resource being requested or searched for where possible. Currently the FHIR resource can either be a `Patient` or an `Organization` resource and will contain any relevant business identifiers to the request.

The table below shows some of the GP Connect API interactions and the expected content for the JWT requested_record claim:

| Request | Known Business Identifiers | requested_record Resource Type | requested_record Content |
| --- | --- | --- | --- |
| Access Record HTML | NHS Number | Patient | SHALL contain an identifier element containing the NHS Number being passed as a parameter to the "gpc.getcarerecord" operation. |
| Patient Search | NHS Number | Patient | SHALL contain an identifier element containing the NHS Number being passed as the search parameter of the request. |
| Patient Read | N/A | Patient | SHOULD contain the logical id of the patient resource being requested. |
| Organization Search | ODS Code | Organization | SHALL contain an identifier element containing the organization ODS Code which is being passed as the parameter to the search. |
| Practitioner Search | User Id | Organization | SHOULD contain any relevant identifier for the organization on which the fhir endpoint resides. |
| Practitioner Read | N/A | Organization | SHOULD contain any relevant identifier for the organization on which the fhir endpoint resides. |
| Search for free slots | N/A | Organization | SHOULD contain any relevant identifier for the organization on which the fhir endpoint resides. |
| Book Appointment | NHS Number | Patient | The consumer will have previously used the patients NHS Number to find the patients logical id on the providers system, therefore the requested_record Patient SHOULD contain the NHS number identifier element. |
| Read Appointment | NHS Number | Patient | The consumer will have previously used the patients NHS Number to find the patients logical id on the providers system, therefore the requested_record Patient SHOULD contain the NHS number identifier element. |

{% include note.html content="The provider SHALL validate that the requested_record claim details match the request parameters where possible, to ensure valid auditing of the requests end-to-end." %}

#### JWT Generation ####
Consumer systems SHALL generate the JSON Web Token (JWT) consisting of three parts seperated by dots (.), which are:

- Header
- Payload
- Signature

Consumer systems SHALL generate an Unsecured JSON Web Token (JWT) using the "none" algorithm parameter in the header to indicate that no digital signature or MAC has been performed (please refer to section 6 of [RFC 7519](https://tools.ietf.org/html/rfc7519){:target="_blank"} for details).

```json
{
  "alg": "none",
  "typ": "JWT"
}
```

Consumer systems SHALL generate an empty signature.

The final output is three Base64url encoded strings separated by dots (note - there is some canonicalisation done to the JSON before it is base64url encoded, which the JWT code libraries will do for you).

For example:

```shell
eyJhbGciOiJub25lIiwidHlwIjoiSldUIn0.eyJpc3MiOiJodHRwczovL1tDb25zdW1lclN5c3RlbVVSTF0iLCJzdWIiOiJbUHJhY3RpdGlvbmVySURdIiwiYXVkIjoiaHR0cHM6Ly9hdXRob3JpemUuZmhpci5uaHMubmV0L3Rva2VuIiwiZXhwIjoxNDY5NDM2OTg3LCJpYXQiOjE0Njk0MzY2ODcsInJlYXNvbl9mb3JfcmVxdWVzdCI6ImRpcmVjdGNhcmUiLCJyZXF1ZXN0ZWRfcmVjb3JkIjp7InJlc291cmNlVHlwZSI6IlBhdGllbnQiLCJpZGVudGlmaWVyIjpbeyJzeXN0ZW0iOiJodHRwczovL2ZoaXIubmhzLnVrL0lkL25ocy1udW1iZXIiLCJ2YWx1ZSI6IltOSFNOdW1iZXJdIn1dfSwicmVxdWVzdGluZ19kZXZpY2UiOnsicmVzb3VyY2VUeXBlIjoiRGV2aWNlIiwiaWQiOiJbRGV2aWNlSURdIiwiaWRlbnRpZmllciI6W3sic3lzdGVtIjoiW0RldmljZVN5c3RlbV0iLCJ2YWx1ZSI6IltEZXZpY2VJRF0ifV0sIm1vZGVsIjoiW1NvZnR3YXJlTmFtZV0iLCJ2ZXJzaW9uIjoiW1NvZnR3YXJlVmVyc2lvbl0ifSwicmVxdWVzdGluZ19vcmdhbml6YXRpb24iOnsicmVzb3VyY2VUeXBlIjoiT3JnYW5pemF0aW9uIiwiaWQiOiJbT3JnYW5pemF0aW9uSURdIiwiaWRlbnRpZmllciI6W3sic3lzdGVtIjoiaHR0cHM6Ly9maGlyLm5ocy51ay9JZC9vZHMtb3JnYW5pemF0aW9uLWNvZGUiLCJ2YWx1ZSI6IltPRFNDb2RlXSJ9XSwibmFtZSI6IlJlcXVlc3RpbmcgT3JnYW5pc2F0aW9uIE5hbWUifSwicmVxdWVzdGluZ19wcmFjdGl0aW9uZXIiOnsicmVzb3VyY2VUeXBlIjoiUHJhY3RpdGlvbmVyIiwiaWQiOiJbUHJhY3RpdGlvbmVySURdIiwiaWRlbnRpZmllciI6W3sic3lzdGVtIjoiaHR0cHM6Ly9maGlyLm5ocy51ay9JZC9zZHMtdXNlci1pZCIsInZhbHVlIjoiW1NEU1VzZXJJRF0ifSx7InN5c3RlbSI6IltVc2VyU3lzdGVtXSIsInZhbHVlIjoiW1VzZXJJRF0ifV0sIm5hbWUiOnsiZmFtaWx5IjpbIltGYW1pbHldIl0sImdpdmVuIjpbIltHaXZlbl0iXSwicHJlZml4IjpbIltQcmVmaXhdIl19fX0.
```

NOTE: The final section (the signature) is empty, so the JWT will end with a trailing `.` (this must not be omitted, otherwise it would be an invalid token).

{% include tip.html content="The [JWT.io](https://jwt.io/) website includes a number of rich resources to aid in developing JWT enabled applications." %}

## JWT Payload Example ##

```json
{
	"iss": "https://[ConsumerSystemURL]",
	"sub": "[PractitionerID]",
	"aud": "https://authorize.fhir.nhs.net/token",
	"exp": 1469436987,
	"iat": 1469436687,
	"reason_for_request": "directcare",
	"requested_record": {
		"resourceType": "Patient",
		"identifier": [{
			"system": "https://fhir.nhs.uk/Id/nhs-number",
			"value": "[NHSNumber]"
		}]
	},
	"requesting_device": {
		"resourceType": "Device",
		"id": "[DeviceID]",
		"identifier": [{
			"system": "[DeviceSystem]",
			"value": "[DeviceID]"
		}],
		"model": "[SoftwareName]",
		"version": "[SoftwareVersion]"
	},
	"requesting_organization": {
		"resourceType": "Organization",
		"id": "[OrganizationID]",
		"identifier": [{
			"system": "https://fhir.nhs.uk/Id/ods-organization-code",
			"value": "[ODSCode]"
		}],
		"name": "Requesting Organisation Name"
	},
	"requesting_practitioner": {
		"resourceType": "Practitioner",
		"id": "[PractitionerID]",
		"identifier": [{
			"system": "https://fhir.nhs.uk/Id/sds-user-id",
			"value": "[SDSUserID]"
		},
		{
			"system": "[UserSystem]",
			"value": "[UserID]"
		}],
		"name": {
			"family": ["[Family]"],
			"given": ["[Given]"],
			"prefix": ["[Prefix]"]
		}
	}
}
```

{% include important.html content="Whilst the use of a JWT and the claims naming is inspired by the [SMART on FHIR](https://github.com/smart-on-fhir/smart-on-fhir.github.io/wiki/cross-organizational-auth) the GP Connect programme hasn't commit to using the SMART on FHIR specification." %}

Where the Practitioner has both a local system role as well as a Spine RBAC role, then the Spine RBAC role SHALL be supplied
{% include todo.html content="Spine RBAC role support to be added in [Stage 2.](designprinciples_maturity_model.html)" %}

### Example Code ###

#### C# ####

{% include tip.html content="The following code snippet utilise the [Microsoft Identity Model JWT Token Nuget Package](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt/) for creating, serializing and validating JWT tokens." %}

{% gist michaelmeasures/d6a75e52acdbee93c4c30d23e639fb1a %}

## External Documents / Policy Documents ##

| Name | Author | Version | Updated |
| GPSoC IG Requirements for GP Systems | NHS Digital | v4.0 | 19/09/2014 |
| GP Systems Interface Mechanism Requirements V 1| NHS Digital | v0.10|
---
title: Register a patient
keywords: foundations, patient, nhsnumber, pid, register
tags: [foundations,use_case]
sidebar: foundations_sidebar
permalink: foundations_use_case_register_a_patient.html
summary: "Use case for registering a patient with an organization"
---

## API use case ##

The "Register a patient" capability should either create a new temporary patient registration or re-activate an existing "inactive" patient registration, as a temporary patient registration within the GP practice system ([Definition of a GP Connect active patient](/overview_glossary.html#active-patient)).

This specification describes a single use case. For complete details and background please see the [Foundations capability bundle](foundations.html).

{% include note.html content="This API use case is designed only to support the need to  register a **temporary** patient at a federated organisation as an enabler for federated appointment bookings. It does not change a patient's registered practice information as held on Personal Demographics Service (PDS)." %}

## Security ##

- GP Connect utilises TLS Mutual Authentication for system level authorization
- GP Connect utilises a JSON Web Tokens (JWT) to transmit clinical audit and provenance details 

## Prerequisites ##

### Consumer ###

The consumer system:

- SHALL have previously resolved the organisation's FHIR endpoint Base URL through the [Spine Directory Service](https://nhsconnect.github.io/gpconnect/integration_spine_directory_service.html)
- SHALL have previously traced the patient's NHS number using the [Personal Demographics Service]( https://nhsconnect.github.io/gpconnect/integration_personal_demographic_service.html) or an equivalent service


## API usage ##

### Request operation ###

#### FHIR relative request ####

```http
POST /Patient/$gpc.registerpatient
```

#### FHIR absolute request ####

```http
POST https://[proxy_server]/https://[provider_server]/[fhir_base]/Patient/$gpc.registerpatient
```

#### Request headers ####

Consumers SHALL include the following additional HTTP request headers:

| Header               | Value |
|----------------------|-------|
| `Ssp-TraceID`        | Consumer's TraceID (i.e. GUID/UUID) |
| `Ssp-From`           | Consumer's ASID |
| `Ssp-To`             | Provider's ASID |
| `Ssp-InteractionID`  | `urn:nhs:names:services:gpconnect:fhir:operation:gpc.registerpatient-1`|

Example HTTP request headers:

```http
POST http://gpconnect.aprovider.nhs.net/GP001/DSTU2/1/Patient/$gpc.registerpatient HTTP/1.1
User-Agent: .NET FhirClient for FHIR 1.2.0
Accept: application/fhir+json;charset=utf-8
Prefer: return=representation
Host: michaelm-pc
Content-Type: application/fhir+json;charset=utf-8
Content-Length: 289
Expect: 100-continue
Connection: Keep-Alive
Ssp-TraceID: 629ea9ba-a077-4d99-b289-7a9b19fd4e03
Ssp-From: 200000000115
Ssp-To: 200000000116
Ssp-InteractionID: urn:nhs:names:services:gpconnect:fhir:operation:gpc.registerpatient-1
```

#### Payload request body ####

The request payload is a [Parameters](https://www.hl7.org/fhir/STU3/parameters.html) resource conforming to the [GPConnect-RegisterPatient-Operation-1](https://fhir.nhs.uk/STU3/OperationDefinition/GPConnect-RegisterPatient-Operation-1/) profile, with a single parameter of `registerPatient` containing a `Patient` resource profiled to the [CareConnect-GPC-Patient-1](https://fhir.nhs.uk/STU3/StructureDefinition/CareConnect-GPC-Patient-1) profile.  This is the patient to be registered.

Within the `Patient` resource: 

- The following fields SHALL be populated to allow the provider system to verify the patient's identity on PDS:
  - `identifier` with the patient's NHS number
  - `birthDate`
  - `name` including `family` and `given`, with the `use` element set to `official`

- The following fields MAY be populated in order to record temporary details known to the consuming system:
  - `telecom` with temporary telecom details, with the `use` element set to `temp`.  No more than one instance of this SHALL be populated.
  - `address` with temporary address details, with the `use` element set to `temp`.  No more than one instance of this SHALL be populated.

- The following field MAY be populated by appointment booking consumers:
    - the `preferredBranchSurgery` within the `registrationDetails` extension, with a location reference where available and relevant to the registration.
  
      When a consumer is using the patient registration as part of an appointment booking they will have previously selected a Slot which will be associated with a Location. The consumer may use this location as the preferred branch surgery within the patient registration.

- **All other fields MUST NOT be populated.**

On the wire a JSON serialised `$gpc.registerpatient` request would look something like the following:

```json
{
  "resourceType": "Parameters",
  "parameter": [
    {
      "name": "registerPatient",
      "resource": {
        "resourceType": "Patient",
        "meta": {
          "profile": [
            "https://fhir.nhs.uk/STU3/StructureDefinition/CareConnect-GPC-Patient-1"
          ]
        },
        "extension": [
          {
            "url": "https://fhir.nhs.uk/STU3/StructureDefinition/Extension-CareConnect-GPC-RegistrationDetails-1",
            "extension": [
              {
                "url": "preferredBranchSurgery",
                "valueReference": {
                  "reference": "Location/14"
                }
              }
            ]
          }
        ],
        "identifier": [
          {
            "extension": [
              {
                "url": "https://fhir.nhs.uk/STU3/StructureDefinition/Extension-CareConnect-GPC-NHSNumberVerificationStatus-1",
                "valueCodeableConcept": {
                  "coding": [
                    {
                      "system": "https://fhir.nhs.uk/CareConnect-NHSNumberVerificationStatus-1",
                      "code": "01",
                      "display": "Number present and verified"
                    }
                  ]
                }
              }
            ],
            "system": "https://fhir.nhs.uk/Id/nhs-number",
            "value": "9476719931"
          }
        ],
        "name": [
          {
            "use": "official",
            "text": "Minnie DAWES",
            "family": "Jackson",
            "given": [
              "Jane"
            ],
            "prefix": [
              "Miss"
            ]
          }
        ],
        "telecom": [
          {
            "system": "phone",
            "value": "01454587554",
            "use": "temp"
          }
        ],
        "birthDate": "1952-05-31",
        "address": [
          {
            "use": "temp",
            "type": "physical",
            "line": [
              "Trevelyan Square",
              "Boar Ln"
            ],
            "city": "Leeds",
            "district": "West Yorkshire",
            "postalCode": "LS1 6AE"
          }
        ]
      }
    }
  ]
}
```

### Provider system registration handling requirements ###

The following registration handling requirements MUST be implemented by providers, however due to provider system variances the implemented flow MAY deviate where required to accomodate these:

#### PDS registration requirements

Before registering the patient record on the local system, the provider SHALL retrieve the patient's demographic record from PDS using NHS number, and then:

- **Verify the patient's NHS number according to the following rules**:

  - The NHS number within the request is considered verified if:
    - The NHS number is found on PDS and the date of birth in the request exactly matches the date of birth held on PDS
    - OR should 2 out of 3 parts of the date of birth match (YYYY or MM or DD) AND the first 3 characters of the family name match and the initial character of the forename match that held for the record on PDS.
  - If both of the above checks fail to find a match then the NHS number is treated as not verified

- **Check that the patient is not recorded as deceased on PDS**

- **Check that patient's PDS record does not have any of the following PDS flags**:
  - Invalid
  - Sensitive
  - Superseded

- **If any of the following conditions occur, the registration MUST be halted and an error returned to the consuming system**:

  - the patient is recorded as deceased
  - the patient's record has a PDS flag shown above
  - the patient's record could not be found on PDS
  - the connection to PDS could not be made

#### Local system registration requirements

Before registering the patient record on the local system, the provider SHALL check the practice patient index for matching patients using NHS number, and then:

- **If a matching patient record IS found**:
  
  - and is **active** (i.e. a currently registered patient, of any registration type):

    - The registration SHALL be halted and an a `409` `DUPLICATE_REJECTED` returned to the consumer

  - and is **inactive** (i.e. a patient whose registration has lapsed of any registration type):

    - The patient's record SHOULD be re-activated as a **temporary** patient

      {% include note.html content="Provider systems MAY create a new **temporary** record in this instance but only in line with existing system handling for re-activation of inactive records for temporary patients." %}

    - Temporary address or telecom details sent by the consuming system (where provided) SHALL be added to the record, and marked as *temporary* address and telecom details
      - The provider system SHALL not push/synchronise these temporary telecom or temporary address details with PDS.

    - the patient's record SHALL be returned to the consuming system shown in [Payload response body](foundations_use_case_register_a_patient.html#payload-response-body) below.

  - AND is recorded as **deceased**:

    - The registration MUST be halted and an error returned to the consuming system

- **If a matching patient record IS NOT found**:

  - A new **temporary** patient record SHALL be created:

    - using the demographic details returned from the PDS record
    - and temporary address or telecom details sent by the consuming system (where provided), and marked as *temporary* address and telecom details
      - The provider system SHALL not push/synchronise these temporary telecom or temporary address details to PDS.
    - the patient's preferred branch surgery SHOULD be set if provided by the consumer
    - the patient's record SHALL be returned to the consuming system shown in [Payload response body](foundations_use_case_register_a_patient.html#payload-response-body) below.

{% include warning.html content="Provider systems MUST NOT create or re-activate a patient as a GMS (regular) patient.  Doing so would adversely affect national systems and interfere with the practice's caseload." %}

#### Error Handling ####

The Provider system SHALL return an error if:

- the `Parameters` resource passed by the consuming system including the embedded `Patient` resource is invalid, or does not include the minimum mandatory details (see above)
- the NHS number could not be found on PDS, or verified against a PDS record (see above)
- the PDS record contains an invalid, sensitive or superseded flag
- the patient is marked as deceased on PDS, or on the provider system
- PDS is unavailable, or could not be contacted
- the patient is already registered in which case a `409` `DUPLICATE_REJECTED` error shall be returned

Provider systems SHALL return an [GPConnect-OperationOutcome-1](https://fhir.nhs.uk/STU3/StructureDefinition/GPConnect-OperationOutcome-1) resource that provides additional detail when one or more data fields are corrupt or a specific business rule/constraint is breached.

Refer to [Development - FHIR API Guidance - Error Handling](development_fhir_error_handling_guidance.html) for details of error codes.

### Request response ###

#### Response headers ####

```http
HTTP/1.1 200 OK
Cache-Control: no-store
Content-Type: application/fhir+json; charset=utf-8
Date: Sun, 07 Aug 2016 11:13:05 GMT
Content-Length: 1464
```

#### Payload response body ####

Provider systems:

- SHALL return a `200` **OK** HTTP status code on successful registration of the patient into the provider system.
- SHALL include the URI of the relevant GP Connect `StructureDefinition` profile in the `Patient.meta.profile` element of the returned resources.
- SHALL return a searchset `Bundle` profiled to [GPConnect-Searchset-Bundle-1](https://fhir.nhs.uk/STU3/StructureDefinition/GPConnect-Searchset-Bundle-1) with a `Patient` profiled to [CareConnect-GPC-Patient-1](https://fhir.nhs.uk/STU3/StructureDefinition/CareConnect-GPC-Patient-1):

  The patient SHALL contain details of the newly registered or re-activated patient including details sourced from PDS as well as consumer-sent temporary contact (telecom and/or address) details, AND:

  - SHALL populate the sub-elements of the `registrationDetails` extension:
    - `preferredBranchSurgery` with either the preferred branch surgery location passed in by the consumer or main surgery location
    - `registrationType` with the registration type used within the provider system. If an appropriate registration type is not available within the valueset then the `Other` type SHALL be use and the name of the registration type SHOULD be added using the `text` element of the CodeableConcept
- SHALL NOT populate `ethnicCategory`, `religiousAffiliation`, `patient-cadavericDonor`, `maritalStatus`.


```json
{
  "resourceType": "Bundle",
  "meta": {
    "profile": [
      "https://fhir.nhs.uk/STU3/StructureDefinition/GPConnect-Searchset-Bundle-1"
    ]
  },
  "type": "searchset",
  "entry": [
    {
      "resource": {
        "resourceType": "Patient",
        "id": "2",
        "meta": {
          "versionId": "1469448000000",
          "profile": [
            "https://fhir.nhs.uk/STU3/StructureDefinition/CareConnect-GPC-Patient-1"
          ]
        },
        "extension": [
          {
            "url": "https://fhir.nhs.uk/STU3/StructureDefinition/Extension-CareConnect-GPC-RegistrationDetails-1",
            "extension": [
              {
                "url": "registrationPeriod",
                "valuePeriod": {
                  "start": "2017-09-07T14:17:44+01:00"
                }
              },
              {
                "url": "registrationType",
                "valueCodeableConcept": {
                  "coding": [
                    {
                      "system": "https://fhir.nhs.uk/CareConnect-RegistrationType-1",
                      "code": "T"
                    }
                  ]
                }
              },
              {
                "url": "https://fhir.nhs.uk/STU3/StructureDefinition/Extension-CareConnect-GPC-RegistrationDetails-1",
                "extension": [
                  {
                    "url": "preferredBranchSurgery",
                    "valueReference": {
                      "reference": "Location/785b4ff5-aced-4bdf-b7ed-34f92131ce97"
                    }
                  }
                ]
              }
            ]
          }
        ],
        "identifier": [
          {
            "extension": [
              {
                "url": "https://fhir.nhs.uk/STU3/StructureDefinition/Extension-CareConnect-GPC-NHSNumberVerificationStatus-1",
                "valueCodeableConcept": {
                  "coding": [
                    {
                      "system": "https://fhir.nhs.uk/CareConnect-NHSNumberVerificationStatus-1",
                      "code": "01",
                      "display": "Number present and verified"
                    }
                  ]
                }
              }
            ],
            "system": "https://fhir.nhs.uk/Id/nhs-number",
            "value": "9476719931"
          }
        ],
        "active": true,
        "name": [
          {
            "use": "official",
            "text": "JACKSON Jane (Miss)",
            "family": "Jackson",
            "given": [
              "Jane"
            ],
            "prefix": [
              "Miss"
            ]
          }
        ],
        "telecom": [
          {
            "system": "phone",
            "value": "01234567891",
            "use": "mobile"
          },
          {
            "system": "phone",
            "value": "01454587554",
            "use": "temp"
          }
        ],
        "gender": "female",
        "birthDate": "1952-05-31",
        "address": [
          {
            "use": "temp",
            "type": "physical",
            "line": [
              "Trevelyan Square",
              "Boar Ln"
            ],
            "city": "Leeds",
            "district": "West Yorkshire",
            "postalCode": "LS1 6AE"
          },
          {
            "use": "home",
            "type": "physical",
            "line": [
              "Bridgewater Place",
              "1 Water Ln"
            ],
            "city": "Leeds",
            "district": "West Yorkshire",
            "postalCode": "LS11 5RU"
          }
        ]
      }
    }
  ]
}
```

## Examples ##

### C# ###

{% include tip.html content="C# code snippets utilise Ewout Kramer's [fhir-net-api](https://github.com/ewoutkramer/fhir-net-api) library which is the official .NET API for HL7&reg; FHIR&reg;." %}

```csharp
var client = new FhirClient("http://gpconnect.aprovider.nhs.net/GP001/STU3/1/");
client.PreferredFormat = ResourceFormat.Json;
var parameters = new Parameters();
parameters.Add("registerPatient", new Patient
{
	Active = true,
	BirthDate = "1976-01-10",
	Gender = (AdministrativeGender)Enum.Parse(typeof(AdministrativeGender),"Male"),
	Name = new List<HumanName>
	{
		new HumanName()
		{
			Given = new[] {"Mike"},
			Family = new[] {"Smith"},
			Use = HumanName.NameUse.Usual
		}
	},
	Identifier =
	{
		new Identifier("https://fhir.nhs.uk/Id/nhs-number","1234569999")
	}
});
var resource = client.TypeOperation("gpc.registerpatient","Patient",parameters);
FhirSerializer.SerializeResourceToJson(resource).Dump();
```

### Java ###

{% include tip.html content="Java code snippets utilise James Agnew's [hapi-fhir](https://github.com/jamesagnew/hapi-fhir/
) library." %}

```java
FhirContext ctx = FhirContext.forStu3();
IGenericClient client = ctx.newRestfulGenericClient("http://gpconnect.aprovider.nhs.net/GP001/STU3/1/");
Patient patient = new Patient();
patient.addIdentifier()
   .setSystem("https://fhir.nhs.uk/Id/nhs-number")
   .setValue("1234569999");
patient.addName()
   .addFamily("Smith")
   .addGiven("Mike")
   .setUse(NameUseEnum.USUAL);
patient.setGender(AdministrativeGenderEnum.MALE);
patient.setBirthDate(new DateDt("1976-01-10"));

// Create the input parameters to pass to the server
Parameters inParams = new Parameters();
inParams.addParameter().setName("registerPatient").setValue(patient);

Parameters outParams = client
		.operation()
		.onType(Patient.class)
		.named("gpc.registerpatient")
		.withParameters(inParams)
		.execute();

Bundle responseBundle = (Bundle) outParams.getParameter().get(0).getResource();
System.out.println(ctx.newXmlParser().setPrettyPrint(true).encodeResourceToString(responseBundle));
```

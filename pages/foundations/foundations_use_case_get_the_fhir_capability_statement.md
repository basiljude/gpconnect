---
title: Get the FHIR CapabilityStatement
keywords: foundations, fhir
tags: [foundations,use_case,fhir]
sidebar: foundations_sidebar
permalink: foundations_use_case_get_the_fhir_capability_statement.html
summary: "Use case for getting the GP Connect FHIR server's capability statement."
---

## Prerequisites ##

### Consumer ###

The Consumer system:

- SHALL have previously resolved the organisation's FHIR endpoint Base URL through the [Spine Directory Service](https://nhsconnect.github.io/gpconnect/integration_spine_directory_service.html)

## API Usage ##

### Request Operation ###

#### FHIR Relative Request ####

```http
GET /metadata
```

#### FHIR Absolute Request ####

```http
GET https://[proxy_server]/https://[provider_server]/[fhir_base]/metadata
```

#### Request Headers ####

Consumers SHALL include the following additional HTTP request headers:

| Header               | Value |
|----------------------|-------|
| `Ssp-TraceID`        | Consumer's TraceID (i.e. GUID/UUID) |
| `Ssp-From`           | Consumer's ASID |
| `Ssp-To`             | Provider's ASID |
| `Ssp-InteractionID`  | `urn:nhs:names:services:gpconnect:fhir:rest:read:metadata`|

#### Payload Request Body ####

N/A

#### Error Handling ####

Provider systems are expected to always be able to return a valid capability statement.

### Request Response ###

#### Response Headers ####

Provider systems are not expected to add any specific headers beyond that described in the HTTP and FHIR&reg; standards.

#### Payload Response Body ####

Provider systems:

- SHALL return a `200` **OK** HTTP status code on successful retrival of the capability statement.
- SHALL return a capability statement which conforms to the standard [FHIR CapabilityStatement](http://hl7.org/fhir/STU3/capabilitystatement.html)

An example GP Connect CapabilityStatement of type `Instance` is shown below:

```xml
<CapabilityStatement xmlns="http://hl7.org/fhir">
	<version value="1.0.0-rc.1" />
	<name value="GP Connect" />
	<status value="draft" />
	<experimental value="true" />
	<date value="2017-11-02" />
	<publisher value="NHS Digital" />
	<contact>
		<name value="Software Vendor Contact Name" />
	</contact>
	<description value="This server is a reference implementation of the GP Connect FHIR APIs" />
	<copyright value="Copyright NHS Digital 2016" />
	<software>
		<name value="Software Name" />
		<version value="Software Verson" />
		<releaseDate value="2017-11-02" />
	</software>
	<fhirVersion value="3.0.1" />
	<acceptUnknown value="both" />
	<format value="application/fhir+xml" />
	<format value="application/fhir+json" />
	<format value="application/xml+fhir" />
	<format value="application/json+fhir" />
	<format value="application/xml" />
	<format value="application/json" />
	<format value="text/json" />
	<profile>
		<reference value="https://fhir.nhs.uk/STU3/StructureDefinition/GPConnect-Device-1"/>
		<reference value="https://fhir.nhs.uk/STU3/StructureDefinition/CareConnect-GPC-Location-1"/>
		<reference value="https://fhir.nhs.uk/STU3/StructureDefinition/GPConnect-OperationOutcome-1"/>
		<reference value="https://fhir.nhs.uk/STU3/StructureDefinition/CareConnect-GPC-Organization-1"/>
 		<reference value="https://fhir.nhs.uk/STU3/StructureDefinition/CareConnect-GPC-Patient-1"/>
		<reference value="https://fhir.nhs.uk/STU3/StructureDefinition/CareConnect-GPC-Practitioner-1"/>
		<reference value="https://fhir.nhs.uk/STU3/StructureDefinition/GPConnect-Appointment-1"/>
		<reference value="https://fhir.nhs.uk/STU3/StructureDefinition/GPConnect-Schedule-1"/>
		<reference value="https://fhir.nhs.uk/STU3/StructureDefinition/GPConnect-Slot-1"/>
	</profile>
	<rest>
		<mode value="server" />
		<security>
			<cors value="true" />
			<certificate>
				<blob />
			</certificate>
		</security>
		<resource>
			<type value="Patient" />
			<interaction>
				<code value="read" />
			</interaction>
			<interaction>
				<code value="search-type" />
			</interaction>
			<versioning value="versioned" />
			<readHistory value="false" />
			<updateCreate value="false" />
			<searchParam>
				<name value="identifier" />
				<type value="token" />
				<documentation value="NHS Number (i.e. https://fhir.nhs.uk/Id/nhs-number|123456789)" />
			</searchParam>
		</resource>
		<resource>
			<type value="Organization" />
			<interaction>
				<code value="read" />
			</interaction>
			<interaction>
				<code value="search-type" />
			</interaction>
			<versioning value="versioned" />
			<readHistory value="false" />
			<updateCreate value="false" />
			<searchParam>
				<name value="identifier" />
				<type value="token" />
				<documentation value="ODS Code (i.e. https://fhir.nhs.uk/Id/ods-organization-code|Y12345)" />
			</searchParam>
		</resource>
		<resource>
			<type value="Practitioner" />
			<interaction>
				<code value="read" />
			</interaction>
			<interaction>
				<code value="search-type" />
			</interaction>
			<versioning value="versioned" />
			<readHistory value="false" />
			<updateCreate value="false" />
			<searchParam>
				<name value="identifier" />
				<type value="token" />
				<documentation value="SDS User Id (i.e. https://fhir.nhs.uk/Id/sds-user-id|999999)" />
			</searchParam>
		</resource>
		<resource>
			<type value="Appointment" />
			<interaction>
				<code value="read" />
			</interaction>
			<interaction>
				<code value="create" />
			</interaction>
			<interaction>
				<code value="update" />
			</interaction>
			<interaction>
				<code value="patch" />
			</interaction>
			<interaction>
				<code value="search-type" />
			</interaction>
			<versioning value="versioned" />
			<readHistory value="false" />
			<updateCreate value="false" />
			<searchParam>
				<name value="identifier" />
				<type value="token" />
				<documentation value="NHS Number (i.e. https://fhir.nhs.uk/Id/nhs-number|123456789)" />
			</searchParam>
		</resource>
		<resource>
			<type value="Location" />
			<interaction>
				<code value="read" />
			</interaction>
			<interaction>
				<code value="search-type" />
			</interaction>
			<versioning value="versioned" />
			<readHistory value="false" />
			<updateCreate value="false" />
			<searchParam>
				<name value="identifier" />
				<type value="token" />
				<documentation value="ODS Code (i.e. ODS Site Code (i.e. https://fhir.nhs.uk/Id/ods-site-code|Y12345678)" />
			</searchParam>
		</resource>
		<resource>
			<type value="Slot" />
			<interaction>
				<code value="search-type" />
			</interaction>
			<versioning value="versioned" />
			<readHistory value="false" />
			<updateCreate value="false" />
			<searchParam>
				<name value="start" />
				<type value="date" />
			</searchParam>
			<searchParam>
				<name value="end" />
				<type value="date" />
			</searchParam>
			<searchParam>
				<name value="fb-type" />
				<type value="token" />
			</searchParam>
		</resource>
		<operation>
			<name value="gpc.getcarerecord" />
			<definition>
				<reference value="OperationDefinition/gpc.getcarerecord" />
			</definition>
		</operation>
		<operation>
			<name value="gpc.registerpatient" />
			<definition>
				<reference value="OperationDefinition/gpc.registerpatient" />
			</definition>
		</operation>
	</rest>
</CapabilityStatement>
```

Consumer Systems:
- SHOULD, request the capability statement from the FHIR server endpoint in order to ascertain details of the implementation of GPConnect capabilities delivered by the FHIR server.
- SHOULD cache capability statement information retrieved from an endpoint at run-time on a per-session basis.

### C# client request to get the capability statement ###

{% include tip.html content="C# code snippets utilise Ewout Kramer's [fhir-net-api](https://github.com/ewoutkramer/fhir-net-api) library which is the official .NET API for HL7&reg; FHIR&reg;." %}

```csharp
var client = new FhirClient("http://gpconnect.aprovider.nhs.net/GP001/STU3/1/");
client.PreferredFormat = ResourceFormat.Json;
var resource = client.CapabilityStatement();
FhirSerializer.SerializeResourceToXml(resource).Dump();
```
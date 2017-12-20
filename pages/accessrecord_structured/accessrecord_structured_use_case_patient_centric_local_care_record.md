---
title: View patient-centric summary
keywords: access record structured, use case, retrieve
tags: [accessrecord_structured,use_case]
sidebar: accessrecord_rest_sidebar
permalink: accessrecord_structured_use_case_patient_centric_local_care_record.html
summary: "Use case for viewing patient-centric summary in local care record"
---

## Use Case ##
Patient-centric summary in local care record.

## Description ##
The Local Care Record (LCR) development team intends to deliver an integrated patient summary, presented to every user when they first log into a patient record. To do this, it will require structured data from GP systems (hospital, community, mental health and social care data already comes in structured form).

### Scenarios ###

- district nurse checking a patient’s record before visiting the patient at their home so they have an up to date understanding of their medical status and treatments
- mental health professional reviewing a patient’s holistic situation as part of initial assessment - understanding the various encounters the patient is receiving across all care settings
- hospital pharmacist assessing medications and allergies across all care settings as part of medications reconciliation process

## Use case justification ##

Structured GP data is an essential element for the Local Care Record. This is due to extensive feedback we have received that frontline users find the current layout too complex to navigate and they require a snapshot view of the patient across all care settings. This patient summary could be re-used in other locations once delivered.

## Primary actors ##
Any clinician/care giver who views the Local Care Record and is not a hospital user (hospital users will receive their own patient summary with a hospital focused scope). Approximately 2,100 users currently fit into this category.

## Triggers ##
Non-hospital user accesses patient’s Local Care Record view of their patient to gain a snapshot of who is involved with the patient, what alerts, allergies, medications and so on are currently available.

## Preconditions ##
Patient must be registered at a Local Care Record GP practice.
Patient must have a valid NHS number.
Patient must have data sharing preferences in their GP practice clinical system set to allow the sharing of data.

## Postconditions ##
**On success**
-	patient data will be retrieved from the patient’s GP practice clinical system and incorporated into the summary screen displayed to the user

**Guaranteed**
-	either the patient’s data, or an error message explaining why there is no data passed will be available to display in the patient summary screen

## Potential error messages ##
-	No data available
-	Patient not found
-	Sharing agreement is not set up
-	Patient does not consent
-	Response payload is invoked
-	Unable to invoke endpoint
-	Unable to resolve NACS
-	Unknown error details
-	Timeout
-	Message processing error
-	Request channel timeout
-	Underlying connection was closed
-	No endpoint listening

## Basic flow with alternative and exception flows ##

| Step number | Step description |
|:---- |:-------------- | 
| Step 1 | LCR user attempts to access patient LCR record (either using a browser or a link within their local clinical system). |
| Step 2 | LCR system identifies the patient’s GP practice endpoint using PDS/SDS lookup. |
| Step 2a | Patient is not found at a GP practice. Standard hospital view is presented to user \- error message advising why no GP data available. |
| Step 3 | Spine Security Proxy (SSP) checks organisation-to-organisation sharing agreement exists between requesting organisation (doctors) and the patient’s registered GP practice, and that the interaction (such as Get Medications) is part of the sharing agreement. |
| Step 3a | Patient is not registered at an LCR practice. Therefore, there is no data sharing agreement. A standard hospital view is presented to user \- error message advising why no GP data available. |
| Step 4 | GP practice clinical system checks patient permissions and consent to share. |
| Step 4a | Patient has not consented to share. Standard hospital view is presented to user \- error message advising why no GP data available. |
| Step 5 | System makes a series of calls to GP Connect:<br>- Medications – medications within their prescribed periods (that is, active). For example, GP prescribes for 8 weeks on 1st February. This medication would show as active until 29th March, 8 weeks later. <br>- Acute<br>- Repeat<br>- Hospital (prescribed by hospital, recorded on GPSS).<br>- Who and when prescribed it. <br>- Allergies<br>- Allergy and severity (if available)<br>- who and when identified it<br>- conditions/problems<br>- Severity<br>- Who and when identified it<br>- Immunisation<br>- Which version<br>- Who and when gave vaccination<br>- Date<br>- Vaccine code<br>- Performer<br>- Vital signs<br>- Observations<br>- Procedures<br>- Encounter<br> For all data retrieved include date recorded. |
| Step 6 | System makes a series of calls to other 4 systems to which the patient has an open referral with (hospital, community, social care, mental health) requesting the summary dataset from each. |
| Step 7 | The results of the calls made to each system are integrated into a series of melded views. |
| Step 7a | Any errors returned from any/all the calls in previous steps will be displayed as part of the summary. |

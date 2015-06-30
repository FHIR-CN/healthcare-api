## Organizations

# Get all organizations associated with the user
curl "https://api.humanapi.co/v1/human/medical/organizations?access_token=demo"

# Returns a json array of organizations:
[{
  "id": "53c050ac51c69003200aa9b5",
  "name": "Genesis Health System",
  "imageUrl": "Genesis.PNG"
}, {
  "id": "53c050ae51c69003200aaa29",
  "name": "Singing River Health System",
  "imageUrl": "Singing_River_Health_System.png"
}]

# Get an organization by id
curl "https://api.humanapi.co/v1/human/medical/organizations/53c050ae51c69003200aaa29?access_token=demo"

{
  "id": "53c050ae51c69003200aaa29",
  "name": "Singing River Health System",
  "imageUrl": "Singing_River_Health_System.png"
}

Get a list of medical organizations in Human API associated with the user
Get Organizations

Returns a list of medical organizations

GET https://api.humanapi.co/v1/human/medical/organizations

Returns a single organization

GET https://api.humanapi.co/v1/human/medical/organizations/{id}
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication


## Vitals

# Get all vitals readings associated with the user
curl "https://api.humanapi.co/v1/human/medical/vitals?access_token=demo"

# Returns a json array of vitals:
[{
  "id": "54e4c0d3303d780900c9ea4b",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:41:55.278Z",
  "createdAt": "2015-02-18T16:41:55.278Z",
  "dateTime": "Mon Jul 07 2014 14:40:26 GMT-0700 (PDT)",
  "author": "Christine Johnson",
  "results": [{
    "name": "SYSTOLIC BLOOD PRESSURE",
    "value": "120",
    "unit": "mm[Hg]"
  }, {
    "name": "DIASTOLIC BLOOD PRESSURE",
    "value": "70",
    "unit": "mm[Hg]"
  }, {
    "name": "HEART RATE",
    "value": "75",
    "unit": "/min"
  }, {
    "name": "BODY TEMPERATURE",
    "value": "36.4",
    "unit": "Cel"
  }, {
    "name": "WEIGHT",
    "value": "84.823",
    "unit": "kg"
  }, {
    "name": "BODY MASS INDEX",
    "value": "23.52",
    "unit": "kg/m2"
  }, {
    "name": "OXYGEN SATURATION",
    "value": "98",
    "unit": "%"
  }],
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}]

# Get an vitals reading by id
curl "https://api.humanapi.co/v1/human/medical/vitals/54e4c0d3303d780900c9ea4b?access_token=demo"

{
  "id": "54e4c0d3303d780900c9ea4b",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:41:55.278Z",
  "createdAt": "2015-02-18T16:41:55.278Z",
  "dateTime": "Mon Jul 07 2014 14:40:26 GMT-0700 (PDT)",
  "author": "Christine Johnson",
  "results": [{
    "name": "SYSTOLIC BLOOD PRESSURE",
    "value": "120",
    "unit": "mm[Hg]"
  }, {
    "name": "DIASTOLIC BLOOD PRESSURE",
    "value": "70",
    "unit": "mm[Hg]"
  }, {
    "name": "HEART RATE",
    "value": "75",
    "unit": "/min"
  }, {
    "name": "BODY TEMPERATURE",
    "value": "36.4",
    "unit": "Cel"
  }, {
    "name": "WEIGHT",
    "value": "84.823",
    "unit": "kg"
  }, {
    "name": "BODY MASS INDEX",
    "value": "23.52",
    "unit": "kg/m2"
  }, {
    "name": "OXYGEN SATURATION",
    "value": "98",
    "unit": "%"
  }],
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}

Get the user’s Vitals reading
Get Vitals

Returns a list of vitals readings

GET https://api.humanapi.co/v1/human/medical/vitals

Returns a single organization

GET https://api.humanapi.co/v1/human/medical/vitals/{id}
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication



## Narratives
Get a list of medical narratives associated with the user
Get Narratives

Returns a list of medical narratives

GET https://api.humanapi.co/v1/human/medical/narratives

Returns a single narrative

GET https://api.humanapi.co/v1/human/medical/narratives/{id}
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication


# Get all narratives associated with the user
curl "https://api.humanapi.co/v1/human/medical/narratives?access_token=demo"

# Returns a json array of narratives:
[{
  "id": "54e3521f79192a2e4993dfdc",
  "source": "emr-1-320",
  "updatedAt": "2015-02-11T06:26:53.217Z",
  "createdAt": "2015-02-11T06:26:53.217Z",
  "dateTime": "Mon Jul 07 2014 14:40:26 GMT-0700 (PDT)",
  "author": "Christine Johnson",
  "entries": [{
    "title": "Note from Sutter Health Affiliates and Community Connect Practices",
    "text": "This document contains information that was shared with Maxwell Forrest. It may not contain the entire record from Sutter Health Affiliates and Community Connect Practices."
  }, {
    "title": "Reason for Visit",
    "text": "Reason Comments Diarrhea"
  }, {
    "title": "Encounter Details",
    "text": "Date Type Department Care Team 07/07/2014 Office Visit Palo Alto Family Medicine 795 El Camino Real Palo Alto, CA 94301 650-333-4444 Johnson, Christine, NP 795 EL CAMINO REAL Palo Alto, CA 94301 650-333-4444 650-333-4444 (Fax)"
  }, {
    "title": "Active Allergies and Adverse Reactions - as of 02/09/2015",
    "text": "No Known Allergies"
  }, {
    "title": "Current Medications - as of 02/09/2015",
    "text": "No known medications"
  }, {
    "title": "Active Problems - as of 02/09/2015",
    "text": "Problem Noted Date No Active Medical Problems 04/14/2014"
  }, {
    "title": "Social History - as of 02/09/2015",
    "text": "Tobacco Use Types Packs/Day Years Used Date Never smoker 0 0 Alcohol Use Drinks/Week oz/Week Yes 1 Drinks containing 0.5 oz of alcohol 0.5"
  }, {
    "title": "Last Filed Vital Signs",
    "text": "Vital Sign Reading Time Taken Blood Pressure 120 / 70 07/07/2014  2:47 PM PDT Pulse 75 07/07/2014  2:47 PM PDT Temperature 36.4 °C (97.6 °F) 07/07/2014  2:47 PM PDT Respiratory Rate - - Height - - Weight 84.823 kg (187 lb) 07/07/2014  2:47 PM PDT Body Mass Index 23.52 07/07/2014  2:47 PM PDT Oxygen Saturation 98% 07/07/2014  2:47 PM PDT"
  }, {
    "title": "Plan of Care",
    "text": "Not on file"
  }, {
    "title": "Procedures",
    "text": ""
  }, {
    "title": "Results",
    "text": "Not on file"
  }, {
    "title": "Visit Diagnoses",
    "text": "Viral enteritis - Primary Intestinal infection due to other organism, not elsewhere classified Diarrhea Diarrhea"
  }, {
    "title": "Insurance",
    "text": "Payer Benefit Plan / Group Subscriber ID Type Phone Address BLUE SHIELD BLUE SHIELD PPO XEK901245582 PRIVATE FFS PPO/EPO PO BOX 272550 CHICO, CA 95927 Guarantor Name Account Type Relation to Patient Date of Birth Phone Billing Address FORREST,MAXWELL Personal/Family Self 05/30/1978 Work: +1-650-333-4444 Home: +1-650-333-4444 1 MILKY WAY WAY APT E MENLO PARK, CA 94025"
  }],
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}]

# Get a narrative by id
curl "https://api.humanapi.co/v1/human/medical/narratives/54e3521f79192a2e4993dfdc?access_token=demo"

{
  "id": "54e3521f79192a2e4993dfdc",
  "source": "emr-1-320",
  "updatedAt": "2015-02-11T06:26:53.217Z",
  "createdAt": "2015-02-11T06:26:53.217Z",
  "dateTime": "Mon Jul 07 2014 14:40:26 GMT-0700 (PDT)",
  "author": "Christine Johnson",
  "entries": [{
    "title": "Note from Sutter Health Affiliates and Community Connect Practices",
    "text": "This document contains information that was shared with Maxwell Forrest. It may not contain the entire record from Sutter Health Affiliates and Community Connect Practices."
  }, {
    "title": "Reason for Visit",
    "text": "Reason Comments Diarrhea"
  }, {
    "title": "Encounter Details",
    "text": "Date Type Department Care Team 07/07/2014 Office Visit Palo Alto Family Medicine 795 El Camino Real Palo Alto, CA 94301 650-333-4444 Johnson, Christine, NP 795 EL CAMINO REAL Palo Alto, CA 94301 650-333-4444 650-333-4444 (Fax)"
  }, {
    "title": "Active Allergies and Adverse Reactions - as of 02/09/2015",
    "text": "No Known Allergies"
  }, {
    "title": "Current Medications - as of 02/09/2015",
    "text": "No known medications"
  }, {
    "title": "Active Problems - as of 02/09/2015",
    "text": "Problem Noted Date No Active Medical Problems 04/14/2014"
  }, {
    "title": "Social History - as of 02/09/2015",
    "text": "Tobacco Use Types Packs/Day Years Used Date Never smoker 0 0 Alcohol Use Drinks/Week oz/Week Yes 1 Drinks containing 0.5 oz of alcohol 0.5"
  }, {
    "title": "Last Filed Vital Signs",
    "text": "Vital Sign Reading Time Taken Blood Pressure 120 / 70 07/07/2014  2:47 PM PDT Pulse 75 07/07/2014  2:47 PM PDT Temperature 36.4 °C (97.6 °F) 07/07/2014  2:47 PM PDT Respiratory Rate - - Height - - Weight 84.823 kg (187 lb) 07/07/2014  2:47 PM PDT Body Mass Index 23.52 07/07/2014  2:47 PM PDT Oxygen Saturation 98% 07/07/2014  2:47 PM PDT"
  }, {
    "title": "Plan of Care",
    "text": "Not on file"
  }, {
    "title": "Procedures",
    "text": ""
  }, {
    "title": "Results",
    "text": "Not on file"
  }, {
    "title": "Visit Diagnoses",
    "text": "Viral enteritis - Primary Intestinal infection due to other organism, not elsewhere classified Diarrhea Diarrhea"
  }, {
    "title": "Insurance",
    "text": "Payer Benefit Plan / Group Subscriber ID Type Phone Address BLUE SHIELD BLUE SHIELD PPO XEK901245582 PRIVATE FFS PPO/EPO PO BOX 272550 CHICO, CA 95927 Guarantor Name Account Type Relation to Patient Date of Birth Phone Billing Address FORREST,MAXWELL Personal/Family Self 05/30/1978 Work: +1-650-333-4444 Home: +1-650-333-4444 1 MILKY WAY WAY APT E MENLO PARK, CA 94025"
  }],
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}

## Test Results
Get a list of medical lab’s results from a user’s medical record
Get Test Results

Returns a list of medical test_results

GET https://api.humanapi.co/v1/human/medical/test_results

Returns a single test results

GET https://api.humanapi.co/v1/human/medical/test_results/{id}
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication
# Get all test_results associated with the user
curl "https://api.humanapi.co/v1/human/medical/test_results?access_token=demo"

# Returns a json array of test_results:
[{
  "id": "546d41318a6d23e01a728654",
  "source": "emr-1-320",
  "updatedAt": "2014-10-20T21:02:17.956Z",
  "createdAt": "2014-10-30T21:02:17.956Z",
  "components": [{
    "refRange": "<0.3",
    "name": "Bilirubin, Direct",
    "unit": "mg/dL",
    "value": "0.2"
  }],
  "name": "BILIRUBIN, DIRECT",
  "patient": {
    "name": "Maxwell Forrest"
  },
  "orderedBy": "James Kirk, MD",
  "recipients": [{
    "name": "Jean-Luc Picard, MD",
    "objectID": "50006",
    "isPCP": true,
    "recipTemplate": "WPMessageRecipientTemplateUnknown"
  }],
  "resultDateTime": "2012-09-18T04:00:00.000Z",
  "status": "Final result",
  "allResults": [{
    "name": "total bilirubin",
    "unit": "mg/dL",
    "value": "0.2",
    "resultDateTime": "2012-09-18T04:00:00.000Z"
  }],
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "546d41c68a6d23e01a728655",
  "source": "emr-1-320",
  "updatedAt": "2014-10-20T21:02:17.956Z",
  "createdAt": "2014-10-30T21:02:17.956Z",
  "components": [{
    "high": "4.8",
    "low": "3.5",
    "name": "Albumin, Serum / Plasma",
    "unit": "g/dL",
    "value": "4.4"
  }],
  "name": "ALBUMIN, SERUM / PLASMA",
  "patient": {
    "name": "Maxwell Forrest"
  },
  "orderedBy": "James Kirk, MD",
  "recipients": [{
    "name": "Jean-Luc Picard, MD",
    "objectID": "50006",
    "isPCP": true,
    "recipTemplate": "WPMessageRecipientTemplateUnknown"
  }],
  "resultDateTime": "2012-09-18T04:00:00.000Z",
  "status": "Final result",
  "allResults": [{
    "name": "albumin",
    "resultDateTime": "2012-09-18T04:00:00.000Z",
    "unit": "g/dL",
    "value": "4.84"
  }],
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}]

# Get a test results by id
curl "https://api.humanapi.co/v1/human/medical/test_results/546d41c68a6d23e01a728655?access_token=demo"

{
  "id": "546d41c68a6d23e01a728655",
  "source": "emr-1-320",
  "updatedAt": "2014-10-20T21:02:17.956Z",
  "createdAt": "2014-10-30T21:02:17.956Z",
  "components": [{
    "high": "4.8",
    "low": "3.5",
    "name": "Albumin, Serum / Plasma",
    "unit": "g/dL",
    "value": "4.4"
  }],
  "name": "ALBUMIN, SERUM / PLASMA",
  "patient": {
    "name": "Maxwell Forrest"
  },
  "orderedBy": "James Kirk, MD",
  "recipients": [{
    "name": "Jean-Luc Picard, MD",
    "objectID": "50006",
    "isPCP": true,
    "recipTemplate": "WPMessageRecipientTemplateUnknown"
  }],
  "resultDateTime": "2012-09-18T04:00:00.000Z",
  "status": "Final result",
  "allResults": [{
    "name": "albumin",
    "resultDateTime": "2012-09-18T04:00:00.000Z",
    "unit": "g/dL",
    "value": "4.84"
  }],
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}

## Encounters
Get a list of medical encounters the user has had so far
Get Encounters

Returns a list of medical encounters

GET https://api.humanapi.co/v1/human/medical/encounters

Returns a single encounter

GET https://api.humanapi.co/v1/human/medical/encounters/{id}
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication

# Get all encounters associated with the user
curl "https://api.humanapi.co/v1/human/medical/encounters?access_token=demo"

# Returns a json array of encounters:
[{
  "id": "54442e1f8bbb040d5388e842",
  "source": "emr-1-320",
  "updatedAt": "2014-10-19T21:33:19.971Z",
  "createdAt": "2014-10-19T21:33:19.971Z",
  "dateTime": "2014-07-21T17:30:00.000Z",
  "visitType": "Office Visit",
  "diagnoses": [],
  "patient": {
    "name": "Maxwell Forrest"
  },
  "provider": {
    "name": "LAB",
    "departmentName": "Asheville Goosetown Laboratory"
  },
  "reasons": ["Ear Problem"],
  "orders": [{
    "name": "LIPID PROFILE",
    "codeType": "Custom",
    "expectedDate": "2012-01-19T05:00:00.000Z",
    "expireDate": "2013-01-18T05:00:00.000Z",
    "procedureCode": "LABLIPID",
    "type": "LAB CHEMISTRY"
  }],
  "medications": [{
    "canRefill": false,
    "genericProductIndicator": "42200032301810",
    "hasPendingRefill": false,
    "name": "FLUTICASONE PROPIONATE (NASAL) 50 MCG/ACT NA SUSP",
    "ndcCode": "0054-3270-99",
    "startDate": "2010-09-17T04:00:00.000Z"
  }, {
    "canRefill": false,
    "genericProductIndicator": "01200010100315",
    "hasPendingRefill": false,
    "name": "Amoxicillin 875 MG OR TABS",
    "ndcCode": "0093-2264-01",
    "startDate": "2010-09-17T04:00:00.000Z"
  }],
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54442e2bde98810f537ba16f",
  "source": "emr-1-320",
  "updatedAt": "2014-10-19T21:33:31.112Z",
  "createdAt": "2014-10-19T21:33:31.112Z",
  "dateTime": "2011-04-04T18:15:00.000Z",
  "visitType": "Hospital Outpatient Visit",
  "diagnoses": [{
    "name": "Senile Nuclear Sclerosis"
  }, {
    "name": "Dermatochalasis"
  }],
  "patient": {
    "name": "Maxwell Forrest"
  },
  "provider": {
    "name": "Lenard Linkovich, MD",
    "departmentName": "Physiatry"
  },
  "reasons": [],
  "orders": [],
  "medications": [],
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54442e34afe02911538d01e9",
  "source": "emr-1-320",
  "updatedAt": "2014-10-19T21:33:40.086Z",
  "createdAt": "2014-10-19T21:33:40.086Z",
  "dateTime": "2013-01-25T05:00:00.000Z",
  "visitType": "Office Visit",
  "diagnoses": [{
    "name": "Unspecified Constipation"
  }, {
    "name": "Rectal Bleeding"
  }],
  "patient": {
    "name": "Maxwell Forrest"
  },
  "provider": {
    "name": "Jarred Bancroft, MD",
    "departmentName": "Dermatology"
  },
  "reasons": [],
  "orders": [{
    "name": "Multi-test Laboratory Panels",
    "codeType": "CPT(R)",
    "procedureCode": "80053",
    "type": "Lab"
  }, {
    "name": "Multi-test Laboratory Panels",
    "codeType": "CPT(R)",
    "procedureCode": "80061",
    "type": "Lab"
  }, {
    "name": "Hemoglobin A1C level",
    "codeType": "CPT(R)",
    "procedureCode": "83036",
    "type": "Lab"
  }, {
    "name": "Specimen Collection: Phlebotomy",
    "codeType": "CPT(R)",
    "procedureCode": "36415",
    "type": "Other"
  }, {
    "name": "Blood Counts",
    "codeType": "CPT(R)",
    "procedureCode": "85025",
    "type": "Lab"
  }, {
    "name": "MYCHART ACTIVATION",
    "codeType": "Custom",
    "procedureCode": "00001.000",
    "type": "Procedures"
  }, {
    "name": "Urine microalbumin (protein) level",
    "codeType": "CPT(R)",
    "procedureCode": "82043",
    "type": "Lab"
  }, {
    "name": "REFERRAL TO OPHTHALMOLOGY - GENERAL OPHTHALMOLOGY",
    "codeType": "Custom",
    "procedureCode": "9024",
    "type": "Referral"
  }, {
    "name": "Analysis for antibody to HIV-1 and HIV-2 virus",
    "codeType": "CPT(R)",
    "procedureCode": "86703",
    "type": "Lab"
  }, {
    "name": "AUTO DIFFERENTIAL",
    "codeType": "Custom",
    "procedureCode": "01001.069",
    "type": "Lab"
  }, {
    "name": "Blood creatinine level",
    "codeType": "CPT(R)",
    "procedureCode": "82565",
    "type": "Lab"
  }],
  "medications": [],
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}]
# Get an encounter by id
curl "https://api.humanapi.co/v1/human/medical/encounters/54442e34afe02911538d01e9?access_token=demo"

{
  "id": "54442e1f8bbb040d5388e842",
  "source": "emr-1-320",
  "updatedAt": "2014-10-19T21:33:19.971Z",
  "createdAt": "2014-10-19T21:33:19.971Z",
  "dateTime": "2014-07-21T17:30:00.000Z",
  "visitType": "Office Visit",
  "diagnoses": [],
  "patient": {
    "name": "Maxwell Forrest"
  },
  "provider": {
    "name": "LAB",
    "departmentName": "Asheville Goosetown Laboratory"
  },
  "reasons": ["Ear Problem"],
  "orders": [{
    "name": "LIPID PROFILE",
    "codeType": "Custom",
    "expectedDate": "2012-01-19T05:00:00.000Z",
    "expireDate": "2013-01-18T05:00:00.000Z",
    "procedureCode": "LABLIPID",
    "type": "LAB CHEMISTRY"
  }],
  "medications": [{
    "canRefill": false,
    "genericProductIndicator": "42200032301810",
    "hasPendingRefill": false,
    "name": "FLUTICASONE PROPIONATE (NASAL) 50 MCG/ACT NA SUSP",
    "ndcCode": "0054-3270-99",
    "startDate": "2010-09-17T04:00:00.000Z"
  }, {
    "canRefill": false,
    "genericProductIndicator": "01200010100315",
    "hasPendingRefill": false,
    "name": "Amoxicillin 875 MG OR TABS",
    "ndcCode": "0093-2264-01",
    "startDate": "2010-09-17T04:00:00.000Z"
  }],
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}

## Allergies
Get a list of medical allergies the user has had so far
Get Allergies

Returns a list of medical allergies

GET https://api.humanapi.co/v1/human/medical/allergies

Returns a single allergy

GET https://api.humanapi.co/v1/human/medical/allergies/{id}
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication
# Get all allergies associated with the user
curl "https://api.humanapi.co/v1/human/medical/allergies?access_token=demo"

# Returns a json array of allergies:
[{
  "id": "544323ae5b48098829dcc437",
  "source": "emr-1-320",
  "updatedAt": "2014-10-19T21:02:17.949Z",
  "createdAt": "2014-10-19T21:02:17.949Z",
  "name": "Oxycodone",
  "patient": {
    "name": "Maxwell Forrest"
  },
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "544323ae5b48098829dcc438",
  "source": "emr-1-320",
  "updatedAt": "2014-10-19T21:02:17.949Z",
  "createdAt": "2014-10-19T21:02:17.949Z",
  "name": "Metaxalone",
  "patient": {
    "name": "Maxwell Forrest"
  },
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}]

# Get an allergy by id
curl "https://api.humanapi.co/v1/human/medical/allergies/544323ae5b48098829dcc438?access_token=demo"

{
  "id": "544323ae5b48098829dcc438",
  "source": "emr-1-320",
  "updatedAt": "2014-10-19T21:02:17.949Z",
  "createdAt": "2014-10-19T21:02:17.949Z",
  "name": "Metaxalone",
  "patient": {
    "name": "Maxwell Forrest"
  },
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}
## Medications
Get Medications

Returns a list of medical medications

GET https://api.humanapi.co/v1/human/medical/medications

Returns a single medication

GET https://api.humanapi.co/v1/human/medical/medications/{id}
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication


# Get all medications associated with the user
curl "https://api.humanapi.co/v1/human/medical/medications?access_token=demo"

# Returns a json array of medications:
[{
  "id": "5443fdb22913d35d4e9db54f",
  "source": "emr-1-320",
  "updatedAt": "2014-10-19T18:06:42.100Z",
  "createdAt": "2014-10-19T18:06:42.100Z",
  "instructions": "Take 600 mg by mouth every 6 hours. take with food.",
  "genericProductIndicator": "46100010101820",
  "patient": {
    "name": "Maxwell Forrest"
  },
  "name": "oxycodone-acetaminophen (PERCOCET) 5-325 mg tab",
  "provider": "Isaias Kingston, MD",
  "providerID": "34665",
  "ndcCode": "11845-09611",
  "productName": "Ibuprofen",
  "commonBrandName": "ADVIL,MOTRIN",
  "dosageInfo": "600 mg tablet",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "5443fdb22913d35d4e9db550",
  "source": "emr-1-320",
  "updatedAt": "2014-10-19T18:06:42.105Z",
  "createdAt": "2014-10-19T18:06:42.105Z",
  "instructions": "No route applicable. as needed",
  "genericProductIndicator": "97051050146366",
  "patient": {
    "name": "Maxwell Forrest"
  },
  "name": "FERROUS SULFATE (FEOSOL ORAL)",
  "provider": "Merlin Konicek, MD",
  "providerID": "86672",
  "ndcCode": "54569-5633-0",
  "productName": "Insulin Pen Needle",
  "commonBrandName": "CRESTOR",
  "dosageInfo": "32G X 4 MM Misc",
  "pharmacy": {
    "phone": "207-674-9406",
    "name": "Misty Cider Pharmacy",
    "iD": "5792",
    "address": "687 Misty Cider Glen\nBridgewater MN 55559"
  },
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}]

# Get an medication by id
curl "https://api.humanapi.co/v1/human/medical/medications/5443fdb22913d35d4e9db550?access_token=demo"

{
  "id": "5443fdb22913d35d4e9db550",
  "source": "emr-1-320",
  "updatedAt": "2014-10-19T18:06:42.105Z",
  "createdAt": "2014-10-19T18:06:42.105Z",
  "instructions": "No route applicable. as needed",
  "genericProductIndicator": "97051050146366",
  "patient": {
    "name": "Maxwell Forrest"
  },
  "name": "FERROUS SULFATE (FEOSOL ORAL)",
  "provider": "Merlin Konicek, MD",
  "providerID": "86672",
  "ndcCode": "54569-5633-0",
  "productName": "Insulin Pen Needle",
  "commonBrandName": "CRESTOR",
  "dosageInfo": "32G X 4 MM Misc",
  "pharmacy": {
    "phone": "207-674-9406",
    "name": "Misty Cider Pharmacy",
    "iD": "5792",
    "address": "687 Misty Cider Glen\nBridgewater MN 55559"
  },
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}

##  Immunizations
Get Immunizations

Returns a list of medical immunizations

GET https://api.humanapi.co/v1/human/medical/immunizations

Returns a single immunization

GET https://api.humanapi.co/v1/human/medical/immunizations/{id}
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication
# Get all immunizations associated with the user
curl "https://api.humanapi.co/v1/human/medical/immunizations?access_token=demo"

# Returns a json array of immunizations:
[{
  "id": "5443d24d0d2a0e0c4b3672e8",
  "name": "Tetanus+Dip ADULT (Td)",
  "source": "emr-1-320",
  "dates": ["2005-09-27T04:00:00.000Z", "1995-01-01T05:00:00.000Z"],
  "patient": {
    "name": "Maxwell Forrest"
  },
  "updatedAt": "2014-10-19T21:02:17.949Z",
  "createdAt": "2014-10-19T21:02:17.949Z",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "5443d24d0d2a0e0c4b3672eb",
  "name": "Pneumococcal Conjugate PCV13",
  "source": "emr-1-320",
  "dates": ["2014-03-14T04:00:00.000Z"],
  "patient": {
    "name": "Maxwell Forrest"
  },
  "updatedAt": "2014-10-19T21:02:17.949Z",
  "createdAt": "2014-10-19T21:02:17.949Z",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}]

# Get an immunization by id
curl "https://api.humanapi.co/v1/human/medical/immunizations/5443d24d0d2a0e0c4b3672eb?access_token=demo"

{
  "id": "5443d24d0d2a0e0c4b3672eb",
  "name": "Pneumococcal Conjugate PCV13",
  "source": "emr-1-320",
  "dates": ["2014-03-14T04:00:00.000Z"],
  "patient": {
    "name": "Maxwell Forrest"
  },
  "updatedAt": "2014-10-19T21:02:17.949Z",
  "createdAt": "2014-10-19T21:02:17.949Z",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}

## Problems
Get a list of medical problems the user has had so far
Get Problems

Returns a list of medical problems

GET https://api.humanapi.co/v1/human/medical/issues

Returns a single problem

GET https://api.humanapi.co/v1/human/medical/issues/{id}
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication
# Get all problems associated with the user
curl "https://api.humanapi.co/v1/human/medical/issues?access_token=demo"

# Returns a json array of problems:
[{
  "id": "5443d6ca71b5c7f1a243879e",
  "source": "emr-1-320",
  "updatedAt": "2014-10-19T21:02:17.949Z",
  "createdAt": "2014-10-19T21:02:17.949Z",
  "name": "Limb pain",
  "patient": {
    "name": "Maxwell Forrest"
  },
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "5443d6ca71b5c7f1a243879f",
  "source": "emr-1-320",
  "updatedAt": "2014-10-19T21:02:17.949Z",
  "createdAt": "2014-10-19T21:02:17.949Z",
  "name": "Exophthalmos",
  "patient": {
    "name": "Maxwell Forrest"
  },
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}]

# Get an problem by id
curl "https://api.humanapi.co/v1/human/medical/issues/5443d6ca71b5c7f1a243879f?access_token=demo"

{
  "id": "5443d6ca71b5c7f1a243879f",
  "source": "emr-1-320",
  "updatedAt": "2014-10-19T21:02:17.949Z",
  "createdAt": "2014-10-19T21:02:17.949Z",
  "name": "Exophthalmos",
  "patient": {
    "name": "Maxwell Forrest"
  },
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}
## Profile
Get the current medical health profile of the user. Returns user’s data that most likely doesn’t change: demographics, smoking status, alcohol use, etc.
Get Profile

Returns a list of medical profile

GET https://api.humanapi.co/v1/human/medical/profile
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authenticatio
# Get user's medical profile
curl "https://api.humanapi.co/v1/human/medical/profile?access_token=demo"

# Returns a json object as profile:
{
  "id": "54e344fa79192a2e4993dfda",
  "updatedAt": "2015-02-13T21:27:42.078Z",
  "createdAt": "2015-02-13T21:27:42.078Z",
  "demographics": {
    "address": {
      "city": "Vulcun City",
      "country": "USA",
      "state": "CA",
      "street": ["1 Milky way"],
      "zip": "94025"
    },
    "ethnicity": "Non Hispanic",
    "gender": "male",
    "language": "ENG",
    "race": "White/Caucasian",
    "dob": "03-05-1985",
    "name": {
      "family": "Forrest",
      "given": ["Maxwell"]
    }
  },
  "alcohol": {
    "use": "yes"
  },
  "smoking": {
    "status": "Never smoker"
  }
}
## Timeline
Get a list of medical timeline the user has had so far
Get timeline

Returns a timeline of the user. It shows every timestamped medical object recorded into our system. The full object details are not available here but they are linked and available via own resource end points.
Current list of objects injected into the timeline output:

    encounters
    test results
    immunizations
    narratives
    vitals

For each object type we provide following set of attributes:

    id: unique internal ID ­ for all object types
    organisation: source organisation details ­ for all object types
    type: type of the object ­ for all object types
    dateTime: object timestamp ­ for all object types
    href: object URL for details retrieval ­ for all object types
    author: author(e.g. doctor) name ­ for encounters, vitals, narratives
    department: source department details ­ for encounters, vitals, narratives
    name: object name ­ for encounters, test results, immunizations

GET https://api.humanapi.co/v1/human/medical/timeline
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication
# Get all timeline associated with the user
curl "https://api.humanapi.co/v1/human/medical/timeline?access_token=demo"

# Returns a json array of timeline:
[{
  "id": "54e4bba074874bfc058cc56b",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.002Z",
  "createdAt": "2015-02-18T16:19:45.002Z",
  "type": "encounters",
  "dateTime": "Wed Sep 26 2012 18:00:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "Radiology",
  "href": "/medical/encounters/54442e3ea1d1261353c16962",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc57d",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.243Z",
  "createdAt": "2015-02-18T16:19:45.243Z",
  "type": "test_results",
  "dateTime": "Wed Sep 10 2014 04:00:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "n/a",
  "href": "/medical/test_results/546d43db8a6d23e01a728659",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc581",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.253Z",
  "createdAt": "2015-02-18T16:19:45.253Z",
  "type": "test_results",
  "dateTime": "Wed Sep 10 2014 04:00:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "LAB ASPARTATE AMINOTRANSFERASE",
  "href": "/medical/test_results/546d43508a6d23e01a728658",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc584",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.274Z",
  "createdAt": "2015-02-18T16:19:45.274Z",
  "type": "test_results",
  "dateTime": "Wed Sep 10 2014 04:00:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "Hemoglobin",
  "href": "/medical/test_results/546d45838a6d23e01a72865c",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba274874bfc058cc68f",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:46.080Z",
  "createdAt": "2015-02-18T16:19:46.080Z",
  "type": "immunizations",
  "dateTime": "Wed Sep 07 2011 04:00:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "Influenza vaccine 3+ YEARS",
  "href": "/medical/immunizations/5443d24d0d2a0e0c4b3672ec",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc572",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.211Z",
  "createdAt": "2015-02-18T16:19:45.211Z",
  "type": "test_results",
  "dateTime": "Wed Oct 08 2014 04:00:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "Multi-test Laboratory Panels",
  "href": "/medical/test_results/546d376b8a6d23e01a72864a",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc58a",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.289Z",
  "createdAt": "2015-02-18T16:19:45.289Z",
  "type": "test_results",
  "dateTime": "Wed Nov 05 2014 04:00:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "Complete Blood Count",
  "href": "/medical/test_results/546d326c8a6d23e01a728648",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc58c",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.294Z",
  "createdAt": "2015-02-18T16:19:45.294Z",
  "type": "test_results",
  "dateTime": "Wed Jan 10 2007 23:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "CBC W/AUTO DIFF",
  "href": "/medical/test_results/547685b8412813fc27974809",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc594",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.322Z",
  "createdAt": "2015-02-18T16:19:45.322Z",
  "type": "test_results",
  "dateTime": "Wed Jan 10 2007 23:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "US Abdomen Retroperitoneal",
  "href": "/medical/test_results/547685b8412813fc27974811",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc59c",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.355Z",
  "createdAt": "2015-02-18T16:19:45.355Z",
  "type": "test_results",
  "dateTime": "Wed Jan 10 2007 23:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "DUPLEX ABDOMEN/PELVIS/RETROPERITONEAL LIMITED",
  "href": "/medical/test_results/547685b8412813fc27974819",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc5a4",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.373Z",
  "createdAt": "2015-02-18T16:19:45.373Z",
  "type": "test_results",
  "dateTime": "Wed Jan 10 2007 23:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "CBC BATTERY",
  "href": "/medical/test_results/547685b8412813fc27974821",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc5ac",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.393Z",
  "createdAt": "2015-02-18T16:19:45.393Z",
  "type": "test_results",
  "dateTime": "Wed Jan 10 2007 23:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "EMG(NEURO/NI)",
  "href": "/medical/test_results/547685b8412813fc27974829",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc5b4",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.424Z",
  "createdAt": "2015-02-18T16:19:45.424Z",
  "type": "test_results",
  "dateTime": "Wed Jan 10 2007 23:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "COMPLETE BLOOD COUNT",
  "href": "/medical/test_results/547685b8412813fc27974831",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc5bc",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.440Z",
  "createdAt": "2015-02-18T16:19:45.440Z",
  "type": "test_results",
  "dateTime": "Wed Jan 10 2007 23:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "Multi-test Laboratory Panels",
  "href": "/medical/test_results/547685b8412813fc27974839",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc5c4",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.459Z",
  "createdAt": "2015-02-18T16:19:45.459Z",
  "type": "test_results",
  "dateTime": "Wed Jan 10 2007 23:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "MICROALBUMIN/CREAT URINE RATIO",
  "href": "/medical/test_results/547685b8412813fc27974841",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc5cc",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.491Z",
  "createdAt": "2015-02-18T16:19:45.491Z",
  "type": "test_results",
  "dateTime": "Wed Jan 10 2007 23:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "GLUCOSE RANDOM",
  "href": "/medical/test_results/547685b8412813fc27974849",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc5d4",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.511Z",
  "createdAt": "2015-02-18T16:19:45.511Z",
  "type": "test_results",
  "dateTime": "Wed Jan 10 2007 23:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "GLUCOSE, RANDOM",
  "href": "/medical/test_results/547685b8412813fc27974851",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc5dc",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.532Z",
  "createdAt": "2015-02-18T16:19:45.532Z",
  "type": "test_results",
  "dateTime": "Wed Jan 10 2007 23:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "LAB GLUCOSE",
  "href": "/medical/test_results/547685b8412813fc27974859",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc5e4",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.557Z",
  "createdAt": "2015-02-18T16:19:45.557Z",
  "type": "test_results",
  "dateTime": "Wed Jan 10 2007 23:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "Blood test, thyroid stimulating hormone (TSH)",
  "href": "/medical/test_results/547685b8412813fc27974861",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc5ec",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.586Z",
  "createdAt": "2015-02-18T16:19:45.586Z",
  "type": "test_results",
  "dateTime": "Wed Jan 10 2007 23:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "THYROID PROFILE",
  "href": "/medical/test_results/547685b8412813fc27974869",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc5f4",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.608Z",
  "createdAt": "2015-02-18T16:19:45.608Z",
  "type": "test_results",
  "dateTime": "Wed Jan 10 2007 23:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "BILIRUBIN TOTAL",
  "href": "/medical/test_results/547685b8412813fc27974871",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc5fc",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.631Z",
  "createdAt": "2015-02-18T16:19:45.631Z",
  "type": "test_results",
  "dateTime": "Wed Jan 10 2007 23:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "BILIRUBIN, DIRECT",
  "href": "/medical/test_results/547685b8412813fc27974879",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc604",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.661Z",
  "createdAt": "2015-02-18T16:19:45.661Z",
  "type": "test_results",
  "dateTime": "Wed Jan 10 2007 23:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "ALBUMIN, SERUM / PLASMA",
  "href": "/medical/test_results/547685b8412813fc27974881",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc60c",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.685Z",
  "createdAt": "2015-02-18T16:19:45.685Z",
  "type": "test_results",
  "dateTime": "Wed Jan 10 2007 23:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "ALBUMIN",
  "href": "/medical/test_results/547685b8412813fc27974889",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc614",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.707Z",
  "createdAt": "2015-02-18T16:19:45.707Z",
  "type": "test_results",
  "dateTime": "Wed Jan 10 2007 23:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "n/a",
  "href": "/medical/test_results/547685b8412813fc27974891",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc61c",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.729Z",
  "createdAt": "2015-02-18T16:19:45.729Z",
  "type": "test_results",
  "dateTime": "Wed Jan 10 2007 23:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "LAB ALANINE AMINOTRANSFERASE",
  "href": "/medical/test_results/547685b8412813fc27974899",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc624",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.755Z",
  "createdAt": "2015-02-18T16:19:45.755Z",
  "type": "test_results",
  "dateTime": "Wed Jan 10 2007 23:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "ALANINE AMINOTRANSFERASE, SERUM",
  "href": "/medical/test_results/547685b8412813fc279748a1",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc62c",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.778Z",
  "createdAt": "2015-02-18T16:19:45.778Z",
  "type": "test_results",
  "dateTime": "Wed Jan 10 2007 23:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "ASPARTATE AMINOTRANSFERASE, SERUM",
  "href": "/medical/test_results/547685b8412813fc279748a9",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc634",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.801Z",
  "createdAt": "2015-02-18T16:19:45.801Z",
  "type": "test_results",
  "dateTime": "Wed Jan 10 2007 23:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "LAB ASPARTATE AMINOTRANSFERASE",
  "href": "/medical/test_results/547685b8412813fc279748b1",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc63c",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.826Z",
  "createdAt": "2015-02-18T16:19:45.826Z",
  "type": "test_results",
  "dateTime": "Wed Jan 10 2007 23:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "LAB TOTAL PROTEIN+ALB",
  "href": "/medical/test_results/547685b8412813fc279748b9",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc644",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.849Z",
  "createdAt": "2015-02-18T16:19:45.849Z",
  "type": "test_results",
  "dateTime": "Wed Jan 10 2007 23:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "COMPREHENSIVE METABOLIC PANEL",
  "href": "/medical/test_results/547685b8412813fc279748c1",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc64c",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.873Z",
  "createdAt": "2015-02-18T16:19:45.873Z",
  "type": "test_results",
  "dateTime": "Wed Jan 10 2007 23:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "Hemoglobin",
  "href": "/medical/test_results/547685b8412813fc279748c9",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc654",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.896Z",
  "createdAt": "2015-02-18T16:19:45.896Z",
  "type": "test_results",
  "dateTime": "Wed Jan 10 2007 23:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "Hemoglobin",
  "href": "/medical/test_results/547685b8412813fc279748d1",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc65c",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.920Z",
  "createdAt": "2015-02-18T16:19:45.920Z",
  "type": "test_results",
  "dateTime": "Wed Jan 10 2007 23:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "HEMOGLOBIN A1C PANEL",
  "href": "/medical/test_results/547685b8412813fc279748d9",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc664",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.942Z",
  "createdAt": "2015-02-18T16:19:45.942Z",
  "type": "test_results",
  "dateTime": "Wed Jan 10 2007 23:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "HEMOGLOBIN A1C PANEL",
  "href": "/medical/test_results/547685b8412813fc279748e1",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc66c",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.965Z",
  "createdAt": "2015-02-18T16:19:45.965Z",
  "type": "test_results",
  "dateTime": "Wed Jan 10 2007 23:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "COMPLETE BLOOD COUNT",
  "href": "/medical/test_results/547685b8412813fc279748e9",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc674",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.989Z",
  "createdAt": "2015-02-18T16:19:45.989Z",
  "type": "test_results",
  "dateTime": "Wed Jan 10 2007 23:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "Complete Blood Count",
  "href": "/medical/test_results/547685b8412813fc279748f1",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba274874bfc058cc67c",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:46.015Z",
  "createdAt": "2015-02-18T16:19:46.015Z",
  "type": "test_results",
  "dateTime": "Wed Jan 10 2007 23:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "Complete Blood Count",
  "href": "/medical/test_results/547685b8412813fc279748f9",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba274874bfc058cc684",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:46.038Z",
  "createdAt": "2015-02-18T16:19:46.038Z",
  "type": "test_results",
  "dateTime": "Wed Jan 10 2007 23:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "LAB COMPLETE BLOOD COUNT W/DIFF",
  "href": "/medical/test_results/547685b8412813fc27974901",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc593",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.319Z",
  "createdAt": "2015-02-18T16:19:45.319Z",
  "type": "test_results",
  "dateTime": "Wed Apr 02 2014 01:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "CBC W/AUTO DIFF",
  "href": "/medical/test_results/547685b8412813fc27974810",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc59b",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.353Z",
  "createdAt": "2015-02-18T16:19:45.353Z",
  "type": "test_results",
  "dateTime": "Wed Apr 02 2014 01:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "US Abdomen Retroperitoneal",
  "href": "/medical/test_results/547685b8412813fc27974818",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc5a3",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.370Z",
  "createdAt": "2015-02-18T16:19:45.370Z",
  "type": "test_results",
  "dateTime": "Wed Apr 02 2014 01:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "DUPLEX ABDOMEN/PELVIS/RETROPERITONEAL LIMITED",
  "href": "/medical/test_results/547685b8412813fc27974820",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc5ab",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.391Z",
  "createdAt": "2015-02-18T16:19:45.391Z",
  "type": "test_results",
  "dateTime": "Wed Apr 02 2014 01:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "CBC BATTERY",
  "href": "/medical/test_results/547685b8412813fc27974828",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc5b3",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.421Z",
  "createdAt": "2015-02-18T16:19:45.421Z",
  "type": "test_results",
  "dateTime": "Wed Apr 02 2014 01:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "EMG(NEURO/NI)",
  "href": "/medical/test_results/547685b8412813fc27974830",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc5bb",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.438Z",
  "createdAt": "2015-02-18T16:19:45.438Z",
  "type": "test_results",
  "dateTime": "Wed Apr 02 2014 01:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "COMPLETE BLOOD COUNT",
  "href": "/medical/test_results/547685b8412813fc27974838",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc5c3",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.457Z",
  "createdAt": "2015-02-18T16:19:45.457Z",
  "type": "test_results",
  "dateTime": "Wed Apr 02 2014 01:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "Multi-test Laboratory Panels",
  "href": "/medical/test_results/547685b8412813fc27974840",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc5cb",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.489Z",
  "createdAt": "2015-02-18T16:19:45.489Z",
  "type": "test_results",
  "dateTime": "Wed Apr 02 2014 01:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "MICROALBUMIN/CREAT URINE RATIO",
  "href": "/medical/test_results/547685b8412813fc27974848",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc5d3",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.508Z",
  "createdAt": "2015-02-18T16:19:45.508Z",
  "type": "test_results",
  "dateTime": "Wed Apr 02 2014 01:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "GLUCOSE RANDOM",
  "href": "/medical/test_results/547685b8412813fc27974850",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc5db",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.529Z",
  "createdAt": "2015-02-18T16:19:45.529Z",
  "type": "test_results",
  "dateTime": "Wed Apr 02 2014 01:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "GLUCOSE, RANDOM",
  "href": "/medical/test_results/547685b8412813fc27974858",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}, {
  "id": "54e4bba174874bfc058cc5e3",
  "source": "emr-1-320",
  "updatedAt": "2015-02-18T16:19:45.552Z",
  "createdAt": "2015-02-18T16:19:45.552Z",
  "type": "test_results",
  "dateTime": "Wed Apr 02 2014 01:51:00 GMT+0000 (UTC)",
  "author": "n/a",
  "name": "LAB GLUCOSE",
  "href": "/medical/test_results/547685b8412813fc27974860",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}]


## CCD
Returns full CCDs available for the user including the raw xml.
Get CCDS

Returns a list of CCDs availabe for a user

GET https://api.humanapi.co/v1/human/medical/ccds

Returns single CCD raw payload in XML format

GET https://api.humanapi.co/v1/human/medical/ccds/{id}/raw

chrome-or-firefox-click-here-for-full-xml
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication
Response Headers:
Header 	Value
Content-Type 	text/xml
# Get all ccds associated with the user
curl "https://api.humanapi.co/v1/human/medical/ccds?access_token=demo"

# Returns a json array of ccds:
[{
  "id": "54e35f5de0b3d53af2c65921",
  "source": "emr-1-320",
  "updatedAt": "2015-02-17T16:54:24.778Z",
  "createdAt": "2015-02-10T00:09:36.679Z",
  "dateTime": "Mon Apr 14 2014 11:38:44 GMT-0700 (PDT)",
  "author": "John Smith",
  "organization": {
    "id": "53c050ac51c69003200aa998",
    "name": "Cleveland Clinic",
    "href": "/medical/organizations/53c050ac51c69003200aa998"
  }
}]

# Get an ccd by id raw (xml payload)
curl "https://api.humanapi.co/v1/human/medical/ccds/54e35f5de0b3d53af2c65921/raw?access_token=demo"

# Respond with a CCD xml:
<?xml version="1.0" encoding="UTF-8"?>
<ClinicalDocument>
  <realmCode code="US"/>
  <typeId extension="POCD_HD000040" root="2.16.840.1.113883.1.3"/>
  <templateId root="1.2.840.114350.1.72.1.51693"/>
  <templateId root="2.16.840.1.113883.10.20.22.1.1"/>
  <templateId root="2.16.840.1.113883.10.20.22.1.2"/>
  <id assigningAuthorityName="EPC" root="1.2.840.114350.1.13.76.2.7.8.688883.60780139"/>
  <code code="34133-9" codeSystem="2.16.840.1.113883.6.1" codeSystemName="LOINC" displayName="Summarization of Episode Note"/>
  <title>Continuity of Care Document</title>
  ...
  ...
</ClinicalDocument>

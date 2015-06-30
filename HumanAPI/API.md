https://reference.humanapi.co/#introduction

api接口总则

Patterns and Conventions

This page details the few simple conventions to follow when working with Human API.
Example Queries

Human API lets you query a user's health data with an access_token query parameter or a Bearer token authorization header. For details on how to authorize users and retrieve these tokens please click here.

Query Parameter:

curl https://api.humanapi.co/v1/human?access_token=demo

Bearer token:

curl -H "Authorization: Bearer demo" https://api.humanapi.co/v1/human

The demo access token will serve demo user data. Replace it with a valid accessToken to get data for that specific user.
Media Types

Human API uses JSON (JavaScript Object Notation) for all payloads. To accommodate for this:

    Set the Accept Header to application/json when querying data.
    Set the Content-Type Header to application/json when sending data.

API Versions

Different versions of Human API can be used by specifying the version number in the url, i.e./v1:

https://api.humanapi.co/[version]/human
https://api.humanapi.co/v1/human

The most current version of Human API is v1
Query Options

The standard query options for Human API are as described in the table below. These parameters make it easier for you to implement smart synchronization for your application. For instance, by using the updated_since parameter you only retrieve the data that was changed or added since the specified time.

These are the available query options in Human API:
Parameter 	Description

limit


The max number of records to return in one query.
Default is 20

offset


The index of the first record to return. Default is 0

created_since


Returns records that are created after the specified date.

updated_since


Returns records that are updated after the specified date.

created_before


Returns records that are created after the specified date.

updated_before


Returns records that are updated after the specified date.

source


Returns filtered data from a specific source.

All dates are in UTC date format: YYYYMMDDTHHmmssZ
Data Types
Discrete Measurements

A discrete measurement is a unit of data that is captured at a point in time and has a timestamp, a value, and a type. A measurement can also have an optional location or other meta data. To retrieve the discrete measurements you can follow the following pattern:

To retrieve the latest reading:

/v1/human/[type]

To retrieve a reading by ID:

/v1/human/[type]/{id}

To retrieve the list of readings:

/v1/human/[type]/readings

To retrieve a list of readings for a specific date:

/v1/human/[type]/readings/{YYYY-MM-DD}

You can also query by date range(s) with the following parameters:
Parameter 	Description

start_date


The oldest date to retrieve.
Format YYYY-MM-DD

end_date


The most recent date to retrieve.
Format YYYY-MM-DD
Periodic Data

Periodic data, unlike discrete data, always has a start and end date. The data types that fall into this category are activities, sleeps, and locations. This type of data can be retrieved as a list of objects or as a summary object for a specific date or time period.

To retrieve a list of periodic data objects:

/v1/human/[type]

To retrieve a single object by ID:

/v1/human/[type]/{id}

To retrieve a list of objects for a specific date:

/v1/human/[type]/daily/{YYYY-MM-DD}

To retrieve the latest summary objects:

/v1/human/[type]/summaries

To retrieve a summary object for a specific date:

/v1/human/[type]/summaries/{YYYY-MM-DD}

To retrieve a summary object by Id:

/v1/human/[type]/summaries/{id}

You can also query by date range(s) with the following parameters:
Parameter 	Description

start_date


The oldest date to retrieve.
Format YYYY-MM-DD

end_date


The most recent date to retrieve.
Format YYYY-MM-DD
Pagination Conventions

To make it easier to retrieve all the data for a user you can iterate through their historical data using the Link Header and, optionally, the X-Total-Count Header. The Link Header contains the first, prev, next, and last links that will help you easily iterate through all the records available for a query. The Link Header follows the RFC5988 standard.

For example, if you query a user with the following parameters:

/activities?start_date=2013-01-01&limit=50&offset=100

Your response headers would return like this:

Link:   https://api.humanapi.co/v1/human/activities?&limit=50&start_date=2013-01-01&offset=50;
rel="prev",
   https://api.humanapi.co/v1/human/activities?limit=50&start_date=2013-01-01&offset=8450;
rel="last",
   https://api.humanapi.co/v1/human/activities?limit=50&start_date=2013-01-01&offset=0;
rel="first"

There is also a total count for the query stored in the X-Total-Count Header:

X-Total-Count: 8490




# Introduction

    # An example Human API request...
    curl "https://api.humanapi.co/v1/human/medical/profile?access_token=demo"

    # which would return a json payload:

    {
     "demographics": {
      "name": {
       "given": ["Maxwell"],
       "family": "Forrest"
      },
      "dob": "03-05-1985",
      "race": "White/Caucasian",
      "language": "ENG",
      "gender": "male",
      "ethnicity": "Non Hispanic",
      "address": {
       "zip": "94025",
       "street": ["1 Milky way"],
       "state": "CA",
       "country": "USA",
       "city": "Vulcun City"
      }
     },
     "smoking": {
      "status": "never smoker"
     },
     "alcohol": {
      "use": "yes"
     }
    }

    // An example Human API request in nodejs...
    var request = require('request');
    request
    .get('https://api.humanapi.co/v1/human/medical/profile?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data))
    });

    // which would return a json payload:

    {
     "demographics": {
      "name": {
       "given": ["Maxwell"],
       "family": "Forrest"
      },
      "dob": "03-05-1985",
      "race": "White/Caucasian",
      "language": "ENG",
      "gender": "male",
      "ethnicity": "Non Hispanic",
      "address": {
       "zip": "94025",
       "street": ["1 Milky way"],
       "state": "CA",
       "country": "USA",
       "city": "Vulcun City"
      }
     },
     "smoking": {
      "status": "never smoker"
     },
     "alcohol": {
      "use": "yes"
     }
    }

Welcome to Human API!

You can use Human API to retrieve normalized health data from your app’s users from thousands of unique sources.

Before you get started, you will need to:

1.  [Create a developer account](https://developer.humanapi.co/signup)
2.  [Set up an application & get the associated keys](http://hub.humanapi.co/v1.0/docs/start-here)
3.  [Integrate the Human Connect authentication flow](http://hub.humanapi.co/v1.0/docs/integrating-human-connect)
4.  [Set up your data retrieval infrastructure](http://hub.humanapi.co/v1.0/docs/data-overview)

Once you’ve completed the above steps, your users can start authenticating their data sources and you can start querying your user’s health data via the endpoints in this reference guide.

For the full documentation including step-by-step implementation guides and information on data sources, see [http://hub.humanapi.co](http://hub.humanapi.co)

# Medical APIs

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

    // Get all organizations associated with the user
    var request = require('request');
    request
    .get('https://api.humanapi.co/v1/human/medical/organizations?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json array of organizations:
    [{
      "id": "53c050ac51c69003200aa9b5",
      "name": "Genesis Health System",
      "imageUrl": "Genesis.PNG"
    }, {
      "id": "53c050ae51c69003200aaa29",
      "name": "Singing River Health System",
      "imageUrl": "Singing_River_Health_System.png"
    }]

    // Get an organization by id
    request
    .get('https://api.humanapi.co/v1/human/medical/organizations/53c050ae51c69003200aaa29?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json object as organization:
    {
      "id": "53c050ae51c69003200aaa29",
      "name": "Singing River Health System",
      "imageUrl": "Singing_River_Health_System.png"
    }

Get a list of medical organizations in Human API associated with the user

### Get Organizations

Returns a list of medical organizations

`GET https://api.humanapi.co/v1/human/medical/organizations`  

Returns a single organization

`GET https://api.humanapi.co/v1/human/medical/organizations/{id}`

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

</tbody>

</table>

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

    // Get all vitals associated with the user
    var request = require('request');
    request
    .get('https://api.humanapi.co/v1/human/medical/vitals?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json array of vitals:
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

    // Get an vitals reading by id
    request
    .get('https://api.humanapi.co/v1/human/medical/vitals/54e4c0d3303d780900c9ea4b?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json object as vitals reading:
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

### Get Vitals

Returns a list of vitals readings

`GET https://api.humanapi.co/v1/human/medical/vitals`  

Returns a single organization

`GET https://api.humanapi.co/v1/human/medical/vitals/{id}`

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

</tbody>

</table>

## Narratives

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

    // Get all narratives associated with the user
    var request = require('request');
    request
    .get('https://api.humanapi.co/v1/human/medical/narratives?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json array of narratives:
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

    // Get an narrative by id
    request
    .get('https://api.humanapi.co/v1/human/medical/narratives/54e3521f79192a2e4993dfdc?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json object as narrative:
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

Get a list of medical narratives associated with the user

### Get Narratives

Returns a list of medical narratives

`GET https://api.humanapi.co/v1/human/medical/narratives`  

Returns a single narrative

`GET https://api.humanapi.co/v1/human/medical/narratives/{id}`

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

</tbody>

</table>

## Test Results

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

    // Get all test_results associated with the user
    var request = require('request');
    request
    .get('https://api.humanapi.co/v1/human/medical/test_results?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json array of test_results:
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

    // Get an test results by id
    request
    .get('https://api.humanapi.co/v1/human/medical/test_results/546d41c68a6d23e01a728655?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json object as test results:
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

Get a list of medical lab’s results from a user’s medical record

### Get Test Results

Returns a list of medical test_results

`GET https://api.humanapi.co/v1/human/medical/test_results`  

Returns a single test results

`GET https://api.humanapi.co/v1/human/medical/test_results/{id}`

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

</tbody>

</table>

## Encounters

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

    // Get all encounters associated with the user
    var request = require('request');
    request
    .get('https://api.humanapi.co/v1/human/medical/encounters?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json array of encounters:
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

    // Get an encounter by id
    request
    .get('https://api.humanapi.co/v1/human/medical/encounters/54442e1f8bbb040d5388e842?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json object as encounter:
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

Get a list of medical encounters the user has had so far

### Get Encounters

Returns a list of medical encounters

`GET https://api.humanapi.co/v1/human/medical/encounters`  

Returns a single encounter

`GET https://api.humanapi.co/v1/human/medical/encounters/{id}`

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

</tbody>

</table>

## Allergies

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

    // Get all allergies associated with the user
    var request = require('request');
    request
    .get('https://api.humanapi.co/v1/human/medical/allergies?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json array of allergies:
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

    // Get an allergy by id
    request
    .get('https://api.humanapi.co/v1/human/medical/allergies/544323ae5b48098829dcc438?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json object as allergy:
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

Get a list of medical allergies the user has had so far

### Get Allergies

Returns a list of medical allergies

`GET https://api.humanapi.co/v1/human/medical/allergies`  

Returns a single allergy

`GET https://api.humanapi.co/v1/human/medical/allergies/{id}`

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

</tbody>

</table>

## Medications

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

    // Get all medications associated with the user
    var request = require('request');
    request
    .get('https://api.humanapi.co/v1/human/medical/medications?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json array of medications:
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

    // Get an medication by id
    request
    .get('https://api.humanapi.co/v1/human/medical/medications/5443fdb22913d35d4e9db550?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json object as medication:
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

Get a list of medical medications the user has had so far

### Get Medications

Returns a list of medical medications

`GET https://api.humanapi.co/v1/human/medical/medications`  

Returns a single medication

`GET https://api.humanapi.co/v1/human/medical/medications/{id}`

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

</tbody>

</table>

## Immunizations

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

    // Get all immunizations associated with the user
    var request = require('request');
    request
    .get('https://api.humanapi.co/v1/human/medical/immunizations?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json array of immunizations:
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

    // Get an immunization by id
    request
    .get('https://api.humanapi.co/v1/human/medical/immunizations/5443d24d0d2a0e0c4b3672eb?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json object as immunization:
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

Get a list of immunizations the user has had so far

### Get Immunizations

Returns a list of medical immunizations

`GET https://api.humanapi.co/v1/human/medical/immunizations`  

Returns a single immunization

`GET https://api.humanapi.co/v1/human/medical/immunizations/{id}`

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

</tbody>

</table>

## Problems

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

    // Get all problems associated with the user
    var request = require('request');
    request
    .get('https://api.humanapi.co/v1/human/medical/issues?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json array of problems:
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

    // Get an problem by id
    request
    .get('https://api.humanapi.co/v1/human/medical/issues/5443d6ca71b5c7f1a243879f?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json object as problem:
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

Get a list of medical problems the user has had so far

### Get Problems

Returns a list of medical problems

`GET https://api.humanapi.co/v1/human/medical/issues`  

Returns a single problem

`GET https://api.humanapi.co/v1/human/medical/issues/{id}`

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

</tbody>

</table>

## Profile

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

    // Get all profile associated with the user
    var request = require('request');
    request
    .get('https://api.humanapi.co/v1/human/medical/profile?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json object as profile:
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

Get the current medical health profile of the user. Returns user’s data that most likely doesn’t change: demographics, smoking status, alcohol use, etc.

### Get Profile

Returns a list of medical profile

`GET https://api.humanapi.co/v1/human/medical/profile`  

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

</tbody>

</table>

## Timeline

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

    // Get all timeline associated with the user
    var request = require('request');
    request
    .get('https://api.humanapi.co/v1/human/medical/timeline?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json array of timeline:
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

Get a list of medical timeline the user has had so far

### Get timeline

Returns a timeline of the user. It shows every timestamped medical object recorded into our system. The full object details are not available here but they are linked and available via own resource end points.

#### Current list of objects injected into the timeline output:

*   encounters
*   test results
*   immunizations
*   narratives
*   vitals

#### For each object type we provide following set of attributes:

*   id: unique internal ID ­ for all object types
*   organisation: source organisation details ­ for all object types
*   type: type of the object ­ for all object types
*   dateTime: object timestamp ­ for all object types
*   href: object URL for details retrieval ­ for all object types
*   author: author(e.g. doctor) name ­ for encounters, vitals, narratives
*   department: source department details ­ for encounters, vitals, narratives
*   name: object name ­ for encounters, test results, immunizations

`GET https://api.humanapi.co/v1/human/medical/timeline`  

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

</tbody>

</table>

## CCD

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

    // Get all ccds associated with the user
    var request = require('request');
    request
    .get('https://api.humanapi.co/v1/human/medical/ccds?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json array of ccds:
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

    // Get an ccd by id raw (xml payload)
    request
    .get('https://api.humanapi.co/v1/human/medical/ccds/54e35f5de0b3d53af2c65921/raw?access_token=demo')
    .on('data', function(data) {
      // Will be string xml data
      console.log(String(data));
    });

    // Respond with a CCD xml:
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

Returns full [CCDs](http://en.wikipedia.org/wiki/Continuity_of_Care_Document) available for the user including the raw xml.

### Get CCDS

Returns a list of CCDs availabe for a user

`GET https://api.humanapi.co/v1/human/medical/ccds`

Returns single CCD raw payload in XML format

`GET https://api.humanapi.co/v1/human/medical/ccds/{id}/raw`

[chrome-or-firefox-click-here-for-full-xml](view-source:https://api.humanapi.co/v1/human/medical/ccds/54e35f5de0b3d53af2c65921/raw?access_token=demo)

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

</tbody>

</table>

### Response Headers:

<table>

<thead>

<tr>

<th>Header</th>

<th>Value</th>

</tr>

</thead>

<tbody>

<tr>

<td>Content-Type</td>

<td>text/xml</td>

</tr>

</tbody>

</table>

# Wellness APIs

## Activities

    # Get all activities associated with the user
    curl "https://api.humanapi.co/v1/human/activities?access_token=demo"

    OR

    curl "https://api.humanapi.co/v1/human/activities?access_token=demo&updated_since=20140110T000000Z"

    # Returns a json array of activities:
    [{
      "id": "5510259685cc310900f67753",
      "userId": "52e20cb2fff56aac62000001",
      "startTime": "2015-03-23T14:29:34.000Z",
      "endTime": "2015-03-23T14:30:17.000Z",
      "tzOffset": "-04:00",
      "type": "walking",
      "source": "moves",
      "duration": 43,
      "distance": 29,
      "steps": 57,
      "calories": 0,
      "sourceData": {
        "manual": false
      },
      "createdAt": "2015-03-23T14:39:19.065Z",
      "updatedAt": "2015-03-23T14:39:19.065Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }, {
      "id": "550fe4b58c34cc6661f46f66",
      "userId": "52e20cb2fff56aac62000001",
      "startTime": "2015-03-23T08:19:24.000Z",
      "endTime": "2015-03-23T08:31:26.000Z",
      "tzOffset": "+00:00",
      "type": "running",
      "source": "runkeeper",
      "duration": 722,
      "distance": 1506.71611260169,
      "steps": 0,
      "calories": 118,
      "sourceData": {},
      "createdAt": "2015-03-23T14:32:27.978Z",
      "updatedAt": "2015-03-23T14:32:27.978Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }, {
      "id": "550fbaa568d29c4133b6531a",
      "userId": "52e20cb2fff56aac62000001",
      "startTime": "2015-03-23T00:01:00.000Z",
      "endTime": "2015-03-23T06:32:00.000Z",
      "tzOffset": "-07:00",
      "type": "unknown",
      "source": "jawbone",
      "duration": 480,
      "distance": 669,
      "steps": 949,
      "calories": 25.2287654541,
      "sourceData": {},
      "createdAt": "2015-03-23T07:03:01.454Z",
      "updatedAt": "2015-03-23T13:36:29.756Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }]
    # Get an activity by id
    curl "https://api.humanapi.co/v1/human/activities/52e20cb2fff56aac62000001?access_token=demo"

    # Returns a json object as activity:
    {
      "id": "550fbaa568d29c4133b6531a",
      "userId": "52e20cb2fff56aac62000001",
      "startTime": "2015-03-23T00:01:00.000Z",
      "endTime": "2015-03-23T06:32:00.000Z",
      "tzOffset": "-07:00",
      "type": "unknown",
      "source": "jawbone",
      "duration": 480,
      "distance": 669,
      "steps": 949,
      "calories": 25.2287654541,
      "sourceData": {},
      "createdAt": "2015-03-23T07:03:01.454Z",
      "updatedAt": "2015-03-23T13:36:29.756Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }

    // Get all activities associated with the user
    var request = require('request');
    request
    .get('https://api.humanapi.co/v1/human/activities?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    OR

    request
    .get('https://api.humanapi.co/v1/human/activities?access_token=demo&updated_since=20140110T000000Z')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json array of activities:
    [{
      "id": "5510259685cc310900f67753",
      "userId": "52e20cb2fff56aac62000001",
      "startTime": "2015-03-23T14:29:34.000Z",
      "endTime": "2015-03-23T14:30:17.000Z",
      "tzOffset": "-04:00",
      "type": "walking",
      "source": "moves",
      "duration": 43,
      "distance": 29,
      "steps": 57,
      "calories": 0,
      "sourceData": {
        "manual": false
      },
      "createdAt": "2015-03-23T14:39:19.065Z",
      "updatedAt": "2015-03-23T14:39:19.065Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }, {
      "id": "550fe4b58c34cc6661f46f66",
      "userId": "52e20cb2fff56aac62000001",
      "startTime": "2015-03-23T08:19:24.000Z",
      "endTime": "2015-03-23T08:31:26.000Z",
      "tzOffset": "+00:00",
      "type": "running",
      "source": "runkeeper",
      "duration": 722,
      "distance": 1506.71611260169,
      "steps": 0,
      "calories": 118,
      "sourceData": {},
      "createdAt": "2015-03-23T14:32:27.978Z",
      "updatedAt": "2015-03-23T14:32:27.978Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }, {
      "id": "550fbaa568d29c4133b6531a",
      "userId": "52e20cb2fff56aac62000001",
      "startTime": "2015-03-23T00:01:00.000Z",
      "endTime": "2015-03-23T06:32:00.000Z",
      "tzOffset": "-07:00",
      "type": "unknown",
      "source": "jawbone",
      "duration": 480,
      "distance": 669,
      "steps": 949,
      "calories": 25.2287654541,
      "sourceData": {},
      "createdAt": "2015-03-23T07:03:01.454Z",
      "updatedAt": "2015-03-23T13:36:29.756Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }]

    // Get an activity by id
    request
    .get('https://api.humanapi.co/v1/human/activities/550ff8f985cc310900ea927d?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json object as activity:
    {
      "id": "550fbaa568d29c4133b6531a",
      "userId": "52e20cb2fff56aac62000001",
      "startTime": "2015-03-23T00:01:00.000Z",
      "endTime": "2015-03-23T06:32:00.000Z",
      "tzOffset": "-07:00",
      "type": "unknown",
      "source": "jawbone",
      "duration": 480,
      "distance": 669,
      "steps": 949,
      "calories": 25.2287654541,
      "sourceData": {},
      "createdAt": "2015-03-23T07:03:01.454Z",
      "updatedAt": "2015-03-23T13:36:29.756Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }

Activities are specific types of physical activities that occur during a period of time with a start time and an end time. These activities are often captured automatically using applications that track motion throughout the day, such as [Moves](http://moves-app.com/), or they are recorded from applications such as [Runkeeper](http://runkeeper.com/) or [MapMyRun](http://www.mapmyrun.com/). They can also be manually entered with applications like [Fitbit](http://www.fitbit.com/).

### Get Activities

Get a list of activities the user has

`GET https://api.humanapi.co/v1/human/activities`  

Returns a single activity

`GET https://api.humanapi.co/v1/human/activities/{id}`

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

</tbody>

</table>

Returns a list of activities updated since a given time

`GET https://api.humanapi.co/v1/human/activities`  

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

<tr>

<td>updated_since</td>

<td>Date in YYYYMMDDTHHmmssZ format</td>

</tr>

</tbody>

</table>

### Response properties

<table>

<thead>

<tr>

<th>Property</th>

<th>Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>id</td>

<td>String</td>

<td>The Id of the resource</td>

</tr>

<tr>

<td>userId</td>

<td>String</td>

<td>The global Id of the user</td>

</tr>

<tr>

<td>humanId</td>

<td>String</td>

<td>Unique user identifier</td>

</tr>

<tr>

<td>startTime</td>

<td>Date</td>

<td>The start time of the activity in UTC time</td>

</tr>

<tr>

<td>endTime</td>

<td>Date</td>

<td>The end time of the activity in UTC time</td>

</tr>

<tr>

<td>tzOffset</td>

<td>String</td>

<td>The offset from UTC time in +/-hh:mm (e.g. -04:00)</td>

</tr>

<tr>

<td>type</td>

<td>String</td>

<td>The type of activity, such as walking, running, cycling</td>

</tr>

<tr>

<td>source</td>

<td>String</td>

<td>The name of the originating service</td>

</tr>

<tr>

<td>duration</td>

<td>Number</td>

<td>The duration in seconds</td>

</tr>

<tr>

<td>distance</td>

<td>Number</td>

<td>The distance in meters</td>

</tr>

<tr>

<td>steps</td>

<td>Number</td>

<td>The number of steps taken during the activity</td>

</tr>

<tr>

<td>calories</td>

<td>Number</td>

<td>The number of estimated calories burned during the activity</td>

</tr>

<tr>

<td>sourceData</td>

<td>Object</td>

<td>Additional data from the source that does not fit into the humanapi model. This data can include things such as “Fuel points” for the Nike service.</td>

</tr>

<tr>

<td>timeSeries</td>

<td>Object</td>

<td>Time series data of different data such as heart rate, gps location etc.</td>

</tr>

</tbody>

</table>

## Activity Summaries

    # Get all activities summaries associated with the user
    curl "https://api.humanapi.co/v1/human/activities/summaries?access_token=demo"

    OR

    curl "https://api.humanapi.co/v1/human/activities/summaries?access_token=demo&updated_since=20140110T000000Z"

    # Returns a json array of activities summaries:
    [{
      "id": "550fe4b68c34cc6661f46f7f",
      "userId": "52e20cb2fff56aac62000001",
      "date": "2015-03-23",
      "duration": 722,
      "distance": 1506.71611260169,
      "steps": 0,
      "calories": 118,
      "source": "runkeeper",
      "vigorous": 0,
      "moderate": 0,
      "light": 0,
      "sedentary": 0,
      "sourceData": {},
      "createdAt": "2015-03-23T18:02:29.578Z",
      "updatedAt": "2015-03-23T18:02:29.578Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }, {
      "id": "550fb80568d29c4133b5e134",
      "userId": "52e20cb2fff56aac62000001",
      "date": "2015-03-23",
      "duration": 108,
      "distance": 171,
      "steps": 207,
      "calories": 12.1920387848,
      "source": "jawbone",
      "vigorous": 0,
      "moderate": 0,
      "light": 0,
      "sedentary": 254,
      "sourceData": {},
      "createdAt": "2015-03-23T06:51:49.355Z",
      "updatedAt": "2015-03-23T17:48:16.610Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }, {
      "id": "550f597085cc310900c73f72",
      "userId": "52e20cb2fff56aac62000001",
      "date": "2015-03-23",
      "duration": 0,
      "distance": 0,
      "steps": 0,
      "calories": 0,
      "source": "moves",
      "vigorous": 0,
      "moderate": 0,
      "light": 0,
      "sedentary": 0,
      "sourceData": {},
      "createdAt": "2015-03-23T00:08:16.979Z",
      "updatedAt": "2015-03-23T17:44:05.818Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }]

    # Get an activity summary by id
    curl "https://api.humanapi.co/v1/human/activities/summaries/550f597085cc310900c73f72?access_token=demo"

    # Returns a json object as activity summary:
    {
      "id": "550f597085cc310900c73f72",
      "userId": "52e20cb2fff56aac62000001",
      "date": "2015-03-23",
      "duration": 0,
      "distance": 0,
      "steps": 0,
      "calories": 0,
      "source": "moves",
      "vigorous": 0,
      "moderate": 0,
      "light": 0,
      "sedentary": 0,
      "sourceData": {},
      "createdAt": "2015-03-23T00:08:16.979Z",
      "updatedAt": "2015-03-23T17:44:05.818Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }

    // Get all activities summaries associated with the user
    var request = require('request');
    request
    .get('https://api.humanapi.co/v1/human/activities/summaries?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    OR

    request
    .get('https://api.humanapi.co/v1/human/activities/summaries?access_token=demo&updated_since=20140110T000000Z')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json array of activities summaries:
    [{
      "id": "550fe4b68c34cc6661f46f7f",
      "userId": "52e20cb2fff56aac62000001",
      "date": "2015-03-23",
      "duration": 722,
      "distance": 1506.71611260169,
      "steps": 0,
      "calories": 118,
      "source": "runkeeper",
      "vigorous": 0,
      "moderate": 0,
      "light": 0,
      "sedentary": 0,
      "sourceData": {},
      "createdAt": "2015-03-23T18:02:29.578Z",
      "updatedAt": "2015-03-23T18:02:29.578Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }, {
      "id": "550fb80568d29c4133b5e134",
      "userId": "52e20cb2fff56aac62000001",
      "date": "2015-03-23",
      "duration": 108,
      "distance": 171,
      "steps": 207,
      "calories": 12.1920387848,
      "source": "jawbone",
      "vigorous": 0,
      "moderate": 0,
      "light": 0,
      "sedentary": 254,
      "sourceData": {},
      "createdAt": "2015-03-23T06:51:49.355Z",
      "updatedAt": "2015-03-23T17:48:16.610Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }, {
      "id": "550f597085cc310900c73f72",
      "userId": "52e20cb2fff56aac62000001",
      "date": "2015-03-23",
      "duration": 0,
      "distance": 0,
      "steps": 0,
      "calories": 0,
      "source": "moves",
      "vigorous": 0,
      "moderate": 0,
      "light": 0,
      "sedentary": 0,
      "sourceData": {},
      "createdAt": "2015-03-23T00:08:16.979Z",
      "updatedAt": "2015-03-23T17:44:05.818Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }]

    // Get an activity summary by id
    request
    .get('https://api.humanapi.co/v1/human/activities/summaries/550f597085cc310900c73f72?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json object as activity summary:
    {
      "id": "550f597085cc310900c73f72",
      "userId": "52e20cb2fff56aac62000001",
      "date": "2015-03-23",
      "duration": 0,
      "distance": 0,
      "steps": 0,
      "calories": 0,
      "source": "moves",
      "vigorous": 0,
      "moderate": 0,
      "light": 0,
      "sedentary": 0,
      "sourceData": {},
      "createdAt": "2015-03-23T00:08:16.979Z",
      "updatedAt": "2015-03-23T17:44:05.818Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }

Get a list of daily summaries of activities summaries reported by the data sources.

### Get Activity Summaries

Returns a list of activities summaries

`GET https://api.humanapi.co/v1/human/activities/summaries`  

Returns a single activity summary

`GET https://api.humanapi.co/v1/human/activities/summaries/{id}`

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

</tbody>

</table>

Returns a list of activities summaries updated since a given time

`GET https://api.humanapi.co/v1/human/activities/summaries`  

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

<tr>

<td>updated_since</td>

<td>Date in YYYYMMDDTHHmmssZ format</td>

</tr>

</tbody>

</table>

### Response properties

<table>

<thead>

<tr>

<th>Property</th>

<th>Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>id</td>

<td>String</td>

<td>The Id of the resource</td>

</tr>

<tr>

<td>userId</td>

<td>String</td>

<td>The global Id of the user</td>

</tr>

<tr>

<td>humanId</td>

<td>String</td>

<td>Unique user identifier</td>

</tr>

<tr>

<td>date</td>

<td>Date</td>

<td>The date of the activity</td>

</tr>

<tr>

<td>source</td>

<td>String</td>

<td>The name of the originating service</td>

</tr>

<tr>

<td>duration</td>

<td>Number</td>

<td>The duration in seconds</td>

</tr>

<tr>

<td>distance</td>

<td>Number</td>

<td>The distance in meters</td>

</tr>

<tr>

<td>steps</td>

<td>Number</td>

<td>The number of steps taken during the activity</td>

</tr>

<tr>

<td>vigorous</td>

<td>Number</td>

<td>The number of minutes of vigorous activity</td>

</tr>

<tr>

<td>moderate</td>

<td>Number</td>

<td>The number of minutes of moderate activity</td>

</tr>

<tr>

<td>light</td>

<td>Number</td>

<td>The number of minutes of light activity</td>

</tr>

<tr>

<td>sedentary</td>

<td>Number</td>

<td>The number of minutes of sedentary activity</td>

</tr>

<tr>

<td>calories</td>

<td>Number</td>

<td>The number of estimated calories burned during the activity</td>

</tr>

<tr>

<td>sourceData</td>

<td>Object</td>

<td>Additional data from the source that does not fit into the humanapi model. This data can include things such as “Fuel points” for the Nike service.</td>

</tr>

<tr>

<td>timeSeries</td>

<td>Object</td>

<td>Time series data of different data such as heart rate, gps location etc.</td>

</tr>

<tr>

<td>createdAt</td>

<td>Date</td>

<td>The time the activity was created on the Human API server</td>

</tr>

<tr>

<td>updatedAt</td>

<td>Date</td>

<td>The time the activity was updated on the Human API server</td>

</tr>

</tbody>

</table>

## Blood Glucose

    # Get blood glucose measurement associated with the user
    curl "https://api.humanapi.co/v1/human/blood_glucose?access_token=demo"

    # Returns a json object of blood glucose:
    {
      "id": "54be39e6c3fa43b8057d59fc",
      "userId": "52e20cb2fff56aac62000001",
      "timestamp": "2015-01-20T12:20:04.000Z",
      "value": 94,
      "unit": "mg/dL",
      "source": "ihealth",
      "sourceData": {},
      "createdAt": "2015-03-16T23:48:43.696Z",
      "updatedAt": "2015-03-16T23:48:43.696Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }

    // Get all blood glucose associated with the user
    var request = require('request');
    request
    .get('https://api.humanapi.co/v1/human/blood_glucose?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json object of blood glucose:
    {
      "id": "54be39e6c3fa43b8057d59fc",
      "userId": "52e20cb2fff56aac62000001",
      "timestamp": "2015-01-20T12:20:04.000Z",
      "value": 94,
      "unit": "mg/dL",
      "source": "ihealth",
      "sourceData": {},
      "createdAt": "2015-03-16T23:48:43.696Z",
      "updatedAt": "2015-03-16T23:48:43.696Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }

Get the blood glucose measurement from a specific point in time.

### Get Blood Glucose

Returns a blood glucose reading

`GET https://api.humanapi.co/v1/human/blood_glucose`  

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

</tbody>

</table>

### Response properties

<table>

<thead>

<tr>

<th>Property</th>

<th>Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>id</td>

<td>String</td>

<td>The id of the blood glucose reading</td>

</tr>

<tr>

<td>userId</td>

<td>String</td>

<td>The global Id of the user</td>

</tr>

<tr>

<td>humanId</td>

<td>String</td>

<td>Unique user identifier</td>

</tr>

<tr>

<td>timestamp</td>

<td>Date</td>

<td>The original date and time of the measurement</td>

</tr>

<tr>

<td>source</td>

<td>String</td>

<td>The source service for the measurement, where it was created</td>

</tr>

<tr>

<td>value</td>

<td>Number</td>

<td>The value of the measurement in the unit specified</td>

</tr>

<tr>

<td>unit</td>

<td>String</td>

<td>The unit of the measurement value</td>

</tr>

<tr>

<td>notes</td>

<td>String</td>

<td>User created notes</td>

</tr>

<tr>

<td>mealTag</td>

<td>String</td>

<td>Indication if value was captured before/after a meal</td>

</tr>

<tr>

<td>medicationTag</td>

<td>String</td>

<td>Indication if value was captured before/after taking medication</td>

</tr>

<tr>

<td>createdAt</td>

<td>Date</td>

<td>The time the measurement was created on the Human API server</td>

</tr>

<tr>

<td>updatedAt</td>

<td>Date</td>

<td>The time the measurement was updated on the Human API server</td>

</tr>

</tbody>

</table>

## Blood Oxygen

    # Get blood oxygen measurement associated with the user
    curl "https://api.humanapi.co/v1/human/blood_oxygen?access_token=demo"

    # Returns a json object of blood oxygen:
    {
      "id": "54d1f018810a5ba429951b07",
      "userId": "52e20cb2fff56aac62000001",
      "timestamp": "2015-01-26T06:58:37.000Z",
      "value": 99,
      "unit": "SpO2(%)",
      "source": "withings",
      "sourceData": {},
      "createdAt": "2015-02-04T10:10:32.691Z",
      "updatedAt": "2015-02-04T10:10:32.691Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }

    // Get all blood oxygen associated with the user
    var request = require('request');
    request
    .get('https://api.humanapi.co/v1/human/blood_oxygen?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json object of blood oxygen:
    {
      "id": "54d1f018810a5ba429951b07",
      "userId": "52e20cb2fff56aac62000001",
      "timestamp": "2015-01-26T06:58:37.000Z",
      "value": 99,
      "unit": "SpO2(%)",
      "source": "withings",
      "sourceData": {},
      "createdAt": "2015-02-04T10:10:32.691Z",
      "updatedAt": "2015-02-04T10:10:32.691Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }

Get the blood oxygen measurement from a specific point in time.

### Get Blood Oxygen

Returns a blood oxygen reading

`GET https://api.humanapi.co/v1/human/blood_oxygen`  

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

</tbody>

</table>

### Response properties

<table>

<thead>

<tr>

<th>Property</th>

<th>Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>id</td>

<td>String</td>

<td>The id of the blood oxygen reading</td>

</tr>

<tr>

<td>userId</td>

<td>String</td>

<td>The global Id of the user</td>

</tr>

<tr>

<td>humanId</td>

<td>String</td>

<td>Unique user identifier</td>

</tr>

<tr>

<td>timestamp</td>

<td>Date</td>

<td>The original date and time of the measurement</td>

</tr>

<tr>

<td>source</td>

<td>String</td>

<td>The source service for the measurement, where it was created</td>

</tr>

<tr>

<td>value</td>

<td>Number</td>

<td>The value of the measurement in the unit specified</td>

</tr>

<tr>

<td>unit</td>

<td>String</td>

<td>The unit of the measurement value</td>

</tr>

<tr>

<td>createdAt</td>

<td>Date</td>

<td>The time the measurement was created on the Human API server</td>

</tr>

<tr>

<td>updatedAt</td>

<td>Date</td>

<td>The time the measurement was updated on the Human API server</td>

</tr>

</tbody>

</table>

## Blood Pressure

    # Get blood pressure measurement associated with the user
    curl "https://api.humanapi.co/v1/human/blood_pressure?access_token=demo"

    # Returns a json object of blood pressure:
    {
      "id": "550b8a8e834dd16f259683b1",
      "userId": "52e20cb2fff56aac62000001",
      "timestamp": "2015-03-19T22:48:26.000Z",
      "systolic": 117,
      "diastolic": 76,
      "unit": "mmHg",
      "heartRate": 66,
      "source": "ihealth",
      "createdAt": "2015-03-20T02:48:46.925Z",
      "updatedAt": "2015-03-20T02:48:46.925Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }

    // Get all blood pressure associated with the user
    var request = require('request');
    request
    .get('https://api.humanapi.co/v1/human/blood_pressure?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json object of blood pressure:
    {
      "id": "550b8a8e834dd16f259683b1",
      "userId": "52e20cb2fff56aac62000001",
      "timestamp": "2015-03-19T22:48:26.000Z",
      "systolic": 117,
      "diastolic": 76,
      "unit": "mmHg",
      "heartRate": 66,
      "source": "ihealth",
      "createdAt": "2015-03-20T02:48:46.925Z",
      "updatedAt": "2015-03-20T02:48:46.925Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }

Get the blood pressure measurement from a specific point in time.

### Get Blood Pressure

Returns a blood pressure reading

`GET https://api.humanapi.co/v1/human/blood_pressure`  

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

</tbody>

</table>

### Response properties

<table>

<thead>

<tr>

<th>Property</th>

<th>Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>id</td>

<td>String</td>

<td>The id of the blood pressure reading</td>

</tr>

<tr>

<td>userId</td>

<td>String</td>

<td>The global Id of the user</td>

</tr>

<tr>

<td>humanId</td>

<td>String</td>

<td>Unique user identifier</td>

</tr>

<tr>

<td>timestamp</td>

<td>Date</td>

<td>The original date and time of the measurement</td>

</tr>

<tr>

<td>source</td>

<td>String</td>

<td>The source service for the measurement, where it was created</td>

</tr>

<tr>

<td>value</td>

<td>Number</td>

<td>The value of the measurement in the unit specified</td>

</tr>

<tr>

<td>unit</td>

<td>String</td>

<td>The unit of the measurement value</td>

</tr>

<tr>

<td>heartRate</td>

<td>String</td>

<td>The heart rate in BPM captured at the time of measurement</td>

</tr>

<tr>

<td>systolic</td>

<td>String</td>

<td>The systolic value captured at the time of measurement</td>

</tr>

<tr>

<td>diastolic</td>

<td>String</td>

<td>The diastolic value captured at the time of measurement</td>

</tr>

<tr>

<td>createdAt</td>

<td>Date</td>

<td>The time the measurement was created on the Human API server</td>

</tr>

<tr>

<td>updatedAt</td>

<td>Date</td>

<td>The time the measurement was updated on the Human API server</td>

</tr>

</tbody>

</table>

## Body Mass Index (BMI)

    # Get bmi measurement associated with the user
    curl "https://api.humanapi.co/v1/human/bmi?access_token=demo"

    # Returns a json object of bmi:
    {
      "id": "5506f5080ba9b35f0af6cc89",
      "userId": "52e20cb2fff56aac62000001",
      "timestamp": "2015-03-16T23:59:59.000Z",
      "value": 30.26,
      "unit": "kg/m2",
      "source": "fitbit",
      "sourceData": {},
      "createdAt": "2015-03-16T15:21:44.304Z",
      "updatedAt": "2015-03-16T15:24:48.492Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }

    // Get all bmi associated with the user
    var request = require('request');
    request
    .get('https://api.humanapi.co/v1/human/bmi?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json object of bmi:
    {
      "id": "5506f5080ba9b35f0af6cc89",
      "userId": "52e20cb2fff56aac62000001",
      "timestamp": "2015-03-16T23:59:59.000Z",
      "value": 30.26,
      "unit": "kg/m2",
      "source": "fitbit",
      "sourceData": {},
      "createdAt": "2015-03-16T15:21:44.304Z",
      "updatedAt": "2015-03-16T15:24:48.492Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }

Get the bmi measurement from a specific point in time.

### Get BMI

Returns a bmi reading

`GET https://api.humanapi.co/v1/human/bmi`  

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

</tbody>

</table>

### Response properties

<table>

<thead>

<tr>

<th>Property</th>

<th>Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>id</td>

<td>String</td>

<td>The id of the bmi reading</td>

</tr>

<tr>

<td>userId</td>

<td>String</td>

<td>The global Id of the user</td>

</tr>

<tr>

<td>humanId</td>

<td>String</td>

<td>Unique user identifier</td>

</tr>

<tr>

<td>timestamp</td>

<td>Date</td>

<td>The original date and time of the measurement</td>

</tr>

<tr>

<td>source</td>

<td>String</td>

<td>The source service for the measurement, where it was created</td>

</tr>

<tr>

<td>value</td>

<td>Number</td>

<td>The value of the measurement in the unit specified</td>

</tr>

<tr>

<td>unit</td>

<td>String</td>

<td>The unit of the measurement value</td>

</tr>

<tr>

<td>createdAt</td>

<td>Date</td>

<td>The time the measurement was created on the Human API server</td>

</tr>

<tr>

<td>updatedAt</td>

<td>Date</td>

<td>The time the measurement was updated on the Human API server</td>

</tr>

</tbody>

</table>

## Body Fat

    # Get body fat measurement associated with the user
    curl "https://api.humanapi.co/v1/human/body_fat?access_token=demo"

    # Returns a json object of body fat:
    {
      "id": "54ed75ccd9f8e13b13eaf17f",
      "userId": "52e20cb2fff56aac62000001",
      "timestamp": "2015-02-25T07:09:52.000Z",
      "value": 18.695,
      "unit": "percent",
      "source": "withings",
      "sourceData": {},
      "createdAt": "2015-02-26T05:21:01.021Z",
      "updatedAt": "2015-02-26T05:21:01.021Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }

    // Get all body fat associated with the user
    var request = require('request');
    request
    .get('https://api.humanapi.co/v1/human/body_fat?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json object of body fat:
    {
      "id": "54ed75ccd9f8e13b13eaf17f",
      "userId": "52e20cb2fff56aac62000001",
      "timestamp": "2015-02-25T07:09:52.000Z",
      "value": 18.695,
      "unit": "percent",
      "source": "withings",
      "sourceData": {},
      "createdAt": "2015-02-26T05:21:01.021Z",
      "updatedAt": "2015-02-26T05:21:01.021Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }

Get the body fat measurement from a specific point in time.

### Get Body Fat

Returns a body fat reading

`GET https://api.humanapi.co/v1/human/body_fat`  

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

</tbody>

</table>

### Response properties

<table>

<thead>

<tr>

<th>Property</th>

<th>Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>id</td>

<td>String</td>

<td>The id of the bmi reading</td>

</tr>

<tr>

<td>userId</td>

<td>String</td>

<td>The global Id of the user</td>

</tr>

<tr>

<td>humanId</td>

<td>String</td>

<td>Unique user identifier</td>

</tr>

<tr>

<td>timestamp</td>

<td>Date</td>

<td>The original date and time of the measurement</td>

</tr>

<tr>

<td>source</td>

<td>String</td>

<td>The source service for the measurement, where it was created</td>

</tr>

<tr>

<td>value</td>

<td>Number</td>

<td>The value of the measurement in the unit specified</td>

</tr>

<tr>

<td>unit</td>

<td>String</td>

<td>The unit of the measurement value</td>

</tr>

<tr>

<td>createdAt</td>

<td>Date</td>

<td>The time the measurement was created on the Human API server</td>

</tr>

<tr>

<td>updatedAt</td>

<td>Date</td>

<td>The time the measurement was updated on the Human API server</td>

</tr>

</tbody>

</table>

## Genetics

    # Get genetic traits measurement associated with the user
    curl "https://api.humanapi.co/v1/human/genetic/traits?access_token=demo"

    # Returns a json object of genetic traits:
    [{
      "userId": "52e20cb2fff56aac62000001",
      "description": "Hair Curl",
      "trait": "Straighter Hair on Average",
      "possibleTraits": ["Slightly Curlier Hair on Average", "Straighter Hair on Average"],
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }, {
      "userId": "52e20cb2fff56aac62000001",
      "description": "Male Pattern Baldness",
      "trait": "Decreased Odds",
      "possibleTraits": ["Decreased Odds", "Increased Odds", "No Data", "Not Applicable", "Typical Odds"],
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }, {
      "userId": "52e20cb2fff56aac62000001",
      "description": "Smoking Behavior",
      "trait": "Typical",
      "possibleTraits": ["If a Smoker, Likely to Smoke More", "Typical"],
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }]

    // Get all genetic traits associated with the user
    var request = require('request');
    request
    .get('https://api.humanapi.co/v1/human/genetic/traits?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json object of genetic traits:
    [{
      "userId": "52e20cb2fff56aac62000001",
      "description": "Hair Curl",
      "trait": "Straighter Hair on Average",
      "possibleTraits": ["Slightly Curlier Hair on Average", "Straighter Hair on Average"],
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }, {
      "userId": "52e20cb2fff56aac62000001",
      "description": "Male Pattern Baldness",
      "trait": "Decreased Odds",
      "possibleTraits": ["Decreased Odds", "Increased Odds", "No Data", "Not Applicable", "Typical Odds"],
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }, {
      "userId": "52e20cb2fff56aac62000001",
      "description": "Smoking Behavior",
      "trait": "Typical",
      "possibleTraits": ["If a Smoker, Likely to Smoke More", "Typical"],
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }]

The Genetics resource is a simplified version of the genetic model data we store. Today it returns a list of genetic traits for a user.

### Get Genetic Traits

Returns a genetic traits reading

`GET https://api.humanapi.co/v1/human/genetic/traits`  

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

</tbody>

</table>

### Response properties

<table>

<thead>

<tr>

<th>Property</th>

<th>Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>userId</td>

<td>String</td>

<td>The global Id of the user</td>

</tr>

<tr>

<td>humanId</td>

<td>String</td>

<td>Unique user identifier</td>

</tr>

<tr>

<td>trait</td>

<td>Date</td>

<td>The most likely present trait</td>

</tr>

<tr>

<td>possibleTraits</td>

<td>String</td>

<td>A list of all the possible values for a specific trait, for easy comparison</td>

</tr>

<tr>

<td>description</td>

<td>Number</td>

<td>A description/name of the trait</td>

</tr>

</tbody>

</table>

## Heart Rate

    # Get heart rate measurement associated with the user
    curl "https://api.humanapi.co/v1/human/heart_rate?access_token=demo"

    # Returns a json object of heart rate:
    {
      "id": "550b8a8e834dd16f259683b2",
      "userId": "52e20cb2fff56aac62000001",
      "timestamp": "2015-03-19T22:48:26.000Z",
      "value": 66,
      "unit": "bpm",
      "source": "ihealth",
      "sourceData": {},
      "createdAt": "2015-03-20T02:48:46.945Z",
      "updatedAt": "2015-03-20T02:48:46.945Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }

    // Get all heart rate associated with the user
    var request = require('request');
    request
    .get('https://api.humanapi.co/v1/human/heart_rate?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json object of heart rate:
    {
      "id": "550b8a8e834dd16f259683b2",
      "userId": "52e20cb2fff56aac62000001",
      "timestamp": "2015-03-19T22:48:26.000Z",
      "value": 66,
      "unit": "bpm",
      "source": "ihealth",
      "sourceData": {},
      "createdAt": "2015-03-20T02:48:46.945Z",
      "updatedAt": "2015-03-20T02:48:46.945Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }

Get the heart rate measurement from a specific point in time.

### Get Heart Rate

Returns a heart rate reading

`GET https://api.humanapi.co/v1/human/heart_rate`  

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

</tbody>

</table>

### Response properties

<table>

<thead>

<tr>

<th>Property</th>

<th>Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>id</td>

<td>String</td>

<td>The id of the bmi reading</td>

</tr>

<tr>

<td>userId</td>

<td>String</td>

<td>The global Id of the user</td>

</tr>

<tr>

<td>humanId</td>

<td>String</td>

<td>Unique user identifier</td>

</tr>

<tr>

<td>timestamp</td>

<td>Date</td>

<td>The original date and time of the measurement</td>

</tr>

<tr>

<td>source</td>

<td>String</td>

<td>The source service for the measurement, where it was created</td>

</tr>

<tr>

<td>value</td>

<td>Number</td>

<td>The value of the measurement in the unit specified</td>

</tr>

<tr>

<td>unit</td>

<td>String</td>

<td>The unit of the measurement value</td>

</tr>

<tr>

<td>createdAt</td>

<td>Date</td>

<td>The time the measurement was created on the Human API server</td>

</tr>

<tr>

<td>updatedAt</td>

<td>Date</td>

<td>The time the measurement was updated on the Human API server</td>

</tr>

</tbody>

</table>

## Height

    # Get height measurement associated with the user
    curl "https://api.humanapi.co/v1/human/height?access_token=demo"

    # Returns a json object of height:
    {
      "id": "5509b45bb384e1620a5933c5",
      "userId": "52e20cb2fff56aac62000001",
      "timestamp": "2015-03-18T00:00:00.000Z",
      "value": 1753,
      "unit": "mm",
      "source": "fitbit",
      "sourceData": {},
      "createdAt": "2015-03-18T17:22:35.859Z",
      "updatedAt": "2015-03-18T17:22:35.859Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }

    // Get all height associated with the user
    var request = require('request');
    request
    .get('https://api.humanapi.co/v1/human/height?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json object of height:
    {
      "id": "5509b45bb384e1620a5933c5",
      "userId": "52e20cb2fff56aac62000001",
      "timestamp": "2015-03-18T00:00:00.000Z",
      "value": 1753,
      "unit": "mm",
      "source": "fitbit",
      "sourceData": {},
      "createdAt": "2015-03-18T17:22:35.859Z",
      "updatedAt": "2015-03-18T17:22:35.859Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }

Get the height measurement from a specific point in time.

### Get Height

Returns a height reading

`GET https://api.humanapi.co/v1/human/height`  

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

</tbody>

</table>

### Response properties

<table>

<thead>

<tr>

<th>Property</th>

<th>Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>id</td>

<td>String</td>

<td>The id of the bmi reading</td>

</tr>

<tr>

<td>userId</td>

<td>String</td>

<td>The global Id of the user</td>

</tr>

<tr>

<td>humanId</td>

<td>String</td>

<td>Unique user identifier</td>

</tr>

<tr>

<td>timestamp</td>

<td>Date</td>

<td>The original date and time of the measurement</td>

</tr>

<tr>

<td>source</td>

<td>String</td>

<td>The source service for the measurement, where it was created</td>

</tr>

<tr>

<td>value</td>

<td>Number</td>

<td>The value of the measurement in the unit specified</td>

</tr>

<tr>

<td>unit</td>

<td>String</td>

<td>The unit of the measurement value</td>

</tr>

<tr>

<td>createdAt</td>

<td>Date</td>

<td>The time the measurement was created on the Human API server</td>

</tr>

<tr>

<td>updatedAt</td>

<td>Date</td>

<td>The time the measurement was updated on the Human API server</td>

</tr>

</tbody>

</table>

## Human (Summary)

    # Get user's health summary
    curl "https://api.humanapi.co/v1/human?access_token=demo"

    # Returns a json object:
    {
      "userId": "52e20cb2fff56aac62000001",
      "email": "demo@humanapi.co",
      "gender": "male",
      "createdAt": "2014-01-24T06:48:18.361Z",
      "bloodGlucose": {
        "id": "54be39e6c3fa43b8057d59fc",
        "timestamp": "2015-01-20T12:20:04.000Z",
        "source": "ihealth",
        "value": 94,
        "unit": "mg/dL"
      },
      "bloodOxygen": {
        "id": "54d1f018810a5ba429951b07",
        "timestamp": "2015-01-26T06:58:37.000Z",
        "source": "withings",
        "value": 99,
        "unit": "SpO2"
      },
      "bloodPressure": {
        "id": "550b8a8e834dd16f259683b1",
        "userId": "52e20cb2fff56aac62000001",
        "timestamp": "2015-03-19T22:48:26.000Z",
        "source": "ihealth",
        "systolic": 117,
        "diastolic": 76,
        "unit": "mmHg",
        "heartRate": 66
      },
      "bmi": {
        "id": "5506f5080ba9b35f0af6cc89",
        "timestamp": "2015-03-16T23:59:59.000Z",
        "source": "fitbit",
        "value": 30.26,
        "unit": "kg/m2"
      },
      "bodyFat": {
        "id": "54ed75ccd9f8e13b13eaf17f",
        "timestamp": "2015-02-25T07:09:52.000Z",
        "source": "withings",
        "value": 18.695,
        "unit": "%"
      },
      "height": {
        "id": "5509b45bb384e1620a5933c5",
        "timestamp": "2015-03-18T00:00:00.000Z",
        "source": "fitbit",
        "value": 1753,
        "unit": "mm"
      },
      "heartRate": {
        "id": "550b8a8e834dd16f259683b2",
        "timestamp": "2015-03-19T22:48:26.000Z",
        "source": "ihealth",
        "value": 66,
        "unit": "bpm"
      },
      "weight": {
        "id": "5506f5080ba9b35f0af6cc8a",
        "timestamp": "2015-03-16T23:59:59.000Z",
        "source": "fitbit",
        "value": 92.9,
        "unit": "kg"
      },
      "activitySummary": {
        "id": "53adbf5dbef3fe0d24a1d453",
        "date": "2014-06-27",
        "duration": 1468.137,
        "distance": 3043.21010200387,
        "sedentary": 0,
        "light": 0,
        "moderate": 0,
        "vigorous": 0,
        "total": 1468.137,
        "steps": 0,
        "calories": 241,
        "source": "runkeeper"
      },
      "sleepSummary": {
        "id": "540e96b74247c3ea71d671b9",
        "userId": "52e20cb2fff56aac62000001",
        "date": "2014-09-09",
        "source": "fitbit",
        "timeAsleep": 456,
        "timeAwake": 0,
        "updatedAt": "2014-09-11T02:28:27.377Z"
      },
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }

    // Get user's health summary
    var request = require('request');
    request
    .get('https://api.humanapi.co/v1/human?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json object:
    {
      "userId": "52e20cb2fff56aac62000001",
      "email": "demo@humanapi.co",
      "gender": "male",
      "createdAt": "2014-01-24T06:48:18.361Z",
      "bloodGlucose": {
        "id": "54be39e6c3fa43b8057d59fc",
        "timestamp": "2015-01-20T12:20:04.000Z",
        "source": "ihealth",
        "value": 94,
        "unit": "mg/dL"
      },
      "bloodOxygen": {
        "id": "54d1f018810a5ba429951b07",
        "timestamp": "2015-01-26T06:58:37.000Z",
        "source": "withings",
        "value": 99,
        "unit": "SpO2"
      },
      "bloodPressure": {
        "id": "550b8a8e834dd16f259683b1",
        "userId": "52e20cb2fff56aac62000001",
        "timestamp": "2015-03-19T22:48:26.000Z",
        "source": "ihealth",
        "systolic": 117,
        "diastolic": 76,
        "unit": "mmHg",
        "heartRate": 66
      },
      "bmi": {
        "id": "5506f5080ba9b35f0af6cc89",
        "timestamp": "2015-03-16T23:59:59.000Z",
        "source": "fitbit",
        "value": 30.26,
        "unit": "kg/m2"
      },
      "bodyFat": {
        "id": "54ed75ccd9f8e13b13eaf17f",
        "timestamp": "2015-02-25T07:09:52.000Z",
        "source": "withings",
        "value": 18.695,
        "unit": "%"
      },
      "height": {
        "id": "5509b45bb384e1620a5933c5",
        "timestamp": "2015-03-18T00:00:00.000Z",
        "source": "fitbit",
        "value": 1753,
        "unit": "mm"
      },
      "heartRate": {
        "id": "550b8a8e834dd16f259683b2",
        "timestamp": "2015-03-19T22:48:26.000Z",
        "source": "ihealth",
        "value": 66,
        "unit": "bpm"
      },
      "weight": {
        "id": "5506f5080ba9b35f0af6cc8a",
        "timestamp": "2015-03-16T23:59:59.000Z",
        "source": "fitbit",
        "value": 92.9,
        "unit": "kg"
      },
      "activitySummary": {
        "id": "53adbf5dbef3fe0d24a1d453",
        "date": "2014-06-27",
        "duration": 1468.137,
        "distance": 3043.21010200387,
        "sedentary": 0,
        "light": 0,
        "moderate": 0,
        "vigorous": 0,
        "total": 1468.137,
        "steps": 0,
        "calories": 241,
        "source": "runkeeper"
      },
      "sleepSummary": {
        "id": "540e96b74247c3ea71d671b9",
        "userId": "52e20cb2fff56aac62000001",
        "date": "2014-09-09",
        "source": "fitbit",
        "timeAsleep": 456,
        "timeAwake": 0,
        "updatedAt": "2014-09-11T02:28:27.377Z"
      },
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }

Get summary object for a person’s latest health metrics. The data consists of all the discrete health measurements and summary information about the person’s activity and sleep data.

### Get Human Summary

Returns the human profile

`GET https://api.humanapi.co/v1/human`  

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

</tbody>

</table>

### Response properties

<table>

<thead>

<tr>

<th>Property</th>

<th>Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>userId</td>

<td>String</td>

<td>The global Id of the user</td>

</tr>

<tr>

<td>humanId</td>

<td>String</td>

<td>Unique user identifier</td>

</tr>

<tr>

<td>gender</td>

<td>String</td>

<td>The gender of the person, if known</td>

</tr>

<tr>

<td>bloodGlucose</td>

<td>BloodGlucose</td>

<td>The latest blood glucose measurement</td>

</tr>

<tr>

<td>bloodPressure</td>

<td>BloodPressure</td>

<td>The latest blood pressure measurement</td>

</tr>

<tr>

<td>bmi</td>

<td>Bmi</td>

<td>The latest body mass index measurement</td>

</tr>

<tr>

<td>bodyFat</td>

<td>BodyFat</td>

<td>The latest body fat percentage measurement</td>

</tr>

<tr>

<td>height</td>

<td>Height</td>

<td>The latest height measurement</td>

</tr>

<tr>

<td>heartRate</td>

<td>HeartRate</td>

<td>The latest heart rate measurement</td>

</tr>

<tr>

<td>weight</td>

<td>Weight</td>

<td>The latest weight measurement</td>

</tr>

<tr>

<td>activitySummary</td>

<td>ActivitySummary</td>

<td>The latest activity summary</td>

</tr>

<tr>

<td>sleepSummary</td>

<td>SleepSummary</td>

<td>The latest sleep summary</td>

</tr>

</tbody>

</table>

## Locations

    # Get locations measurement associated with the user
    curl "https://api.humanapi.co/v1/human/locations?access_token=demo"

    # Returns a json array of locations:
    [{
      "id": "54aa4c5623e093b72ba122ec",
      "userId": "52e20cb2fff56aac62020001",
      "startTime": "2015-03-23T16:48:37.000Z",
      "endTime": "2015-03-23T17:15:40.000Z",
      "name": "",
      "source": "moves",
      "location": {
        "lon": 77.3092283085,
        "lat": 28.5941775291
      },
      "createdAt": "2015-02-06T21:21:13.679Z",
      "updatedAt": "2015-02-06T21:21:13.679Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }, {
      "id": "54c8681d94ae47d0702e8624",
      "userId": "52e20cb2aaf56aac62000001",
      "startTime": "2015-03-23T02:04:02.000Z",
      "endTime": "2015-03-23T03:38:26.000Z",
      "name": "Walmart",
      "source": "moves",
      "location": {
        "lon": -87.06979948824468,
        "lat": 20.63034803007769
      },
      "createdAt": "2015-01-31T22:32:58.332Z",
      "updatedAt": "2015-01-31T22:32:58.332Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }]

    // Get all locations associated with the user
    var request = require('request');
    request
    .get('https://api.humanapi.co/v1/human/locations?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json array of locations:
    [{
      "id": "54aa4c5623e093b72ba122ec",
      "userId": "52e20cb2fff56aac62020001",
      "startTime": "2015-03-23T16:48:37.000Z",
      "endTime": "2015-03-23T17:15:40.000Z",
      "name": "",
      "source": "moves",
      "location": {
        "lon": 77.3092283085,
        "lat": 28.5941775291
      },
      "createdAt": "2015-02-06T21:21:13.679Z",
      "updatedAt": "2015-02-06T21:21:13.679Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }, {
      "id": "54c8681d94ae47d0702e8624",
      "userId": "52e20cb2aaf56aac62000001",
      "startTime": "2015-03-23T02:04:02.000Z",
      "endTime": "2015-03-23T03:38:26.000Z",
      "name": "Walmart",
      "source": "moves",
      "location": {
        "lon": -87.06979948824468,
        "lat": 20.63034803007769
      },
      "createdAt": "2015-01-31T22:32:58.332Z",
      "updatedAt": "2015-01-31T22:32:58.332Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }]

Get the locations measurement from a specific point in time.

### Get Locations

Returns a locations readings

`GET https://api.humanapi.co/v1/human/locations`  

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

</tbody>

</table>

### Response properties

<table>

<thead>

<tr>

<th>Property</th>

<th>Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>id</td>

<td>String</td>

<td>The id of the bmi reading</td>

</tr>

<tr>

<td>userId</td>

<td>String</td>

<td>The global Id of the user</td>

</tr>

<tr>

<td>humanId</td>

<td>String</td>

<td>Unique user identifier</td>

</tr>

<tr>

<td>timestamp</td>

<td>Date</td>

<td>The original date and time of the measurement</td>

</tr>

<tr>

<td>name</td>

<td>String</td>

<td>The name of the place</td>

</tr>

<tr>

<td>source</td>

<td>String</td>

<td>The source service for the measurement, where it was created</td>

</tr>

<tr>

<td>type</td>

<td>String</td>

<td>The type of location</td>

</tr>

<tr>

<td>location</td>

<td>Object</td>

<td>The coordinate point with a lat/lon value</td>

</tr>

<tr>

<td>createdAt</td>

<td>Date</td>

<td>The time the measurement was created on the Human API server</td>

</tr>

<tr>

<td>updatedAt</td>

<td>Date</td>

<td>The time the measurement was updated on the Human API server</td>

</tr>

</tbody>

</table>

## Meals

    # Get all meals associated with the user
    curl "https://api.humanapi.co/v1/human/food/meals?access_token=demo"

    OR

    curl "https://api.humanapi.co/v1/human/food/meals?access_token=demo&updated_since=20140110T000000Z"

    # Returns a json array of meals:
    [{
      "id": "5508205f6101270148721599",
      "userId": "52e20cb2fff56aac62000001",
      "source": "jawbone",
      "timestamp": "2015-03-17T00:00:00.000Z",
      "type": "lunch",
      "name": "Apfelstückchen mit Banane",
      "calories": 124,
      "carbohydrate": 26.6000003815,
      "fat": 0.600000023842,
      "protein": 1.60000002384,
      "sodium": 0,
      "sugar": 0,
      "calcium": 0,
      "cholesterol": 0,
      "fiber": 3,
      "iron": 0,
      "monounsaturatedFat": 0,
      "polyunsaturatedFat": 0,
      "potassium": 0,
      "saturatedFat": 0,
      "vitaminA": 0,
      "vitaminC": 0,
      "createdAt": "2015-03-17T12:38:55.712Z",
      "updatedAt": "2015-03-17T12:38:55.712Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }, {
      "id": "54fe0ad40f701cf4630be975",
      "userId": "52e20cb2fff56aac62000001",
      "source": "jawbone",
      "timestamp": "2015-03-04T00:00:00.000Z",
      "type": "lunch",
      "name": "3 Cereais e Leite",
      "calories": 633.999984741,
      "carbohydrate": 75.9680023193,
      "fat": 22.6160001755,
      "protein": 32.4159994125,
      "sodium": 723.199981689,
      "sugar": 40.2880020142,
      "calcium": 896,
      "cholesterol": 54.4000015259,
      "fiber": 3.07999992371,
      "iron": 0,
      "monounsaturatedFat": 0,
      "polyunsaturatedFat": 0,
      "potassium": 0,
      "saturatedFat": 11.1679999828,
      "vitaminA": 0,
      "vitaminC": 0,
      "createdAt": "2015-03-09T21:04:20.973Z",
      "updatedAt": "2015-03-09T21:04:20.973Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }]

    # Get an meal by id
    curl "https://api.humanapi.co/v1/human/food/meals/54fe0ad40f701cf4630be975?access_token=demo"
    {
      "id": "54fe0ad40f701cf4630be975",
      "userId": "52e20cb2fff56aac62000001",
      "source": "jawbone",
      "timestamp": "2015-03-04T00:00:00.000Z",
      "type": "lunch",
      "name": "3 Cereais e Leite",
      "calories": 633.999984741,
      "carbohydrate": 75.9680023193,
      "fat": 22.6160001755,
      "protein": 32.4159994125,
      "sodium": 723.199981689,
      "sugar": 40.2880020142,
      "calcium": 896,
      "cholesterol": 54.4000015259,
      "fiber": 3.07999992371,
      "iron": 0,
      "monounsaturatedFat": 0,
      "polyunsaturatedFat": 0,
      "potassium": 0,
      "saturatedFat": 11.1679999828,
      "vitaminA": 0,
      "vitaminC": 0,
      "createdAt": "2015-03-09T21:04:20.973Z",
      "updatedAt": "2015-03-09T21:04:20.973Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }

    // Get all meals associated with the user
    var request = require('request');
    request
    .get('https://api.humanapi.co/v1/human/food/meals?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    OR

    request
    .get('https://api.humanapi.co/v1/human/food/meals?access_token=demo&updated_since=20140110T000000Z')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json array of meals:
    [{
      "id": "5508205f6101270148721599",
      "userId": "52e20cb2fff56aac62000001",
      "source": "jawbone",
      "timestamp": "2015-03-17T00:00:00.000Z",
      "type": "lunch",
      "name": "Apfelstückchen mit Banane",
      "calories": 124,
      "carbohydrate": 26.6000003815,
      "fat": 0.600000023842,
      "protein": 1.60000002384,
      "sodium": 0,
      "sugar": 0,
      "calcium": 0,
      "cholesterol": 0,
      "fiber": 3,
      "iron": 0,
      "monounsaturatedFat": 0,
      "polyunsaturatedFat": 0,
      "potassium": 0,
      "saturatedFat": 0,
      "vitaminA": 0,
      "vitaminC": 0,
      "createdAt": "2015-03-17T12:38:55.712Z",
      "updatedAt": "2015-03-17T12:38:55.712Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }, {
      "id": "54fe0ad40f701cf4630be975",
      "userId": "52e20cb2fff56aac62000001",
      "source": "jawbone",
      "timestamp": "2015-03-04T00:00:00.000Z",
      "type": "lunch",
      "name": "3 Cereais e Leite",
      "calories": 633.999984741,
      "carbohydrate": 75.9680023193,
      "fat": 22.6160001755,
      "protein": 32.4159994125,
      "sodium": 723.199981689,
      "sugar": 40.2880020142,
      "calcium": 896,
      "cholesterol": 54.4000015259,
      "fiber": 3.07999992371,
      "iron": 0,
      "monounsaturatedFat": 0,
      "polyunsaturatedFat": 0,
      "potassium": 0,
      "saturatedFat": 11.1679999828,
      "vitaminA": 0,
      "vitaminC": 0,
      "createdAt": "2015-03-09T21:04:20.973Z",
      "updatedAt": "2015-03-09T21:04:20.973Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }]

    // Get an meal by id
    request
    .get('https://api.humanapi.co/v1/human/food/meals/54fe0ad40f701cf4630be975?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json object as meal:
    {
      "id": "54fe0ad40f701cf4630be975",
      "userId": "52e20cb2fff56aac62000001",
      "source": "jawbone",
      "timestamp": "2015-03-04T00:00:00.000Z",
      "type": "lunch",
      "name": "3 Cereais e Leite",
      "calories": 633.999984741,
      "carbohydrate": 75.9680023193,
      "fat": 22.6160001755,
      "protein": 32.4159994125,
      "sodium": 723.199981689,
      "sugar": 40.2880020142,
      "calcium": 896,
      "cholesterol": 54.4000015259,
      "fiber": 3.07999992371,
      "iron": 0,
      "monounsaturatedFat": 0,
      "polyunsaturatedFat": 0,
      "potassium": 0,
      "saturatedFat": 11.1679999828,
      "vitaminA": 0,
      "vitaminC": 0,
      "createdAt": "2015-03-09T21:04:20.973Z",
      "updatedAt": "2015-03-09T21:04:20.973Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }

The Meals resource contains detailed data about nutrition consumed by the human separated by date and type: breakfast/lunch/dinner/other.

### Get Meals

Get a list of meals the user has

`GET https://api.humanapi.co/v1/human/food/meals`  

Returns a single meal

`GET https://api.humanapi.co/v1/human/food/meals/{id}`

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

</tbody>

</table>

Returns a list of meals updated since a given time

`GET https://api.humanapi.co/v1/human/food/meals`  

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

<tr>

<td>updated_since</td>

<td>Date in YYYYMMDDTHHmmssZ format</td>

</tr>

</tbody>

</table>

### Response properties

<table>

<thead>

<tr>

<th>Property</th>

<th>Units</th>

<th>Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>id</td>

<td>String</td>

<td>The id of the meal object</td>

</tr>

<tr>

<td>userId</td>

<td>String</td>

<td>The global Id of the user</td>

</tr>

<tr>

<td>humanId</td>

<td>String</td>

<td>Unique user identifier</td>

</tr>

<tr>

<td>timestamp</td>

<td>Date</td>

<td>The original date of the meal</td>

</tr>

<tr>

<td>source</td>

<td>String</td>

<td>The source service for the meal data</td>

</tr>

<tr>

<td>type</td>

<td>String</td>

<td>Type of the meal: breakfast/lunch/dinner/other</td>

</tr>

<tr>

<td>name</td>

<td>String</td>

<td>Descriptive name of the meal</td>

</tr>

<tr>

<td>calories</td>

<td>kcal</td>

<td>Number</td>

<td>The amount of consumed calories</td>

</tr>

<tr>

<td>carbohydrate</td>

<td>g</td>

<td>Number</td>

<td>The amount of consumed carbohydrate</td>

</tr>

<tr>

<td>fat</td>

<td>g</td>

<td>Number</td>

<td>The amount of consumed fat</td>

</tr>

<tr>

<td>protein</td>

<td>g</td>

<td>Number</td>

<td>The amount of consumed protein</td>

</tr>

<tr>

<td>sodium</td>

<td>g</td>

<td>Number</td>

<td>The amount of consumed sodium</td>

</tr>

<tr>

<td>sugar</td>

<td>g</td>

<td>Number</td>

<td>The amount of consumed sugar</td>

</tr>

<tr>

<td>fiber</td>

<td>g</td>

<td>Number</td>

<td>The amount of consumed fiber (optional)</td>

</tr>

<tr>

<td>saturatedFat</td>

<td>g</td>

<td>Number</td>

<td>The amount of consumed saturated fat (optional)</td>

</tr>

<tr>

<td>monounsaturatedFat</td>

<td>g</td>

<td>Number</td>

<td>The amount of consumed monounsaturated fat (optional)</td>

</tr>

<tr>

<td>polyunsaturatedFat</td>

<td>g</td>

<td>Number</td>

<td>The amount of consumed polyunsaturated fat (optional)</td>

</tr>

<tr>

<td>cholesterol</td>

<td>mg</td>

<td>Number</td>

<td>The amount of consumed cholesterol (optional)</td>

</tr>

<tr>

<td>vitaminA</td>

<td>mg</td>

<td>Number</td>

<td>The amount of consumed vitamin A (optional)</td>

</tr>

<tr>

<td>vitaminC</td>

<td>mg</td>

<td>Number</td>

<td>The amount of consumed vitamin C (optional)</td>

</tr>

<tr>

<td>calcium</td>

<td>mg</td>

<td>Number</td>

<td>The amount of consumed calcium (optional)</td>

</tr>

<tr>

<td>iron</td>

<td>mg</td>

<td>Number</td>

<td>The amount of consumed iron (optional)</td>

</tr>

<tr>

<td>potassium</td>

<td>mg</td>

<td>Number</td>

<td>The amount of consumed potassium (optional)</td>

</tr>

<tr>

<td>createdAt</td>

<td>Date</td>

<td>The time the measurement was created on the Human API server</td>

</tr>

<tr>

<td>updatedAt</td>

<td>Date</td>

<td>The time the measurement was updated on the Human API server</td>

</tr>

</tbody>

</table>

## Profile

    # Get user's health profile
    curl "https://api.humanapi.co/v1/human/profile?access_token=demo"

    # Returns a json object:
    {
      "userId": "52e20cb2fff56aac62000001",
      "createdAt": "2014-01-24T06:48:18.361Z",
      "email": "demo@humanapi.co",
      "defaultTimeZone": {
        "name": "UTC"
      },
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }

    // Get user's health profile
    var request = require('request');
    request
    .get('https://api.humanapi.co/v1/human/profile?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json object:
    {
      "userId": "52e20cb2fff56aac62000001",
      "createdAt": "2014-01-24T06:48:18.361Z",
      "email": "demo@humanapi.co",
      "defaultTimeZone": {
        "name": "UTC"
      },
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }

The Profile resource contains reference attributes that are convenient when you link a local app user from your application with a Human API user.

### Get Human Profile

Returns the human profile

`GET https://api.humanapi.co/v1/human/profile`  

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

</tbody>

</table>

### Response properties

<table>

<thead>

<tr>

<th>Property</th>

<th>Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>userId</td>

<td>String</td>

<td>The global Id of the user</td>

</tr>

<tr>

<td>humanId</td>

<td>String</td>

<td>Unique user identifier</td>

</tr>

<tr>

<td>email</td>

<td>String</td>

<td>The user’s email address, if known</td>

</tr>

<tr>

<td>createdAt</td>

<td>Date</td>

<td>The time the measurement was created on the Human API server</td>

</tr>

<tr>

<td>updatedAt</td>

<td>Date</td>

<td>The time the measurement was updated on the Human API server</td>

</tr>

</tbody>

</table>

## Sleeps

    # Get all sleeps associated with the user
    curl "https://api.humanapi.co/v1/human/sleeps?access_token=demo"

    OR

    curl "https://api.humanapi.co/v1/human/sleeps?access_token=demo&updated_since=20140110T000000Z"

    # Returns a json array of sleeps:
    [{
      "id": "551058f168d29c4133c4d1f4",
      "userId": "52e20cb2fff56aac62000001",
      "day": "2015-03-23",
      "startTime": "2015-03-23T02:50:27.000Z",
      "endTime": "2015-03-23T09:21:54.000Z",
      "tzOffset": "-07:00",
      "source": "jawbone",
      "mainSleep": true,
      "timeAsleep": 362,
      "timeAwake": 28,
      "efficiency": 57,
      "timeToFallAsleep": 12,
      "timeAfterWakeup": 0,
      "timeInBed": 391,
      "createdAt": "2015-03-23T18:18:25.687Z",
      "updatedAt": "2015-03-23T18:18:25.687Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }, {
      "id": "550facab68d29c4133b4cf18",
      "userId": "52e20cb2fff56aac62000001",
      "day": "2015-03-23",
      "startTime": "2015-03-22T23:07:33.000Z",
      "endTime": "2015-03-23T06:54:35.000Z",
      "tzOffset": "+01:00",
      "source": "jawbone",
      "mainSleep": true,
      "timeAsleep": 442,
      "timeAwake": 24,
      "efficiency": 80,
      "timeToFallAsleep": 4,
      "timeAfterWakeup": 0,
      "timeInBed": 467,
      "createdAt": "2015-03-23T06:03:23.091Z",
      "updatedAt": "2015-03-23T06:03:23.091Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }, {
      "id": "550f8daf68d29c4133b122b5",
      "userId": "52e20cb2fff56aac62000001",
      "day": "2015-03-22",
      "startTime": "2015-03-22T04:06:58.000Z",
      "endTime": "2015-03-22T13:36:10.000Z",
      "tzOffset": "-04:00",
      "source": "jawbone",
      "mainSleep": true,
      "timeAsleep": 377,
      "timeAwake": 180,
      "efficiency": 68,
      "timeToFallAsleep": 16,
      "timeAfterWakeup": 0,
      "timeInBed": 569,
      "createdAt": "2015-03-23T03:51:11.219Z",
      "updatedAt": "2015-03-23T03:51:11.219Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }]

    # Get an sleep by id
    curl "https://api.humanapi.co/v1/human/sleeps/550f8daf68d29c4133b122b5?access_token=demo"

    # Returns a json object as sleep:
    {
      "id": "550f8daf68d29c4133b122b5",
      "userId": "52e20cb2fff56aac62000001",
      "day": "2015-03-22",
      "startTime": "2015-03-22T04:06:58.000Z",
      "endTime": "2015-03-22T13:36:10.000Z",
      "tzOffset": "-04:00",
      "source": "jawbone",
      "mainSleep": true,
      "timeAsleep": 377,
      "timeAwake": 180,
      "efficiency": 68,
      "timeToFallAsleep": 16,
      "timeAfterWakeup": 0,
      "timeInBed": 569,
      "createdAt": "2015-03-23T03:51:11.219Z",
      "updatedAt": "2015-03-23T03:51:11.219Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }

    // Get all sleeps associated with the user
    var request = require('request');
    request
    .get('https://api.humanapi.co/v1/human/sleeps?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    OR

    request
    .get('https://api.humanapi.co/v1/human/sleeps?access_token=demo&updated_since=20140110T000000Z')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json array of sleeps:
    [{
      "id": "551058f168d29c4133c4d1f4",
      "userId": "52e20cb2fff56aac62000001",
      "day": "2015-03-23",
      "startTime": "2015-03-23T02:50:27.000Z",
      "endTime": "2015-03-23T09:21:54.000Z",
      "tzOffset": "-07:00",
      "source": "jawbone",
      "mainSleep": true,
      "timeAsleep": 362,
      "timeAwake": 28,
      "efficiency": 57,
      "timeToFallAsleep": 12,
      "timeAfterWakeup": 0,
      "timeInBed": 391,
      "createdAt": "2015-03-23T18:18:25.687Z",
      "updatedAt": "2015-03-23T18:18:25.687Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }, {
      "id": "550facab68d29c4133b4cf18",
      "userId": "52e20cb2fff56aac62000001",
      "day": "2015-03-23",
      "startTime": "2015-03-22T23:07:33.000Z",
      "endTime": "2015-03-23T06:54:35.000Z",
      "tzOffset": "+01:00",
      "source": "jawbone",
      "mainSleep": true,
      "timeAsleep": 442,
      "timeAwake": 24,
      "efficiency": 80,
      "timeToFallAsleep": 4,
      "timeAfterWakeup": 0,
      "timeInBed": 467,
      "createdAt": "2015-03-23T06:03:23.091Z",
      "updatedAt": "2015-03-23T06:03:23.091Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }, {
      "id": "550f8daf68d29c4133b122b5",
      "userId": "52e20cb2fff56aac62000001",
      "day": "2015-03-22",
      "startTime": "2015-03-22T04:06:58.000Z",
      "endTime": "2015-03-22T13:36:10.000Z",
      "tzOffset": "-04:00",
      "source": "jawbone",
      "mainSleep": true,
      "timeAsleep": 377,
      "timeAwake": 180,
      "efficiency": 68,
      "timeToFallAsleep": 16,
      "timeAfterWakeup": 0,
      "timeInBed": 569,
      "createdAt": "2015-03-23T03:51:11.219Z",
      "updatedAt": "2015-03-23T03:51:11.219Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }]

    // Get an sleep by id
    request
    .get('https://api.humanapi.co/v1/human/sleeps/550f8daf68d29c4133b122b5?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json object as sleep:
    {
      "id": "550f8daf68d29c4133b122b5",
      "userId": "52e20cb2fff56aac62000001",
      "day": "2015-03-22",
      "startTime": "2015-03-22T04:06:58.000Z",
      "endTime": "2015-03-22T13:36:10.000Z",
      "tzOffset": "-04:00",
      "source": "jawbone",
      "mainSleep": true,
      "timeAsleep": 377,
      "timeAwake": 180,
      "efficiency": 68,
      "timeToFallAsleep": 16,
      "timeAfterWakeup": 0,
      "timeInBed": 569,
      "createdAt": "2015-03-23T03:51:11.219Z",
      "updatedAt": "2015-03-23T03:51:11.219Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }

The Sleep resource are segments of sleep that occur during a specific period of time.

### Get Sleeps

Get a list of sleeps the user has

`GET https://api.humanapi.co/v1/human/sleeps`  

Returns a single sleep

`GET https://api.humanapi.co/v1/human/sleeps/{id}`

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

</tbody>

</table>

Returns a list of sleeps updated since a given time

`GET https://api.humanapi.co/v1/human/sleeps`  

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

<tr>

<td>updated_since</td>

<td>Date in YYYYMMDDTHHmmssZ format</td>

</tr>

</tbody>

</table>

### Response properties

<table>

<thead>

<tr>

<th>Property</th>

<th>Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>id</td>

<td>String</td>

<td>The id of the sleep measurement</td>

</tr>

<tr>

<td>userId</td>

<td>String</td>

<td>The global Id of the user</td>

</tr>

<tr>

<td>humanId</td>

<td>String</td>

<td>Unique user identifier</td>

</tr>

<tr>

<td>day</td>

<td>Date</td>

<td>The day the sleep was recorded</td>

</tr>

<tr>

<td>startTime</td>

<td>Date</td>

<td>The original start date and time of the sleep segment in UTC time</td>

</tr>

<tr>

<td>endTime</td>

<td>Date</td>

<td>The original end date and time of the sleep segment in UTC time</td>

</tr>

<tr>

<td>tzOffset</td>

<td>String</td>

<td>The offset from UTC time in +/-hh:mm (e.g. -04:00)</td>

</tr>

<tr>

<td>source</td>

<td>String</td>

<td>The source service for the measurement, where it was created</td>

</tr>

<tr>

<td>mainSleep</td>

<td>Boolean</td>

<td>A boolean value indicating if this sleep was the main sleep of the day</td>

</tr>

<tr>

<td>timeAsleep</td>

<td>Number</td>

<td>The time asleep during the segment (in minutes)</td>

</tr>

<tr>

<td>timeAwake</td>

<td>Number</td>

<td>The time awake during the segment (in minutes)</td>

</tr>

<tr>

<td>efficiency</td>

<td>Number</td>

<td>The efficiency score</td>

</tr>

<tr>

<td>timeToFallAsleep</td>

<td>Number</td>

<td>The number of minutes it took to fall asleep</td>

</tr>

<tr>

<td>timeAfterWakeup</td>

<td>Number</td>

<td>The number of minutes in bed after waking up</td>

</tr>

<tr>

<td>timeInBed</td>

<td>Number</td>

<td>The total number of minutes spend in bed</td>

</tr>

<tr>

<td>createdAt</td>

<td>Date</td>

<td>The time the sleep was created on the Human API server</td>

</tr>

<tr>

<td>updatedAt</td>

<td>Date</td>

<td>The time the sleep was updated on the Human API server</td>

</tr>

<tr>

<td>timeSeries</td>

<td>Object</td>

<td>Time series data for the sleep, such as quality (see below)</td>

</tr>

</tbody>

</table>

### Time Series

Add the query parameter time_series=true to retrieve the time series data if available.

The timeSeries object can have multiple embedded objects. In the example below there is one object called “quality”, some services might provide other properties such as “heartRate” or “breathing” etc.

The embedded objects have the following properties:

<table>

<thead>

<tr>

<th>Property</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>type</td>

<td>Type of data, indexed with fixed time interval or timestamped.</td>

</tr>

<tr>

<td>intervalInMillis</td>

<td>If the type is indexed this indicates the number of milliseconds between each value.</td>

</tr>

<tr>

<td>values</td>

<td>The array of values, single values (indexed) or key-value pairs (timestamped).</td>

</tr>

</tbody>

</table>

## Sleep Summaries

    # Get all sleeps summaries associated with the user
    curl "https://api.humanapi.co/v1/human/sleeps/summaries?access_token=demo"

    OR

    curl "https://api.humanapi.co/v1/human/sleeps/summaries?access_token=demo&updated_since=20140110T000000Z"

    # Returns a json array of sleeps summaries:
    [{
      "id": "550facab68d29c4133b4cf22",
      "userId": "52e20cb2fff56aac62000001",
      "date": "2015-03-23",
      "source": "jawbone",
      "timeAsleep": 391.45,
      "timeAwake": 28,
      "createdAt": "2015-03-23T06:03:23.141Z",
      "updatedAt": "2015-03-23T18:18:25.678Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }, {
      "id": "550e92b25ee14de827d16689",
      "userId": "52e20cb2fff56aac62000001",
      "date": "2015-03-22",
      "source": "jawbone",
      "timeAsleep": 443.6,
      "timeAwake": 23,
      "createdAt": "2015-03-22T10:00:18.804Z",
      "updatedAt": "2015-03-23T15:05:28.591Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }, {
      "id": "550d599c47be23e06c83bbde",
      "userId": "52e20cb2fff56aac62000001",
      "date": "2015-03-21",
      "source": "jawbone",
      "timeAsleep": 412.5,
      "timeAwake": 96,
      "createdAt": "2015-03-21T11:44:28.969Z",
      "updatedAt": "2015-03-23T15:05:28.599Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }]

    # Get an sleep summary by id
    curl "https://api.humanapi.co/v1/human/sleeps/summaries/550d599c47be23e06c83bbde?access_token=demo"

    # Returns a json object as sleep summary:
    {
      "id": "550d599c47be23e06c83bbde",
      "userId": "52e20cb2fff56aac62000001",
      "date": "2015-03-21",
      "source": "jawbone",
      "timeAsleep": 412.5,
      "timeAwake": 96,
      "createdAt": "2015-03-21T11:44:28.969Z",
      "updatedAt": "2015-03-23T15:05:28.599Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }

    // Get all sleeps summaries associated with the user
    var request = require('request');
    request
    .get('https://api.humanapi.co/v1/human/sleeps/summaries?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    OR

    request
    .get('https://api.humanapi.co/v1/human/sleeps/summaries?access_token=demo&updated_since=20140110T000000Z')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json array of sleeps summaries:
    [{
      "id": "550facab68d29c4133b4cf22",
      "userId": "52e20cb2fff56aac62000001",
      "date": "2015-03-23",
      "source": "jawbone",
      "timeAsleep": 391.45,
      "timeAwake": 28,
      "createdAt": "2015-03-23T06:03:23.141Z",
      "updatedAt": "2015-03-23T18:18:25.678Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }, {
      "id": "550e92b25ee14de827d16689",
      "userId": "52e20cb2fff56aac62000001",
      "date": "2015-03-22",
      "source": "jawbone",
      "timeAsleep": 443.6,
      "timeAwake": 23,
      "createdAt": "2015-03-22T10:00:18.804Z",
      "updatedAt": "2015-03-23T15:05:28.591Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }, {
      "id": "550d599c47be23e06c83bbde",
      "userId": "52e20cb2fff56aac62000001",
      "date": "2015-03-21",
      "source": "jawbone",
      "timeAsleep": 412.5,
      "timeAwake": 96,
      "createdAt": "2015-03-21T11:44:28.969Z",
      "updatedAt": "2015-03-23T15:05:28.599Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }]

    // Get an sleep summary by id
    request
    .get('https://api.humanapi.co/v1/human/sleeps/summaries/550d599c47be23e06c83bbde?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json object as sleep summary:
    {
      "id": "550d599c47be23e06c83bbde",
      "userId": "52e20cb2fff56aac62000001",
      "date": "2015-03-21",
      "source": "jawbone",
      "timeAsleep": 412.5,
      "timeAwake": 96,
      "createdAt": "2015-03-21T11:44:28.969Z",
      "updatedAt": "2015-03-23T15:05:28.599Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }

Get a list of daily summaries of sleeps summaries reported by the data sources.

### Get Sleep Summaries

Returns a list of sleeps summaries

`GET https://api.humanapi.co/v1/human/sleeps/summaries`  

Returns a single sleep summary

`GET https://api.humanapi.co/v1/human/sleeps/summaries/{id}`

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

</tbody>

</table>

Returns a list of sleeps summaries updated since a given time

`GET https://api.humanapi.co/v1/human/sleeps/summaries`  

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

<tr>

<td>updated_since</td>

<td>Date in YYYYMMDDTHHmmssZ format</td>

</tr>

</tbody>

</table>

### Response properties

<table>

<thead>

<tr>

<th>Property</th>

<th>Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>id</td>

<td>String</td>

<td>The Id of the resource</td>

</tr>

<tr>

<td>userId</td>

<td>String</td>

<td>The global Id of the user</td>

</tr>

<tr>

<td>humanId</td>

<td>String</td>

<td>Unique user identifier</td>

</tr>

<tr>

<td>date</td>

<td>Date</td>

<td>The date of the sleep</td>

</tr>

<tr>

<td>source</td>

<td>String</td>

<td>The name of the originating service</td>

</tr>

<tr>

<td>timeAsleep</td>

<td>Number</td>

<td>The time asleep during the segment (in minutes)</td>

</tr>

<tr>

<td>timeAwake</td>

<td>Number</td>

<td>The time awake during the segment (in minutes)</td>

</tr>

<tr>

<td>createdAt</td>

<td>Date</td>

<td>The time the sleep was created on the Human API server</td>

</tr>

<tr>

<td>updatedAt</td>

<td>Date</td>

<td>The time the sleep was updated on the Human API server</td>

</tr>

</tbody>

</table>

## Weight

    # Get weight measurement associated with the user
    curl "https://api.humanapi.co/v1/human/weight?access_token=demo"

    # Returns a json object of weight:
    {
      "id": "5506f5080ba9b35f0af6cc8a",
      "userId": "52e20cb2fff56aac62000001",
      "timestamp": "2015-03-16T23:59:59.000Z",
      "value": 92.9,
      "unit": "kg",
      "source": "fitbit",
      "sourceData": {},
      "createdAt": "2015-03-16T15:21:44.289Z",
      "updatedAt": "2015-03-16T15:24:48.475Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }

    // Get all weight associated with the user
    var request = require('request');
    request
    .get('https://api.humanapi.co/v1/human/weight?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json object of weight:
    {
      "id": "5506f5080ba9b35f0af6cc8a",
      "userId": "52e20cb2fff56aac62000001",
      "timestamp": "2015-03-16T23:59:59.000Z",
      "value": 92.9,
      "unit": "kg",
      "source": "fitbit",
      "sourceData": {},
      "createdAt": "2015-03-16T15:21:44.289Z",
      "updatedAt": "2015-03-16T15:24:48.475Z",
      "humanId": "5dc2527186aaf9de560e5841f1a44bd6"
    }

Get the weight measurement from a specific point in time.

### Get Weight

Returns a weight reading

`GET https://api.humanapi.co/v1/human/weight`  

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

</tbody>

</table>

### Response properties

<table>

<thead>

<tr>

<th>Property</th>

<th>Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>id</td>

<td>String</td>

<td>The id of the bmi reading</td>

</tr>

<tr>

<td>userId</td>

<td>String</td>

<td>The global Id of the user</td>

</tr>

<tr>

<td>humanId</td>

<td>String</td>

<td>Unique user identifier</td>

</tr>

<tr>

<td>timestamp</td>

<td>Date</td>

<td>The original date and time of the measurement</td>

</tr>

<tr>

<td>source</td>

<td>String</td>

<td>The source service for the measurement, where it was created</td>

</tr>

<tr>

<td>value</td>

<td>Number</td>

<td>The value of the measurement in the unit specified</td>

</tr>

<tr>

<td>unit</td>

<td>String</td>

<td>The unit of the measurement value</td>

</tr>

<tr>

<td>createdAt</td>

<td>Date</td>

<td>The time the measurement was created on the Human API server</td>

</tr>

<tr>

<td>updatedAt</td>

<td>Date</td>

<td>The time the measurement was updated on the Human API server</td>

</tr>

</tbody>

</table>

# Utility APIs

## Sources

    # Get sources measurement associated with the user
    curl "https://api.humanapi.co/v1/human/sources?access_token=demo"

    # Returns a json object of sources:
    [{
      "source": "bodymedia",
      "supportedDataTypes": ["activities"],
      "connectedSince": "2014-07-24T14:59:31.756Z",
      "devices": ["Bodymedia LINK"],
      "historySync": {
        "status": "not_started"
      },
      "syncStatus": {
        "status": "not_started"
      }
    }, {
      "source": "fitbit",
      "supportedDataTypes": ["activities", "sleeps"],
      "connectedSince": "2014-09-15T13:30:23.571Z",
      "devices": ["One"],
      "historySync": {
        "status": "completed",
        "oldestDate": "1970-01-01T00:00:00.000Z"
      },
      "syncStatus": {
        "status": "ok",
        "synchedAt": "2015-03-20T00:05:49.582Z",
        "newestDate": "2015-03-20T00:00:00.000Z"
      }
    }]

    // Get all sources associated with the user
    var request = require('request');
    request
    .get('https://api.humanapi.co/v1/human/sources?access_token=demo')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json object of sources:
    [{
      "source": "bodymedia",
      "supportedDataTypes": ["activities"],
      "connectedSince": "2014-07-24T14:59:31.756Z",
      "devices": ["Bodymedia LINK"],
      "historySync": {
        "status": "not_started"
      },
      "syncStatus": {
        "status": "not_started"
      }
    }, {
      "source": "fitbit",
      "supportedDataTypes": ["activities", "sleeps"],
      "connectedSince": "2014-09-15T13:30:23.571Z",
      "devices": ["One"],
      "historySync": {
        "status": "completed",
        "oldestDate": "1970-01-01T00:00:00.000Z"
      },
      "syncStatus": {
        "status": "ok",
        "synchedAt": "2015-03-20T00:05:49.582Z",
        "newestDate": "2015-03-20T00:00:00.000Z"
      }
    }]

The Sources endpoint returns the synchronization status of the different services that a specific user has connected To see a more detailed description on how this can be used see the documentation [here](http://hub.humanapi.co/v1.0/docs/source-sync-status).

### Get Sources

Returns a sources reading

`GET https://api.humanapi.co/v1/human/sources`  

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>access_token</td>

<td>Returned for each user from a successful authentication</td>

</tr>

</tbody>

</table>

### Response properties

<table>

<thead>

<tr>

<th>Property</th>

<th>Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>id</td>

<td>String</td>

<td>The id of the bmi reading</td>

</tr>

<tr>

<td>userId</td>

<td>String</td>

<td>The global Id of the user</td>

</tr>

<tr>

<td>humanId</td>

<td>String</td>

<td>Unique user identifier</td>

</tr>

<tr>

<td>timestamp</td>

<td>Date</td>

<td>The original date and time of the measurement</td>

</tr>

<tr>

<td>source</td>

<td>String</td>

<td>The source service for the measurement, where it was created</td>

</tr>

<tr>

<td>value</td>

<td>Number</td>

<td>The value of the measurement in the unit specified</td>

</tr>

<tr>

<td>unit</td>

<td>String</td>

<td>The unit of the measurement value</td>

</tr>

<tr>

<td>createdAt</td>

<td>Date</td>

<td>The time the measurement was created on the Human API server</td>

</tr>

<tr>

<td>updatedAt</td>

<td>Date</td>

<td>The time the measurement was updated on the Human API server</td>

</tr>

</tbody>

</table>

## Users

    # Get all users associated with an app.
    curl -X GET -H 'Accept: application/json' \
        -u e7db255f4828e1d482743eba04faacb945ab7ca8: \
        https://api.humanapi.co/v1/apps/1d129c20acf6fcef9be0b067cc7859d872ed5ade/users

    # Returns a json array of users
    [
      {
        "humanId": "0a34dca6b9fe0b2361038bd371d96539",
        "externalId": "sample.user1@example.com",
        "appId": "1d129c20acf6fcef9be0b067cc7859d872ed5ade",
        "createdAt": "2013-12-28T18:46:10.436Z",
        "updatedAt": "2013-12-29T05:05:50.601Z"
      },
      {
        "humanId": "8bd9999a4f902e609343dcb850361f23",
        "externalId": "sample.user2@example.com",
        "appId": "1d129c20acf6fcef9be0b067cc7859d872ed5ade",
        "createdAt": "2013-12-28T23:46:37.701Z",
        "updatedAt": "2013-12-28T23:46:37.701Z"
      },
      {
        "humanId": "f29b82027297553353acbf4901683830",
        "externalId": "sample.user3@example.com",
        "appId": "1d129c20acf6fcef9be0b067cc7859d872ed5ade",
        "createdAt": "2013-12-29T05:08:11.610Z",
        "updatedAt": "2013-12-29T05:08:11.610Z"
      }
    ]

    # Get a specific user associated with and app
    curl -X GET -H 'Accept: application/json' \
        -u e7db255f4828e1d482743eba04faacb945ab7ca8: \
        https://api.humanapi.co/v1/apps/1d129c20acf6fcef9be0b067cc7859d872ed5ade/users/0a34dca6b9fe0b2361038bd371d96539

    # Returns a json object

    {
      "humanId": "0a34dca6b9fe0b2361038bd371d96539",
      "externalId": "sample.user1@example.com",
      "appId": "1d129c20acf6fcef9be0b067cc7859d872ed5ade",
      "createdAt": "2013-12-28T18:46:10.436Z",
      "updatedAt": "2013-12-29T05:05:50.601Z"
    }

    # Delete a user
    curl -X DELETE -u e7db255f4828e1d482743eba04faacb945ab7ca8: \
    https://api.humanapi.co/v1/apps/1d129c20acf6fcef9be0b067cc7859d872ed5ade/users/0a34dca6b9fe0b2361038bd371d96539

    # Returns status code
    204

    var request = require('request');

    // Get all users associated with an app
    request
    .get('https://api.humanapi.co/v1/apps/1d129c20acf6fcef9be0b067cc7859d872ed5ade/users')
    .auth('e7db255f4828e1d482743eba04faacb945ab7ca8','')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    //Returns a json array of users
    [
      {
        "humanId": "0a34dca6b9fe0b2361038bd371d96539",
        "externalId": "sample.user1@example.com",
        "appId": "1d129c20acf6fcef9be0b067cc7859d872ed5ade",
        "createdAt": "2013-12-28T18:46:10.436Z",
        "updatedAt": "2013-12-29T05:05:50.601Z"
      },
      {
        "humanId": "8bd9999a4f902e609343dcb850361f23",
        "externalId": "sample.user2@example.com",
        "appId": "1d129c20acf6fcef9be0b067cc7859d872ed5ade",
        "createdAt": "2013-12-28T23:46:37.701Z",
        "updatedAt": "2013-12-28T23:46:37.701Z"
      },
      {
        "humanId": "f29b82027297553353acbf4901683830",
        "externalId": "sample.user3@example.com",
        "appId": "1d129c20acf6fcef9be0b067cc7859d872ed5ade",
        "createdAt": "2013-12-29T05:08:11.610Z",
        "updatedAt": "2013-12-29T05:08:11.610Z"
      }
    ]

    //Get a specific user associated with an app
    request
    .get('https://api.humanapi.co/v1/apps/1d129c20acf6fcef9be0b067cc7859d872ed5ade/users/0a34dca6b9fe0b2361038bd371d96539')
    .auth('e7db255f4828e1d482743eba04faacb945ab7ca8','')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    //Returns a json object

    {
      "humanId": "0a34dca6b9fe0b2361038bd371d96539",
      "externalId": "sample.user1@example.com",
      "appId": "1d129c20acf6fcef9be0b067cc7859d872ed5ade",
      "createdAt": "2013-12-28T18:46:10.436Z",
      "updatedAt": "2013-12-29T05:05:50.601Z"
    }

    //Delete a user
    request
    .del('https://api.humanapi.co/v1/apps/1d129c20acf6fcef9be0b067cc7859d872ed5ade/users/0a34dca6b9fe0b2361038bd371d96539')
    .auth('e7db255f4828e1d482743eba04faacb945ab7ca8','')
    .on('response', function(response) {
        console.log(response.statusCode)
    });

    //Returns status code
    204

The users endpoint returns users of a specific app.

Users for your app are primarily created using [Human Connect](http://hub.humanapi.co/v1.0/docs/overview-of-human-connect). This endpoint is best used for management of existing users.

As a part of the [Application API](http://hub.humanapi.co/v1.0/docs/application-api) this endpoint is authenticated using Basic Authentication. You must use your App Key as username and a blank password. _Make sure you include the “:” after the username to avoid being asked to enter a password._

For a more detailed description on how to manage your app’s users, see the documentation [here](http://hub.humanapi.co/v1.0/docs/managing-users).

### GET USERS

Returns a list of users

`GET https://api.humanapi.co/v1/apps/{appId}/users`

Returns a single user

`GET https://api.humanapi.co/v1/apps/{appId}/users/{humanId}`

### Response properties

<table>

<thead>

<tr>

<th>Property</th>

<th>Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>humanId</td>

<td>String</td>

<td>The id of the user in Human API</td>

</tr>

<tr>

<td>externalId</td>

<td>String</td>

<td>The id of the user provided from your application</td>

</tr>

<tr>

<td>appId</td>

<td>String</td>

<td>Unique app identifier found in the Developer Portal.</td>

</tr>

<tr>

<td>createdAt</td>

<td>Date</td>

<td>The time the measurement was created on the Human API server</td>

</tr>

<tr>

<td>updatedAt</td>

<td>Date</td>

<td>The time the measurement was updated on the Human API server</td>

</tr>

</tbody>

</table>

### DELETE A USER

Deletes a user

`DELETE https://api.humanapi.co/v1/apps/{appId}/users/{humanId}`

The response code for this is `204`

## Batch Query

    # Get activities for all of your users
    curl -X GET -H 'Accept: application/json' \
        -u e7db255f4828e1d482743eba04faacb945ab7ca8: \
    https://api.humanapi.co/v1/apps/1d129c20acf6fcef9be0b067cc7859d872ed5ade/users/activities

    # Returns a json array of activities
    [
      {
        id: "52867cbfde3155565f000b01",
        userId: "51cc7cb31a154bf215000002",
        humanId: "7eddd553c81b8de9ac271bc21f50e32e",
        startTime: "2013-11-01T17:27:00.346Z",
        endTime: "2013-11-01T18:27:11.346Z",
        type: "walking",
        source: "runkeeper",
        duration: 3611,
        distance: 1251,
        steps: 2689,
        calories: 482,
        timeZone: "America/Los_Angeles",
        sourceData: {},
        timeSeries: {},
        createdAt: "2013-11-01T17:27:00.346Z",
        updatedAt: "2013-11-01T17:27:00.346Z"
      },
      {
        id: "52867cbfde3155565f000b02",
        userId: "538b9d3a9b1e35be7302607e",
        humanId: "7eddd553c81b8de9ac271bc21f50e32e",
        startTime: "2014-05-31T16:18:19.000Z",
        endTime: "2014-05-31T19:00:27.000Z",
        type: "cycling",
        source: "strava",
        duration: 9728,
        distance: 45468.6,
        steps: 0,
        calories: 0,
        sourceData: {
          maxHeartrate: 196,
          averageHeartrate: 145,
          maxSpeed: 17.3,
          mapId: "a147785724",
          movingTime: 7782,
          type: "Ride"
        },
        createdAt: "2014-05-31T19:03:32.032Z",
        updatedAt: "2014-05-31T19:03:32.032Z"
      },
      {
        id: "52867cbfde3155565f000b03",
        userId: "538029c700850beb2601c578",
        humanId: "7eddd553c81b8de9ac271bc21f50e32e",
        startTime: "2014-05-28T20:04:02.000Z",
        endTime: "2014-05-29T00:00:00.000Z",
        type: "walking",
        source: "mapmyfitness",
        duration: 0,
        distance: 178636.74,
        steps: 0,
        calories: 1916,
        sourceData: { },
        createdAt: "2014-05-31T00:21:09.386Z",
        updatedAt: "2014-05-31T00:21:09.386Z"
      }
    ]

    // Get activities for all of your users
    var request = require('request');

    request
    .get('https://api.humanapi.co/v1/apps/1d129c20acf6fcef9be0b067cc7859d872ed5ade/users/activities')
    .auth('e7db255f4828e1d482743eba04faacb945ab7ca8','')
    .on('data', function(data) {
      console.log(JSON.parse(data));
    });

    // Returns a json array of activities
    [
      {
        id: "52867cbfde3155565f000b01",
        userId: "51cc7cb31a154bf215000002",
        humanId: "7eddd553c81b8de9ac271bc21f50e32e",
        startTime: "2013-11-01T17:27:00.346Z",
        endTime: "2013-11-01T18:27:11.346Z",
        type: "walking",
        source: "runkeeper",
        duration: 3611,
        distance: 1251,
        steps: 2689,
        calories: 482,
        timeZone: "America/Los_Angeles",
        sourceData: {},
        timeSeries: {},
        createdAt: "2013-11-01T17:27:00.346Z",
        updatedAt: "2013-11-01T17:27:00.346Z"
      },
      {
        id: "52867cbfde3155565f000b02",
        userId: "538b9d3a9b1e35be7302607e",
        humanId: "7eddd553c81b8de9ac271bc21f50e32e",
        startTime: "2014-05-31T16:18:19.000Z",
        endTime: "2014-05-31T19:00:27.000Z",
        type: "cycling",
        source: "strava",
        duration: 9728,
        distance: 45468.6,
        steps: 0,
        calories: 0,
        sourceData: {
          maxHeartrate: 196,
          averageHeartrate: 145,
          maxSpeed: 17.3,
          mapId: "a147785724",
          movingTime: 7782,
          type: "Ride"
        },
        createdAt: "2014-05-31T19:03:32.032Z",
        updatedAt: "2014-05-31T19:03:32.032Z"
      },
      {
        id: "52867cbfde3155565f000b03",
        userId: "538029c700850beb2601c578",
        humanId: "7eddd553c81b8de9ac271bc21f50e32e",
        startTime: "2014-05-28T20:04:02.000Z",
        endTime: "2014-05-29T00:00:00.000Z",
        type: "walking",
        source: "mapmyfitness",
        duration: 0,
        distance: 178636.74,
        steps: 0,
        calories: 1916,
        sourceData: { },
        createdAt: "2014-05-31T00:21:09.386Z",
        updatedAt: "2014-05-31T00:21:09.386Z"
      }
    ]

Returns data for all of your app’s users in one query per data type.

These endpoints replicate those available for the individual user queries. The payload data model is the same for individual and batch queries, except the batch query results return a list multiple users so you need to parse the data accordingly.

As a part of the [Application API](http://hub.humanapi.co/v1.0/docs/application-api) this endpoint is authenticated using Basic Authentication. You must use your App Key as username and a blank password. _Make sure you include the “:” after the username to avoid being asked to enter a password._

For a more detailed description of batch queries including best practices, see the documentation [here](http://hub.humanapi.co/v1.0/docs/batch-queries).

Activities

`/apps/:clientId/users/activities`

Activity Summaries

`/apps/:clientId/users/activities/summaries`

Heart Rates

`/apps/:clientId/users/heart_rates`

BMIs

`/apps/:clientId/users/bmis`

Body Fats

`/apps/:clientId/users/body_fats`

Heights

`/apps/:clientId/users/heights`

Weights

`/apps/:clientId/users/weights`

Blood Glucoses

`/apps/:clientId/users/blood_glucoses`

Blood Oxygens

`/apps/:clientId/users/blood_oxygens`

Sleeps

`/apps/:clientId/users/sleeps`

Blood Pressures

`/apps/:clientId/users/blood_pressures`

Genetic Traits

`/apps/:clientId/users/genetic_traits`

Locations

`/apps/:clientId/users/locations`

Meals

`/apps/:clientId/users/food/meals`

### Query Parameters

<table>

<thead>

<tr>

<th>Parameter</th>

<th>Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>updated_since</td>

<td>Date in YYYYMMDDTHHmmssZ format.</td>

<td>Use this in order to retrieve only the new data since your last query.</td>

</tr>

</tbody>

</table>

</div>

<div class="dark-box">

<div class="lang-selector">[shell](#) [javascript](#)</div>

</div>

</div>

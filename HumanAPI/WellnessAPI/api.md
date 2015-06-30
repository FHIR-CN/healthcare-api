## Activities

Activities are specific types of physical activities that occur during a period of time with a start time and an end time. These activities are often captured automatically using applications that track motion throughout the day, such as Moves, or they are recorded from applications such as Runkeeper or MapMyRun. They can also be manually entered with applications like Fitbit.
Get Activities

Get a list of activities the user has

GET https://api.humanapi.co/v1/human/activities

Returns a single activity

GET https://api.humanapi.co/v1/human/activities/{id}
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication

Returns a list of activities updated since a given time

GET https://api.humanapi.co/v1/human/activities
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication
updated_since 	Date in YYYYMMDDTHHmmssZ format
Response properties
Property 	Type 	Description
id 	String 	The Id of the resource
userId 	String 	The global Id of the user
humanId 	String 	Unique user identifier
startTime 	Date 	The start time of the activity in UTC time
endTime 	Date 	The end time of the activity in UTC time
tzOffset 	String 	The offset from UTC time in +/-hh:mm (e.g. -04:00)
type 	String 	The type of activity, such as walking, running, cycling
source 	String 	The name of the originating service
duration 	Number 	The duration in seconds
distance 	Number 	The distance in meters
steps 	Number 	The number of steps taken during the activity
calories 	Number 	The number of estimated calories burned during the activity
sourceData 	Object 	Additional data from the source that does not fit into the humanapi model. This data can include things such as “Fuel points” for the Nike service.
timeSeries 	Object 	Time series data of different data such as heart rate, gps location etc.



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

## Activity Summaries
Get a list of daily summaries of activities summaries reported by the data sources.
Get Activity Summaries

Returns a list of activities summaries

GET https://api.humanapi.co/v1/human/activities/summaries

Returns a single activity summary

GET https://api.humanapi.co/v1/human/activities/summaries/{id}
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication

Returns a list of activities summaries updated since a given time

GET https://api.humanapi.co/v1/human/activities/summaries
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication
updated_since 	Date in YYYYMMDDTHHmmssZ format
Response properties
Property 	Type 	Description
id 	String 	The Id of the resource
userId 	String 	The global Id of the user
humanId 	String 	Unique user identifier
date 	Date 	The date of the activity
source 	String 	The name of the originating service
duration 	Number 	The duration in seconds
distance 	Number 	The distance in meters
steps 	Number 	The number of steps taken during the activity
vigorous 	Number 	The number of minutes of vigorous activity
moderate 	Number 	The number of minutes of moderate activity
light 	Number 	The number of minutes of light activity
sedentary 	Number 	The number of minutes of sedentary activity
calories 	Number 	The number of estimated calories burned during the activity
sourceData 	Object 	Additional data from the source that does not fit into the humanapi model. This data can include things such as “Fuel points” for the Nike service.
timeSeries 	Object 	Time series data of different data such as heart rate, gps location etc.
createdAt 	Date 	The time the activity was created on the Human API server
updatedAt 	Date 	The time the activity was updated on the Human API server

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

## Blood Glucose
Get the blood glucose measurement from a specific point in time.
Get Blood Glucose

Returns a blood glucose reading

GET https://api.humanapi.co/v1/human/blood_glucose
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication
Response properties
Property 	Type 	Description
id 	String 	The id of the blood glucose reading
userId 	String 	The global Id of the user
humanId 	String 	Unique user identifier
timestamp 	Date 	The original date and time of the measurement
source 	String 	The source service for the measurement, where it was created
value 	Number 	The value of the measurement in the unit specified
unit 	String 	The unit of the measurement value
notes 	String 	User created notes
mealTag 	String 	Indication if value was captured before/after a meal
medicationTag 	String 	Indication if value was captured before/after taking medication
createdAt 	Date 	The time the measurement was created on the Human API server
updatedAt 	Date 	The time the measurement was updated on the Human API server
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

## Blood Oxygen
Get the blood oxygen measurement from a specific point in time.
Get Blood Oxygen

Returns a blood oxygen reading

GET https://api.humanapi.co/v1/human/blood_oxygen
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication
Response properties
Property 	Type 	Description
id 	String 	The id of the blood oxygen reading
userId 	String 	The global Id of the user
humanId 	String 	Unique user identifier
timestamp 	Date 	The original date and time of the measurement
source 	String 	The source service for the measurement, where it was created
value 	Number 	The value of the measurement in the unit specified
unit 	String 	The unit of the measurement value
createdAt 	Date 	The time the measurement was created on the Human API server
updatedAt 	Date 	The time the measurement was updated on the Human API server


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

## Blood Pressure
Get the blood pressure measurement from a specific point in time.
Get Blood Pressure

Returns a blood pressure reading

GET https://api.humanapi.co/v1/human/blood_pressure
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication
Response properties
Property 	Type 	Description
id 	String 	The id of the blood pressure reading
userId 	String 	The global Id of the user
humanId 	String 	Unique user identifier
timestamp 	Date 	The original date and time of the measurement
source 	String 	The source service for the measurement, where it was created
value 	Number 	The value of the measurement in the unit specified
unit 	String 	The unit of the measurement value
heartRate 	String 	The heart rate in BPM captured at the time of measurement
systolic 	String 	The systolic value captured at the time of measurement
diastolic 	String 	The diastolic value captured at the time of measurement
createdAt 	Date 	The time the measurement was created on the Human API server
updatedAt 	Date 	The time the measurement was updated on the Human API server

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

## Body Mass Index (BMI)

Get the bmi measurement from a specific point in time.
Get BMI

Returns a bmi reading

GET https://api.humanapi.co/v1/human/bmi
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication
Response properties
Property 	Type 	Description
id 	String 	The id of the bmi reading
userId 	String 	The global Id of the user
humanId 	String 	Unique user identifier
timestamp 	Date 	The original date and time of the measurement
source 	String 	The source service for the measurement, where it was created
value 	Number 	The value of the measurement in the unit specified
unit 	String 	The unit of the measurement value
createdAt 	Date 	The time the measurement was created on the Human API server
updatedAt 	Date 	The time the measurement was updated on the Human API server


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


## Body Fat

Get the body fat measurement from a specific point in time.
Get Body Fat

Returns a body fat reading

GET https://api.humanapi.co/v1/human/body_fat
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication
Response properties
Property 	Type 	Description
id 	String 	The id of the bmi reading
userId 	String 	The global Id of the user
humanId 	String 	Unique user identifier
timestamp 	Date 	The original date and time of the measurement
source 	String 	The source service for the measurement, where it was created
value 	Number 	The value of the measurement in the unit specified
unit 	String 	The unit of the measurement value
createdAt 	Date 	The time the measurement was created on the Human API server
updatedAt 	Date 	The time the measurement was updated on the Human API server

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

## Genetics

The Genetics resource is a simplified version of the genetic model data we store. Today it returns a list of genetic traits for a user.
Get Genetic Traits

Returns a genetic traits reading

GET https://api.humanapi.co/v1/human/genetic/traits
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication
Response properties
Property 	Type 	Description
userId 	String 	The global Id of the user
humanId 	String 	Unique user identifier
trait 	Date 	The most likely present trait
possibleTraits 	String 	A list of all the possible values for a specific trait, for easy comparison
description 	Number 	A description/name of the trait

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


## Heart Rate
Get the heart rate measurement from a specific point in time.
Get Heart Rate

Returns a heart rate reading

GET https://api.humanapi.co/v1/human/heart_rate
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication
Response properties
Property 	Type 	Description
id 	String 	The id of the bmi reading
userId 	String 	The global Id of the user
humanId 	String 	Unique user identifier
timestamp 	Date 	The original date and time of the measurement
source 	String 	The source service for the measurement, where it was created
value 	Number 	The value of the measurement in the unit specified
unit 	String 	The unit of the measurement value
createdAt 	Date 	The time the measurement was created on the Human API server
updatedAt 	Date 	The time the measurement was updated on the Human API server

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

## Height
Get the height measurement from a specific point in time.
Get Height

Returns a height reading

GET https://api.humanapi.co/v1/human/height
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication
Response properties
Property 	Type 	Description
id 	String 	The id of the bmi reading
userId 	String 	The global Id of the user
humanId 	String 	Unique user identifier
timestamp 	Date 	The original date and time of the measurement
source 	String 	The source service for the measurement, where it was created
value 	Number 	The value of the measurement in the unit specified
unit 	String 	The unit of the measurement value
createdAt 	Date 	The time the measurement was created on the Human API server
updatedAt 	Date 	The time the measurement was updated on the Human API server

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

## Human (Summary)
Get summary object for a person’s latest health metrics. The data consists of all the discrete health measurements and summary information about the person’s activity and sleep data.
Get Human Summary

Returns the human profile

GET https://api.humanapi.co/v1/human
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication
Response properties
Property 	Type 	Description
userId 	String 	The global Id of the user
humanId 	String 	Unique user identifier
gender 	String 	The gender of the person, if known
bloodGlucose 	BloodGlucose 	The latest blood glucose measurement
bloodPressure 	BloodPressure 	The latest blood pressure measurement
bmi 	Bmi 	The latest body mass index measurement
bodyFat 	BodyFat 	The latest body fat percentage measurement
height 	Height 	The latest height measurement
heartRate 	HeartRate 	The latest heart rate measurement
weight 	Weight 	The latest weight measurement
activitySummary 	ActivitySummary 	The latest activity summary
sleepSummary 	SleepSummary 	The latest sleep summary

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

## Locations
Get the locations measurement from a specific point in time.
Get Locations

Returns a locations readings

GET https://api.humanapi.co/v1/human/locations
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication
Response properties
Property 	Type 	Description
id 	String 	The id of the bmi reading
userId 	String 	The global Id of the user
humanId 	String 	Unique user identifier
timestamp 	Date 	The original date and time of the measurement
name 	String 	The name of the place
source 	String 	The source service for the measurement, where it was created
type 	String 	The type of location
location 	Object 	The coordinate point with a lat/lon value
createdAt 	Date 	The time the measurement was created on the Human API server
updatedAt 	Date 	The time the measurement was updated on the Human API server

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

## Meals
The Meals resource contains detailed data about nutrition consumed by the human separated by date and type: breakfast/lunch/dinner/other.
Get Meals

Get a list of meals the user has

GET https://api.humanapi.co/v1/human/food/meals

Returns a single meal

GET https://api.humanapi.co/v1/human/food/meals/{id}
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication

Returns a list of meals updated since a given time

GET https://api.humanapi.co/v1/human/food/meals
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication
updated_since 	Date in YYYYMMDDTHHmmssZ format
Response properties
Property 	Units 	Type 	Description
id 		String 	The id of the meal object
userId 		String 	The global Id of the user
humanId 		String 	Unique user identifier
timestamp 		Date 	The original date of the meal
source 		String 	The source service for the meal data
type 		String 	Type of the meal: breakfast/lunch/dinner/other
name 		String 	Descriptive name of the meal
calories 	kcal 	Number 	The amount of consumed calories
carbohydrate 	g 	Number 	The amount of consumed carbohydrate
fat 	g 	Number 	The amount of consumed fat
protein 	g 	Number 	The amount of consumed protein
sodium 	g 	Number 	The amount of consumed sodium
sugar 	g 	Number 	The amount of consumed sugar
fiber 	g 	Number 	The amount of consumed fiber (optional)
saturatedFat 	g 	Number 	The amount of consumed saturated fat (optional)
monounsaturatedFat 	g 	Number 	The amount of consumed monounsaturated fat (optional)
polyunsaturatedFat 	g 	Number 	The amount of consumed polyunsaturated fat (optional)
cholesterol 	mg 	Number 	The amount of consumed cholesterol (optional)
vitaminA 	mg 	Number 	The amount of consumed vitamin A (optional)
vitaminC 	mg 	Number 	The amount of consumed vitamin C (optional)
calcium 	mg 	Number 	The amount of consumed calcium (optional)
iron 	mg 	Number 	The amount of consumed iron (optional)
potassium 	mg 	Number 	The amount of consumed potassium (optional)
createdAt 		Date 	The time the measurement was created on the Human API server
updatedAt 		Date 	The time the measurement was updated on the Human API server

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

## Profile
The Profile resource contains reference attributes that are convenient when you link a local app user from your application with a Human API user.
Get Human Profile

Returns the human profile

GET https://api.humanapi.co/v1/human/profile
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication
Response properties
Property 	Type 	Description
userId 	String 	The global Id of the user
humanId 	String 	Unique user identifier
email 	String 	The user’s email address, if known
createdAt 	Date 	The time the measurement was created on the Human API server
updatedAt 	Date 	The time the measurement was updated on the Human API server

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

## Sleeps
The Sleep resource are segments of sleep that occur during a specific period of time.
Get Sleeps

Get a list of sleeps the user has

GET https://api.humanapi.co/v1/human/sleeps

Returns a single sleep

GET https://api.humanapi.co/v1/human/sleeps/{id}
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication

Returns a list of sleeps updated since a given time

GET https://api.humanapi.co/v1/human/sleeps
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication
updated_since 	Date in YYYYMMDDTHHmmssZ format
Response properties
Property 	Type 	Description
id 	String 	The id of the sleep measurement
userId 	String 	The global Id of the user
humanId 	String 	Unique user identifier
day 	Date 	The day the sleep was recorded
startTime 	Date 	The original start date and time of the sleep segment in UTC time
endTime 	Date 	The original end date and time of the sleep segment in UTC time
tzOffset 	String 	The offset from UTC time in +/-hh:mm (e.g. -04:00)
source 	String 	The source service for the measurement, where it was created
mainSleep 	Boolean 	A boolean value indicating if this sleep was the main sleep of the day
timeAsleep 	Number 	The time asleep during the segment (in minutes)
timeAwake 	Number 	The time awake during the segment (in minutes)
efficiency 	Number 	The efficiency score
timeToFallAsleep 	Number 	The number of minutes it took to fall asleep
timeAfterWakeup 	Number 	The number of minutes in bed after waking up
timeInBed 	Number 	The total number of minutes spend in bed
createdAt 	Date 	The time the sleep was created on the Human API server
updatedAt 	Date 	The time the sleep was updated on the Human API server
timeSeries 	Object 	Time series data for the sleep, such as quality (see below)

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

## Sleep Summaries
Get a list of daily summaries of sleeps summaries reported by the data sources.
Get Sleep Summaries

Returns a list of sleeps summaries

GET https://api.humanapi.co/v1/human/sleeps/summaries

Returns a single sleep summary

GET https://api.humanapi.co/v1/human/sleeps/summaries/{id}
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication

Returns a list of sleeps summaries updated since a given time

GET https://api.humanapi.co/v1/human/sleeps/summaries
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication
updated_since 	Date in YYYYMMDDTHHmmssZ format
Response properties
Property 	Type 	Description
id 	String 	The Id of the resource
userId 	String 	The global Id of the user
humanId 	String 	Unique user identifier
date 	Date 	The date of the sleep
source 	String 	The name of the originating service
timeAsleep 	Number 	The time asleep during the segment (in minutes)
timeAwake 	Number 	The time awake during the segment (in minutes)
createdAt 	Date 	The time the sleep was created on the Human API server
updatedAt 	Date 	The time the sleep was updated on the Human API server

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


## Weight
Get the weight measurement from a specific point in time.
Get Weight

Returns a weight reading

GET https://api.humanapi.co/v1/human/weight
Query Parameters
Parameter 	Description
access_token 	Returned for each user from a successful authentication
Response properties
Property 	Type 	Description
id 	String 	The id of the bmi reading
userId 	String 	The global Id of the user
humanId 	String 	Unique user identifier
timestamp 	Date 	The original date and time of the measurement
source 	String 	The source service for the measurement, where it was created
value 	Number 	The value of the measurement in the unit specified
unit 	String 	The unit of the measurement value
createdAt 	Date 	The time the measurement was created on the Human API server
updatedAt 	Date 	The time the measurement was updated on the Human API server

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

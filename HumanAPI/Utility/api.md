## Sources
The Sources endpoint returns the synchronization status of the different services that a specific user has connected To see a more detailed description on how this can be used see the documentation here.
Get Sources

Returns a sources reading

GET https://api.humanapi.co/v1/human/sources
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


## Users
The users endpoint returns users of a specific app.

Users for your app are primarily created using Human Connect. This endpoint is best used for management of existing users.

As a part of the Application API this endpoint is authenticated using Basic Authentication. You must use your App Key as username and a blank password. Make sure you include the “:” after the username to avoid being asked to enter a password.

For a more detailed description on how to manage your app’s users, see the documentation here.
GET USERS

Returns a list of users

GET https://api.humanapi.co/v1/apps/{appId}/users

Returns a single user

GET https://api.humanapi.co/v1/apps/{appId}/users/{humanId}
Response properties
Property 	Type 	Description
humanId 	String 	The id of the user in Human API
externalId 	String 	The id of the user provided from your application
appId 	String 	Unique app identifier found in the Developer Portal.
createdAt 	Date 	The time the measurement was created on the Human API server
updatedAt 	Date 	The time the measurement was updated on the Human API server
DELETE A USER

Deletes a user

DELETE https://api.humanapi.co/v1/apps/{appId}/users/{humanId}

The response code for this is 204

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

## Batch Query

Returns data for all of your app’s users in one query per data type.

These endpoints replicate those available for the individual user queries. The payload data model is the same for individual and batch queries, except the batch query results return a list multiple users so you need to parse the data accordingly.

As a part of the Application API this endpoint is authenticated using Basic Authentication. You must use your App Key as username and a blank password. Make sure you include the “:” after the username to avoid being asked to enter a password.

For a more detailed description of batch queries including best practices, see the documentation here.

Activities

/apps/:clientId/users/activities

Activity Summaries

/apps/:clientId/users/activities/summaries

Heart Rates

/apps/:clientId/users/heart_rates

BMIs

/apps/:clientId/users/bmis

Body Fats

/apps/:clientId/users/body_fats

Heights

/apps/:clientId/users/heights

Weights

/apps/:clientId/users/weights

Blood Glucoses

/apps/:clientId/users/blood_glucoses

Blood Oxygens

/apps/:clientId/users/blood_oxygens

Sleeps

/apps/:clientId/users/sleeps

Blood Pressures

/apps/:clientId/users/blood_pressures

Genetic Traits

/apps/:clientId/users/genetic_traits

Locations

/apps/:clientId/users/locations

Meals

/apps/:clientId/users/food/meals
Query Parameters
Parameter 	Type 	Description
updated_since 	Date in YYYYMMDDTHHmmssZ format. 	Use this in order to retrieve only the new data since your last query.

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

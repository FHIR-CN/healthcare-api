

### Overview

The PokitDok API allows you to perform X12 transactions, find healthcare providers, and get information on health care procedure pricing. The API uses JavaScript Object Notation ([JSON](http://json.org/)) for requests and responses and also allows batch processing of [ASC X12](http://x12.org/) 5010 compatible [EDI](http://www.x12.org/about/faqs.cfm#a1) files. All API traffic is encrypted over HTTPS and authentication is handled with OAuth2.

The API is designed according to REST (Representational State Transfer) principles. The available API calls are organized into Resources. These resources are represented by endpoints that clients can invoke to describe and modify their state. All resources have the same available top level parameters available for finer grained access (described below). Here's an example of accessing a resource directly, using the "cURL" utility (info about cURL can be found [here](http://en.wikipedia.org/wiki/CURL)):

<div hljs="" class="hljs" language="bash">$curl -i https://platform.pokitdok.com/api/v4/providers/?limit=20 HTTP/1.1 200 OK Content-Type: application/json; charset=utf-8 { "meta": { "activity_id": "53d94ff156c02c67a0070096", "application_mode": "production", "rate_limit_cap": 1000, "rate_limit_reset": 1372700873, "rate_limit_amount": 122, "credits_billed": 1, "credits_remaining": 10000, "processing_time": 80, "next": "https://platform.pokitdok.com/api/v4/providers/?offset=20", "previous": null }, "data": [{ ... }] }</div>

The above is a truncated example that gets a list of providers from the Providers resource, and uses some common keyword arguments in the process. Notice that the response payload consists of a meta key and a data key. The meta key contains information regarding rate limiting, linked urls for convenient paging of lists, record counts, response time and API billing information. The data key contains an object, or list of objects returned from the resource.

The following is a list of values that are contained in the meta block, along with their types. Credit and rate limiting information is only included in the meta information for APIs that are billable and/or rate limited. Processing time is always included in the meta information.

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Key</th>

<th nowrap="" width="5%">Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>activity_id</td>

<td nowrap="">{uuid}</td>

<td>The id of the activity used to process the API request.</td>

</tr>

<tr>

<td>application_mode</td>

<td nowrap="">{string}</td>

<td>Indicates if the application is configured for test or production use.</td>

</tr>

<tr>

<td>credits_billed</td>

<td nowrap="">{int}</td>

<td>The amount of credits billed for this request</td>

</tr>

<tr>

<td>credits_remaining</td>

<td nowrap="">{int}</td>

<td>The amount of credits remaining on your API account</td>

</tr>

<tr>

<td>next</td>

<td nowrap="">{int}</td>

<td>A url pointing to the next page of results</td>

</tr>

<tr>

<td>previous</td>

<td nowrap="">{int}</td>

<td>A url pointing to the previous page of results</td>

</tr>

<tr>

<td>processing_time</td>

<td nowrap="">{int}</td>

<td>The time to process the request in milliseconds</td>

</tr>

<tr>

<td>rate_limit_amount</td>

<td nowrap="">{int}</td>

<td>The amount of requests made during the current rate limit period</td>

</tr>

<tr>

<td>rate_limit_cap</td>

<td nowrap="">{int}</td>

<td>The amount of requests available per hour</td>

</tr>

<tr>

<td>rate_limit_reset</td>

<td nowrap="">{int}</td>

<td>The time (Unix Timestamp) when the rate limit amount resets</td>

</tr>

</tbody>

</table>

Below are the available keyword arguments that resources can use. All of these keywords can be added to collection requests from Resources - singular Resource (/resource/{id}) requests do not accept parameters intended to limit a list of responses.

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="25%">Argument</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>async</td>

<td>Whether the API call is asynchronous

For Resources that offer both synchronous and asynchronous operation, a boolean can be used for this parameter to specify which mode of operation you desire; if the async parameter is omitted, the synchronous mode will be used. For POST requests, the async parameter should be included along with other JSON data being POSTed. When async is true, the API client has the option of including a callback URL so that it can be notified when the asynchronous processing is complete.</td>

</tr>

<tr>

<td>application_data</td>

<td>API client applications may include custom application data in requests to help support scenarios where an application is unable to store the activity id and wishes to include application specific data in their API requests so that the information will be stored on the request's activity and returned to the application in asynchronous callbacks. This can be useful for scenarios where you want to directly associate a PokitDok Platform API request with some identifier(s) in your system so that you can do direct lookups to associate responses with the appropriate information. For example, suppose you wish to fire off a number of eligibility or claims requests and want to include some identifiers specific to your application. By including the identifier(s) you need in the request's application_data section, you can easily do direct lookups using those identifiers when you receive the API response.</td>

</tr>

<tr>

<td>dir</td>

<td>The direction that a list is sorted, ascending or descending (only for collection requests)</td>

</tr>

<tr>

<td>limit</td>

<td>The number of Resources to return in a list (only for collection requests)</td>

</tr>

<tr>

<td>offset</td>

<td>The number of Resources to skip when paging through a list (only for collection requests)</td>

</tr>

<tr>

<td>sort</td>

<td>The field to sort the list of Resources by (only for collection requests)</td>

</tr>

</tbody>

</table>

### Client Libraries

To simplify authorization and access to the PokitDok API, we provide client libraries for a number of programming languages. These libraries offer full access to the PokitDok API with a minimum of code, usually just a few lines. These libraries are hosted on [PokitDok's github.](https://github.com/pokitdok)

*   Python - [pokitdok-python](https://github.com/pokitdok/pokitdok-python)
*   NodeJS - [pokitdok-nodejs](https://github.com/pokitdok/pokitdok-nodejs)
*   Ruby - [pokitdok-ruby](https://github.com/pokitdok/pokitdok-ruby)
*   PHP - [pokitdok-php](https://github.com/pokitdok/pokitdok-php)
*   Java - [pokitdok-java](https://github.com/pokitdok/pokitdok-java)
*   C# - [pokitdok-csharp](https://github.com/pokitdok/pokitdok-csharp)

The following sections go into further detail on the underlying details of connecting directly to the PokitDok API. This is of use if you're implementing the APIs in a programming language for which we currently don't have a client library.

### JSON and EDI Support

All resource endpoints that result in the transmission of EDI transaction sets to our trading partners accept parameters via a JSON representation of an EDI transaction set. However, EDI files can also be sent directly to the /files/ endpoint. The content-type header of a POST request informs the endpoint what is being sent. Acceptable content-type values are "application/json" for JSON requests and "application/edi-x12", "text/plain", and "application/octet-stream" for EDI files.

Claim processing questions? Learn more about our [claims API workflow](/claim-processing).

### Security and Authorization

All calls to the PokitDok API are encrypted over HTTPS. Access to the API is controlled via OAuth2 (for more information, reference "OAuth 2.0 Authorization Framework" at [http://tools.ietf.org/html/rfc6749](http://tools.ietf.org/html/rfc6749)). If you've already signed up and received your client id and client secret, you’ll be able to start calling APIs. Here's a quick example using Python and the requests library to demonstrate how to authenticate as your application and access some information about an ongoing activity:

### Python Example

If you're using Python, you might want to check out the official [PokitDok Platform API Client for Python](https://github.com/pokitdok/pokitdok-python). You may also choose to use the API directly in your Python project like this:

<div hljs="" class="hljs" language="python">import requests from base64 import urlsafe_b64encode client_id = YOUR_APP_CLIENT_ID client_secret = YOUR_APP_CLIENT_SECRET access_token = requests.post('https://platform.pokitdok.com/oauth2/token', headers={'Authorization': 'Basic ' + urlsafe_b64encode(client_id + ':' + client_secret)}, data={'grant_type':'client_credentials'}).json()['access_token'] activity = requests.get('https://platform.pokitdok.com/api/v4/activities/53187d2027a27620f2ec7537', headers={'Authorization': 'Bearer ' + access_token}).json()</div>

If you prefer to start experimenting with the APIs from the command line, here’s an example using curl:

### curl Example

<div hljs="" class="hljs" language="bash">CLIENT_ID=YOUR_APP_CLIENT_ID CLIENT_SECRET=YOUR_APP_CLIENT_SECRET BASIC_HEADER=`echo "$CLIENT_ID:$CLIENT_SECRET" | base64` # On some operating systems, base64 may include extra # whitespace that needs to be removed BASIC_HEADER=$(echo $BASIC_HEADER | sed 's/\r//g') BASIC_HEADER=$(echo $BASIC_HEADER | sed 's/ *//g') curl -i -XPOST -H "Authorization: Basic $BASIC_HEADER" -d "grant_type=client_credentials" https://platform.pokitdok.com/oauth2/token HTTP/1.1 200 OK Server: nginx Date: Thu, 06 Mar 2014 04:20:43 GMT Content-Type: application/json;charset=UTF-8 Content-Length: 127 Connection: keep-alive Pragma: no-cache Cache-Control: no-store { "access_token": "s8KYRJGTO0rWMy0zz1CCSCwsSesDyDlbNdZoRqVR", "token_type": "bearer", "expires": 1393350569, "expires_in": 3600 } ACCESS_TOKEN='s8KYRJGTO0rWMy0zz1CCSCwsSesDyDlbNdZoRqVR' curl -i -H "Authorization: Bearer $ACCESS_TOKEN" https://platform.pokitdok.com/api/v4/activities/5317f51527a27620f2ec7533</div>

### Errors

Error information may be returned to an API client. Common error scenarios include:

*   the data that was provided is invalid
*   required information is missing
*   rate limits have been exceeded
*   an API access token is not valid or no longer valid
*   insufficient credits are available for billable resources

Some examples of these are included below.

Unauthorized access

This may be encountered when an invalid or no longer valid access token is supplied. Access tokens expire one hour after being acquired. Applications should handle 401s properly and request a new access token when this is encountered.

<div hljs="" class="hljs" language="bash">curl -i -H "Authorization: Bearer $ACCESS_TOKEN" https://platform.pokitdok.com/api/v4/activities/5317f51527a27620f2ec7533 HTTP/1.1 401 UNAUTHORIZED Server: nginx Date: Thu, 06 Mar 2014 16:44:07 GMT Content-Type: application/json Content-Length: 31 { "message": "Unauthorized" }</div>

Insufficient credits

This may be encountered when credits are not available to an application for a billable resource requested in the API call. If this is encountered, the application owner will need to load more credits on their application or change their billing tier.

<div hljs="" class="hljs" language="bash">curl -i -H "Authorization: Bearer $ACCESS_TOKEN" https://platform.pokitdok.com/api/v4/providers/?limit=20 HTTP/1.0 402 PAYMENT REQUIRED Server: nginx Date: Tue, 25 Feb 2014 16:15:31 GMT Content-Type: application/json Content-Length: 35 { "message": "Payment Required" }</div>

Rate limit exceeded

This may be encountered when too many API calls are made within a period of time for a rate limited resource. Rate limits are currently enforced on an hourly basis. If your application receives a 403, you can wait for the rate limit period to renew and then make the API call again.

<div hljs="" class="hljs" language="bash">curl -i -H "Authorization: Bearer $ACCESS_TOKEN" https://platform.pokitdok.com/api/v4/activities/5317f51527a27620f2ec7533 HTTP/1.0 403 FORBIDDEN Server: nginx Date: Tue, 25 Feb 2014 16:04:50 GMT Content-Type: application/json Content-Length: 28 { "message": "Forbidden" }</div>

Required information missing or invalid

You may encounter errors like this when required information is omitted from an API call. Simply supply the appropriate information on the next API call to resolve.

<div hljs="" class="hljs" language="bash">curl -i -H "Content-Type: application/json" -H "Authorization: Bearer $ACCESS_TOKEN" -d "{}" https://platform.pokitdok.com/api/v4/eligibility/ HTTP/1.1 422 UNPROCESSABLE ENTITY Server: nginx Date: Thu, 06 Mar 2014 19:50:17 GMT Content-Type: text/html; charset=utf-8 Content-Length: 510 Connection: keep-alive mimetype: application/json charset: utf-8 { "meta": { "activity_id": "53d9506356c02c67c4070096", "application_mode": "production", "credits_billed": 1, "credits_remaining": 995, "processing_time": 3, "rate_limit_amount": 2, "rate_limit_cap": 1000, "rate_limit_reset": 1394138991 }, "data": { "errors": { "validation": { "member": [ "This field is required." ], "provider": [ "This field is required." ], "trading_partner_id": [ "This field is required." ] } } } }</div>

### API Dashboard

To manage and configure your usage of the PokitDok API, we provide an API Dashboard. The Dashboard provides the following functionality:

*   Tracking of long-running asynchronous activity status
*   Monitoring of system health, connectivity, and uptime
*   Account usage and billing information
*   Management of API keys and authentication details for access
*   Documentation, examples, and API references
*   System messages and notifications, such as for scheduled maintenance downtime

### Available Resources

As stated above, the API exposes resources for use in applications. The overview covered working with resources in general and the available parameters. Each resource section here will cover working with specific resources, what fields are available and what actions are available for a particular resource.

### Activities

_Available modes of operation: real-time_

Long-running operations are performed asynchronously. Upon initiating those operations via an API endpoint, activity tracking information is returned to the caller, which can be used to query the status of the activity later on. Throughout processing, activities may transition through the following states:

*   init - The activity is initializing
*   queued - The activity is in queue waiting to start/resume
*   scheduled - The activity is scheduled for the next available transmission to the trading partner
*   generating - The activity is generating X12 transactions
*   processing - The activity is processing X12 transactions that have been received
*   transmitting - The activity is transmitting X12 transactions to trading partner
*   waiting - The activity is waiting on a trading partner Response
*   receiving - The activity is receiving X12 transactions from a trading partner
*   paused - The activity is paused
*   notifying - The activity is notifying the client application about activity results
*   stored - The activity has stored uploaded batch transactions for later processing
*   canceled - The activity was canceled by the client application
*   failed - The activity was unable to process successfully

Information concerning the activity’s progression through the system is available via the API Dashboard, as well as the endpoints listed below.

Available Activity Endpoints:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Endpoint</th>

<th nowrap="" width="5%">HTTP Method</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>/activities/</td>

<td>GET</td>

<td>List current activities

A query string parameter ‘parent_id’ may also be used with this API to get information about sub-activities that were initiated from a batch file upload.</td>

</tr>

<tr>

<td>/activities/{id}</td>

<td>GET</td>

<td>Return detailed information about the specified activity

API applications will receive an activity ID in the API response for all operations that are asynchronous.</td>

</tr>

<tr>

<td>/activities/{id}</td>

<td>PUT</td>

<td>Updates an existing activity -- useful for canceling pending activities that a client application no longer wishes to execute</td>

</tr>

</tbody>

</table>

The /activities/ response includes the following fields:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Field</th>

<th nowrap="" width="15%">Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>callback_url</td>

<td nowrap="">{string}</td>

<td>URL that will be invoked to notify the client application that this Activity has completed

You should always use https for callback URLs used by your application.</td>

</tr>

<tr>

<td>history</td>

<td nowrap="">{array}</td>

<td>Historical status of the progress of this Activity</td>

</tr>

<tr>

<td>id</td>

<td nowrap="">{string}</td>

<td>ID of this Activity</td>

</tr>

<tr>

<td>name</td>

<td nowrap="">{string}</td>

<td>Activity name</td>

</tr>

<tr>

<td>parameters</td>

<td nowrap="">{dict}</td>

<td>The parameters that were originally supplied to the activity</td>

</tr>

<tr>

<td>remaining_transitions</td>

<td nowrap="">{array}</td>

<td>The list of remaining state transitions that the activity has yet to go through</td>

</tr>

<tr>

<td>state</td>

<td nowrap="">{dict}</td>

<td>Current state of this Activity</td>

</tr>

<tr>

<td>transition_path</td>

<td nowrap="">{array}</td>

<td>The list of state transitions that will be used for this Activity</td>

</tr>

<tr>

<td>units_of_work</td>

<td nowrap="">{int}</td>

<td>The number of 'units of work' that the activity is operating on

This will typically be 1 for real-time requests like /eligibility/. When uploading batch X12 files via the /files/ endpoint, this will be the number of ‘transactions’ within that file. For example, if a client application POSTs a file of 20 eligibility requests to the /files/ API, the units_of_work value for that activity will be 20 after the X12 file has been analyzed. If an activity does show a value greater than 1 for units_of_work, the client application can fetch detailed information about each one of the activities processing those units of work by using the /activities/?parent_id=<activity_id> API</td>

</tr>

</tbody>

</table>

curl example fetching activities for the current application

<div hljs="" class="hljs" language="bash">curl -i -H "Authorization: Bearer $ACCESS_TOKEN" https://platform.pokitdok.com/api/v4/activities/</div>

curl example fetching information for a specific activity

<div hljs="" class="hljs" language="bash">curl -i -H "Authorization: Bearer $ACCESS_TOKEN" https://platform.pokitdok.com/api/v4/activities/5317f51527a27620f2ec7533</div>

curl example to cancel an existing activity

<div hljs="" class="hljs" language="bash">curl -XPUT -i -H "Content-Type: application/json" -H "Authorization: Bearer $ACCESS_TOKEN" -d '{"transition": "cancel"}' https://platform.pokitdok.com/api/v4/activities/5317f51527a27620f2ec7533</div>

### Authorizations

_Available modes of operation: batch/async or real-time_

The Authorizations resource allows an application to submit a request for the review of health care in order to obtain an authorization for that health care.

Available Authorizations Endpoints:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Endpoint</th>

<th width="5%">HTTP Method</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>/authorizations/</td>

<td>POST</td>

<td>Submit a request for the review of health care in order to obtain an authorization for that health care</td>

</tr>

</tbody>

</table>

The /authorizations/ endpoint accepts the following parameters:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="60%">Argument</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>event</td>

<td>The patient event that is being submitted for review</td>

</tr>

<tr>

<td>event.category</td>

<td>The category of the event being submitted for review.</td>

</tr>

<tr>

<td>event.certification_type</td>

<td>The type of certification being requested. For new authorization requests, a certification value of "initial" should be used.</td>

</tr>

<tr>

<td>event.delivery</td>

<td>Specifies the delivery pattern of the health care services</td>

</tr>

<tr>

<td>event.delivery.quantity</td>

<td>The quantity of services being requested</td>

</tr>

<tr>

<td>event.delivery.quantity_qualifier</td>

<td>The qualifier used to indicate the quantity type (e.g. visits, month, hours, units, days)</td>

</tr>

<tr>

<td>event.diagnoses</td>

<td>An array of diagnosis information related to the event</td>

</tr>

<tr>

<td>event.diagnoses.code</td>

<td>The diagnosis code (e.g. 789.00)</td>

</tr>

<tr>

<td>event.diagnoses.date</td>

<td>The date of the diagnosis</td>

</tr>

<tr>

<td>event.place_of_service</td>

<td>The location where health care services are rendered</td>

</tr>

<tr>

<td>event.provider</td>

<td>Information about the provider being requested for this event</td>

</tr>

<tr>

<td>event.provider.first_name</td>

<td>The event provider’s first name when the provider is an individual</td>

</tr>

<tr>

<td>event.provider.last_name</td>

<td>The event provider’s last name when the provider is an individual</td>

</tr>

<tr>

<td>event.provider.npi</td>

<td>The NPI for the event provider.</td>

</tr>

<tr>

<td>event.provider.organization_name</td>

<td>The event provider’s name when the provider is an organization. first_name and last_name should be omitted when sending organization_name</td>

</tr>

<tr>

<td>event.type</td>

<td>The type of service being requested. For example, a value of "diagnostic_medical" would be used when an abdominal ultrasound for a patient.</td>

</tr>

<tr>

<td>patient.birth_date</td>

<td>The patient’s birth date as specified on their policy</td>

</tr>

<tr>

<td>patient.id</td>

<td>The patient’s member identifier</td>

</tr>

<tr>

<td>patient.first_name</td>

<td>The patient’s first name as specified on their policy</td>

</tr>

<tr>

<td>patient.last_name</td>

<td>The patient’s last name as specified on their policy</td>

</tr>

<tr>

<td>provider.first_name</td>

<td>The requesting provider’s first name when the provider is an individual</td>

</tr>

<tr>

<td>provider.last_name</td>

<td>The requesting provider’s last name when the provider is an individual</td>

</tr>

<tr>

<td>provider.npi</td>

<td>The NPI for the requesting provider.</td>

</tr>

<tr>

<td>provider.organization_name</td>

<td>The requesting provider’s name when the provider is an organization. first_name and last_name should be omitted when sending organization_name</td>

</tr>

<tr>

<td>subscriber.birth_date</td>

<td>Optional: The subscriber’s birth date as specified on their policy. Specify when the patient is not the subscriber on the insurance policy.</td>

</tr>

<tr>

<td>subscriber.first_name</td>

<td>Optional: The subscriber’s first name as specified on their policy. Specify when the patient is not the subscriber on the insurance policy.</td>

</tr>

<tr>

<td>subscriber.id</td>

<td>Optional: The subscriber’s member identifier. Specify when the patient is not the subscriber on the insurance policy.</td>

</tr>

<tr>

<td>subscriber.last_name</td>

<td>Optional: The subscriber’s last name as specified on their policy. Specify when the patient is not the subscriber on the insurance policy.</td>

</tr>

<tr>

<td>trading_partner_id</td>

<td>Unique id for the intended trading partner, as specified by the Trading Partners resource</td>

</tr>

</tbody>

</table>

Here's an example authorizations request for an abdominal ultrasound. In this example, the patient is also the subscriber on the insurance policy:

<div hljs="" class="hljs">{ "event": { "category": "health_services_review", "certification_type": "initial", "delivery": { "quantity": 1, "quantity_qualifier": "visits" }, "diagnoses": [ { "code": "789.00", "date": "2014-10-01" } ], "place_of_service": "office", "provider": { "organization_name": "KELLY ULTRASOUND CENTER, LLC", "npi": "1760779011", "phone": "8642341234" }, "services": [ { "cpt_code": "76700", "measurement": "unit", "quantity": 1 } ], "type": "diagnostic_medical" }, "patient": { "birth_date": "1970-01-01", "first_name": "JANE", "last_name": "DOE", "id": "1234567890" }, "provider": { "first_name": "JEROME", "npi": "1467560003", "last_name": "AYA-AY" }, "trading_partner_id": "MOCKPAYER" }</div>

The /authorizations/ response contains the following fields:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="60%">Field</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>event</td>

<td>The patient event that is being submitted for approval</td>

</tr>

<tr>

<td>event.category</td>

<td>The category of the event being submitted for review.</td>

</tr>

<tr>

<td>event.certification_type</td>

<td>The type of certification being requested.</td>

</tr>

<tr>

<td>event.delivery</td>

<td>Specifies the delivery pattern of the health care services</td>

</tr>

<tr>

<td>event.delivery.quantity</td>

<td>The quantity of services being requested</td>

</tr>

<tr>

<td>event.delivery.quantity_qualifier</td>

<td>The qualifier used to indicate the quantity type (e.g. visits, month, hours, units, days)</td>

</tr>

<tr>

<td>event.diagnoses</td>

<td>An array of diagnosis information related to the event</td>

</tr>

<tr>

<td>event.diagnoses.code</td>

<td>The diagnosis code (e.g. 789.00)</td>

</tr>

<tr>

<td>event.diagnoses.date</td>

<td>The date of the diagnosis</td>

</tr>

<tr>

<td>event.place_of_service</td>

<td>The location where health care services are rendered</td>

</tr>

<tr>

<td>event.provider</td>

<td>Information about the provider being requested for this event</td>

</tr>

<tr>

<td>event.provider.first_name</td>

<td>The event provider’s first name when the provider is an individual</td>

</tr>

<tr>

<td>event.provider.last_name</td>

<td>The event provider’s last name when the provider is an individual</td>

</tr>

<tr>

<td>event.provider.npi</td>

<td>The NPI for the event provider.</td>

</tr>

<tr>

<td>event.provider.organization_name</td>

<td>The event provider’s name when the provider is an organization. first_name and last_name should be omitted when sending organization_name</td>

</tr>

<tr>

<td>event.review</td>

<td>Information about the outcome of a health care services review</td>

</tr>

<tr>

<td>event.review.certification_action</td>

<td>Indicates the outcome of the review. For example, "certified_in_total" will be returned when the event is certified/authorized.</td>

</tr>

<tr>

<td>event.review.certification_number</td>

<td>The review certification/reference number</td>

</tr>

<tr>

<td>event.review.decision_reason</td>

<td>If the event is not authorized, the reason for that decision.</td>

</tr>

<tr>

<td>event.type</td>

<td>The type of service being requested.</td>

</tr>

<tr>

<td>patient.birth_date</td>

<td>The patient’s birth date as specified on their policy</td>

</tr>

<tr>

<td>patient.id</td>

<td>The patient’s member identifier</td>

</tr>

<tr>

<td>patient.first_name</td>

<td>The patient’s first name as specified on their policy</td>

</tr>

<tr>

<td>patient.last_name</td>

<td>The patient’s last name as specified on their policy</td>

</tr>

<tr>

<td>provider.first_name</td>

<td>The requesting provider’s first name when the provider is an individual</td>

</tr>

<tr>

<td>provider.last_name</td>

<td>The requesting provider’s last name when the provider is an individual</td>

</tr>

<tr>

<td>provider.npi</td>

<td>The NPI for the requesting provider.</td>

</tr>

<tr>

<td>provider.organization_name</td>

<td>The requesting provider’s name when the provider is an organization. first_name and last_name should be omitted when sending organization_name</td>

</tr>

<tr>

<td>subscriber.birth_date</td>

<td>Optional: The subscriber’s birth date as specified on their policy. Specify when the patient is not the subscriber on the insurance policy.</td>

</tr>

<tr>

<td>subscriber.first_name</td>

<td>Optional: The subscriber’s first name as specified on their policy. Specify when the patient is not the subscriber on the insurance policy.</td>

</tr>

<tr>

<td>subscriber.id</td>

<td>Optional: The subscriber’s member identifier. Specify when the patient is not the subscriber on the insurance policy.</td>

</tr>

<tr>

<td>subscriber.last_name</td>

<td>Optional: The subscriber’s last name as specified on their policy. Specify when the patient is not the subscriber on the insurance policy.</td>

</tr>

<tr>

<td>trading_partner_id</td>

<td>Unique id for the intended trading partner, as specified by the Trading Partners resource</td>

</tr>

</tbody>

</table>

Example authorizations response when the trading partner has authorized the request:

<div hljs="" class="hljs">{ "event": { "category": "health_services_review", "certification_type": "initial", "delivery": { "quantity": 1, "quantity_qualifier": "visits" }, "diagnoses": [ { "code": "789.00", "date": "2014-10-01" } ], "place_of_service": "office", "provider": { "organization_name": "KELLY ULTRASOUND CENTER, LLC", "npi": "1760779011", "phone": "8642341234" }, "review": { "certification_action": "certified_in_total", "certification_number": "AUTH0002" }, "services": [ { "cpt_code": "76700", "measurement": "unit", "quantity": 1 } ], "type": "diagnostic_medical" }, "patient": { "birth_date": "1970-01-01", "first_name": "JANE", "last_name": "DOE", "id": "1234567890" }, "provider": { "first_name": "JEROME", "npi": "1467560003", "last_name": "AYA-AY" }, "trading_partner_id": "MOCKPAYER" }</div>

curl example submitting an authorization request:

<div hljs="" class="hljs" language="bash">curl -i -H "Authorization: Bearer $ACCESS_TOKEN" -H "Content-Type: application/json" -XPOST -d '{ "event": { "category": "health_services_review", "certification_type": "initial", "delivery": { "quantity": 1, "quantity_qualifier": "visits" }, "diagnoses": [ { "code": "789.00", "date": "2014-10-01" } ], "place_of_service": "office", "provider": { "organization_name": "KELLY ULTRASOUND CENTER, LLC", "npi": "1760779011", "phone": "8642341234" }, "services": [ { "cpt_code": "76700", "measurement": "unit", "quantity": 1 } ], "type": "diagnostic_medical" }, "patient": { "birth_date": "1970-01-01", "first_name": "JANE", "last_name": "DOE", "id": "1234567890" }, "provider": { "first_name": "JEROME", "npi": "1467560003", "last_name": "AYA-AY" }, "trading_partner_id": "MOCKPAYER" } ' https://platform.pokitdok.com/api/v4/authorizations/</div>

### Cash Prices

_Available modes of operation: real-time_

The Cash Prices resource allows access to our internal collection of pricing data. The data comes from actual providers selling actual services. For a location where a cash price has not been collected, a price is estimated using a multivariate model.

Available Cash Prices Endpoints:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Endpoint</th>

<th width="5%">HTTP Method</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>/prices/cash</td>

<td>GET</td>

<td>Return a list of prices for a given procedure (by CPT Code) in a given region (by ZIP Code)</td>

</tr>

</tbody>

</table>

The /prices/cash endpoint accepts the following parameters:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Field</th>

<th nowrap="" width="5%">Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>cpt_code</td>

<td nowrap="">{string}</td>

<td>The CPT code of the procedure in question</td>

</tr>

<tr>

<td>zip_code</td>

<td nowrap="">{string}</td>

<td>Zip code in which to search for procedures</td>

</tr>

</tbody>

</table>

The /prices/cash response contains the following fields:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Field</th>

<th nowrap="" width="5%">Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>amounts.average_price</td>

<td nowrap="">{decimal}</td>

<td>The average cash price for the procedure</td>

</tr>

<tr>

<td>amounts.cpt_code</td>

<td nowrap="">{string}</td>

<td>The CPT code of the procedure</td>

</tr>

<tr>

<td>amounts.geo_zip_area</td>

<td nowrap="">{string}</td>

<td>The three character zip code tabulation area code</td>

</tr>

<tr>

<td>amounts.high_price</td>

<td nowrap="">{decimal}</td>

<td>The maximum price for the procedure</td>

</tr>

<tr>

<td>amounts.low_price</td>

<td nowrap="">{decimal}</td>

<td>The lowest price for the procedure</td>

</tr>

<tr>

<td>amounts.median_price</td>

<td nowrap="">{decimal}</td>

<td>The median price for the procedure</td>

</tr>

<tr>

<td>amounts.standard_deviation</td>

<td nowrap="">{decimal}</td>

<td>The standard deviation, or variation measure, of prices for the procedure</td>

</tr>

</tbody>

</table>

curl example fetching cash price information:

<div hljs="" class="hljs" language="bash">curl -i -H "Authorization: Bearer $ACCESS_TOKEN" https://platform.pokitdok.com/api/v4/prices/cash?cpt_code=87799&zip_code=32218</div>

### Claims

_Available modes of operation: see below_

The claims resource allows the filing of claims via the transmission of EDI 837 transaction sets to the designated trading partner. A callback_url may be supplied in the claims request to indicate that your application should be notified when the asynchronous processing is complete and a claim acknowledgement has been received from the trading partner. When a callback_url is specified, the full claims request activity will be POSTed back to the callback_url. A claim acknowledgement will be returned for each submitted claims request. Depending on the trading partner, claims request activities may complete within an hour of being submitted. In most cases, your application should know within 24 hours if a claims request has passed validations and is accepted for adjudication. Once your claims request activity has completed and you have a claim acknowledgement that indicates your claims request was accepted for adjudication, you may use the [claims status](#claimstatus) API to track the claim all the way through the adjudication/payment process. The claims request activity may also be monitored via the [activities](#activities) API.

We can support Health Care Claim Payment/Advice (835) transactions that are generated in response to claims API requests when the billing provider has authorized us to receive 835 transactions with the trading partner. When claims API requests are submitted for billing providers that have not authorized PokitDok to receive 835 transactions, a claims status (277) response will be returned when claims processing has been completed. Please contact our support team using our [contact form](#/contact) if you'd like more information on using the claims API to also receive 835 transactions and electronic payments.

Learn more about our [claims API workflow](/claim-processing).

Available Claims Endpoints:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Endpoint</th>

<th width="5%">HTTP Method</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>/claims/</td>

<td>POST</td>

<td>Create a new claim, via the filing of an EDI 837 Claim, to the designated Trading Partner
Available modes of operation: batch/async only</td>

</tr>

</tbody>

</table>

The /claims/ endpoint accepts the following parameters:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="50%">Parameter</th>

<th width="30%">Description</th>

<th>CMS 1500 Field</th>

</tr>

</thead>

<tbody>

<tr>

<td>billing_provider</td>

<td>Required: A dictionary of information for the provider that is billing for services</td>

<td>33: Billing Provider Info</td>

</tr>

<tr>

<td>billing_provider.address</td>

<td>A dictionary of information for the billing provider's address</td>

</tr>

<tr>

<td>billing_provider.address.address_lines</td>

<td>List of strings representing the street address for a billing provider. (e.g. ["123 MAIN ST.", "Suite 4"])</td>

</tr>

<tr>

<td>billing_provider.address.city</td>

<td>The city component of a billing provider's address (e.g. "SAN MATEO")</td>

</tr>

<tr>

<td>billing_provider.address.state</td>

<td>The state component of a billing provider's address (e.g. "CA")</td>

</tr>

<tr>

<td>billing_provider.address.zipcode</td>

<td>The billing provider's zip/postal code (e.g. "94401")</td>

</tr>

<tr>

<td>billing_provider.first_name</td>

<td>The first name of the provider billing for services. Required when the billing provider is an individual.</td>

</tr>

<tr>

<td>billing_provider.last_name</td>

<td>The last name of the provider billing for services. Required when the billing provider is an individual.</td>

</tr>

<tr>

<td>billing_provider.organization_name</td>

<td>The billing provider’s name when the provider is an organization. first_name and last_name should be omitted when sending organization_name</td>

</tr>

<tr>

<td>billing_provider.npi</td>

<td>The National Provider Identifier for the provider billing for services.</td>

<td>33a: Billing Provider NPI</td>

</tr>

<tr>

<td>billing_provider.tax_id</td>

<td>The federal tax id for the provider billing for services. For individual providers, this may be the tax id of the medical practice or organization where a provider works.</td>

<td>25: Federal tax ID Number (SSN EIN)</td>

</tr>

<tr>

<td>billing_provider.taxonomy_code</td>

<td>The taxonomy code for the provider billing for services. (e.g. "207Q00000X")</td>

<td>24i: ID Qualifier</td>

</tr>

<tr>

<td>claim</td>

<td>Dictionary of information representing a claim for services that have been performed by a health care provider for the patient.</td>

</tr>

<tr>

<td>claim.onset_date</td>

<td>Optional: the date of first symptoms for the illness</td>

<td>14: Date of current illness OR injury OR pregnancy</td>

</tr>

<tr>

<td>claim.place_of_service</td>

<td>The location where services were performed. (e.g. "office")</td>

<td>24b: Place of service</td>

</tr>

<tr>

<td>claim.patient_paid_amount</td>

<td>Optional: The amount the patient has already paid the provider for the services listed in the claim. When reporting cash payment encounters for the purpose of contributing those amounts toward the member's deductible, the patient_paid_amount will equal the total_charge_amount.</td>

<td>29: Amount Paid</td>

</tr>

<tr>

<td>claim.patient_signature_on_file</td>

<td>Boolean indicator for whether or not a patient's signature is on file to authorize the release of medical records. Defaults to true if not specified.</td>

<td>12: Patient's or authorized person's signature</td>

</tr>

<tr>

<td>claim.service_lines</td>

<td>List of services that were performed as part of this claim</td>

</tr>

<tr>

<td>claim.service_lines.charge_amount</td>

<td>The amount charged for this specific service. (e.g. 100.00)</td>

<td>24f: Charges</td>

</tr>

<tr>

<td>claim.service_lines.diagnosis_codes</td>

<td>A list of diagnosis codes related to this service. (e.g. 487.1)</td>

<td>21: Diagnosis or nature of illness or injury</td>

</tr>

<tr>

<td>claim.service_lines.procedure_code</td>

<td>The CPT code for the service that was performed</td>

<td>24d: Procedures, Services, or Supplies</td>

</tr>

<tr>

<td>claim.service_lines.procedure_modifier_codes</td>

<td>Optional: List of modifier codes for the specified procedure (e.g. ["GT"])</td>

<td>24d: Procedures, Services, or Supplies</td>

</tr>

<tr>

<td>claim.service_lines.service_date</td>

<td>The date the service was performed</td>

<td>24a: Date(s) of service (from, to)</td>

</tr>

<tr>

<td>claim.service_lines.unit_count</td>

<td>Number of units of this service (e.g. 1.0)</td>

<td>24g: Days or Units</td>

</tr>

<tr>

<td>claim.total_charge_amount</td>

<td>The total amount charged/billed for the claim. (e.g. 100.00)</td>

<td>28: Total Charge</td>

</tr>

<tr>

<td>patient</td>

<td>Information about the patient that received services outlined in the claim. Patient information is only required when the patient is not the insurance subscriber.</td>

</tr>

<tr>

<td>patient.address</td>

<td>Required: The patient’s address information.</td>

<td>5: Patient's address</td>

</tr>

<tr>

<td>patient.address.address_lines</td>

<td>The patient’s street address information. (e.g. ["123 N MAIN ST"])</td>

</tr>

<tr>

<td>patient.address.city</td>

<td>The patient’s city information. (e.g. "SAN MATEO")</td>

</tr>

<tr>

<td>patient.address.state</td>

<td>The patient’s state information. (e.g. "CA")</td>

</tr>

<tr>

<td>patient.address.zipcode</td>

<td>The patient’s zip/postal code. (e.g. "94401")</td>

</tr>

<tr>

<td>patient.birth_date</td>

<td>The patient’s birth date as specified on their policy.</td>

<td>3: Patients Birth Date</td>

</tr>

<tr>

<td>patient.first_name</td>

<td>Required: The patient’s first name.</td>

<td>2: Patient's Name</td>

</tr>

<tr>

<td>patient.gender</td>

<td>The patient’s gender.</td>

<td>3: The patient's sex</td>

</tr>

<tr>

<td>patient.member_id</td>

<td>Required: The patient’s member identifier.</td>

</tr>

<tr>

<td>patient.middle_name</td>

<td>Optional: The patient’s middle name.</td>

<td>2: Patient's Name</td>

</tr>

<tr>

<td>patient.last_name</td>

<td>Required: The patient’s last name.</td>

<td>2: Patient's Name</td>

</tr>

<tr>

<td>patient.pregnant</td>

<td>Patient pregnancy indicator. Defaults to false.</td>

</tr>

<tr>

<td>patient.relationship</td>

<td>Required: The patient’s relationship to the subscriber. May be one of these values: spouse, child, employee, unknown, organ_donor, cadaver_donor, life_partner, other_relationship</td>

<td>6: Patient's relationship to the insured</td>

</tr>

<tr>

<td>subscriber</td>

<td>Information about the insurance subscriber as it appears on their policy</td>

</tr>

<tr>

<td>subscriber.address</td>

<td>The subscriber’s address information as specified on their policy.</td>

<td>7: Insured's address</td>

</tr>

<tr>

<td>subscriber.address.address_lines</td>

<td>The subscriber’s street address information as specified on their policy. (e.g. ["123 N MAIN ST"])</td>

</tr>

<tr>

<td>subscriber.address.city</td>

<td>The subscriber’s city information as specified on their policy. (e.g. "SAN MATEO")</td>

</tr>

<tr>

<td>subscriber.address.state</td>

<td>The subscriber’s state information as specified on their policy. (e.g. "CA")</td>

</tr>

<tr>

<td>subscriber.address.zipcode</td>

<td>The subscriber’s zip/postal code as specified on their policy. (e.g. "94401")</td>

</tr>

<tr>

<td>subscriber.birth_date</td>

<td>The subscriber’s birth date as specified on their policy.</td>

<td>11a: Insured's date of birth</td>

</tr>

<tr>

<td>subscriber.first_name</td>

<td>Required: The subscriber’s first name as specified on their policy.</td>

<td>4: Insured's name</td>

</tr>

<tr>

<td>subscriber.gender</td>

<td>The subscriber’s gender as specified on their policy.</td>

<td>11a: Insured's sex</td>

</tr>

<tr>

<td>subscriber.group_name</td>

<td>Optional: The subscriber’s group name as specified on their policy.</td>

<td>11b: Employer's name or school name</td>

</tr>

<tr>

<td>subscriber.member_id</td>

<td>Required: The subscriber’s member identifier.</td>

<td>1a: Insured's ID number</td>

</tr>

<tr>

<td>subscriber.last_name</td>

<td>Required: The subscriber’s last name as specified on their policy.</td>

<td>4: Insured's name</td>

</tr>

<tr>

<td>trading_partner_id</td>

<td>Required: Unique id for the intended trading partner, as specified by the Trading Partners resource</td>

<td>11c: Insurance plan name or program name</td>

</tr>

<tr>

<td>transaction_code</td>

<td>Required: The type of claim transaction that is being submitted. (e.g. "chargeable")</td>

</tr>

</tbody>

</table>

Sample Claims Request:

<div hljs="" class="hljs">{ "transaction_code": "chargeable", "trading_partner_id": "MOCKPAYER", "billing_provider": { "taxonomy_code": "207Q00000X", "first_name": "Jerome", "last_name": "Aya-Ay", "npi": "1467560003", "address": { "address_lines": [ "8311 WARREN H ABERNATHY HWY" ], "city": "SPARTANBURG", "state": "SC", "zipcode": "29301" }, "tax_id": "123456789" }, "subscriber": { "first_name": "Jane", "last_name": "Doe", "member_id": "W000000000", "address": { "address_lines": ["123 N MAIN ST"], "city": "SPARTANBURG", "state": "SC", "zipcode": "29301" }, "birth_date": "1970-01-01", "gender": "female" }, "claim": { "total_charge_amount": 60.0, "service_lines": [ { "procedure_code": "99213", "charge_amount": 60.0, "unit_count": 1.0, "diagnosis_codes": [ "487.1" ], "service_date": "2014-06-01" } ] } }</div>

Sample Claims Request that includes custom application data for easy handling of asynchronous responses:

<div hljs="" class="hljs">{ "application_data": { "patient_id": "ABC1234XYZ", "location_id": 123, "transaction_uuid": "93f38f1b-b2cd-4da1-8b55-c6e3ab380dbf" }, "transaction_code": "chargeable", "trading_partner_id": "MOCKPAYER", "billing_provider": { "taxonomy_code": "207Q00000X", "first_name": "Jerome", "last_name": "Aya-Ay", "npi": "1467560003", "address": { "address_lines": [ "8311 WARREN H ABERNATHY HWY" ], "city": "SPARTANBURG", "state": "SC", "zipcode": "29301" }, "tax_id": "123456789" }, "subscriber": { "first_name": "Jane", "last_name": "Doe", "member_id": "W000000000", "address": { "address_lines": ["123 N MAIN ST"], "city": "SPARTANBURG", "state": "SC", "zipcode": "29301" }, "birth_date": "1970-01-01", "gender": "female" }, "claim": { "total_charge_amount": 60.0, "service_lines": [ { "procedure_code": "99213", "charge_amount": 60.0, "unit_count": 1.0, "diagnosis_codes": [ "487.1" ], "service_date": "2014-06-01" } ] } }</div>

Sample Claims Request using the patient paid amount to report a cash payment encounter for contributing toward a member's deductible.

<div hljs="" class="hljs">{ "transaction_code": "chargeable", "trading_partner_id": "MOCKPAYER", "billing_provider": { "taxonomy_code": "207Q00000X", "first_name": "JEROME", "last_name": "AYA-AY", "npi": "1467560003", "address": { "address_lines": [ "1703 John B White Blvd, Unit A" ], "city": "SPARTANBURG", "state": "SC", "zipcode": "29301" }, "tax_id": "123456789" }, "subscriber": { "first_name": "JOHN", "last_name": "DOE", "member_id": "W199000000", "address": { "address_lines": ["123 MAIN ST"], "city": "SPARTANBURG", "state": "SC", "zipcode": "29301" }, "birth_date": "1970-01-01", "gender": "male" }, "claim": { "place_of_service": "office", "total_charge_amount": 150.0, "patient_paid_amount": 150.0, "service_lines": [ { "procedure_code": "11100", "charge_amount": 150.00, "unit_count": 1.0, "diagnosis_codes": [ "701.9" ], "service_date": "2014-11-24" } ] } }</div>

Sample Claims Request when using procedure modifier codes. This example uses the "GT" modifier ("via interactive audio and video telecommunications systems") which would be suitable for telehealth applications:

<div hljs="" class="hljs">{ "transaction_code": "chargeable", "trading_partner_id": "MOCKPAYER", "billing_provider": { "taxonomy_code": "207Q00000X", "first_name": "Jerome", "last_name": "Aya-Ay", "npi": "1467560003", "address": { "address_lines": [ "8311 WARREN H ABERNATHY HWY" ], "city": "SPARTANBURG", "state": "SC", "zipcode": "29301" }, "tax_id": "123456789" }, "subscriber": { "first_name": "Jane", "last_name": "Doe", "member_id": "W000000000", "address": { "address_lines": ["123 N MAIN ST"], "city": "SPARTANBURG", "state": "SC", "zipcode": "29301" }, "birth_date": "1970-01-01", "gender": "female" }, "claim": { "total_charge_amount": 100.0, "service_lines": [ { "procedure_code": "99201", "procedure_modifier_codes": ["GT"], "charge_amount": 100.0, "unit_count": 1.0, "diagnosis_codes": [ "487.1" ], "service_date": "2014-06-01" } ] } }</div>

curl example submitting a claim:

<div hljs="" class="hljs" language="bash">curl -i -H "Authorization: Bearer $ACCESS_TOKEN" -H "Content-Type: application/json" -XPOST -d '{ "transaction_code": "chargeable", "trading_partner_id": "MOCKPAYER", "billing_provider": { "taxonomy_code": "207Q00000X", "first_name": "Jerome", "last_name": "Aya-Ay", "npi": "1467560003", "address": { "address_lines": [ "8311 WARREN H ABERNATHY HWY" ], "city": "SPARTANBURG", "state": "SC", "zipcode": "29301" }, "tax_id": "123456789" }, "subscriber": { "first_name": "Jane", "last_name": "Doe", "member_id": "W000000000", "address": { "address_lines": ["123 N MAIN ST"], "city": "SPARTANBURG", "state": "SC", "zipcode": "29301" }, "birth_date": "1970-01-01", "gender": "female" }, "claim": { "total_charge_amount": 60.0, "service_lines": [ { "procedure_code": "99213", "charge_amount": 60.0, "unit_count": 1.0, "diagnosis_codes": [ "487.1" ], "service_date": "2014-06-01" } ] } }' https://platform.pokitdok.com/api/v4/claims/</div>

### Claims Status

_Available modes of operation: batch/async or real-time_

The Claims Status resource allows an application to request information about previously submitted claims. Learn more about our [claims API workflow](/claim-processing).

Available Claims Status Endpoints:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Endpoint</th>

<th width="5%">HTTP Method</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>/claims/status</td>

<td>POST</td>

<td>Submit a claim status request to the specified trading partner.</td>

</tr>

</tbody>

</table>

The /claims/status endpoint accepts the following parameters:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="60%">Argument</th>

<th width="40%">Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>patient.birth_date</td>

<td>The patient’s birth date as specified on their policy</td>

</tr>

<tr>

<td>patient.id</td>

<td>The patient’s member identifier</td>

</tr>

<tr>

<td>patient.first_name</td>

<td>The patient’s first name as specified on their policy</td>

</tr>

<tr>

<td>patient.last_name</td>

<td>The patient’s last name as specified on their policy</td>

</tr>

<tr>

<td>provider.first_name</td>

<td>The provider’s first name when the provider is an individual</td>

</tr>

<tr>

<td>provider.last_name</td>

<td>The provider’s last name when the provider is an individual</td>

</tr>

<tr>

<td>provider.npi</td>

<td>The NPI for the provider.</td>

</tr>

<tr>

<td>provider.organization_name</td>

<td>The provider’s name when the provider is an organization. first_name and last_name should be omitted when sending organization_name</td>

</tr>

<tr>

<td>service_date</td>

<td>The date services were performed or started for the claim service period</td>

</tr>

<tr>

<td>service_end_date</td>

<td>Optional: The date services ended for the claim service period.</td>

</tr>

<tr>

<td>subscriber.birth_date</td>

<td>Optional: The subscriber’s birth date as specified on their policy. Specify when the patient is not the subscriber on the insurance policy.</td>

</tr>

<tr>

<td>subscriber.first_name</td>

<td>Optional: The subscriber’s first name as specified on their policy. Specify when the patient is not the subscriber on the insurance policy.</td>

</tr>

<tr>

<td>subscriber.id</td>

<td>Optional: The subscriber’s member identifier. Specify when the patient is not the subscriber on the insurance policy.</td>

</tr>

<tr>

<td>subscriber.last_name</td>

<td>Optional: The subscriber’s last name as specified on their policy. Specify when the patient is not the subscriber on the insurance policy.</td>

</tr>

<tr>

<td>tracking_id</td>

<td>Optional: The payer's claim tracking id. Specify a tracking id to refine the search criteria for a specific claim.</td>

</tr>

<tr>

<td>trading_partner_id</td>

<td>Unique id for the intended trading partner, as specified by the Trading Partners resource</td>

</tr>

</tbody>

</table>

Example claim status request when the patient is also the subscriber on the insurance policy:

<div hljs="" class="hljs">{ "patient": { "birth_date": "1970-01-01", "first_name": "JANE", "last_name": "DOE", "id": "1234567890" }, "provider": { "first_name": "Jerome", "last_name": "Aya-Ay", "npi": "1467560003" }, "service_date": "2014-01-01", "trading_partner_id": "MOCKPAYER" }</div>

Example claim status request when the patient is not the subscriber on the insurance policy:

<div hljs="" class="hljs">{ "patient": { "birth_date": "2000-01-01", "first_name": "JOHN", "last_name": "DOE", "id": "1234567890" }, "provider": { "first_name": "Jerome", "last_name": "Aya-Ay", "npi": "1467560003" }, "service_date": "2014-01-01", "subscriber": { "birth_date": "1970-01-01", "first_name": "JANE", "last_name": "DOE", "id": "1234567890" }, "trading_partner_id": "MOCKPAYER" }</div>

Example claim status request when the claim service period covers several days:

<div hljs="" class="hljs">{ "patient": { "birth_date": "1970-01-01", "first_name": "JANE", "last_name": "DOE", "id": "1234567890" }, "provider": { "first_name": "Jerome", "last_name": "Aya-Ay", "npi": "1467560003" }, "service_date": "2014-01-01", "service_end_date": "2014-01-04", "trading_partner_id": "MOCKPAYER" }</div>

Example claim status request using a claim tracking id to refine the search:

<div hljs="" class="hljs">{ "patient": { "birth_date": "1970-01-01", "first_name": "JANE", "last_name": "DOE", "id": "1234567890" }, "provider": { "first_name": "Jerome", "last_name": "Aya-Ay", "npi": "1467560003" }, "service_date": "2014-01-01", "tracking_id": "ABC12345", "trading_partner_id": "MOCKPAYER" }</div>

The /claim/status response contains the following fields:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="60%">Field</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>patient</td>

<td>Information about a patient including any matching claims</td>

</tr>

<tr>

<td>patient.claims</td>

<td>A list of matching claims returned by the trading partner for a claims status request</td>

</tr>

<tr>

<td>patient.claims.adjudication_finalized_date</td>

<td>The date adjudication was finalized for the claim</td>

</tr>

<tr>

<td>patient.claims.applied_to_deductible</td>

<td>Boolean that indicates whether or not claim charges are applied to the deductible</td>

</tr>

<tr>

<td>patient.claims.check_number</td>

<td>The check or EFT trace number for a claim payment.</td>

</tr>

<tr>

<td>patient.claims.claim_control_number</td>

<td>The Payer's Claim Control Number</td>

</tr>

<tr>

<td>patient.claims.claim_payment_amount</td>

<td>The amount that's been paid on the claim</td>

</tr>

<tr>

<td>patient.claims.remittance_date</td>

<td>The date the check was issued or EFT funds became available</td>

</tr>

<tr>

<td>patient.claims.service_date</td>

<td>The date services were performed or started for the claim service period</td>

</tr>

<tr>

<td>patient.claims.service_end_date</td>

<td>The date services ended for the claim service period</td>

</tr>

<tr>

<td>patient.claims.services</td>

<td>A list of services linked to the claim</td>

</tr>

<tr>

<td>patient.claims.services.charge_amount</td>

<td>The amount charged for a particular service on the claim</td>

</tr>

<tr>

<td>patient.claims.services.cpt_code</td>

<td>The CPT code indicating the type of service that was performed</td>

</tr>

<tr>

<td>patient.claims.services.payment_amount</td>

<td>The amount paid for a particular service on the claim</td>

</tr>

<tr>

<td>patient.claims.services.service_date</td>

<td>The date the service was performed or started for the claim service period</td>

</tr>

<tr>

<td>patient.claims.services.service_end_date</td>

<td>The date the service ended for the claim service period</td>

</tr>

<tr>

<td>patient.claims.services.statuses</td>

<td>A listing of status information for the claim</td>

</tr>

<tr>

<td>patient.claims.services.statuses.status_category</td>

<td>A verbose message about the general category of the service's claim status (e.g. accepted, rejected, etc.) This value is suitable for display to users of a system utilizing the claims status API. A full listing of possible values can be found [here](http://www.wpc-edi.com/reference/codelists/healthcare/claim-status-category-codes/)</td>

</tr>

<tr>

<td>patient.claims.services.statuses.status_category_code</td>

<td>The code indicating the general category of the service's claim status. The status category code is more appropriate for use by the software using the claims status API to categorize the information. A full listing of codes can be found [here](http://www.wpc-edi.com/reference/codelists/healthcare/claim-status-category-codes/)</td>

</tr>

<tr>

<td>patient.claims.services.statuses.status_code</td>

<td>Indicates the status of the service on the claim. This status code provides more detail to support the status category. This value is suitable for display to users of a system utilizing the claims status API. A full listing of possible values can be found [here](http://www.wpc-edi.com/reference/codelists/healthcare/claim-status-codes/)</td>

</tr>

<tr>

<td>patient.claims.statuses</td>

<td>A listing of status information for the claim</td>

</tr>

<tr>

<td>patient.claims.statuses.status_category</td>

<td>A verbose message about the general category of the claim's status (e.g. accepted, rejected, etc.) This value is suitable for display to users of a system utilizing the claims status API. A full listing of possible values can be found [here](http://www.wpc-edi.com/reference/codelists/healthcare/claim-status-category-codes/)</td>

</tr>

<tr>

<td>patient.claims.statuses.status_category_code</td>

<td>The code indicating the general category of the claim's status. The status category code is more appropriate for use by the software using the claims status API to categorize the information. A full listing of codes can be found [here](http://www.wpc-edi.com/reference/codelists/healthcare/claim-status-category-codes/)</td>

</tr>

<tr>

<td>patient.claims.statuses.status_code</td>

<td>Indicates the status of the claim. This status code provides more detail to support the status category. This value is suitable for display to users of a system utilizing the claims status API. A full listing of possible values can be found [here](http://www.wpc-edi.com/reference/codelists/healthcare/claim-status-codes/)</td>

</tr>

<tr>

<td>patient.claims.total_claim_amount</td>

<td>The total charges submitted for the claim</td>

</tr>

<tr>

<td>patient.first_name</td>

<td>The patient’s first name as specified on their policy</td>

</tr>

<tr>

<td>patient.id</td>

<td>The patient’s member identifier</td>

</tr>

<tr>

<td>patient.last_name</td>

<td>The patient’s last name as specified on their policy</td>

</tr>

</tbody>

</table>

Example claims status response when the trading partner is unable to locate any matching claims:

<div hljs="" class="hljs">{ "patient": { "claims": [ { "applied_to_deductible": false, "claim_control_number": "NOT APPLICABLE", "claim_payment_amount": { "amount": "0", "currency": "USD" }, "service_date": "2014-01-01", "service_end_date": "2014-01-01", "statuses": [ { "claim_payment_amount": { "amount": "0", "currency": "USD" }, "status_category": "Acknowledgement/Not Found-The claim/encounter can not be found in the adjudication system.", "status_code": "Claim/encounter not found.", "status_effective_date": "2014-07-29", "total_claim_amount": { "amount": "0", "currency": "USD" } } ], "total_claim_amount": { "amount": "0", "currency": "USD" }, "tracking_id": "47635D71-B08B-47D9-9C11-4630F5" } ], "first_name": "JANE", "id": "W100000000", "last_name": "DOE" }, "payer": { "id": "MOCKPAYER", "name": "MOCKPAYER" }, "providers": [ { "first_name": "Jerome", "last_name": "Aya-Ay", "npi": "1467560003" } ], "submitter": { "first_name": "Jerome", "id": "1467560003", "last_name": "Aya-Ay" }, "trading_partner_id": "MOCKPAYER" }</div>

Example claims status response when adjudication is finalized and the claim has been paid:

<div hljs="" class="hljs">{ "patient": { "claims": [ { "adjudication_finalized_date": "2014-06-20", "applied_to_deductible": false, "check_number": "08608-000000000", "claim_control_number": "EV30000WY00", "claim_payment_amount": { "amount": "156", "currency": "USD" }, "remittance_date": "2014-06-24", "service_date": "2014-06-17", "service_end_date": "2014-06-17", "services": [ { "charge_amount": { "amount": "20", "currency": "USD" }, "cpt_code": "D1206", "payment_amount": { "amount": "0", "currency": "USD" }, "service_date": "2014-06-17", "service_end_date": "2014-06-17", "statuses": [ { "status_category": "Finalized/Denial-The claim/line has been denied.", "status_code": "Processed according to contract provisions (Contract refers to provisions that exist between the Health Plan and a Provider of Health Care Services)", "status_effective_date": "2014-07-29" } ] }, { "charge_amount": { "amount": "72", "currency": "USD" }, "cpt_code": "D1110", "payment_amount": { "amount": "72", "currency": "USD" }, "service_date": "2014-06-17", "service_end_date": "2014-06-17", "statuses": [ { "status_category": "Finalized/Payment-The claim/line has been paid.", "status_code": "Processed according to contract provisions (Contract refers to provisions that exist between the Health Plan and a Provider of Health Care Services)", "status_effective_date": "2014-07-29" } ] }, { "charge_amount": { "amount": "29", "currency": "USD" }, "cpt_code": "D0431", "payment_amount": { "amount": "0", "currency": "USD" }, "service_date": "2014-06-17", "service_end_date": "2014-06-17", "statuses": [ { "status_category": "Finalized/Denial-The claim/line has been denied.", "status_code": "Processed according to contract provisions (Contract refers to provisions that exist between the Health Plan and a Provider of Health Care Services)", "status_effective_date": "2014-07-29" } ] }, { "charge_amount": { "amount": "52", "currency": "USD" }, "cpt_code": "D0274", "payment_amount": { "amount": "48", "currency": "USD" }, "service_date": "2014-06-17", "service_end_date": "2014-06-17", "statuses": [ { "status_category": "Finalized/Payment-The claim/line has been paid.", "status_code": "Processed according to contract provisions (Contract refers to provisions that exist between the Health Plan and a Provider of Health Care Services)", "status_effective_date": "2014-07-29" } ] }, { "charge_amount": { "amount": "41", "currency": "USD" }, "cpt_code": "D0120", "payment_amount": { "amount": "36", "currency": "USD" }, "service_date": "2014-06-17", "service_end_date": "2014-06-17", "statuses": [ { "status_category": "Finalized/Payment-The claim/line has been paid.", "status_code": "Processed according to contract provisions (Contract refers to provisions that exist between the Health Plan and a Provider of Health Care Services)", "status_effective_date": "2014-07-29" } ] } ], "statuses": [ { "adjudication_finalized_date": "2014-06-20", "check_number": "08608-000000000", "claim_payment_amount": { "amount": "156", "currency": "USD" }, "remittance_date": "2014-06-24", "status_category": "Finalized/Payment-The claim/line has been paid.", "status_code": "Processed according to contract provisions (Contract refers to provisions that exist between the Health Plan and a Provider of Health Care Services)", "status_effective_date": "2014-07-29", "total_claim_amount": { "amount": "214", "currency": "USD" } } ], "total_claim_amount": { "amount": "214", "currency": "USD" }, "tracking_id": "B82A26AE-984A-480B-9B20-81DD3E" } ], "first_name": "JANE", "id": "W199000000", "last_name": "DOE" }, "payer": { "id": "MOCKPAYER", "name": "MOCKPAYER" }, "providers": [ { "first_name": "ADAM", "last_name": "SMITH", "npi": "1320000000" } ], "submitter": { "first_name": "ADAM", "id": "1326000000", "last_name": "SMITH" }, "trading_partner_id": "MOCKPAYER" }</div>

Example claims status response when adjudication is finalized and the claim has been denied:

<div hljs="" class="hljs">{ "patient": { "claims": [ { "adjudication_finalized_date": "2013-07-24", "applied_to_deductible": false, "claim_control_number": "EM000000000", "claim_payment_amount": { "amount": "0", "currency": "USD" }, "remittance_date": "2013-07-30", "service_date": "2013-07-15", "service_end_date": "2013-07-15", "services": [ { "charge_amount": { "amount": "375", "currency": "USD" }, "cpt_code": "D9220", "payment_amount": { "amount": "0", "currency": "USD" }, "service_date": "2013-07-15", "service_end_date": "2013-07-15", "statuses": [ { "status_category": "Finalized/Denial-The claim/line has been denied.", "status_effective_date": "2014-07-29" } ] }, { "charge_amount": { "amount": "460", "currency": "USD" }, "cpt_code": "D7240", "payment_amount": { "amount": "0", "currency": "USD" }, "service_date": "2013-07-15", "service_end_date": "2013-07-15", "statuses": [ { "status_category": "Finalized/Denial-The claim/line has been denied.", "status_effective_date": "2014-07-29" } ] }, { "charge_amount": { "amount": "140", "currency": "USD" }, "cpt_code": "D7140", "payment_amount": { "amount": "0", "currency": "USD" }, "service_date": "2013-07-15", "service_end_date": "2013-07-15", "statuses": [ { "status_category": "Finalized/Denial-The claim/line has been denied.", "status_effective_date": "2014-07-29" } ] } ], "statuses": [ { "adjudication_finalized_date": "2013-07-24", "claim_payment_amount": { "amount": "0", "currency": "USD" }, "remittance_date": "2013-07-30", "status_category": "Finalized/Denial-The claim/line has been denied.", "status_effective_date": "2014-07-29", "total_claim_amount": { "amount": "975", "currency": "USD" } } ], "total_claim_amount": { "amount": "975", "currency": "USD" }, "tracking_id": "D92BD44D-9F9F-4DE1-890B-61EF86" }, { "adjudication_finalized_date": "2013-08-23", "applied_to_deductible": false, "claim_control_number": "EH000000000", "claim_payment_amount": { "amount": "0", "currency": "USD" }, "remittance_date": "2013-08-27", "service_date": "2013-07-15", "service_end_date": "2013-07-15", "services": [ { "charge_amount": { "amount": "375", "currency": "USD" }, "cpt_code": "D9220", "payment_amount": { "amount": "0", "currency": "USD" }, "service_date": "2013-07-15", "service_end_date": "2013-07-15", "statuses": [ { "status_category": "Finalized/Denial-The claim/line has been denied.", "status_effective_date": "2014-07-29" } ] }, { "charge_amount": { "amount": "460", "currency": "USD" }, "cpt_code": "D7240", "payment_amount": { "amount": "0", "currency": "USD" }, "service_date": "2013-07-15", "service_end_date": "2013-07-15", "statuses": [ { "status_category": "Finalized/Denial-The claim/line has been denied.", "status_effective_date": "2014-07-29" } ] }, { "charge_amount": { "amount": "140", "currency": "USD" }, "cpt_code": "D7140", "payment_amount": { "amount": "0", "currency": "USD" }, "service_date": "2013-07-15", "service_end_date": "2013-07-15", "statuses": [ { "status_category": "Finalized/Denial-The claim/line has been denied.", "status_effective_date": "2014-07-29" } ] } ], "statuses": [ { "adjudication_finalized_date": "2013-08-23", "claim_payment_amount": { "amount": "0", "currency": "USD" }, "remittance_date": "2013-08-27", "status_category": "Finalized/Denial-The claim/line has been denied.", "status_effective_date": "2014-07-29", "total_claim_amount": { "amount": "975", "currency": "USD" } } ], "total_claim_amount": { "amount": "975", "currency": "USD" }, "tracking_id": "D92BD44D-9F9F-4DE1-890B-61EF86" } ], "first_name": "JANE", "id": "W100000000", "last_name": "SMITH" }, "payer": { "id": "MOCKPAYER", "name": "MOCKPAYER" }, "providers": [ { "first_name": "JOHN", "last_name": "DOE", "npi": "1234567890" } ], "submitter": { "first_name": "JOHN", "id": "1234567890", "last_name": "DOE" }, "trading_partner_id": "MOCKPAYER" }</div>

Example claims status response when adjudication is finalized, the claim has been denied (not paid) and the charges are applied to the deductible:

<div hljs="" class="hljs">{ "patient": { "claims": [ { "adjudication_finalized_date": "2014-04-04", "applied_to_deductible": true, "check_number": "814000000000000", "claim_control_number": "E6Y0C7NQG00", "claim_payment_amount": { "amount": "0", "currency": "USD" }, "remittance_date": "2014-04-14", "service_date": "2014-03-26", "service_end_date": "2014-03-26", "services": [ { "charge_amount": { "amount": "137", "currency": "USD" }, "cpt_code": "99213", "payment_amount": { "amount": "0", "currency": "USD" }, "service_date": "2014-03-26", "service_end_date": "2014-03-26", "statuses": [ { "status_category": "Finalized/Denial-The claim/line has been denied.", "status_code": "Processed according to contract provisions (Contract refers to provisions that exist between the Health Plan and a Provider of Health Care Services)", "status_effective_date": "2014-07-29" } ] }, { "charge_amount": { "amount": "290", "currency": "USD" }, "cpt_code": "76830", "payment_amount": { "amount": "0", "currency": "USD" }, "service_date": "2014-03-26", "service_end_date": "2014-03-26", "statuses": [ { "status_category": "Finalized/Denial-The claim/line has been denied.", "status_code": "Processed according to contract provisions (Contract refers to provisions that exist between the Health Plan and a Provider of Health Care Services)", "status_effective_date": "2014-07-29" } ] } ], "statuses": [ { "adjudication_finalized_date": "2014-04-04", "check_number": "814000000000000", "claim_payment_amount": { "amount": "0", "currency": "USD" }, "remittance_date": "2014-04-14", "status_category": "Finalized/Denial-The claim/line has been denied.", "status_code": "Processed according to contract provisions (Contract refers to provisions that exist between the Health Plan and a Provider of Health Care Services)", "status_effective_date": "2014-07-29", "total_claim_amount": { "amount": "427", "currency": "USD" } } ], "total_claim_amount": { "amount": "427", "currency": "USD" }, "tracking_id": "4AAA5F0D-986E-4C69-A428-67DA60" } ], "first_name": "JANE", "last_name": "DOE" }, "payer": { "id": "MOCKPAYER", "name": "MOCKPAYER" }, "providers": [ { "first_name": "JOHN", "last_name": "DOE", "npi": "1700000000" } ], "submitter": { "first_name": "JOHN", "id": "1700000000", "last_name": "DOE" }, "trading_partner_id": "MOCKPAYER" }</div>

curl example submitting a claims status request:

<div hljs="" class="hljs" language="bash">curl -i -H "Authorization: Bearer $ACCESS_TOKEN" -H "Content-Type: application/json" -XPOST -d '{ "patient": { "birth_date": "1970-01-01", "first_name": "JANE", "last_name": "DOE", "id": "1234567890" }, "provider": { "first_name": "Jerome", "last_name": "Aya-Ay", "npi": "1467560003" }, "service_date": "2014-01-01", "trading_partner_id": "MOCKPAYER" } ' https://platform.pokitdok.com/api/v4/claims/status</div>

Still have questions? Visit our [claims API workflow page](/claim-processing) for more information.

### Eligibility

_Available modes of operation: batch/async or real-time_

The eligibility resource makes it easy to verify a member's insurance information in real-time. You can check co-insurance, copay, deductible and out of pocket amounts for a member along with other information about a member's plan. Asynchronous eligibility requests can also be made. A callback_url can be supplied in the eligibility request to indicate that your application should be notified when an asynchronous eligibility request has completed and an eligibility response has been received from the trading partner. When a callback_url is specified, the full eligibility request activity will be POSTed back to the callback_url. The eligibility request activity may also be monitored via the [activities](#activities) API. Since some trading partners may take a few seconds (typically between 3-6 seconds) to process a 'real-time' eligibility request, applications may wish to use the callback_url and async operation mode so they don't block.

Use the [tradingpartners](#tradingpartners) API to determine available trading_partner_id values for use with the eligibility API.

Available Eligibility Endpoints:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Endpoint</th>

<th width="5%">HTTP Method</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>/eligibility/</td>

<td>POST</td>

<td>Determine eligibility via an EDI 270 Request For Eligibility</td>

</tr>

</tbody>

</table>

The /eligibility/ endpoint accepts the following parameters:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="60%">Argument</th>

<th width="40%">Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>cpt_code</td>

<td>The CPT code that should be used to request specific eligibility information. Note: requests based on CPT code are not supported by all trading partners.</td>

</tr>

<tr>

<td>member.birth_date</td>

<td>The named insured’s birth date as specified on their policy. May be omitted if member.id is provided.</td>

</tr>

<tr>

<td>member.first_name</td>

<td>The named insured’s first name as specified on their policy</td>

</tr>

<tr>

<td>member.id</td>

<td>The named insured’s member identifier. May be omitted if member.birth_date is provided.</td>

</tr>

<tr>

<td>member.last_name</td>

<td>The named insured’s last name as specified on their policy</td>

</tr>

<tr>

<td>provider.first_name</td>

<td>The provider’s first name when the provider is an individual</td>

</tr>

<tr>

<td>provider.last_name</td>

<td>The provider’s last name when the provider is an individual</td>

</tr>

<tr>

<td>provider.npi</td>

<td>The NPI for the provider.</td>

</tr>

<tr>

<td>provider.organization_name</td>

<td>The provider’s name when the provider is an organization. first_name and last_name should be omitted when sending organization_name</td>

</tr>

<tr>

<td>service_types</td>

<td>The line(s) of coverage in the insurance policy the eligibility request is being made against. For example, health_benefit_plan_coverage may be used to perform a general benefits inquiry to learn about the service types that are supported by the member's plan. If you're only interested in a particular service type or types, you may specify those type constants in the request. If you only wish to obtain eligibility information about chiropractic care, you can specify the chiropractic service type in the request. Note that some trading partners may not support specific service type inquiries. If no service_types are specified in the eligibility request, health_benefit_plan_coverage will be used as the default. A full listing of possible service_types values is included [below](#service_types).</td>

</tr>

<tr>

<td>trading_partner_id</td>

<td>Unique id for the intended trading partner, as specified by the Trading Partners resource</td>

</tr>

</tbody>

</table>

Example eligibility request to determine general health benefit coverage:

<div hljs="" class="hljs">{ "member": { "birth_date": "1970-01-01", "first_name": "Jane", "last_name": "Doe", "id": "W000000000" }, "provider": { "first_name": "JEROME", "last_name": "AYA-AY", "npi": "1467560003" }, "trading_partner_id": "MOCKPAYER" }</div>

Example eligibility request when operating on behalf of a member and a specific provider is not yet known:

<div hljs="" class="hljs">{ "member": { "birth_date": "1970-01-01", "first_name": "Jane", "last_name": "Doe", "id": "W000000000" }, "trading_partner_id": "MOCKPAYER" }</div>

Some trading partners support eligibility requests using a CPT code. Here's an example using a CPT code to request eligibility information:

<div hljs="" class="hljs">{ "member": { "birth_date": "1970-01-01", "first_name": "Jane", "last_name": "Doe", "id": "W000000000" }, "provider": { "first_name": "JEROME", "last_name": "AYA-AY", "npi": "1467560003" }, "cpt_code": "81291", "trading_partner_id": "MOCKPAYER" }</div>

Example eligibility request using custom application data for easy handling of asynchronous responses:

<div hljs="" class="hljs">{ "member": { "birth_date": "1970-01-01", "first_name": "Jane", "last_name": "Doe", "id": "W000000000" }, "provider": { "first_name": "JEROME", "last_name": "AYA-AY", "npi": "1467560003" }, "trading_partner_id": "MOCKPAYER", "application_data": { "patient_id": "ABC1234XYZ", "location_id": 123, "transaction_uuid": "93f38f1b-b2cd-4da1-8b55-c6e3ab380dbf" } }</div>

curl example submitting an eligibility request:

<div hljs="" class="hljs" language="bash">curl -i -H "Authorization: Bearer $ACCESS_TOKEN" -H "Content-Type: application/json" -XPOST -d '{ "member": { "birth_date": "1970-01-01", "first_name": "Jane", "last_name": "Doe", "id": "W000000000" }, "provider": { "first_name": "JEROME", "last_name": "AYA-AY", "npi": "1467560003" }, "service_types": ["health_benefit_plan_coverage"], "trading_partner_id": "MOCKPAYER" } ' https://platform.pokitdok.com/api/v4/eligibility/</div>

The /eligibility/ response contains the following fields:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="60%">Field</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>coverage.active</td>

<td>A boolean value that is true when the member has active coverage. It is false when membership information could not be returned or when inactive coverage is indicated by the trading partner.</td>

</tr>

<tr>

<td>coverage.coinsurance</td>

<td>List of co-insurance information for the member</td>

</tr>

<tr>

<td>coverage.coinsurance.benefit_percent</td>

<td>A percentage that represents the patient's portion of the responsibility for a benefit. (e.g. 0.2 when the patient's portion of the responsibility is 20% )</td>

</tr>

<tr>

<td>coverage.coinsurance.coverage_level</td>

<td>The coverage level that applies to this co-insurance information. When a family (or more than one person) is covered, you'll see deductible information for the family as well as the individual that was referenced in the eligibility request.</td>

</tr>

<tr>

<td>coverage.coinsurance.in_plan_network</td>

<td>Indicates whether or not the co-insurance information applies to in or out of network providers. If the co-insurance information is not dependent upon network status, not_applicable may be returned to indicate the value is the same for in and out of network providers.</td>

</tr>

<tr>

<td>coverage.coinsurance.service_types</td>

<td>A list of service types that apply to this co-insurance information. A full list of possible values is included [below](#service_types).</td>

</tr>

<tr>

<td>coverage.limitations.benefit_amount</td>

<td>Monetary amount for this deductible item. For calendar year deductible information, this will be the deductible for the calendar year for the associated coverage level and in/out of plan network indicator. (e.g. $7000.00 for in network, family coverage)</td>

</tr>

<tr>

<td>coverage.limitations.coverage_level</td>

<td>The coverage level that applies to this co-insurance information. When a family (or more than one person) is covered, you'll see deductible information for the family as well as the individual that was referenced in the eligibility request.</td>

</tr>

<tr>

<td>coverage.limitations.in_plan_network</td>

<td>Indicates whether or not the co-insurance information applies to in or out of network providers. If the co-insurance information is not dependent upon network status, not_applicable may be returned to indicate the value is the same for in and out of network providers.</td>

</tr>

<tr>

<td>coverage.limitations.service_types</td>

<td>A list of service types that apply to this co-insurance information. A full list of possible values is included [below](#service_types).</td>

</tr>

<tr>

<td>coverage.limitations.delivery</td>

<td>Specifies the delivery pattern of the health care services</td>

</tr>

<tr>

<td>coverage.limitations.delivery.quantity</td>

<td>The quantity of services being requested</td>

</tr>

<tr>

<td>coverage.limitations.delivery.quantity_qualifier</td>

<td>The qualifier used to indicate the quantity type (e.g. visits, month, hours, units, days)</td>

</tr>

<tr>

<td>coverage.contacts</td>

<td>List of contacts related to the member's coverage. This may include contact information for the payer as well as vendors or third party administrators.</td>

</tr>

<tr>

<td>coverage.contacts.name</td>

<td>The name of this contact.</td>

</tr>

<tr>

<td>coverage.contacts.id</td>

<td>The primary identifier for this contact.</td>

</tr>

<tr>

<td>coverage.contacts.email</td>

<td>email address that may be used for this contact.</td>

</tr>

<tr>

<td>coverage.contacts.phone</td>

<td>phone number for this contact.</td>

</tr>

<tr>

<td>coverage.contacts.url</td>

<td>URL for this contact. This is typically a link to the contact's web site.</td>

</tr>

<tr>

<td>coverage.contacts.contact_type</td>

<td>The type of entity related to this contact information. Possible values include: vendor, third_party_administrator, plan_sponsor, payer, primary_payer, secondary_payer, tertiary_payer, utilization_management_organization</td>

</tr>

<tr>

<td>coverage.contacts.service_types</td>

<td>A list of service types that apply to this contact information. A full list of possible values is included [below](#service_types).</td>

</tr>

<tr>

<td>coverage.copay</td>

<td>List of co-payment information for the member</td>

</tr>

<tr>

<td>coverage.copay.copayment</td>

<td>Monetary amount for this copay item. (e.g. 15 for a $15 co-pay)</td>

</tr>

<tr>

<td>coverage.copay.coverage_level</td>

<td>The coverage level that applies to this co-pay information. When a family (or more than one person) is covered, you'll see deductible information for the family as well as the individual that was referenced in the eligibility request.</td>

</tr>

<tr>

<td>coverage.copay.in_plan_network</td>

<td>Indicates whether or not the copay information applies to in or out of network providers. If the copay information is not dependent upon network status, not_applicable may be returned to indicate the value is the same for in and out of network providers.</td>

</tr>

<tr>

<td>coverage.copay.service_types</td>

<td>A list of service types that apply to this co-pay information. A full list of possible values is included [below](#service_types).</td>

</tr>

<tr>

<td>coverage.copay.delivery</td>

<td>Specifies the delivery pattern of the health care services</td>

</tr>

<tr>

<td>coverage.copay.delivery.quantity</td>

<td>The quantity of services being requested</td>

</tr>

<tr>

<td>coverage.copay.delivery.quantity_qualifier</td>

<td>The qualifier used to indicate the quantity type (e.g. visits, month, hours, units, days)</td>

</tr>

<tr>

<td>coverage.deductibles</td>

<td>List of deductible information for the member</td>

</tr>

<tr>

<td>coverage.deductibles.benefit_amount</td>

<td>Monetary amount for this deductible item. For calendar year deductible information, this will be the deductible for the calendar year for the associated coverage level and in/out of plan network indicator. (e.g. $7000.00 for in network, family coverage)</td>

</tr>

<tr>

<td>coverage.deductibles.coverage_level</td>

<td>The coverage level that applies to this deductible information. When a family (or more than one person) is covered, you'll see deductible information for the family as well as the individual that was referenced in the eligibility request.</td>

</tr>

<tr>

<td>coverage.deductibles.in_plan_network</td>

<td>Indicates whether or not the deductible information applies to in or out of network providers. If the deductible information is not dependent upon network status, not_applicable may be returned to indicate the value is the same for in and out of network providers.</td>

</tr>

<tr>

<td>coverage.deductibles.time_period</td>

<td>The period of time for which this deductible item applies. Possible values include: calendar_year and remaining. remaining indicates the amount that remains in the calendar year for the member to reach their deductible. calendar_year indicates that the amount represents the total deductible amount for the current year. When no time period applies to deductible information, time_period will not be included for that deductible in the response.</td>

</tr>

<tr>

<td>coverage.deductibles.service_types</td>

<td>A list of service types that apply to this deductible information. A full list of possible values is included [below](#service_types).</td>

</tr>

<tr>

<td>coverage.other_payers</td>

<td>A list of other payers that provide coverage for the member. This list of payers is primarily used to communicate information related to the coordination of benefits.</td>

</tr>

<tr>

<td>coverage.other_payers.coordination_of_benefits</td>

<td>The role of this payer in the coordination of benefits. Possible values include: primary_payer, secondary_payer, tertiary_payer</td>

</tr>

<tr>

<td>coverage.other_payers.coordination_of_benefits_date</td>

<td>The date when this payer started participating in the coordination of benefits.</td>

</tr>

<tr>

<td>coverage.other_payers.coverage_level</td>

<td>The coverage level for this plan. Possible values include: employee_only, employee_and_spouse, employee_and_children, family, individual</td>

</tr>

<tr>

<td>coverage.other_payers.id</td>

<td>The unique id used for this payer.</td>

</tr>

<tr>

<td>coverage.other_payers.name</td>

<td>The name of this payer. (e.g. MEDICARE)</td>

</tr>

<tr>

<td>coverage.other_payers.subscriber</td>

<td>The subscriber information associated with this payer.</td>

</tr>

<tr>

<td>coverage.other_payers.subscriber.id</td>

<td>The id for the subscriber in this payer's system.</td>

</tr>

<tr>

<td>coverage.other_payers.subscriber.first_name</td>

<td>The subscriber's first name</td>

</tr>

<tr>

<td>coverage.other_payers.subscriber.middle_name</td>

<td>The subscriber's middle name</td>

</tr>

<tr>

<td>coverage.other_payers.subscriber.last_name</td>

<td>The subscriber's last name</td>

</tr>

<tr>

<td>coverage.other_payers.service_types</td>

<td>A list of service types that apply to this payer information. A full list of possible values is included [below](#service_types).</td>

</tr>

<tr>

<td>coverage.out_of_pocket</td>

<td>List of out of pocket (stop loss) information for the member</td>

</tr>

<tr>

<td>coverage.out_of_pocket.benefit_amount</td>

<td>Monetary amount for this out of pocket item. For calendar year out of pocket information, this will be the out of pocket amount for the calendar year for the associated coverage level and in/out of plan network indicator. (e.g. $12600.00 for in network, family coverage)</td>

</tr>

<tr>

<td>coverage.out_of_pocket.coverage_level</td>

<td>The coverage level that applies to this out of pocket information. When a family (or more than one person) is covered, you'll see out of pocket information for the family as well as the individual that was referenced in the eligibility request.</td>

</tr>

<tr>

<td>coverage.out_of_pocket.in_plan_network</td>

<td>Indicates whether or not the out of pocket information applies to in or out of network providers. If the out of pocket information is not dependent upon network status, not_applicable may be returned to indicate the value is the same for in and out of network providers.</td>

</tr>

<tr>

<td>coverage.out_of_pocket.time_period</td>

<td>The period of time for which this out of pocket item applies. Possible values include: calendar_year and remaining. remaining indicates the amount that remains in the calendar year for the member to reach their out of pocket amount. calendar_year indicates that the amount represents the total out of pocket amount for the current year. When no time period applies to deductible information, time_period will not be included for that out of pocket item in the response.</td>

</tr>

<tr>

<td>coverage.out_of_pocket.service_types</td>

<td>A list of service types that apply to this out of pocket information. A full list of possible values is included [below](#service_types).</td>

</tr>

<tr>

<td>coverage.eligibility_begin_date</td>

<td>The date eligibility started for the member's plan.</td>

</tr>

<tr>

<td>coverage.group_description</td>

<td>Group description for the member specified in the eligibility request</td>

</tr>

<tr>

<td>coverage.group_number</td>

<td>Group number for the member specified in the eligibility request</td>

</tr>

<tr>

<td>coverage.insurance_type</td>

<td>The type of insurance coverage. Possible values include: hmo, ppo, pos, cobra, commercial, medicaid, medicare_part_a, medicare_part_b, other</td>

</tr>

<tr>

<td>coverage.level</td>

<td>The coverage level the member has for their plan. Possible values include: employee_only, employee_and_spouse, employee_and_children, family, individual</td>

</tr>

<tr>

<td>coverage.plan_begin_date</td>

<td>The date that plan coverage started for the member specified in the eligibility request</td>

</tr>

<tr>

<td>coverage.plan_end_date</td>

<td>The date that plan coverage ends for the member specified in the eligibility request</td>

</tr>

<tr>

<td>coverage.plan_description</td>

<td>The product name or special program name for the insurance plan. This is often the brand or marketing name for the plan.</td>

</tr>

<tr>

<td>coverage.plan_number</td>

<td>Plan ID/number for the member specified in the eligibility request</td>

</tr>

<tr>

<td>coverage.service_date</td>

<td>The date the eligibility request was processed</td>

</tr>

<tr>

<td>follow_up_action</td>

<td>When an eligibility request is rejected, a follow up action will be provided to inform your application how to proceed. Possible values include: correct_and_resubmit, resubmit_original, resubmission_not_allowed</td>

</tr>

<tr>

<td>payer.id</td>

<td>Payer ID returned by the trading partner for the eligibility request.</td>

</tr>

<tr>

<td>payer.name</td>

<td>Payer name returned by the trading partner for the eligibility request.</td>

</tr>

<tr>

<td>provider.npi</td>

<td>The NPI for the provider.</td>

</tr>

<tr>

<td>provider.first_name</td>

<td>The provider’s first name when the provider is an individual</td>

</tr>

<tr>

<td>provider.last_name</td>

<td>The provider’s last name when the provider is an individual</td>

</tr>

<tr>

<td>provider.organization_name</td>

<td>The provider’s name when the provider is an organization. first_name and last_name should be omitted when sending organization_name</td>

</tr>

<tr>

<td>reject_reason</td>

<td>When a trading partner is unable to provide eligibility information for an eligibility request, they will provide a reject reason. Possible values for reject_reason include: unable_to_respond_now, invalid_provider_id, patient_birth_date_mismatch, subscriber_insured_not_found.</td>

</tr>

<tr>

<td>subscriber.address</td>

<td>The subscriber's address information</td>

</tr>

<tr>

<td>subscriber.address.address_lines</td>

<td>The subscriber’s street address information as specified on their policy. (e.g. ["123 N MAIN ST"])</td>

</tr>

<tr>

<td>subscriber.address.city</td>

<td>The subscriber's city (e.g. "SAN MATEO")</td>

</tr>

<tr>

<td>subscriber.address.state</td>

<td>The subscriber's state (e.g. "CA")</td>

</tr>

<tr>

<td>subscriber.address.zipcode</td>

<td>The subscriber's zipcode (e.g. "94401")</td>

</tr>

<tr>

<td>subscriber.id</td>

<td>The subscriber's member identifier</td>

</tr>

<tr>

<td>subscriber.first_name</td>

<td>The subscriber’s first name as specified on their policy</td>

</tr>

<tr>

<td>subscriber.last_name</td>

<td>The subscriber’s last name as specified on their policy</td>

</tr>

<tr>

<td>subscriber.birth_date</td>

<td>The subscriber’s birth date as specified on their policy</td>

</tr>

<tr>

<td>subscriber.gender</td>

<td>The subscriber’s gender as specified on their policy. Possible values include: 'female', 'male', and 'unknown'. 'unknown' will be returned when gender is not specified in the trading partner's eligibility data or when the trading partner explicitly returns a value of 'unknown'</td>

</tr>

<tr>

<td>trading_partner_id</td>

<td>Unique id for the trading partner used to process the request</td>

</tr>

<tr>

<td>valid_request</td>

<td>A boolean value used to indicate that a trading partner considered the eligibility request valid and returned a full eligibility response. If valid_request is false, it means the trading partner was unable to respond to the request. Check the fields reject_reason and follow_up_action for more information on how to proceed when valid_request is false.</td>

</tr>

</tbody>

</table>

Example eligibility response when the trading partner is unable to respond at this time:

<div hljs="" class="hljs">{ "coverage": { "service_date": "2014-06-26" }, "follow_up_action": "resubmit_original", "provider": { "first_name": "JEROME", "last_name": "AYA-AY", "npi": "1467560003" }, "reject_reason": "unable_to_respond_now", "subscriber": { "birth_date": "1970-01-01", "first_name": "Jane", "id": "W000000000", "last_name": "Doe" }, "trading_partner_id": "MOCKPAYER", "valid_request": false }</div>

Example eligibility response when the trading partner is unable to find the member specified in the eligibility request:

<div hljs="" class="hljs">{ "coverage": { "service_date": "2014-06-26" }, "follow_up_action": "correct_and_resubmit", "provider": { "first_name": "JEROME", "last_name": "AYA-AY", "npi": "1467560003" }, "reject_reason": "subscriber_insured_not_found", "subscriber": { "birth_date": "1970-01-01", "first_name": "Jane", "id": "W000000000", "last_name": "Doe" }, "trading_partner_id": "MOCKPAYER", "valid_request": false }</div>

Example eligibility response when the trading partner is able to find a member based on the eligibility request but the specified birth date does not match their records:

<div hljs="" class="hljs">{ "coverage": { "service_date": "2014-06-26" }, "follow_up_action": "correct_and_resubmit", "provider": { "first_name": "JEROME", "last_name": "AYA-AY", "npi": "1467560003" }, "reject_reason": "patient_birth_date_mismatch", "subscriber": { "birth_date": "1970-01-01", "first_name": "Jane", "id": "W000000000", "last_name": "Doe" }, "trading_partner_id": "MOCKPAYER", "valid_request": false }</div>

Example eligibility response when the trading partner cannot process eligibility requests using CPT code:

<div hljs="" class="hljs">{ "coverage": { "service_date": "2014-06-26" }, "follow_up_action": "resubmission_not_allowed", "provider": { "first_name": "JEROME", "last_name": "AYA-AY", "npi": "1467560003" }, "reject_reason": "unable_to_respond_now", "subscriber": { "birth_date": "1970-01-01", "first_name": "Jane", "id": "W000000000", "last_name": "Doe" }, "trading_partner_id": "MOCKPAYER", "valid_request": false }</div>

Sample eligibility response for a successfully executed eligibility request:

<div hljs="" class="hljs">{ "coverage": { "coinsurance": [ { "benefit_percent": 0.0, "coverage_level": "employee_and_spouse", "in_plan_network": "yes", "messages": [], "service_types": [ "professional_physician_visit_office" ] }, { "benefit_percent": 0.5, "coverage_level": "employee_and_spouse", "in_plan_network": "no", "messages": [], "service_types": [ "professional_physician_visit_office" ] } ], "copay": [ { "copayment": { "amount": "0", "currency": "USD" }, "coverage_level": "employee_and_spouse", "in_plan_network": "yes", "messages": [ { "message": "PRIMARY OFFICE" } ], "service_types": [ "professional_physician_visit_office" ] }, { "copayment": { "amount": "0", "currency": "USD" }, "coverage_level": "employee_and_spouse", "in_plan_network": "not_applicable", "messages": [ { "message": "GYN OFFICE VS" }, { "message": "GYN VISIT" }, { "message": "SPEC OFFICE" }, { "message": "SPEC VISIT" }, { "message": "PRIME CARE VST" }, { "message": "Plan Requires PreCert" }, { "message": "Commercial" }, { "message": "Plan includes NAP" }, { "message": "Pre-Existing may apply" } ], "service_types": [ "professional_physician_visit_office" ] } ], "deductibles": [ { "benefit_amount": { "amount": "6000", "currency": "USD" }, "coverage_level": "family", "eligibility_date": "2013-01-01", "in_plan_network": "yes", "messages": [], "time_period": "calendar_year" }, { "benefit_amount": { "amount": "5956.09", "currency": "USD" }, "coverage_level": "family", "in_plan_network": "yes", "messages": [], "time_period": "remaining" }, { "benefit_amount": { "amount": "3000", "currency": "USD" }, "coverage_level": "individual", "eligibility_date": "2013-01-01", "in_plan_network": "yes", "messages": [], "time_period": "calendar_year" }, { "benefit_amount": { "amount": "2983.57", "currency": "USD" }, "coverage_level": "individual", "in_plan_network": "yes", "messages": [], "time_period": "remaining" }, { "benefit_amount": { "amount": "12000", "currency": "USD" }, "coverage_level": "family", "eligibility_date": "2013-01-01", "in_plan_network": "no", "messages": [], "time_period": "calendar_year" }, { "benefit_amount": { "amount": "11956.09", "currency": "USD" }, "coverage_level": "family", "in_plan_network": "no", "messages": [], "time_period": "remaining" }, { "benefit_amount": { "amount": "6000", "currency": "USD" }, "coverage_level": "individual", "eligibility_date": "2013-01-01", "in_plan_network": "no", "messages": [], "time_period": "calendar_year" }, { "benefit_amount": { "amount": "5983.57", "currency": "USD" }, "coverage_level": "individual", "in_plan_network": "no", "messages": [], "time_period": "remaining" } ], "eligibility_begin_date": "2012-02-01", "group_description": "MOCK INDIVIDUAL ADVANTAGE PLAN", "group_number": "000000000000013", "level": "employee_and_spouse", "out_of_pocket": [ { "benefit_amount": { "amount": "3000", "currency": "USD" }, "coverage_level": "individual", "in_plan_network": "yes" }, { "benefit_amount": { "amount": "2983.57", "currency": "USD" }, "coverage_level": "individual", "in_plan_network": "yes", "time_period": "remaining" }, { "benefit_amount": { "amount": "6000", "currency": "USD" }, "coverage_level": "family", "in_plan_network": "yes" }, { "benefit_amount": { "amount": "5956.09", "currency": "USD" }, "coverage_level": "family", "in_plan_network": "yes", "time_period": "remaining" }, { "benefit_amount": { "amount": "12500", "currency": "USD" }, "coverage_level": "individual", "in_plan_network": "no" }, { "benefit_amount": { "amount": "12483.57", "currency": "USD" }, "coverage_level": "individual", "in_plan_network": "no", "time_period": "remaining" }, { "benefit_amount": { "amount": "25000", "currency": "USD" }, "coverage_level": "family", "in_plan_network": "no" }, { "benefit_amount": { "amount": "24956.09", "currency": "USD" }, "coverage_level": "family", "in_plan_network": "no", "messages": [], "time_period": "remaining" } ], "plan_begin_date": "2013-02-15", "plan_number": "0000000", "service_date": "2013-08-10", "service_types": [ "professional_physician_visit_office" ] }, "payer": { "id": "MOCKPAYER", "name": "MOCK PAYER INC" }, "provider": { "first_name": "JEROME", "last_name": "JEROME AYA-AY", "npi": "1467560003" }, "service_types": [ "professional_physician_visit_office" ], "subscriber": { "address": { "address_lines": [ "123 MAIN ST" ], "city": "SAN MATEO", "state": "CA", "zipcode": "94401" }, "birth_date": "1970-01-01", "first_name": "Jane", "id": "W000000000", "last_name": "Doe" }, "trading_partner_id": "MOCKPAYER", "valid_request": true }</div>

Full listing of possible service type values that may be used in an eligibility request or returned in an eligibility response:

<div hljs="" class="hljs">{ "service_types": [ "abortion", "acupuncture", "adjunctive_dental_services", "aids", "air_transportation", "alcoholism", "allergy", "allergy_testing", "alternate_method_dialysis", "ambulatory_service_facility", "anesthesia", "anesthesiologist", "audiology_exam", "blood_charges", "brand_name_prescription_drug", "brand_name_prescription_drug_formulary", "brand_name_prescription_drug_non_formulary", "burn_care", "cabulance", "cancer", "cardiac", "cardiac_rehabilitation", "case_management", "chemotherapy", "chiropractic", "chiropractic_office_visits", "chronic_renal_disease_equipment", "cognitive_therapy", "consultation", "coronary_care", "day_care_psychiatric", "dental_accident", "dental_care", "dental_crowns", "dermatology", "diabetic_supplies", "diagnostic_dental", "diagnostic_lab", "diagnostic_medical", "diagnostic_x_ray", "dialysis", "donor_procedures", "drug_addiction", "emergency_services", "endocrine", "endodontics", "experimental_drug_therapy", "eye", "eyewear_and_accessories", "family_planning", "flu_vaccination", "frames", "free_standing_prescription_drug", "gastrointestinal", "general_benefits", "generic_prescription_drug", "generic_prescription_drug_formulary", "generic_prescription_drug_non_formulary", "gynecological", "health_benefit_plan_coverage", "home_health_care", "home_health_prescriptions", "home_health_visits", "hospice", "hospital", "hospital_ambulatory_surgical", "hospital_emergency_accident", "hospital_emergency_medical", "hospital_inpatient", "hospital_outpatient", "hospital_room_and_board", "immunizations", "in_vitro_fertilization", "independent_medical_evaluation", "infertility", "inhalation_therapy", "intensive_care", "invasive_procedures", "lenses", "licensed_ambulance", "long_term_care", "lymphatic", "mail_order_prescription_drug", "mail_order_prescription_drug_brand_name", "mail_order_prescription_drug_generic", "major_medical", "mammogram_high_risk_patient", "mammogram_low_risk_patient", "massage_therapy", "maternity", "maxillofacial_prosthetics", "medical_care", "medical_equipment", "medical_equipment_purchase", "medical_equipment_rental", "medically_related_transportation", "mental_health", "mental_health_facility_inpatient", "mental_health_facility_outpatient", "mental_health_provider_inpatient", "mental_health_provider_outpatient", "mri_cat_scan", "neonatal_intensive_care", "neurology", "newborn_care", "nonmedically_necessary_physical", "nursery", "obstetrical", "obstetrical_gynecological", "occupational_therapy", "oncology", "oral_surgery", "orthodontics", "orthopedic", "other_medical", "otological_exam", "partial_hospitalization_psychiatric", "pathology", "pediatric", "periodontics", "pharmacy", "physical_medicine", "physical_therapy", "physician_visit_office_sick", "physician_visit_office_well", "plan_waiting_period", "pneumonia_vaccine", "podiatry", "podiatry_nursing_home_visits", "podiatry_office_visits", "pre_admission_testing", "private_duty_nursing", "private_duty_nursing_home", "private_duty_nursing_inpatient", "professional_physician", "professional_physician_visit_home", "professional_physician_visit_inpatient", "professional_physician_visit_nursing_home", "professional_physician_visit_office", "professional_physician_visit_outpatient", "professional_physician_visit_skilled_nursing_facility", "prosthetic_device", "prosthodontics", "psychiatric", "psychiatric_inpatient", "psychiatric_outpatient", "psychiatric_room_and_board", "psychotherapy", "pulmonary", "pulmonary_rehabilitation", "radiation_therapy", "rehabilitation", "rehabilitation_inpatient", "rehabilitation_outpatient", "rehabilitation_room_and_board", "renal", "renal_supplies_in_the_home", "residential_psychiatric_treatment", "respite_care", "restorative", "routine_exam", "routine_physical", "routine_preventive_dental", "screening_laboratory", "screening_x_ray", "second_surgical_opinion", "skilled_nursing_care", "skilled_nursing_care_room_and_board", "skin", "smoking_cessation", "social_work", "speech_therapy", "substance_abuse", "substance_abuse_facility_inpatient", "substance_abuse_facility_outpatient", "surgical", "surgical_assistance", "surgical_benefits_facility", "surgical_benefits_professional_physician", "third_surgical_opinion", "transitional_care", "transitional_nursery_care", "transplants", "urgent_care", "used_medical_equipment", "vision_optometry", "well_baby_care" ] }</div>

### Enrollment

_Available modes of operation: batch/async only_

The /enrollment/ endpoint is used for benefits enrollment and maintenance. It may be used to enroll a new member in a health plan, update member information, and terminate benefits. Benefits enrollment requests generate activities that may be tracked via the /activities/ API.

Available Enrollment Endpoints:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Endpoint</th>

<th width="5%">HTTP Method</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>/enrollment/</td>

<td>POST</td>

<td>Submit a benefits enrollment request to the specified trading partner</td>

</tr>

</tbody>

</table>

The /enrollment/ endpoint accepts the following parameters:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="60%">Argument</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>dependents</td>

<td>A list of dependents covered under benefits by the subscriber. Each dependent list item may utilize the same request fields as a subscriber</td>

</tr>

<tr>

<td>sponsor</td>

<td>The employer/sponsor of the benefits</td>

</tr>

<tr>

<td>sponsor.name</td>

<td>The name of the sponsor</td>

</tr>

<tr>

<td>sponsor.tax_id</td>

<td>The tax id of the sponsor</td>

</tr>

<tr>

<td>subscriber</td>

<td>The subscriber/employee of the benefits</td>

</tr>

<tr>

<td>subscriber.address</td>

<td>The address for the subscriber</td>

</tr>

<tr>

<td>subscriber.address.line</td>

<td>The first address line for the subscriber</td>

</tr>

<tr>

<td>subscriber.address.line2</td>

<td>The second address line for the subscriber (optional)</td>

</tr>

<tr>

<td>subscriber.address.city</td>

<td>The city for the subscriber</td>

</tr>

<tr>

<td>subscriber.address.postal_code</td>

<td>The postal/zip code for the subscriber</td>

</tr>

<tr>

<td>subscriber.address.county</td>

<td>The county for the subscriber</td>

</tr>

<tr>

<td>subscriber.benefits</td>

<td>The list of benefits for the subscriber</td>

</tr>

<tr>

<td>subscriber.benefits.begin_date</td>

<td>The date benefits start for this list item</td>

</tr>

<tr>

<td>subscriber.benefits.begin_date</td>

<td>The date benefits start for this list item</td>

</tr>

<tr>

<td>subscriber.benefits.benefit_type</td>

<td>The type of benefit (Health, Dental, Vision, etc.)</td>

</tr>

<tr>

<td>subscriber.benefits.late_enrollment</td>

<td>Is the benefit enrolling late? true or false</td>

</tr>

<tr>

<td>subscriber.benefits.maintenance_type</td>

<td>The type of benefit maintenance (Addition, Cancellation or Termination, Delete, Reinstatement, etc)</td>

</tr>

<tr>

<td>subscriber.birth_date</td>

<td>The date of birth for the subscriber</td>

</tr>

<tr>

<td>subscriber.eligibility_begin_date</td>

<td>The date benefits become eligible for the subscriber</td>

</tr>

<tr>

<td>subscriber.employment_status</td>

<td>The employment status for the subscriber (Full-time, Executive, Hourly, etc.)</td>

</tr>

<tr>

<td>subscriber.first_name</td>

<td>The first name for the subscriber</td>

</tr>

<tr>

<td>subscriber.gender</td>

<td>The gender for the subscriber</td>

</tr>

<tr>

<td>subscriber.member_id</td>

<td>The member id for the subscriber if already enrolled in benefits</td>

</tr>

<tr>

<td>subscriber.middle_name</td>

<td>The middle name for the subscriber</td>

</tr>

<tr>

<td>subscriber.last_name</td>

<td>The last name for the subscriber</td>

</tr>

<tr>

<td>subscriber.substance_abuse</td>

<td>Does the subscriber have a problem with substance abuse? true or false</td>

</tr>

<tr>

<td>subscriber.suffix</td>

<td>The suffix for the subscriber (optional)</td>

</tr>

<tr>

<td>subscriber.ssn</td>

<td>The social security number for the subscriber</td>

</tr>

<tr>

<td>subscriber.tobacco_use</td>

<td>Does the subscriber use tobacco? true or false</td>

</tr>

<tr>

<td>trading_partner_id</td>

<td>Unique id for the intended trading partner, as specified by the Trading Partners resource</td>

</tr>

</tbody>

</table>

Example benefits enrollment request to enroll a subscriber in benefits (Health, Dental, Vision):

<div hljs="" class="hljs">{ "action": "Change", "dependents": [], "payer": { "tax_id": "111222333" }, "purpose": "Original", "reference_number": "12456", "sponsor": { "name": "Acme, Inc.", "tax_id": "999888777" }, "subscriber": { "address": { "city": "SAN MATEO", "county": "SAN MATEO", "line": "123 Main Street", "line2": "APT 1", "postal_code": "94403", "state": "CA" }, "benefit_status": "Active", "benefits": [ { "begin_date": "2014-01-01", "benefit_type": "Health", "coverage_level": "Employee Only", "late_enrollment": false, "maintenance_type": "Addition" }, { "begin_date": "2014-01-01", "benefit_type": "Dental", "late_enrollment": false, "maintenance_type": "Addition" }, { "begin_date": "2014-01-01", "benefit_type": "Vision", "late_enrollment": false, "maintenance_type": "Addition" } ], "birth_date": "1970-01-01", "contacts": [ { "communication_number2": "7172341240", "communication_type2": "Work Phone Number", "primary_communication_number": "7172343334", "primary_communication_type": "Home Phone Number" } ], "eligibility_begin_date": "2014-01-01", "employment_status": "Full-time", "first_name": "JANE", "gender": "Female", "group_or_policy_number": "123456001", "handicapped": false, "last_name": "DOE", "maintenance_reason": "Active", "maintenance_type": "Addition", "member_id": "123456789", "middle_name": "P", "relationship": "Self", "ssn": "123456789", "subscriber_number": "123456789", "substance_abuse": false, "tobacco_use": false }, "trading_partner_id": "MOCKPAYER", }</div>

### Insurance Prices

_Available modes of operation: real-time_

The Insurance Prices resource allows access to our collection of insurance pricing data. The data comes from private payer data as well as data from Medicare.

Available Insurance Prices Endpoints:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Endpoint</th>

<th width="5%">HTTP Method</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>/prices/insurance</td>

<td>GET</td>

<td>Return a list of prices for a given procedure (by CPT Code) in a given region (by ZIP Code)</td>

</tr>

</tbody>

</table>

The /prices/insurance endpoint accepts the following parameters:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Field</th>

<th nowrap="" width="5%">Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>cpt_code</td>

<td nowrap="">{string}</td>

<td>The CPT code of the procedure in question</td>

</tr>

<tr>

<td>zip_code</td>

<td nowrap="">{string}</td>

<td>Zip code in which to search for procedures</td>

</tr>

</tbody>

</table>

The /prices/insurance response contains the following fields:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Field</th>

<th nowrap="" width="5%">Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>amounts.average_price</td>

<td nowrap="">{decimal}</td>

<td>The weighted average price based on the number of procedures</td>

</tr>

<tr>

<td>amounts.cpt_code</td>

<td nowrap="">{string}</td>

<td>The CPT code of the procedure</td>

</tr>

<tr>

<td>amounts.geo_zip_area</td>

<td nowrap="">{string}</td>

<td>The three character zip code tabulation area code</td>

</tr>

<tr>

<td>amounts.high_price</td>

<td nowrap="">{decimal}</td>

<td>The maximum price for the procedure</td>

</tr>

<tr>

<td>amounts.low_price</td>

<td nowrap="">{decimal}</td>

<td>The lowest price for the procedure</td>

</tr>

<tr>

<td>amounts.median_price</td>

<td nowrap="">{decimal}</td>

<td>The median price for the procedure</td>

</tr>

<tr>

<td>amounts.payer_type</td>

<td nowrap="">{string}</td>

<td>The insurance payer type: insurance or medicare</td>

</tr>

<tr>

<td>amounts.payment_type</td>

<td nowrap="">{string}</td>

<td>Possible values are "allowed", "submitted", or "paid". The allowed amount is the dollar amount typically considered payment-in-full by a payer and an associated network of healthcare providers. For Medicare, the allowed amount is the average of the Medicare allowed amount for the service; this figure is the sum of the amount Medicare pays, the deductible and coinsurance amounts, and any amounts that a third party is responsible for paying. The submitted amount is the dollar amount the provider submitted to the payer in the insurance claim. The paid amount is the dollar amount that was reimbursed to the provider.</td>

</tr>

</tbody>

</table>

curl example fetching insurance price information:

<div hljs="" class="hljs" language="bash">curl -i -H "Authorization: Bearer $ACCESS_TOKEN" https://platform.pokitdok.com/api/v4/prices/insurance?cpt_code=87799&zip_code=32218</div>

### Medical Procedure Code

_Available modes of operation: real-time only_

The Medical Procedure Code resource provides access to clinical and consumer friendly information related to medical procedures. It's useful for identifying the procedure code (or codes) that match search queries. It can also be used for determining the official descriptions for a specific procedure code.

Available Medical Procedure Code Endpoints:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="30%">Endpoint</th>

<th>HTTP Method</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>/mpc/</td>

<td>GET</td>

<td>Get a list of medical procedure information meeting certain search criteria</td>

</tr>

<tr>

<td>/mpc/{code}</td>

<td>GET</td>

<td>Retrieve the data for a specific procedure code</td>

</tr>

</tbody>

</table>

The /mpc/ endpoint accepts the following parameters:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="20%">Field</th>

<th width="20%">Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>name</td>

<td>{string}</td>

<td>Search medical procedure information by consumer friendly name</td>

</tr>

<tr>

<td>description</td>

<td>{string}</td>

<td>A partial or full description to be used to locate medical procedure information</td>

</tr>

</tbody>

</table>

The /mpc/ response contains the following fields:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th>Field</th>

<th>Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>code</td>

<td>{string}</td>

<td>The procedure code</td>

</tr>

<tr>

<td>name</td>

<td>{string}</td>

<td>A consumer friendly name for the medical procedure</td>

</tr>

<tr>

<td>description</td>

<td>{string}</td>

<td>The medical procedure's clinical description</td>

</tr>

</tbody>

</table>

curl example fetching medical procedure information by code:

<div hljs="" class="hljs" language="bash">curl -i -H "Authorization: Bearer $ACCESS_TOKEN" https://platform.pokitdok.com/api/v4/mpc/99213</div>

curl example searching medical procedure information by consumer friendly name:

<div hljs="" class="hljs" language="bash">curl -i -H "Authorization: Bearer $ACCESS_TOKEN" https://platform.pokitdok.com/api/v4/mpc/?name=office</div>

### Payers

_Available modes of operation: real-time_

_The Payers endpoint will be deprecated in v5\. Use [Trading Partners](#tradingpartners) instead._

The Payers resource provides access to PokitDok’s collection of payer information.

Available Payer Endpoints:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Endpoint</th>

<th width="5%">HTTP Method</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>/payers/</td>

<td>GET</td>

<td>Get a list of payers</td>

</tr>

</tbody>

</table>

The /payers/ response contains the following fields:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Field</th>

<th nowrap="" width="5%">Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>payer_key</td>

<td nowrap="">{string}</td>

<td>A short name or code representing this payer</td>

</tr>

<tr>

<td>payer_name</td>

<td nowrap="">{string}</td>

<td>Full name for the payer</td>

</tr>

<tr>

<td>production_status</td>

<td nowrap="">{boolean}</td>

<td>Specifies if the Platform supports data transmissions with the payer.</td>

</tr>

<tr>

<td>supported_transactions</td>

<td nowrap="">{array}</td>

<td>A list of X12 Transaction set codes, 270, 276, etc, the payer supports.</td>

</tr>

<tr>

<td>trading_partner_id</td>

<td nowrap="">{string}</td>

<td>An id to be used in requests/EDI files to identify this payer</td>

</tr>

</tbody>

</table>

curl example fetching payer information:

<div hljs="" class="hljs" language="bash">curl -i -H "Authorization: Bearer $ACCESS_TOKEN" https://platform.pokitdok.com/api/v4/payers/</div>

### Plans

_Available modes of operation: real-time_

The Plans resource provides access to information about insurance plans.

Available Plans Endpoints:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Endpoint</th>

<th width="5%">HTTP Method</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>/plans/</td>

<td>GET</td>

<td>Search insurance plan information</td>

</tr>

</tbody>

</table>

The /plans/ endpoint accepts the following parameters:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="60%">Argument</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>trading_partner_id</td>

<td>The trading partner id of the payer offering the plan</td>

</tr>

<tr>

<td>county</td>

<td>The county in which the plan is available</td>

</tr>

<tr>

<td>state</td>

<td>The state in which the plan is available</td>

</tr>

<tr>

<td>plan_id</td>

<td>The identifier for the plan</td>

</tr>

<tr>

<td>plan_type</td>

<td>The type of plan (e.g. EPO, PPO, HMO, POS)</td>

</tr>

<tr>

<td>plan_name</td>

<td>The name of the plan</td>

</tr>

<tr>

<td>metallic_level</td>

<td>The metal level of the plan</td>

</tr>

</tbody>

</table>

The /plans/ response contains the following fields:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Field</th>

<th nowrap="" width="5%">Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>benefits_summary_url</td>

<td nowrap="">{string}</td>

<td>URL to benefit summary information for the plan</td>

</tr>

<tr>

<td>customer_service_phone</td>

<td nowrap="">{string}</td>

<td>The customer service phone number for the plan</td>

</tr>

<tr>

<td>deductible</td>

<td nowrap="">{object}</td>

<td>The deductible amounts for individual and family coverage (when available)</td>

</tr>

<tr>

<td>deductible.individual</td>

<td nowrap="">{float}</td>

<td>The deductible amount for individual coverage (when available)</td>

</tr>

<tr>

<td>deductible.family</td>

<td nowrap="">{float}</td>

<td>The deductible amount for family coverage (when available)</td>

</tr>

<tr>

<td>max_out_of_pocket</td>

<td nowrap="">{object}</td>

<td>The maximum out of pocket amounts for individual and family coverage (when available)</td>

</tr>

<tr>

<td>max_out_of_pocket.individual</td>

<td nowrap="">{float}</td>

<td>The maximum out of pocket amount for individual coverage (when available)</td>

</tr>

<tr>

<td>max_out_of_pocket.family</td>

<td nowrap="">{float}</td>

<td>The maximum out of pocket amount for family coverage (when available)</td>

</tr>

<tr>

<td>metallic_level</td>

<td nowrap="">{string}</td>

<td>The metal level for marketplace plans (e.g.: bronze, silver, gold, and platinum)</td>

</tr>

<tr>

<td>plan_id</td>

<td nowrap="">{string}</td>

<td>The ID assigned to the plan by the issuer</td>

</tr>

<tr>

<td>plan_name</td>

<td nowrap="">{string}</td>

<td>Full name of the insurance plan</td>

</tr>

<tr>

<td>plan_type</td>

<td nowrap="">{string}</td>

<td>The type of the plan (e.g.: PPO, HMO, EPO, etc.)</td>

</tr>

<tr>

<td>premiums</td>

<td nowrap="">{array}</td>

<td>A list of monthly premium information for the plan (when available)</td>

</tr>

<tr>

<td>premiums.age</td>

<td nowrap="">{int}</td>

<td>The age of the insurance subscriber</td>

</tr>

<tr>

<td>premiums.adults</td>

<td nowrap="">{int}</td>

<td>Number of adults covered on the plan</td>

</tr>

<tr>

<td>premiums.children</td>

<td nowrap="">{int}</td>

<td>Number of children covered on the plan</td>

</tr>

<tr>

<td>premiums.cost</td>

<td nowrap="">{float}</td>

<td>The monthly premium cost for the plan</td>

</tr>

<tr>

<td>state</td>

<td nowrap="">{string}</td>

<td>The state where the plan is offered (e.g.: CA, SC, etc.)</td>

</tr>

<tr>

<td>trading_partner_id</td>

<td nowrap="">{string}</td>

<td>The trading partner id for the issuer of the plan</td>

</tr>

</tbody>

</table>

curl example fetching plan information:

<div hljs="" class="hljs" language="bash">curl -i -H "Authorization: Bearer $ACCESS_TOKEN" https://platform.pokitdok.com/api/v4/plans/</div>

curl example fetching information for plans in Texas:

<div hljs="" class="hljs" language="bash">curl -i -H "Authorization: Bearer $ACCESS_TOKEN" https://platform.pokitdok.com/api/v4/plans/?state=TX</div>

curl example fetching information for PPO plans in South Carolina:

<div hljs="" class="hljs" language="bash">curl -i -H "Authorization: Bearer $ACCESS_TOKEN" https://platform.pokitdok.com/api/v4/plans/?state=SC&plan_type=PPO</div>

### Providers

_Available modes of operation: real-time only_

The Providers resource provides access to PokitDok’s provider directory. The Providers endpoints can be used to search for Providers, view biographical, education and credential information, and view specialty taxonomies. Any of the above listed keywords can be used to show additional fields, perform searches, and page through results.

Available Provider Endpoints:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Endpoint</th>

<th width="5%">HTTP Method</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>/providers/</td>

<td>GET</td>

<td>Get a list of providers meeting certain search criteria</td>

</tr>

<tr>

<td>/providers/{id}</td>

<td>GET</td>

<td>Retrieve the data for a specified provider; the ID is the provider's NPI</td>

</tr>

</tbody>

</table>

The /providers/ endpoint accepts the following parameters:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Field</th>

<th nowrap="" width="5%">Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>city</td>

<td nowrap="">{string}</td>

<td>Name of city in which to search for providers (e.g. "San Mateo" or "Charleston")</td>

</tr>

<tr>

<td>first_name</td>

<td nowrap="">{string}</td>

<td>The provider's first name</td>

</tr>

<tr>

<td>last_name</td>

<td nowrap="">{string}</td>

<td>The provider's last name</td>

</tr>

<tr>

<td>organization_name</td>

<td nowrap="">{string}</td>

<td>The business practice name</td>

</tr>

<tr>

<td>radius</td>

<td nowrap="">{string}</td>

<td>Search distance from geographic centerpoint, with unit (e.g. "1mi")</td>

</tr>

<tr>

<td>specialty</td>

<td nowrap="">{string}</td>

<td>The provider's specialty name (e.g. "RHEUMATOLOGY")</td>

</tr>

<tr>

<td>state</td>

<td nowrap="">{string}</td>

<td>Name of U.S. state in which to search for providers (e.g. "CA" or "SC")</td>

</tr>

<tr>

<td>zipcode</td>

<td nowrap="">{string}</td>

<td>Geographic center point in which to search for providers (e.g. "94401")</td>

</tr>

</tbody>

</table>

The /providers/ response returns contains the following fields:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Field</th>

<th nowrap="" width="5%">Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>provider.birth_date</td>

<td nowrap="">{string}</td>

<td>The provider’s birth year</td>

</tr>

<tr>

<td>provider.board_certifications</td>

<td nowrap="">{array}</td>

<td>The provider’s board certifications</td>

</tr>

<tr>

<td>provider.board_subcertifications</td>

<td nowrap="">{array}</td>

<td>The provider’s board sub-certifications</td>

</tr>

<tr>

<td>provider.degree</td>

<td nowrap="">{string}</td>

<td>The provider's degree ("MD" or "DO")</td>

</tr>

<tr>

<td>provider.education</td>

<td nowrap="">{dict}</td>

<td>The provider’s medical school and graduation year</td>

</tr>

<tr>

<td>provider.fax</td>

<td nowrap="">{string}</td>

<td>The provider's fax number</td>

</tr>

<tr>

<td>provider.first_name</td>

<td nowrap="">{string}</td>

<td>The provider’s first name</td>

</tr>

<tr>

<td>provider.gender</td>

<td nowrap="">{string}</td>

<td>The provider’s gender</td>

</tr>

<tr>

<td>provider.last_name</td>

<td nowrap="">{string}</td>

<td>The provider’s last name</td>

</tr>

<tr>

<td>provider.licenses</td>

<td nowrap="">{array}</td>

<td>Additional license information</td>

</tr>

<tr>

<td>provider.licensures</td>

<td nowrap="">{array}</td>

<td>State licensure dates, numbers, and activation status</td>

</tr>

<tr>

<td>provider.locations</td>

<td nowrap="">{array}</td>

<td>List of locations associated with the provider. Each location object includes address information with geocode data, fax, and phone when available. The role of the location is also included (e.g. "practice" and/or "mailing" )</td>

</tr>

<tr>

<td>provider.middle_name</td>

<td nowrap="">{string}</td>

<td>The provider’s middle name</td>

</tr>

<tr>

<td>provider.npi</td>

<td nowrap="">{string}</td>

<td>The provider’s NPI</td>

</tr>

<tr>

<td>provider.organization_name</td>

<td nowrap="">{string}</td>

<td>The business practice name</td>

</tr>

<tr>

<td>provider.phone</td>

<td nowrap="">{string}</td>

<td>The provider's phone number</td>

</tr>

<tr>

<td>provider.prefix</td>

<td nowrap="">{string}</td>

<td>The provider’s prefix (Mr., Mrs., Dr., etc)</td>

</tr>

<tr>

<td>provider.residencies</td>

<td nowrap="">{array}</td>

<td>Provider residency and fellowship information</td>

</tr>

<tr>

<td>provider.specialty</td>

<td nowrap="">{string}</td>

<td>List of specialties from the specialty taxonomy associated with the provider</td>

</tr>

<tr>

<td>provider.specialty_primary</td>

<td nowrap="">{array}</td>

<td>The provider's primary specialties</td>

</tr>

<tr>

<td>provider.specialty_secondary</td>

<td nowrap="">{array}</td>

<td>The provider's secondary specialties</td>

</tr>

<tr>

<td>provider.suffix</td>

<td nowrap="">{string}</td>

<td>The provider’s suffix (MD, Jr., etc)</td>

</tr>

<tr>

<td>provider.uuid</td>

<td nowrap="">{uuid}</td>

<td>The provider's unique PokitDok Platform identifier</td>

</tr>

<tr>

<td>provider.verified</td>

<td nowrap="">{boolean}</td>

<td>Provider PokitDok verification status</td>

</tr>

<tr>

<td>provider.description</td>

<td nowrap="">{string}</td>

<td>Optional: (verified providers only) Provider full-text description</td>

</tr>

<tr>

<td>provider.facebook_url</td>

<td nowrap="">{string}</td>

<td>Optional: (verified providers only) Provider Facebook URL</td>

</tr>

<tr>

<td>provider.small_image_url</td>

<td nowrap="">{string}</td>

<td>Optional: (verified providers only) Provider small image URL</td>

</tr>

<tr>

<td>provider.twitter_url</td>

<td nowrap="">{string}</td>

<td>Optional: (verified providers only) Provider Twitter URL</td>

</tr>

<tr>

<td>provider.website_url</td>

<td nowrap="">{string}</td>

<td>Optional: (verified providers only) Provider website URL</td>

</tr>

</tbody>

</table>

curl example fetching provider information by NPI:

<div hljs="" class="hljs" language="bash">curl -i -H "Authorization: Bearer $ACCESS_TOKEN" https://platform.pokitdok.com/api/v4/providers/1467560003</div>

curl example searching providers by zipcode and specialty:

<div hljs="" class="hljs" language="bash">curl -i -H "Authorization: Bearer $ACCESS_TOKEN" https://platform.pokitdok.com/api/v4/providers/?zipcode=29307&specialty=rheumatology&radius=20mi</div>

### Referrals

_Available modes of operation: batch/async or real-time_

The Referrals resource allows an application to request approval for a referral to another health care provider.

Available Referrals Endpoints:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Endpoint</th>

<th width="5%">HTTP Method</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>/referrals/</td>

<td>POST</td>

<td>Submit a specialty care referral request to a trading partner for approval</td>

</tr>

</tbody>

</table>

The /referrals/ endpoint accepts the following parameters:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="60%">Argument</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>event</td>

<td>The patient event that is being submitted for approval</td>

</tr>

<tr>

<td>event.category</td>

<td>The category of the event being submitted for review. For referrals to specialists, a category value of "specialty_care_review" should always be used.</td>

</tr>

<tr>

<td>event.certification_type</td>

<td>The type of certification being requested. For new referrals, a certification value of "initial" should always be used.</td>

</tr>

<tr>

<td>event.delivery</td>

<td>Specifies the delivery pattern of the health care services</td>

</tr>

<tr>

<td>event.delivery.quantity</td>

<td>The quantity of services being requested</td>

</tr>

<tr>

<td>event.delivery.quantity_qualifier</td>

<td>The qualifier used to indicate the quantity type (e.g. visits, month, hours, units, days)</td>

</tr>

<tr>

<td>event.diagnoses</td>

<td>An array of diagnosis information related to the event</td>

</tr>

<tr>

<td>event.diagnoses.code</td>

<td>The diagnosis code (e.g. 384.20)</td>

</tr>

<tr>

<td>event.diagnoses.date</td>

<td>The date of the diagnosis</td>

</tr>

<tr>

<td>event.place_of_service</td>

<td>The location where health care services are rendered</td>

</tr>

<tr>

<td>event.provider</td>

<td>Information about the provider being requested for this event</td>

</tr>

<tr>

<td>event.provider.first_name</td>

<td>The event provider’s first name when the provider is an individual</td>

</tr>

<tr>

<td>event.provider.last_name</td>

<td>The event provider’s last name when the provider is an individual</td>

</tr>

<tr>

<td>event.provider.npi</td>

<td>The NPI for the event provider.</td>

</tr>

<tr>

<td>event.provider.organization_name</td>

<td>The event provider’s name when the provider is an organization. first_name and last_name should be omitted when sending organization_name</td>

</tr>

<tr>

<td>event.type</td>

<td>The type of service being requested. For example, a value of consultation would be used when referring to a specialist for an initial consultation.</td>

</tr>

<tr>

<td>patient.birth_date</td>

<td>The patient’s birth date as specified on their policy</td>

</tr>

<tr>

<td>patient.id</td>

<td>The patient’s member identifier</td>

</tr>

<tr>

<td>patient.first_name</td>

<td>The patient’s first name as specified on their policy</td>

</tr>

<tr>

<td>patient.last_name</td>

<td>The patient’s last name as specified on their policy</td>

</tr>

<tr>

<td>provider.first_name</td>

<td>The referring provider’s first name when the provider is an individual</td>

</tr>

<tr>

<td>provider.last_name</td>

<td>The referring provider’s last name when the provider is an individual</td>

</tr>

<tr>

<td>provider.npi</td>

<td>The NPI for the referring provider.</td>

</tr>

<tr>

<td>provider.organization_name</td>

<td>The referring provider’s name when the provider is an organization. first_name and last_name should be omitted when sending organization_name</td>

</tr>

<tr>

<td>subscriber.birth_date</td>

<td>Optional: The subscriber’s birth date as specified on their policy. Specify when the patient is not the subscriber on the insurance policy.</td>

</tr>

<tr>

<td>subscriber.first_name</td>

<td>Optional: The subscriber’s first name as specified on their policy. Specify when the patient is not the subscriber on the insurance policy.</td>

</tr>

<tr>

<td>subscriber.id</td>

<td>Optional: The subscriber’s member identifier. Specify when the patient is not the subscriber on the insurance policy.</td>

</tr>

<tr>

<td>subscriber.last_name</td>

<td>Optional: The subscriber’s last name as specified on their policy. Specify when the patient is not the subscriber on the insurance policy.</td>

</tr>

<tr>

<td>trading_partner_id</td>

<td>Unique id for the intended trading partner, as specified by the Trading Partners resource</td>

</tr>

</tbody>

</table>

Here's an example referral request to an Otolaryngologist (ENT) by a primary care physician. In this example, the patient is also the subscriber on the insurance policy:

<div hljs="" class="hljs">{ "event": { "category": "specialty_care_review", "certification_type": "initial", "delivery": { "quantity": 1, "quantity_qualifier": "visits" }, "diagnoses": [ { "code": "384.20", "date": "2014-09-30" } ], "place_of_service": "office", "provider": { "first_name": "JOHN", "npi": "1154387751", "last_name": "FOSTER", "phone": "8645822900" }, "type": "consultation" }, "patient": { "birth_date": "1970-01-01", "first_name": "JANE", "last_name": "DOE", "id": "1234567890" }, "provider": { "first_name": "CHRISTINA", "last_name": "BERTOLAMI", "npi": "1619131232" }, "trading_partner_id": "MOCKPAYER" }</div>

The /referrals/ response contains the following fields:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="60%">Field</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>event</td>

<td>The patient event that is being submitted for approval</td>

</tr>

<tr>

<td>event.category</td>

<td>The category of the event being submitted for review. For referrals to specialists, a category value of "specialty_care_review" should always be used.</td>

</tr>

<tr>

<td>event.certification_type</td>

<td>The type of certification being requested. For new referrals, a certification value of "initial" should always be used.</td>

</tr>

<tr>

<td>event.delivery</td>

<td>Specifies the delivery pattern of the health care services</td>

</tr>

<tr>

<td>event.delivery.quantity</td>

<td>The quantity of services being requested</td>

</tr>

<tr>

<td>event.delivery.quantity_qualifier</td>

<td>The qualifier used to indicate the quantity type (e.g. visits, month, hours, units, days)</td>

</tr>

<tr>

<td>event.diagnoses</td>

<td>An array of diagnosis information related to the event</td>

</tr>

<tr>

<td>event.diagnoses.code</td>

<td>The diagnosis code (e.g. 384.20)</td>

</tr>

<tr>

<td>event.diagnoses.date</td>

<td>The date of the diagnosis</td>

</tr>

<tr>

<td>event.place_of_service</td>

<td>The location where health care services are rendered</td>

</tr>

<tr>

<td>event.provider</td>

<td>Information about the provider being requested for this event</td>

</tr>

<tr>

<td>event.provider.first_name</td>

<td>The event provider’s first name when the provider is an individual</td>

</tr>

<tr>

<td>event.provider.last_name</td>

<td>The event provider’s last name when the provider is an individual</td>

</tr>

<tr>

<td>event.provider.npi</td>

<td>The NPI for the event provider.</td>

</tr>

<tr>

<td>event.provider.organization_name</td>

<td>The event provider’s name when the provider is an organization. first_name and last_name should be omitted when sending organization_name</td>

</tr>

<tr>

<td>event.review</td>

<td>Information about the outcome of a health care services review</td>

</tr>

<tr>

<td>event.review.certification_action</td>

<td>Indicates the outcome of the review. For example, "certified_in_total" will be returned when the event is certified/authorized.</td>

</tr>

<tr>

<td>event.review.certification_number</td>

<td>The review certification/reference number</td>

</tr>

<tr>

<td>event.review.decision_reason</td>

<td>If the event is not authorized, the reason for that decision.</td>

</tr>

<tr>

<td>event.type</td>

<td>The type of service being requested. For example, a value of "consultation" would be used when referring to a specialist for an initial consultation.</td>

</tr>

<tr>

<td>patient.birth_date</td>

<td>The patient’s birth date as specified on their policy</td>

</tr>

<tr>

<td>patient.id</td>

<td>The patient’s member identifier</td>

</tr>

<tr>

<td>patient.first_name</td>

<td>The patient’s first name as specified on their policy</td>

</tr>

<tr>

<td>patient.last_name</td>

<td>The patient’s last name as specified on their policy</td>

</tr>

<tr>

<td>provider.first_name</td>

<td>The referring provider’s first name when the provider is an individual</td>

</tr>

<tr>

<td>provider.last_name</td>

<td>The referring provider’s last name when the provider is an individual</td>

</tr>

<tr>

<td>provider.npi</td>

<td>The NPI for the referring provider.</td>

</tr>

<tr>

<td>provider.organization_name</td>

<td>The referring provider’s name when the provider is an organization. first_name and last_name should be omitted when sending organization_name</td>

</tr>

<tr>

<td>subscriber.birth_date</td>

<td>Optional: The subscriber’s birth date as specified on their policy. Specify when the patient is not the subscriber on the insurance policy.</td>

</tr>

<tr>

<td>subscriber.first_name</td>

<td>Optional: The subscriber’s first name as specified on their policy. Specify when the patient is not the subscriber on the insurance policy.</td>

</tr>

<tr>

<td>subscriber.id</td>

<td>Optional: The subscriber’s member identifier. Specify when the patient is not the subscriber on the insurance policy.</td>

</tr>

<tr>

<td>subscriber.last_name</td>

<td>Optional: The subscriber’s last name as specified on their policy. Specify when the patient is not the subscriber on the insurance policy.</td>

</tr>

<tr>

<td>trading_partner_id</td>

<td>Unique id for the intended trading partner, as specified by the Trading Partners resource</td>

</tr>

</tbody>

</table>

Example referrals response when the trading partner has authorized the request:

<div hljs="" class="hljs">{ "event": { "category": "specialty_care_review", "certification_type": "initial", "delivery": { "quantity": 1, "quantity_qualifier": "visits" }, "diagnoses": [ { "code": "384.20", "date": "2014-09-30" } ], "place_of_service": "office", "provider": { "first_name": "JOHN", "npi": "1154387751", "last_name": "FOSTER", "phone": "8645822900" }, "review": { "certification_action": "certified_in_total", "certification_number": "AUTH0001" }, "type": "consultation" }, "patient": { "birth_date": "1970-01-01", "first_name": "JANE", "last_name": "DOE", "id": "1234567890" }, "provider": { "first_name": "CHRISTINA", "last_name": "BERTOLAMI", "npi": "1619131232" }, "trading_partner_id": "MOCKPAYER" }</div>

curl example submitting a referral request:

<div hljs="" class="hljs" language="bash">curl -i -H "Authorization: Bearer $ACCESS_TOKEN" -H "Content-Type: application/json" -XPOST -d '{ "event": { "category": "specialty_care_review", "certification_type": "initial", "delivery": { "quantity": 1, "quantity_qualifier": "visits" }, "diagnoses": [ { "code": "384.20", "date": "2014-09-30" } ], "place_of_service": "office", "provider": { "first_name": "JOHN", "npi": "1154387751", "last_name": "FOSTER", "phone": "8645822900" }, "type": "consultation" }, "patient": { "birth_date": "1970-01-01", "first_name": "JANE", "last_name": "DOE", "id": "1234567890" }, "provider": { "first_name": "CHRISTINA", "last_name": "BERTOLAMI", "npi": "1619131232" }, "trading_partner_id": "MOCKPAYER" } ' https://platform.pokitdok.com/api/v4/referrals/</div>

### Scheduling

_Available modes of operation: real-time_

The PokitDok Scheduling API performs schedule aggregation to multiple third party scheduling applications, typically commercial off-the-shelf Practice Management (PM) software, for private practices, provider networks, hospital systems, etc. The PokitDok Scheduling API also provides a built-in scheduling system for providers without a PM. The API provides consumers a means to schedule appointments in a manner independent of the provider's PM system. OAuth scopes are used to restrict access to provider's PM systems and user's personal information.

Available Scheduling Endpoints:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Endpoint</th>

<th width="5%">HTTP Method</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>/schedule/schedulers/</td>

<td>GET</td>

<td>Get a list of supported scheduling systems and their UUIDs and descriptions.</td>

</tr>

</tbody>

</table>

curl example fetching all schedulers:

<div hljs="" class="hljs" language="bash">curl -s -H "Authorization: Bearer $ACCESS_TOKEN" https://platform.pokitdok.com/api/v4/schedule/schedulers/</div>

#### Response

<div hljs="" class="hljs">[ { "description": "Greenway EHR and PM", "scheduler_uuid": "d8f38f08-8530-11e4-9a71-0800272e8da1", "name": "Greenway" }, { "description": "Athena EHR and PM", "scheduler_uuid": "d8f46c7a-8530-11e4-9a71-0800272e8da1", "name": "Athena" } ]</div>

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Endpoint</th>

<th width="5%">HTTP Method</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>/schedule/schedulers/{uuid}</td>

<td>GET</td>

<td>Retrieve the data for a specified scheduling system.</td>

</tr>

</tbody>

</table>

curl example fetching a specific scheduler:

<div hljs="" class="hljs" language="bash">curl -s -H "Authorization: Bearer $ACCESS_TOKEN" https://platform.pokitdok.com/api/v4/schedule/schedulers/d8f38f08-8530-11e4-9a71-0800272e8da1</div>

#### Response

<div hljs="" class="hljs">[ { "description": "Greenway EHR and PM", "scheduler_uuid": "d8f4fc58-8530-11e4-9a71-0800272e8da1", "name": "Greenway" } ]</div>

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Endpoint</th>

<th width="5%">HTTP Method</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>/schedule/appointmenttypes/</td>

<td>GET</td>

<td>Get a list of appointment types, their UUIDs, and descriptions.</td>

</tr>

</tbody>

</table>

curl example fetching appointment types:

<div hljs="" class="hljs" language="bash">curl -s -H "Authorization: Bearer $ACCESS_TOKEN" https://platform.pokitdok.com/api/v4/schedule/appointmenttypes/</div>

#### Response

<div hljs="" class="hljs">[ { "type": "OV1", "appointment_type_uuid": "ef987691-0a19-447f-814d-f8f3abbf4860", "description": "Office Visit" }, { "type": "PC1", "appointment_type_uuid": "ef987692-0a19-447f-814d-f8f3abbf4860", "description": "Routine Preventive Care" } ]</div>

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Endpoint</th>

<th width="5%">HTTP Method</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>/schedule/appointmenttypes/{uuid}</td>

<td>GET</td>

<td>Retrieve the data for a specified appointment type.</td>

</tr>

</tbody>

</table>

curl example fetching a specified appointment type:

<div hljs="" class="hljs" language="bash">curl -s -H "Authorization: Bearer $ACCESS_TOKEN" https://platform.pokitdok.com/api/v4/schedule/appointmenttypes/ef987691-0a19-447f-814d-f8f3abbf4860</div>

#### Response

<div hljs="" class="hljs">[ { "type": "OV1", "appointment_type_uuid": "ef987693-0a19-447f-814d-f8f3abbf4860", "description": "Office Visit" } ]</div>

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Endpoint</th>

<th width="5%">HTTP Method</th>

<th>Description</th>

<th width="20%">Scope</th>

</tr>

</thead>

<tbody>

<tr>

<td>/schedule/appointments/</td>

<td>GET</td>

<td>Query for open appointment slots (using pd_provider_uuid and location) or booked appointments (using patient_uuid) given query parameters.</td>

<td>user_schedule</td>

</tr>

</tbody>

</table>

The /schedule/appointments/ endpoint accepts the following parameters:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Field</th>

<th nowrap="" width="5%">Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>appointment_type</td>

<td nowrap="">{string}</td>

<td>The "appointment_type" used to identify a specific appointment procedure/category.</td>

</tr>

<tr>

<td>start_date</td>

<td nowrap="">{datetime}</td>

<td>The beginning date and UTC time of the appointment query, formatted as ISO8601.</td>

</tr>

<tr>

<td>end_date</td>

<td nowrap="">{datetime}</td>

<td>The ending date and UTC time of the appointment query, formatted as ISO8601.</td>

</tr>

<tr>

<td>patient_uuid</td>

<td nowrap="">{uuid}</td>

<td>The PokitDok unique identifier for the patient that has booked an appointment.</td>

</tr>

<tr>

<td>pd_provider_uuid</td>

<td nowrap="">{uuid}</td>

<td>The PokitDok unique identifier for the provider that has an open appointment slot.</td>

</tr>

<tr>

<td>location</td>

<td nowrap="">{geo-location}</td>

<td>The geo-location of the physical address where the provider that has an open appointment slot, formatted [latitude, longitude].</td>

</tr>

</tbody>

</table>

curl example querying for open slots and booked appointments:

<div hljs="" class="hljs" language="bash">curl -s -H "Authorization: Bearer $ACCESS_TOKEN" "https://platform.pokitdok.com/api/v4/schedule/appointments/?appointment_type=SS1&start_date=2015-01-14T08:00:00&end_date=2015-01-16T17:00:00&patient_uuid=8ae236ff-9ccc-44b0-8717-42653cd719d0"</div>

#### Response

<div hljs="" class="hljs">[ { "pd_appointment_uuid": "ef987691-0a19-447f-814d-f8f3abbf4859", "appointment_id": "UYQDUHSMIRCA", "appointment_type": "OV1", "patient": { "first_name": "John", "last_name": "Doe", "_uuid": "8b21f7b0-8535-11e4-a6cb-0800272e8da1", "phone": "800-555-1212", "member_id": "M000001", "birth_date": "1970-01-01", "email": "john@johndoe.com" }, "booked": true, "provider_scheduler_uuid": "8b21efa4-8535-11e4-a6cb-0800272e8da1", "start_date": "2014-12-16T15:09:34.197709", "end_date": "2014-12-16T16:09:34.197717" } ]</div>

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Endpoint</th>

<th width="5%">HTTP Method</th>

<th>Description</th>

<th width="10%">Scope</th>

</tr>

</thead>

<tbody>

<tr>

<td>/schedule/appointments/{pd_appointment_uuid}</td>

<td>GET</td>

<td>Query for an open appointment slot or a booked appointment given a specific {pd_appointment_uuid}, the (PokitDok unique appointment identifier).</td>

<td>user_schedule</td>

</tr>

</tbody>

</table>

curl example querying for a specific open slot or booked appointment:

<div hljs="" class="hljs" language="bash">curl -s -H "Authorization: Bearer $ACCESS_TOKEN" https://platform.pokitdok.com/api/v4/schedule/appointments/ef987691-0a19-447f-814d-f8f3abbf4859</div>

#### Response

<div hljs="" class="hljs">[ { "pd_appointment_uuid": "ef987691-0a19-447f-814d-f8f3abbf4859", "appointment_id": "UYQDUHSMIRCA", "appointment_type": "OV1", "patient": { "first_name": "John", "last_name": "Doe", "_uuid": "8b21f7b0-8535-11e4-a6cb-0800272e8da1", "phone": "800-555-1212", "member_id": "M000001", "birth_date": "1970-01-01", "email": "john@johndoe.com" }, "booked": true, "provider_scheduler_uuid": "8b21efa4-8535-11e4-a6cb-0800272e8da1", "start_date": "2014-12-16T15:09:34.197709", "end_date": "2014-12-16T16:09:34.197717" } ]</div>

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Endpoint</th>

<th width="5%">HTTP Method</th>

<th>Description</th>

<th width="10%">Scope</th>

</tr>

</thead>

<tbody>

<tr>

<td>/schedule/appointments/{pd_appointment_uuid}</td>

<td>PUT</td>

<td>Book appointment for an open slot. Post data contains patient attributes and description.</td>

<td>user_schedule</td>

</tr>

</tbody>

</table>

#### Request Body

<div hljs="" class="hljs">{ "patient": { "_uuid": "500ef469-2767-4901-b705-425e9b6f7f83", "email": "john@johndoe.com", "phone": "800-555-1212", "birth_date": "1970-01-01", "first_name": "John", "last_name": "Doe", "member_id": "M000001" }, "description": "Welcome to M0d3rN Healthcare" }</div>

curl example booking an appointment for an open slot:

<div hljs="" class="hljs" language="bash">curl -s -XPUT -H "Authorization: Bearer $ACCESS_TOKEN" -H "Content-Type: application/json" -d '{"patient": {"_uuid": "500ef469-2767-4901-b705-425e9b6f7f83", "email": "john@hondoe.com", "phone": "800-555-1212", "birth_date": "1970-01-01", "first_name": "John", "last_name": "Doe", "member_id": "M000001"}, "description": "Welcome to M0d3rN Healthcare"}' https://platform.pokitdok.com/api/v4/schedule/appointments/ef987691-0a19-447f-814d-f8f3abbf4859</div>

#### Response

<div hljs="" class="hljs">{ "appointment_id": "IIJBNRGBKMGC", "appointment_type": "OV1", "patient": { "first_name": "John", "last_name": "Doe", "_uuid": "500ef469-2767-4901-b705-425e9b6f7f83", "phone": "800-555-1212", "member_id": "M000001", "birth_date": "1970-01-01", "email": "john@hondoe.com" }, "booked": true, "description": "Welcome to M0d3rN Healthcare", "provider_scheduler_uuid": "e781c97c-8535-11e4-b0b3-0800272e8da1", "start_date": "2014-12-16T15:12:09.176207", "end_date": "2014-12-16T16:12:09.176215" }</div>

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Endpoint</th>

<th width="5%">HTTP Method</th>

<th>Description</th>

<th width="10%">Scope</th>

</tr>

</thead>

<tbody>

<tr>

<td>/schedule/appointments/{pd_appointment_uuid}</td>

<td>PUT</td>

<td>Update appointment description.</td>

<td>user_schedule</td>

</tr>

</tbody>

</table>

#### Request Body

<div hljs="" class="hljs">{ "description": "Welcome to M0d3rN Healthcare" }</div>

curl example updating an appointment description:

<div hljs="" class="hljs" language="bash">curl -s -XPUT -H "Authorization: Bearer $ACCESS_TOKEN" -H "Content-Type: application/json" -d '{"description": "Welcome to M0d3rN Healthcare"}' https://platform.pokitdok.com/api/v4/schedule/appointments/ef987691-0a19-447f-814d-f8f3abbf4859</div>

#### Response

<div hljs="" class="hljs">{ "appointment_id": "AAMWKTXGVGWB", "appointment_type": "OV1", "booked": false, "description": "Welcome to M0d3rN Healthcare", "provider_scheduler_uuid": "956a9e10-8536-11e4-ba5d-0800272e8da1", "start_date": "2014-12-16T15:17:00.948099", "end_date": "2014-12-16T16:17:00.948117" }</div>

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Field</th>

<th nowrap="" width="5%">Type</th>

<th>Description</th>

<th width="10%">Scope</th>

</tr>

</thead>

<tbody>

<tr>

<td>/schedule/appointments/{pd_appointment_uuid}</td>

<td>DELETE</td>

<td>Cancel appointment given its {pd_appointment_uuid}.</td>

<td>user_schedule</td>

</tr>

</tbody>

</table>

curl example of cancelling an appointment:

<div hljs="" class="hljs" language="bash">curl -i -XDELETE -H "Authorization: Bearer $ACCESS_TOKEN" https://platform.pokitdok.com/api/v4/schedule/appointments/ef987691-0a19-447f-814d-f8f3abbf4859</div>

### Trading Partners

_Available modes of operation: real-time_

The Trading Partners resource provides access to PokitDok's collection of Trading Partner information.

Available Trading Partner Endpoints:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Endpoint</th>

<th width="5%">HTTP Method</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>/tradingpartners/</td>

<td>GET</td>

<td>Get a list of trading partners</td>

</tr>

<tr>

<td>/tradingpartners/{id}</td>

<td>GET</td>

<td>Retreive the data for a specified trading partner; the ID is the PokitDok trading partner id</td>

</tr>

</tbody>

</table>

The /tradingpartners/ response contains the following fields:

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="45%">Field</th>

<th nowrap="" width="5%">Type</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>id</td>

<td nowrap="">{string}</td>

<td>The "trading_partner_id" used in requests/EDI files to identify this Trading Partner</td>

</tr>

<tr>

<td>is_enabled</td>

<td nowrap="">{boolean}</td>

<td>Indicates if the connection to this Trading Partner is enabled</td>

</tr>

<tr>

<td>name</td>

<td nowrap="">{string}</td>

<td>Full name for the Trading Partner</td>

</tr>

<tr>

<td>supported_transactions</td>

<td nowrap="">{array}</td>

<td>Identifies the x12 transaction sets (270, 276, 837, etc.) the Trading Partner supports. The list of supported transactions also indicates the API endpoints that are enabled for the trading partner. If the trading partner supports 270 transactions, you may use that trading_partner_id with the [eligibility](#eligibility) API. If 276 transactions are supported, you may use that trading_partner_id with the [claim status](#claimstatus) API. If 837 transactions are supported, you may use that trading_partner_id with the [claims](#claims) API.</td>

</tr>

<tr>

<td>enrollment_required</td>

<td nowrap="">{array}</td>

<td>Identifies the x12 transaction sets (270, 276, 837, etc.) that require additional enrollment steps before they may be used by your API application. Contact us if you'd like to use a transaction with a trading partner that requires enrollment prior to use.</td>

</tr>

<tr>

<td>metrics</td>

<td nowrap="">{object}</td>

<td>When a specific trading partner id is requested and metrics are available, they will be included in the response. Timings are in milliseconds unless otherwise specified.</td>

</tr>

<tr>

<td>metrics.real_time_response_average</td>

<td nowrap="">{float}</td>

<td>The average response time (milliseconds) for requests to this trading partner</td>

</tr>

<tr>

<td>metrics.real_time_response_percentiles</td>

<td nowrap="">{object}</td>

<td>Provides a percentile rank of the trading partner's response times, with the following groupings: 50%, 75%, and 95%.</td>

</tr>

<tr>

<td>monitoring</td>

<td nowrap="">{object}</td>

<td>When a specific trading partner id is requested and monitoring data is available, monitoring information will be included in the response. Each key in the monitoring section corresponds to the name of an API where connectivity is enabled (e.g. authorizations, claim_status, eligibility, etc). Each API name will be associated with a monitoring status value and a timestamp to indicate when the status was last updated.</td>

</tr>

<tr>

<td>monitoring.{api_name}.status</td>

<td nowrap="">{string}</td>

<td>The most recent status for the trading partner/API combination as returned by our monitoring system. Possible values include: available, unavailable, delayed, and unknown. A status of "available" indicates that the trading partner is operating normally based successful transactions executing within their average response time range. A status of "unavailable" indicates that the trading partner is unable to respond normally at that time. This may be due to scheduled or unplanned downtime. A status of "delayed" indicates that the trading partner is able to respond successfully to requests but that response times are higher than their average response time. A status of "unknown" will be returned for new trading partners that have just been added to the system and also for cases where the monitoring system encounters an exception and is unable to determine the current status.</td>

</tr>

<tr>

<td>monitoring.{api_name}.last_updated</td>

<td nowrap="">{string}</td>

<td>The date the monitoring status was last updated (ISO 8601 format)</td>

</tr>

</tbody>

</table>

curl example fetching trading partner information:

<div hljs="" class="hljs" language="bash">curl -i -H "Authorization: Bearer $ACCESS_TOKEN" https://platform.pokitdok.com/api/v4/tradingpartners/</div>

curl example fetching information for a specific trading partner:

<div hljs="" class="hljs" language="bash">curl -i -H "Authorization: Bearer $ACCESS_TOKEN" https://platform.pokitdok.com/api/v4/tradingpartners/aetna</div>

</div>

</div>

</div>

</div>

</div>

</div>

<div id="footer" class="footer container-fluid"><a class="link-jump go-up" scroll-to="body"><span class="arrow"></span></a>

<div class="col-xs-4 footer-nav">

<div class="footer-links">

*   [Blog](https://pokitdok.com/blog)
*   [About Us](https://pokitdok.com/about)
*   [News Room](https://pokitdok.com/press)
*   [Work With Us](https://pokitdok.com/career)
*   [Contact Us](https://pokitdok.com/contact?context=Seamless Insurance Connectivity)
*   [FAQ](https://pokitdok.com/faq#developers)
*   [Directory](https://pokitdok.com/directory)

*   [Consumers](https://pokitdok.com/marketplace)
*   [Providers](https://pokitdok.com/providers)
*   [Business Solutions](https://pokitdok.com/)
*   [Developers](/)
*   [Android](https://play.google.com/store/apps/details?id=com.pokitdok.android)
*   [iPhone](https://itunes.apple.com/us/app/pokitdok/id583529861?ls=1&mt=8)

</div>

</div>

<div class="col-xs-4 text-center">

*   [](https://www.facebook.com/pokitdok)
*   [](https://twitter.com/pokitdok)
*   [](https://plus.google.com/116994365842912222314/posts)
*   [](https://www.linkedin.com/company/pokitdok)

</div>

<div class="col-xs-4 text-right footer-copy">

<div class="footer-links">[Privacy policy](https://pokitdok.com/privacy) | [Terms of service](/terms)

©2015 PokitDok, Inc. All rights reserved.

</div>

</div>

</div>

</div>

<script>angular.module('app').run(function (CFG) { CFG.creditTiersDefault = JSON.parse('{"x12_api": {"ppc": "0.04", "prepay": false, "monthlyCost": 0, "description": "Including authorizations, claims, claims status, eligibility, enrollment, and referrals.", "title": "X12 APIs", "active": true, "includedCredits": 0, "notes": "1 Request = 1 Membership Contract (Subscriber and Dependent Records) queried or submitted", "api_list": ["authorizations", "claims", "claim_status", "eligibility", "files", "referrals"]}, "data_api": {"ppc": "0.25", "prepay": false, "monthlyCost": 0, "description": "", "title": "Data APIs", "active": true, "includedCredits": 0, "notes": "1 Request = any request for health data including a provider search, insurance prices or plans.", "api_list": ["insurance_prices", "plans", "providers"]}}'); CFG.notifications = '[]'; });</script> <script>(function (i, s, o, g, r, a, m) { i['GoogleAnalyticsObject'] = r; i[r] = i[r] || function () { (i[r].q = i[r].q || []).push(arguments) }, i[r].l = 1 * new Date(); a = s.createElement(o), m = s.getElementsByTagName(o)[0]; a.async = 1; a.src = g; m.parentNode.insertBefore(a, m); })(window, document, 'script', '//www.google-analytics.com/analytics.js', 'ga'); ga('create', 'UA-26980236-7', {'cookieDomain': 'platform.pokitdok.com'}); ga('send', 'pageview');</script> <script type="text/javascript">(function (d, s, i, r) { if (d.getElementById(i)) { return; } var n = d.createElement(s), e = d.getElementsByTagName(s)[0]; n.id = i; n.src = '//js.hs-analytics.net/analytics/' + (Math.ceil(new Date() / r) * r) + '/457675.js'; e.parentNode.insertBefore(n, e); })(document, "script", "hs-analytics", 300000);</script>

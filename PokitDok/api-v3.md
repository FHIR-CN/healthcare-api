
### Overview

The PokitDok API allows you to perform X12 transactions, find healthcare providers, and get information on health care procedure pricing. The API uses JSON for requests and responses and also allows batch processing of ASC X12 5010 compatible files. All API traffic is encrypted over HTTPS and authentication is handled with OAuth2. The API is designed according to REST (Representational State Transfer) principles. The available API calls are organized into Resources. These resources are represented by endpoints that clients can invoke to describe and modify their state. All resources have the same available top level parameters available for finer grained access (described below). Here's an example of accessing a resource directly, using the "cURL" utility (info about cURL can be found [here](http://en.wikipedia.org/wiki/CURL)):

<div hljs="" class="hljs">$curl -i https://platform.pokitdok.com/api/v3/providers/?limit=20 HTTP/1.1 200 OK Content-Type: application/json; charset=utf-8 { "meta": { "rate_limit_cap": 1000, "rate_limit_reset": 1372700873, "rate_limit_amount": 122, "credits_billed": 1, "credits_remaining": 10000, "processing_time": 80, "next": "https://platform.pokitdok.com/api/v3/providers/?offset=20", "previous": null }, "data": [{ ... }] }</div>

The above is a truncated example that gets a list of providers from the Providers resource, and uses some common keyword arguments in the process. Notice that the response payload consists of a meta key and a data key. The meta key contains information regarding rate limiting, linked urls for convenient paging of lists, record counts, response time and API billing information. The data key contains an object, or list of objects returned from the resource. The following is a list of values that are contained in the meta block, along with their types. Credit and rate limiting information is only included in the meta information for APIs that are billable and/or rate limited. Processing time is always included in the meta information.

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

<td>rate_limit_cap</td>

<td nowrap="">{int}</td>

<td>The amount of requests available per hour</td>

</tr>

<tr>

<td>rate_limit_reset</td>

<td nowrap="">{int}</td>

<td>The time (Unix Timestamp) when the rate limit amount resets</td>

</tr>

<tr>

<td>rate_limit_amount</td>

<td nowrap="">{int}</td>

<td>The amount of requests made during the current rate limit period</td>

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

<td>processing_time</td>

<td nowrap="">{int}</td>

<td>The time to process the request in milliseconds</td>

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

</tbody>

</table>

Below are the available keyword arguments that resources can use. All of these keywords can be added to collection requests from Resources - singular Resource (/resource/{id}) requests do not accept parameters intended to limit a list of responses.

<table class="table table-striped table-bordered">

<thead>

<tr>

<th width="30%">Argument</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>fields</td>

<td>The field returned with a response from a resource All Resources have default fields that are always returned (defined in the Resources section). The fields keyword also allows for getting related resources and keyword chaining on those resources (example below).</td>

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

<tr>

<td>dir</td>

<td>The direction that a list is sorted, ascending or descending (only for collection requests)</td>

</tr>

<tr>

<td>async</td>

<td>Whether the API call is asynchronous For Resources that offer both synchronous and asynchronous operation, a boolean can be used for this parameter to specify which mode of operation you desire; if the async parameter is omitted, the synchronous mode will be used. For POST requests, the async parameter should be included along with other JSON data being POSTed. When async is true, the API client has the option of including a callback URL so that it can be notified when the asynchronous processing is complete.</td>

</tr>

</tbody>

</table>

### Client Libraries To simplify authorization and access to the PokitDok API, we provide a client libraries for a number of common languages. These libraries offer full access to the PokitDok API with a minimum of code, usually just a few lines. These libraries are hosted on [PokitDok's github.](https://github.com/pokitdok) * Python - [pokitdok-python](https://github.com/pokitdok/pokitdok-python) * Ruby - [pokitdok-ruby](https://github.com/pokitdok/pokitdok-ruby) * PHP - [pokitdok-php](https://github.com/pokitdok/pokitdok-php) * C# - [pokitdok-csharp](https://github.com/pokitdok/pokitdok-csharp) The following sections go into further detail on the underlying details of connecting directly to the PokitDok API. This is of use if you're implementing the APIs in a language for which we currently don't have a client library. ### JSON and EDI Support All resource endpoints that result in the transmission of EDI transaction sets to our trading partners accept parameters via a JSON representation of an EDI transaction set. However, EDI files can also be sent directly to the /files endpoint. The content-type header of a POST request informs the endpoint what is being sent. Acceptable content-type values are "application/json" for JSON requests and "application/edi-x12", "text/plain", and "application/octet-stream" for EDI files. ### Security and Authorization All calls to the PokitDok API are encrypted over HTTPS. Access to the API is controlled via OAuth2 (for more information, reference "OAuth 2.0 Authorization Framework" at http://tools.ietf.org/html/rfc6749). If you've already signed up and received your client id and client secret, you’ll be able to start calling APIs. Here's a quick example using Python and the requests library to demonstrate how to authenticate as your application and access some information about an ongoing activity: ### Python Example

<div hljs="" class="hljs" language="python">import requests from base64 import urlsafe_b64encode client_id = YOUR_APP_CLIENT_ID client_secret = YOUR_APP_CLIENT_SECRET access_token = requests.post('https://platform.pokitdok.com/oauth2/token', headers={'Authorization': 'Basic ' + urlsafe_b64encode(client_id + ':' + client_secret)}, data={'grant_type':'client_credentials'}).json()['access_token'] activity = requests.get('https://platform.pokitdok.com/api/v3/activities/53187d2027a27620f2ec7537', headers={'Authorization': 'Bearer ' + access_token}).json()</div>

If you prefer to start experimenting with the APIs from the command line, here’s an example using curl: ### curl Example

<div hljs="" class="hljs">CLIENT_ID=YOUR_APP_CLIENT_ID CLIENT_SECRET=YOUR_APP_CLIENT_SECRET BASIC_HEADER=`echo "$CLIENT_ID:$CLIENT_SECRET" | base64` # On some operating systems, base64 may include extra # whitespace that needs to be removed BASIC_HEADER=$(echo $BASIC_HEADER | sed 's/\r//g') BASIC_HEADER=$(echo $BASIC_HEADER | sed 's/ *//g') curl -i -XPOST -H "Authorization: Basic $BASIC_HEADER" -d "grant_type=client_credentials" https://platform.pokitdok.com/oauth2/token HTTP/1.1 200 OK Server: nginx Date: Thu, 06 Mar 2014 04:20:43 GMT Content-Type: application/json;charset=UTF-8 Content-Length: 127 Connection: keep-alive Pragma: no-cache Cache-Control: no-store { "access_token": "s8KYRJGTO0rWMy0zz1CCSCwsSesDyDlbNdZoRqVR", "token_type": "bearer", "expires": 1393350569, "expires_in": 3600 } ACCESS_TOKEN='s8KYRJGTO0rWMy0zz1CCSCwsSesDyDlbNdZoRqVR' curl -i -H "Authorization: Bearer $ACCESS_TOKEN" https://platform.pokitdok.com/api/v3/activities/5317f51527a27620f2ec7533</div>

### Errors Error information may be returned to an API client. Common error scenarios include: * the data that was provided is invalid * required information is missing * rate limits have been exceeded * an API access token is not valid or no longer valid * insufficient credits are available for billable resources Some examples of these are included below. Unauthorized access This may be encountered when an invalid or no longer valid access token is supplied. Access tokens expire one hour after being acquired. Applications should handle 401s properly and request a new access token when this is encountered.

<div hljs="" class="hljs">curl -i -H "Authorization: Bearer $ACCESS_TOKEN" https://platform.pokitdok.com/api/v3/activities/5317f51527a27620f2ec7533 HTTP/1.1 401 UNAUTHORIZED Server: nginx Date: Thu, 06 Mar 2014 16:44:07 GMT Content-Type: application/json Content-Length: 31 { "message": "Unauthorized" }</div>

Insufficient credits This may be encountered when credits are not available to an application for a billable resource requested in the API call. If this is encountered, the application owner will need to load more credits on their application or change their billing tier.

<div hljs="" class="hljs">curl -i -H "Authorization: Bearer $ACCESS_TOKEN" https://platform.pokitdok.com/api/v3/providers/?limit=20 HTTP/1.0 402 PAYMENT REQUIRED Server: nginx Date: Tue, 25 Feb 2014 16:15:31 GMT Content-Type: application/json Content-Length: 35 { "message": "Payment Required" }</div>

Rate limit exceeded This may be encountered when too many API calls are made within a period of time for a rate limited resource. Rate limits are currently enforced on an hourly basis. If your application receives a 403, you can wait for the rate limit period to renew and then make the API call again.

<div hljs="" class="hljs">curl -i -H "Authorization: Bearer $ACCESS_TOKEN" https://platform.pokitdok.com/api/v3/activities/5317f51527a27620f2ec7533 HTTP/1.0 403 FORBIDDEN Server: nginx Date: Tue, 25 Feb 2014 16:04:50 GMT Content-Type: application/json Content-Length: 28 { "message": "Forbidden" }</div>

Required information missing or invalid You may encounter errors like this when required information is omitted from an API call. Simply supply the appropriate information on the next API call to resolve.

<div hljs="" class="hljs">curl -i -H "Content-Type: application/json" -H "Authorization: Bearer $ACCESS_TOKEN" -d "{}" https://platform.pokitdok.com/api/v3/eligibility/ HTTP/1.1 200 OK Server: nginx Date: Thu, 06 Mar 2014 19:50:17 GMT Content-Type: text/html; charset=utf-8 Content-Length: 510 Connection: keep-alive mimetype: application/json charset: utf-8 { "meta": { "credits_billed": 1, "credits_remaining": 995, "processing_time": 3, "rate_limit_amount": 2, "rate_limit_cap": 1000, "rate_limit_reset": 1394138991 }, "data": { "errors": { "member_birth_date": "member birth date is required.", "member_first_name": "member first name is required.", "member_id": "member id is required.", "member_last_name": "member last name is required.", "payer_id": "payer id is required.", "provider_id": "provider id is required.", "service_types": "service types is required." } } }</div>

### API Dashboard To manage and configure your usage of the PokitDok API, we provide an API Dashboard. The Dashboard provides the following functionality: * Tracking of long-running asynchronous activity status * Monitoring of system health, connectivity, and uptime * Account usage and billing information * Management of API keys and authentication details for access * Documentation, examples, and API references * System messages and notifications, such as for scheduled maintenance downtime ### Available Resources As stated above, the API exposes resources for use in applications. The overview covered working with resources in general and the available parameters. Each resource section here will cover working with specific resources, what fields are available and what actions are available for a particular resource. ### Providers _Available modes of operation: real-time only_ The Providers resource provides access to PokitDok’s provider directory. The Providers endpoints can be used to search for Providers, view biographical, education and credential information, and view specialty taxonomies. Any of the above listed keywords can be used to show additional fields, perform searches, and page through results. Available Provider Endpoints:

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

<td nowrap="">GET</td>

<td>Get a list of providers meeting certain search criteria</td>

</tr>

<tr>

<td>/providers/{id}</td>

<td nowrap="">GET</td>

<td>Retrieve the data for a specified provider; the ID is the PokitDok generated UUID</td>

</tr>

</tbody>

</table>

The /providers/ endpoint accepts the following additional parameters:

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

<td>q</td>

<td nowrap="">{string}</td>

<td>Query parameter, could be a name, specialty, (...)</td>

</tr>

<tr>

<td>zipcode</td>

<td nowrap="">{string}</td>

<td>Geographic center point in which to search for providers</td>

</tr>

<tr>

<td>radius</td>

<td nowrap="">{string}</td>

<td>Search distance from geographic centerpoint, with unit (e.g. "1mi")</td>

</tr>

</tbody>

</table>

Available Provider Fields:

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

<td>first_name</td>

<td nowrap="">{string}</td>

<td>A provider’s first name</td>

</tr>

<tr>

<td>middle_name</td>

<td nowrap="">{string}</td>

<td>A provider’s middle name</td>

</tr>

<tr>

<td>last_name</td>

<td nowrap="">{string}</td>

<td>A provider’s last name</td>

</tr>

<tr>

<td>suffix</td>

<td nowrap="">{string}</td>

<td>A provider’s suffix (MD, Jr., etc)</td>

</tr>

<tr>

<td>prefix</td>

<td nowrap="">{string}</td>

<td>A provider’s prefix (Mr., Mrs., Dr., etc)</td>

</tr>

<tr>

<td>gender</td>

<td nowrap="">{string}</td>

<td>A provider’s gender</td>

</tr>

<tr>

<td>name</td>

<td nowrap="">{string}</td>

<td>The provider name</td>

</tr>

<tr>

<td>specialties</td>

<td nowrap="">{string}</td>

<td>A list of specialties from the specialty taxonomy associated with the provider</td>

</tr>

<tr>

<td>locations</td>

<td nowrap="">{array}</td>

<td>A list of locations (includes addresses with geocode data, phone/fax numbers, and facility names) associated with the provider</td>

</tr>

<tr>

<td>affiliations</td>

<td nowrap="">{array}</td>

<td>A list of affiliations (hospitals and private practices that have a business relationship with the provider) associated with the provider</td>

</tr>

<tr>

<td>bio</td>

<td nowrap="">{string}</td>

<td>A short description of the provider</td>

</tr>

<tr>

<td>npi_id</td>

<td nowrap="">{int}</td>

<td>A provider’s NPI id</td>

</tr>

<tr>

<td>licenses</td>

<td nowrap="">{array}</td>

<td>A list of a provider’s state licensing information</td>

</tr>

<tr>

<td>education</td>

<td nowrap="">{array}</td>

<td>A list of a provider’s education and residency history</td>

</tr>

<tr>

<td>years_in_practice</td>

<td nowrap="">{int}</td>

<td>A count of the years the provider has been practicing</td>

</tr>

<tr>

<td>date_of_birth</td>

<td nowrap="">{string}</td>

<td>A provider’s birthday</td>

</tr>

</tbody>

</table>

### Eligibility _Available modes of operation: batch/async or real-time_ The Eligibility resource allows real-time eligibility determination to be performed. Asynchronous eligibility requests can also be made. When asynchronous eligibility requests are made, a callback_url may be supplied to be notified when the asynchronous processing is complete. The eligibility request activity may also be tracked by the /activities/ API. Use the /payers/ API to determine available payer_id values for use with the /eligibility/ API. Available Eligibility Endpoints:

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

<td nowrap="">POST</td>

<td>Determine eligibility via an EDI 270 Request For Eligibility</td>

</tr>

</tbody>

</table>

Available arguments to this endpoint:

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

<td>Unique id for the intended trading partner, as specified by the Payers resource</td>

</tr>

<tr>

<td>provider_id</td>

<td>Unique identifier for the provider, such as NPI, a PokitDok provider id, etc.</td>

</tr>

<tr>

<td>member_id</td>

<td>The named insured’s member identifier</td>

</tr>

<tr>

<td>member_name</td>

<td>The named insured’s name as specified on their policy</td>

</tr>

<tr>

<td>member_birth_date</td>

<td>The named insured’s birth date as specified on their policy</td>

</tr>

<tr>

<td>service_types</td>

<td>The line of coverage in the insurance policy this eligibility request is being made against</td>

</tr>

</tbody>

</table>

<div hljs="" class="hljs">{ "trading_partner_id": "MOCKPAYER", "member_id": "W34237875729", "provider_id": "1467560003", "provider_name": "AYA-AY", "provider_first_name": "JEROME", "provider_type": "1", "member_name": "JOHN DOE", "member_birth_date": "05/21/1975", "service_types": ["Health Benefit Plan Coverage"] }</div>

### Payers _Available modes of operation: real-time_ The Payers resource provides access to PokitDok’s collection of payer information. Available Payer Endpoints:

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

<td nowrap="">GET</td>

<td>Get a list of payers</td>

</tr>

</tbody>

</table>

Available Payer Fields:

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

<td>payer_id</td>

<td nowrap="">{string}</td>

<td>An id to be used in requests/EDI files to identify this payer</td>

</tr>

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

<td nowrap="">{list}</td>

<td>A list of X12 Transaction set codes, 270, 276, etc, the payer supports.</td>

</tr>

</tbody>

</table>

### Cash Prices _Available modes of operation: real-time_ The Cash Prices resource allows access to our curated collection of procedures and pricing data. The data comes from actual providers selling actual services. Available Procedure Endpoints:

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

<td>/price/cash</td>

<td>GET</td>

<td>Return a list of prices for a given procedure (by CPT Code) in a given region (by ZIP Code)</td>

</tr>

</tbody>

</table>

The /prices/cash/ endpoint accepts the following additional parameters:

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

<td>Geographic center point in which to search for procedures</td>

</tr>

</tbody>

</table>

### Activities _Available modes of operation: real-time_ Long-running operations are performed asynchronously. Upon initiating those operations through via an API endpoint, activity tracking information is returned to the caller, which can be used to query the status of the activity later on. Throughout processing, activities may transition through the following states: * init - The activity is initializing * queued - The activity is in queue waiting to start/resume * generating - The activity is generating X12 transactions * processing - The activity is processing X12 transactions that have been received * transmitting - The activity is transmitting X12 transactions to trading partner * waiting - The activity is waiting on a trading partner response * receiving - The activity is receiving X12 transactions from a trading partner * paused - The activity is paused * notifying - The activity is notifying the client application about activity results * stored - The activity has stored uploaded batch transactions for later processing * canceled - The activity was canceled by the client application * failed - The activity was unable to process successfully Information concerning the activity’s progression through the system is available via the API Dashboard, as well as the endpoints listed below. Available Activity Endpoints:

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

<td>/activities/</td>

<td>GET</td>

<td>List current activities A query string parameter ‘parent_id’ may also be used with this API to get information about sub-activities that were initiated from a batch file upload.</td>

</tr>

<tr>

<td>/activities/{id}</td>

<td>GET</td>

<td>Return detailed information about the specified activity API applications will receive an activity ID in the API response for all operations that are asynchronous.</td>

</tr>

<tr>

<td>/activities/{id}</td>

<td>PUT</td>

<td>Updates an existing activity -- useful for canceling pending activities that a client application no longer wishes to execute</td>

</tr>

</tbody>

</table>

Available Activity Fields:

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

<td>_id</td>

<td nowrap="">{string}</td>

<td>ID of this Activity</td>

</tr>

<tr>

<td>name</td>

<td nowrap="">{string}</td>

<td>Activity name</td>

</tr>

<tr>

<td>callback_url</td>

<td nowrap="">{string}</td>

<td>URL that will be invoked to notify the client application that this Activity has completed We recommend that you always use https for callback URLs used by your application.</td>

</tr>

<tr>

<td>file_url</td>

<td nowrap="">{string}</td>

<td>URL where batch transactions that were uploaded to be processed within this activity are stored X12 files uploaded via the /files endpoint are stored here.</td>

</tr>

<tr>

<td>history</td>

<td nowrap="">{array}</td>

<td>Historical status of the progress of this Activity</td>

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

<td>remaining_transitions</td>

<td nowrap="">{array}</td>

<td>The list of remaining state transitions that the activity has yet to go through</td>

</tr>

<tr>

<td>callback_url</td>

<td nowrap="">{string}</td>

<td>URL that will be invoked to notify the client application that this Activity has completed We recommend that you always use https for callback URLs used by your application.</td>

</tr>

<tr>

<td>parameters</td>

<td nowrap="">{dict}</td>

<td>The parameters that were originally supplied to the activity</td>

</tr>

<tr>

<td>units_of_work</td>

<td nowrap="">{int}</td>

<td>The number of 'units of work' that the activity is operating on This will typically be 1 for real-time requests like /eligibility. When uploading batch X12 files via the /files endpoint, this will be the number of ‘transactions’ within that file. For example, if a client application POSTs a file of 20 eligibility requests to the /files API, the units_of_work value for that activity will be 20 after the X12 file has been analyzed. If an activity does show a value greater than 1 for units_of_work, the client application can fetch detailed information about each one of the activities processing those units of work by using the /activities/?parent_id= <activity_id>API</activity_id></td>

</tr>

</tbody>

</table>

Example using curl to cancel an existing activity

<div hljs="" class="hljs" code="bash">curl -i -H "Content-Type: application/json" -H "Authorization: Bearer $ACCESS_TOKEN" -d '{"transition": "cancel"}' https://platform.pokitdok.com/api/v3/activities/5317f51527a27620f2ec7533</div>

</div>

</div>

</div>

</div>

</div>

</div>

<div id="footer" class="footer container-fluid"><a class="link-jump go-up" scroll-to="body"><span class="arrow"></span></a>

<div class="col-xs-4 footer-nav">

<div class="footer-links">* [Blog](https://pokitdok.com/blog) * [About Us](https://pokitdok.com/about) * [News Room](https://pokitdok.com/press) * [Work With Us](https://pokitdok.com/career) * [Contact Us](https://pokitdok.com/contact?context=Seamless Insurance Connectivity) * [FAQ](https://pokitdok.com/faq#developers) * [Directory](https://pokitdok.com/directory) * [Consumers](https://pokitdok.com/marketplace) * [Providers](https://pokitdok.com/providers) * [Business Solutions](https://pokitdok.com/) * [Developers](/) * [Android](https://play.google.com/store/apps/details?id=com.pokitdok.android) * [iPhone](https://itunes.apple.com/us/app/pokitdok/id583529861?ls=1&mt=8)</div>

</div>

<div class="col-xs-4 text-center">* [](https://www.facebook.com/pokitdok) * [](https://twitter.com/pokitdok) * [](https://plus.google.com/116994365842912222314/posts) * [](https://www.linkedin.com/company/pokitdok)</div>

<div class="col-xs-4 text-right footer-copy">

<div class="footer-links">[Privacy policy](https://pokitdok.com/privacy) | [Terms of service](/terms) ©2015 PokitDok, Inc. All rights reserved.</div>

</div>

</div>

</div>

<script>angular.module('app').run(function (CFG) { CFG.creditTiersDefault = JSON.parse('{"x12_api": {"ppc": "0.04", "prepay": false, "monthlyCost": 0, "description": "Including authorizations, claims, claims status, eligibility, enrollment, and referrals.", "title": "X12 APIs", "active": true, "includedCredits": 0, "notes": "1 Request = 1 Membership Contract (Subscriber and Dependent Records) queried or submitted", "api_list": ["authorizations", "claims", "claim_status", "eligibility", "files", "referrals"]}, "data_api": {"ppc": "0.25", "prepay": false, "monthlyCost": 0, "description": "", "title": "Data APIs", "active": true, "includedCredits": 0, "notes": "1 Request = any request for health data including a provider search, insurance prices or plans.", "api_list": ["insurance_prices", "plans", "providers"]}}'); CFG.notifications = '[]'; });</script> <script>(function (i, s, o, g, r, a, m) { i['GoogleAnalyticsObject'] = r; i[r] = i[r] || function () { (i[r].q = i[r].q || []).push(arguments) }, i[r].l = 1 * new Date(); a = s.createElement(o), m = s.getElementsByTagName(o)[0]; a.async = 1; a.src = g; m.parentNode.insertBefore(a, m); })(window, document, 'script', '//www.google-analytics.com/analytics.js', 'ga'); ga('create', 'UA-26980236-7', {'cookieDomain': 'platform.pokitdok.com'}); ga('send', 'pageview');</script> <script type="text/javascript">(function (d, s, i, r) { if (d.getElementById(i)) { return; } var n = d.createElement(s), e = d.getElementsByTagName(s)[0]; n.id = i; n.src = '//js.hs-analytics.net/analytics/' + (Math.ceil(new Date() / r) * r) + '/457675.js'; e.parentNode.insertBefore(n, e); })(document, "script", "hs-analytics", 300000);</script>

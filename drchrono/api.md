
## Tutorial

This series of examples shows how to do many common activities with the drchrono API.

### Create your application

After you've been set up for API access by drchrono support, you should see a new menu item under the "Account" menu labeled "API". This page is for creating, editing, and deleting your API applications.

To create your application, click the "New Application" button. Fill in the name of your application and add one or more Redirect URIs. Click the "Save Changes" button on the bottom.

> A Redirect URI is a web location starting with `http://` or `https://` that the user will be redirected to during the log-in flow.

For the purpose of the tutorial, `CLIENT_ID`, `CLIENT_SECRET`, and `REDIRECT_URI` represent what the appropriate values for your application.

### Client setup

We reccomend using an OAuth 2.0 library. [This page](http://oauth.net/2/) lists clients for various popular programming languages. To configure your library, use the following values:

*   Client ID: `CLIENT_ID`
*   Client Secret: `CLIENT_SECRET`
*   Redirect URI: `REDIRECT_URI`
*   Authorize URL: `https://drchrono.com/o/authorize/`
*   Access Token URL: `https://drchrono.com/o/token/`
*   Base URL: `https://drchrono.com/api/`

However, OAuth 2.0 isn't terribly complicated, and this tutorial only assumes a general-purpose HTTP client library.

The drchrono API is versioned. When you create an application, it will default to using the latest version. This can be modified at any time, and can be set per-request with the `X-DRC-API-Version` header.

### OAuth 2.0 authorization

If you're not familiar with OAuth 2.0, you should read through [our introduction](/api-docs/v2014_08/oauth/).

### Getting the patient list

The [`/api/patients` endpoint](#apipatients), like all list endpoints, is paginated. You'll need to iterate over the results to get them all.

    import requests

    headers = {
        'Authorization': 'Bearer ACCESS_TOKEN',
    }

    domain = 'https://drchrono.com'

    patients = []
    patients_url = '/api/patients'
    while patients_url:
        data = requests.get(domain + patients_url, headers=headers).json()
        patients.extend(data['results'])
        patients_url = data['next'] # A JSON null on the last page

If you have a patient's name or email address, you can pass the `?search=` parameter (possibly multiple times) to narrow down the patients returned. You can also query by the `gender` and `date_of_birth` fields.

### Creating resources

Resources are created by sending a `POST` to the relevant top-level resouce. For example, to create an appointment, the appointment's information is sent to `/api/appointments` like this:

    import requests

    headers = {
        'Authorization': 'Bearer ACCESS_TOKEN',
    }
    data = {
        'doctor': '/api/doctors/1234',
        'duration: 30, # in minutes
        'office': '/api/offices/3456',
        'patient': '/api/patients/5678',
        'scheduled_time: '2014-08-01T14:30:00',
    }
    url = 'https://drchrono.com/api/appointments'

    r = requests.post(url, data=data, headers=headers)

    assert r.status_code == 201 # HTTP 201 CREATED

### Uploading documents

Uploading a document into a patient's chart is straightforward:

    import datetime, json, requests

    headers = {
        'Authorization': 'Bearer ACCESS_TOKEN',
    }
    data = {
        'doctor': '/api/doctors/1234',
        'patient': '/api/patients/5678',
        'description': 'Short document description here',
        'date': '2014-02-24',
        'metatags': json.dumps(['tag1', 'tag2']),
    }
    with open('/path/to/your.pdf', 'rb') as f:
        files = {'document': f}
        requests.post(
            'https://drchrono.com/api/documents',
            data=data, files=files, headers=headers,
        )

### References to resources

There are two ways resources are referred to within the drchrono API:

*   Responses always return an absolute path to a resource, like `/api/doctor/12345`

*   Query parameters always take the ID directly, like `/api/appointments?doctor=12345`

*   Requests in POST bodies should be given as paths, as demonstrated in the example above.

## Reference

### Applications

Applications bundle together OAuth 2.0 required information with more user-friendly data, like a name and logo, as well as a default API version.

> Applications always start in "Test Mode", which means that only you (or other accounts in your practice group) will be able to authorize the app. When you're ready for wider deployment, [contact support](mailto:support@drchrono.com) to set your application live.

### Resources

*   [`/api/appointments`](#apiappointments)
*   [`/api/appointment_profiles`](#apiappointment_profiles)
*   [`/api/appointment_templates`](#apiappointment_templates)
*   [`/api/doctors`](#apidoctors)
*   [`/api/documents`](#apidocuments)
*   [`/api/offices`](#apioffices)
*   [`/api/patients`](#apipatients)
*   [`/api/reminder_profiles`](#apireminder_profiles)
*   [`/api/users`](#apiusers)
*   [`/api/transactions`](#apitransactions)

Performing a `GET` request on a resource will return a paginated list in the following format:

    {
      "count": integer,                     // total items available
      "next": URL or null,                  // URL of the next page
      "previous": URL or null,              // URL of the previous page
      "results": array of result objects    // This is resource specific, see below
    }

There are a few non-standard JSON types in this documentation. All are encoded as strings.

<table>

<thead>

<tr>

<th>Type</th>

<th>Example</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>URL</td>

<td>`"/api/patients/1234"`</td>

<td>A domain relative URI encoded as a string</td>

</tr>

<tr>

<td>date</td>

<td>`"2014-02-24"`</td>

<td>An ISO 8601 encoded date</td>

</tr>

<tr>

<td>date range</td>

<td>`"2014-02-24/2014-02-28"`</td>

<td>A start and end date seperated by a slash</td>

</tr>

<tr>

<td>file</td>

<td>`"https://drchrono.com/site_media/uploaded_media/path/to/file.ext"`</td>

<td>When uploading, this is provided as a standard `multipart/form-data` file</td>

</tr>

<tr>

<td>time</td>

<td>`"12:34:56"`</td>

<td>An ISO 8601 encoded time</td>

</tr>

<tr>

<td>timestamp</td>

<td>`"2014-02-24T15:32:19"`</td>

<td>An ISO 8601 encoded timestamp</td>

</tr>

</tbody>

</table>

#### `/api/appointments`

##### Result object format

<table>

<thead>

<tr>

<th>Key</th>

<th>Type</th>

<th>Notes</th>

</tr>

</thead>

<tbody>

<tr>

<td>`color`</td>

<td>optional string</td>

<td>Any valid CSS color (like `#ABCDEF` or `rgb(12, 34, 56)`)</td>

</tr>

<tr>

<td>`doctor`</td>

<td>URL</td>

</tr>

<tr>

<td>`duration`</td>

<td>integer</td>

<td>Length of the appointment in minutes. Optional if `profile` is provided</td>

</tr>

<tr>

<td>`exam_room`</td>

<td>integer</td>

<td>Index of the exam room that this appointment occurs in. See [/api/offices](#apioffices)</td>

</tr>

<tr>

<td>`id`</td>

<td>string</td>

<td>Unique identifier. Usually numeric, but not always.</td>

</tr>

<tr>

<td>`office`</td>

<td>URL</td>

</tr>

<tr>

<td>`patient`</td>

<td>URL or `null`</td>

<td>Link to this appointment's patient. Breaks have a null patient field</td>

</tr>

<tr>

<td>`profile`</td>

<td>optional URL</td>

<td>The profile sets default values on creation</td>

</tr>

<tr>

<td>`reason`</td>

<td>optional string</td>

<td>default is `""`</td>

</tr>

<tr>

<td>`reminders`</td>

<td>optional array of objects</td>

<td>See below</td>

</tr>

<tr>

<td>`scheduled_time`</td>

<td>timestamp</td>

<td>The starting time of the appointment</td>

</tr>

<tr>

<td>`status`</td>

<td>optional string</td>

<td>One of `""`, `"Arrived"`, `"Cancelled"`, `"Complete"`, `"Confirmed"`, `"In Session"`, `"No Show"`, `"Not Confirmed"`, or `"Rescheduled"`</td>

</tr>

<tr>

<td>`template`</td>

<td>optional URL</td>

<td>Override values for the appointment. Higher priority than `profile`.</td>

</tr>

<tr>

<td>`blood_pressure`</td>

<td>string</td>

</tr>

<tr>

<td>`icd9_codes`</td>

<td>string</td>

</tr>

<tr>

<td>`notes`</td>

<td>string</td>

</tr>

</tbody>

</table>

When creating an appointment, if a valid `profile` is provided, then `color`, `reason`, and `duration` will use it's values as their defaults. Providing those fields will still override the profile.

When creating an appointment, if a valid `template` is provided, then its values will be used _instead_ of what's provided explicitly. Valid means that the office and the day of the week matches the `office` and `scheduled_time` fields.

##### `reminders` object format

<table>

<thead>

<tr>

<th>Key</th>

<th>Type</th>

<th>Notes</th>

</tr>

</thead>

<tbody>

<tr>

<td>`id`</td>

<td>integer</td>

</tr>

<tr>

<td>`scheduled_time`</td>

<td>timestamp</td>

</tr>

<tr>

<td>`type`</td>

<td>string</td>

<td>One of `"email"`, `"sms"`, or `"auto_call"`</td>

</tr>

</tbody>

</table>

##### Optional `GET` parameters for filtering:

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

<td>`date`</td>

<td>date</td>

<td>Only retrieve appointments that occur on a date</td>

</tr>

<tr>

<td>`date_range`</td>

<td>date range</td>

<td>Retrieve appointments in a time period (inclusive)</td>

</tr>

<tr>

<td>`doctor`</td>

<td>integer</td>

<td>List appointments for a doctor</td>

</tr>

<tr>

<td>`office`</td>

<td>integer</td>

<td>List appointemnts for an office</td>

</tr>

<tr>

<td>`patient`</td>

<td>integer</td>

<td>List appointments for a patient</td>

</tr>

<tr>

<td>`since`</td>

<td>timestamp</td>

<td>Only retrieve appointments that have been updated since a given date</td>

</tr>

</tbody>

</table>

#### `/api/appointment_profiles`

##### Result object format

<table>

<thead>

<tr>

<th>Key</th>

<th>Type</th>

<th>Notes</th>

</tr>

</thead>

<tbody>

<tr>

<td>`color`</td>

<td>string</td>

<td>Any valid CSS color (like `#ABCDEF` or `rgb(12, 34, 56)`)</td>

</tr>

<tr>

<td>`duration`</td>

<td>integer</td>

<td>Length of an appointment in minutes</td>

</tr>

<tr>

<td>`id`</td>

<td>integer</td>

</tr>

<tr>

<td>`name`</td>

<td>string</td>

</tr>

<tr>

<td>`online_scheduling`</td>

<td>boolean</td>

<td>Whether this profile should be availible for online scheduling</td>

</tr>

<tr>

<td>`reason`</td>

<td>string</td>

</tr>

<tr>

<td>`sort_order`</td>

<td>integer</td>

</tr>

</tbody>

</table>

#### `/api/appointment_templates`

##### Result object format

<table>

<thead>

<tr>

<th>Key</th>

<th>Type</th>

<th>Notes</th>

</tr>

</thead>

<tbody>

<tr>

<td>`duration`</td>

<td>integer</td>

<td>Length of an appointment in minutes</td>

</tr>

<tr>

<td>`exam_room`</td>

<td>integer</td>

<td>**1-based** index for the exam room</td>

</tr>

<tr>

<td>`id`</td>

<td>integer</td>

</tr>

<tr>

<td>`office`</td>

<td>URL</td>

</tr>

<tr>

<td>`profile`</td>

<td>URL</td>

<td>Appointment profile to default to</td>

</tr>

<tr>

<td>`scheduled_time`</td>

<td>time</td>

</tr>

<tr>

<td>`week_day`</td>

<td>integer from `0` to `6`</td>

<td>`0` = Monday, `1` = Tuesday, `2` = Wednesday, `3` = Thursday, `4` = Friday, `5` = Saturday, `6` = Sunday</td>

</tr>

</tbody>

</table>

##### Optional `GET` parameters for filtering:

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

<td>`available`</td>

<td>date</td>

<td>Only list templates that are available for a week including a given date</td>

</tr>

<tr>

<td>`office`</td>

<td>integer</td>

<td>List appointemnts for an office</td>

</tr>

<tr>

<td>`week_day`</td>

<td>integer</td>

</tr>

</tbody>

</table>

#### `/api/doctors`

A doctor resource may be read and updated, but not created nor deleted.

##### Result object format

<table>

<thead>

<tr>

<th>Key</th>

<th>Type</th>

<th>Notes</th>

</tr>

</thead>

<tbody>

<tr>

<td>`cell_phone`</td>

<td>optional string</td>

</tr>

<tr>

<td>`country`</td>

<td>optional string</td>

<td>Two-letter country code. Default is `"US"`</td>

</tr>

<tr>

<td>`email`</td>

<td>optional string</td>

</tr>

<tr>

<td>`first_name`</td>

<td>string</td>

</tr>

<tr>

<td>`group_npi_number`</td>

<td>optional string</td>

</tr>

<tr>

<td>`home_phone`</td>

<td>optional string</td>

</tr>

<tr>

<td>`id`</td>

<td>integer</td>

</tr>

<tr>

<td>`job_title`</td>

<td>optional string</td>

</tr>

<tr>

<td>`last_name`</td>

<td>string</td>

</tr>

<tr>

<td>`npi_number`</td>

<td>optional string</td>

<td>If both this field and `group_npi_number` are set, prefer this field.</td>

</tr>

<tr>

<td>`office_phone`</td>

<td>optional string</td>

</tr>

<tr>

<td>`specialty`</td>

<td>optional string</td>

</tr>

<tr>

<td>`suffix`</td>

<td>optional string</td>

</tr>

<tr>

<td>`website`</td>

<td>optional string</td>

</tr>

</tbody>

</table>

#### `/api/documents`

##### Result object format

<table>

<thead>

<tr>

<th>Key</th>

<th>Type</th>

<th>Notes</th>

</tr>

</thead>

<tbody>

<tr>

<td>`date`</td>

<td>date</td>

</tr>

<tr>

<td>`description`</td>

<td>string</td>

</tr>

<tr>

<td>`doctor`</td>

<td>URL</td>

</tr>

<tr>

<td>`document`</td>

<td>file</td>

</tr>

<tr>

<td>`id`</td>

<td>integer</td>

</tr>

<tr>

<td>`metatags`</td>

<td>array of strings</td>

</tr>

<tr>

<td>`patient`</td>

<td>URL</td>

</tr>

</tbody>

</table>

##### Optional `GET` parameters for filtering:

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

<td>`doctor`</td>

<td>integer</td>

<td>List documents for a doctor (by ID)</td>

</tr>

<tr>

<td>`patient`</td>

<td>integer</td>

<td>List documents for a patient (by ID)</td>

</tr>

<tr>

<td>`since`</td>

<td>timestamp</td>

<td>Only retrieve documents that have been updated since a given date</td>

</tr>

</tbody>

</table>

#### `/api/offices`

##### Result object format

<table>

<thead>

<tr>

<th>Key</th>

<th>Type</th>

<th>Notes</th>

</tr>

</thead>

<tbody>

<tr>

<td>`address`</td>

<td>string</td>

</tr>

<tr>

<td>`city`</td>

<td>string</td>

<td>Two-letter country code</td>

</tr>

<tr>

<td>`country`</td>

<td>string</td>

</tr>

<tr>

<td>`end_time`</td>

<td>optional time</td>

<td>Truncated the hour. Default is `24:00`</td>

</tr>

<tr>

<td>`exam_rooms`</td>

<td>array of objects</td>

<td>See below</td>

</tr>

<tr>

<td>`id`</td>

<td>integer</td>

</tr>

<tr>

<td>`name`</td>

<td>string</td>

</tr>

<tr>

<td>`online_scheduling`</td>

<td>boolean</td>

<td>Default is `false`</td>

</tr>

<tr>

<td>`online_timeslots`</td>

<td>array of objects</td>

<td>See below</td>

</tr>

<tr>

<td>`phone_number`</td>

<td>string</td>

</tr>

<tr>

<td>`start_time`</td>

<td>time</td>

<td>Truncated to the hour. Default is `00:00`</td>

</tr>

<tr>

<td>`state`</td>

<td>string</td>

<td>Two-letter abbreviation</td>

</tr>

<tr>

<td>`zip_code`</td>

<td>string</td>

</tr>

</tbody>

</table>

##### `exam_rooms` object format

<table>

<thead>

<tr>

<th>Key</th>

<th>Type</th>

<th>Notes</th>

</tr>

</thead>

<tbody>

<tr>

<td>`index`</td>

<td>integer</td>

<td>**1-based** index for the exam room ordering</td>

</tr>

<tr>

<td>`name`</td>

<td>string</td>

</tr>

<tr>

<td>`online_scheduling`</td>

<td>boolean</td>

<td>`false` means the doctor doesn't want to allow online scheduling in this exam room</td>

</tr>

</tbody>

</table>

##### `online_timeslots` object format

<table>

<thead>

<tr>

<th>Key</th>

<th>Type</th>

<th>Notes</th>

</tr>

</thead>

<tbody>

<tr>

<td>`day`</td>

<td>integer</td>

<td>`0` is Sunday, `1` is Monday, etc.</td>

</tr>

<tr>

<td>`hour`</td>

<td>integer</td>

<td>From `0` to `23`</td>

</tr>

<tr>

<td>`minute`</td>

<td>integer</td>

<td>One of `0`, `15`, `30`, or `45`.</td>

</tr>

</tbody>

</table>

#### `/api/patients`

##### Result object format

<table>

<thead>

<tr>

<th>Key</th>

<th>Type</th>

<th>Available in `patients:summary`?</th>

<th>Notes</th>

</tr>

</thead>

<tbody>

<tr>

<td>`address`</td>

<td>optional string</td>

<td>No</td>

</tr>

<tr>

<td>`cell_phone`</td>

<td>optional string</td>

<td>No</td>

</tr>

<tr>

<td>`chart_id`</td>

<td>string</td>

<td>Yes</td>

</tr>

<tr>

<td>`city`</td>

<td>optional string</td>

<td>No</td>

</tr>

<tr>

<td>`date_of_birth`</td>

<td>date</td>

<td>Yes</td>

</tr>

<tr>

<td>`doctor`</td>

<td>URL</td>

<td>Yes</td>

<td>Primary provider. Other doctors in the practice group may have access to the patient's chart</td>

</tr>

<tr>

<td>`email`</td>

<td>optional string</td>

<td>No</td>

</tr>

<tr>

<td>`emergency_contact_name`</td>

<td>optional string</td>

<td>No</td>

</tr>

<tr>

<td>`emergency_contact_phone`</td>

<td>optional string</td>

<td>No</td>

</tr>

<tr>

<td>`emergency_contact_relation`</td>

<td>optional string</td>

<td>No</td>

</tr>

<tr>

<td>`employer`</td>

<td>optional string</td>

<td>No</td>

</tr>

<tr>

<td>`employer_address`</td>

<td>optional string</td>

<td>No</td>

</tr>

<tr>

<td>`employer_city`</td>

<td>optional string</td>

<td>No</td>

</tr>

<tr>

<td>`employer_state`</td>

<td>optional string</td>

<td>No</td>

<td>Two-letter abbreviation</td>

</tr>

<tr>

<td>`employer_zip_code`</td>

<td>optional string</td>

<td>No</td>

</tr>

<tr>

<td>`ethnicity`</td>

<td>optional string</td>

<td>No</td>

</tr>

<tr>

<td>`first_name`</td>

<td>string</td>

<td>No</td>

</tr>

<tr>

<td>`gender`</td>

<td>string</td>

<td>Yes</td>

<td>One of `"Male"`, `"Female"`, or `"Other"`</td>

</tr>

<tr>

<td>`home_phone`</td>

<td>optional string</td>

<td>No</td>

</tr>

<tr>

<td>`id`</td>

<td>integer</td>

<td>Yes</td>

</tr>

<tr>

<td>`last_name`</td>

<td>string</td>

<td>No</td>

</tr>

<tr>

<td>`middle_name`</td>

<td>optional string</td>

<td>No</td>

</tr>

<tr>

<td>`preferred_language`</td>

<td>optional string</td>

<td>No</td>

</tr>

<tr>

<td>`primary_care_physician`</td>

<td>optional string</td>

<td>No</td>

</tr>

<tr>

<td>`race`</td>

<td>optional string</td>

<td>No</td>

</tr>

<tr>

<td>`social_security_number`</td>

<td>optional string</td>

<td>No</td>

</tr>

<tr>

<td>`state`</td>

<td>optional string</td>

<td>No</td>

<td>Two-letter abbreviation</td>

</tr>

<tr>

<td>`zip_code`</td>

<td>optional string</td>

<td>No</td>

</tr>

<tr>

<td>'primary_insurance_company'</td>

<td>optional string</td>

<td>No</td>

</tr>

<tr>

<td>'primary_insurance_id_number'</td>

<td>optional string</td>

<td>No</td>

</tr>

<tr>

<td>'primary_insurance_group_number'</td>

<td>optional string</td>

<td>No</td>

</tr>

<tr>

<td>'primary_insurance_claim_office_number'</td>

<td>optional string</td>

<td>No</td>

</tr>

<tr>

<td>'primary_insurance_payer_id'</td>

<td>optional string</td>

<td>No</td>

</tr>

<tr>

<td>'primary_insurance_plan_name'</td>

<td>optional string</td>

<td>No</td>

</tr>

<tr>

<td>'primary_insurance_plan_type'</td>

<td>optional string</td>

<td>No</td>

</tr>

</tbody>

</table>

##### Optional `GET` parameters for filtering:

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

<td>`doctor`</td>

<td>integer</td>

<td>List patients for a doctor</td>

</tr>

<tr>

<td>`since`</td>

<td>timestamp</td>

<td>Only retrieve patients that have been updated since a given date</td>

</tr>

<tr>

<td>`search`</td>

<td>string</td>

<td>Case-insensitive search on name and email</td>

</tr>

<tr>

<td>`date_of_birth`</td>

<td>date</td>

<td>Limit to patients born on a given date</td>

</tr>

<tr>

<td>`gender`</td>

<td>string</td>

<td>One of `"Male"`, `"Female"`, or `"Other"`</td>

</tr>

</tbody>

</table>

#### `/api/patients/:id/ccda`

Retrieve a patient's C-CDA XML file

##### Result object format

<table>

<thead>

<tr>

<th>Key</th>

<th>Type</th>

<th>Notes</th>

</tr>

</thead>

<tbody>

<tr>

<td>`ccda`</td>

<td>string</td>

<td>C-CDA XML file as string</td>

</tr>

</tbody>

</table>

#### `/api/reminder_profiles`

<table>

<thead>

<tr>

<th>Key</th>

<th>Type</th>

<th>Notes</th>

</tr>

</thead>

<tbody>

<tr>

<td>`id`</td>

<td>integer</td>

</tr>

<tr>

<td>`doctor`</td>

<td>URL</td>

</tr>

<tr>

<td>`name`</td>

<td>string</td>

</tr>

<tr>

<td>`reminders`</td>

<td>array of objects</td>

<td>See below</td>

</tr>

</tbody>

</table>

##### `reminders` object format

<table>

<thead>

<tr>

<th>Key</th>

<th>Type</th>

<th>Notes</th>

</tr>

</thead>

<tbody>

<tr>

<td>`amount`</td>

<td>integer</td>

</tr>

<tr>

<td>`type`</td>

<td>string</td>

<td>One of `"email"`, `"sms"`, or `"auto_call"`</td>

</tr>

<tr>

<td>`unit`</td>

<td>string</td>

<td>One of `"minutes"`, `"hours"`, `"days"`, or `"weeks"`</td>

</tr>

</tbody>

</table>

#### `/api/reminder_profiles/:id/apply_to_appointment`

Adds reminders to an appointment, but only if it doesn't have any reminders already. Must be called with `POST`. Returns `204 No Content` on success.

<table>

<thead>

<tr>

<th>Key</th>

<th>Type</th>

<th>Notes</th>

</tr>

</thead>

<tbody>

<tr>

<td>`appointment`</td>

<td>URL</td>

<td>Appointment to add reminders to.</td>

</tr>

</tbody>

</table>

##### Errors

<table>

<thead>

<tr>

<th>Status</th>

<th>Explanation</th>

</tr>

</thead>

<tbody>

<tr>

<td>`400 Bad Request`</td>

<td>The `appointment` parameter is either missing or invalid.</td>

</tr>

<tr>

<td>`409 Conflict`</td>

<td>The referenced appointment already has reminders attached.</td>

</tr>

</tbody>

</table>

#### `/api/users`

User records are read-only.

##### Result object format

<table>

<thead>

<tr>

<th>Key</th>

<th>Type</th>

<th>Notes</th>

</tr>

</thead>

<tbody>

<tr>

<td>`doctor`</td>

<td>URL</td>

<td>For staff members, this points to their primary physician. For doctors, this points to their own record.</td>

</tr>

<tr>

<td>`is_doctor`</td>

<td>boolean</td>

<td>Mutually exclusive with `is_doctor`</td>

</tr>

<tr>

<td>`is_staff`</td>

<td>boolean</td>

<td>Mutually exclusive with `is_staff`</td>

</tr>

<tr>

<td>`username`</td>

<td>string</td>

</tr>

</tbody>

</table>

#### `/api/users/current`

This pseudo-resource is simply a redirect to the currently logged in user.

### Scopes

<table>

<thead>

<tr>

<th>Code</th>

<th>Description</th>

</tr>

</thead>

<tbody>

<tr>

<td>`calendar`</td>

<td>View your calendar and schedule appointments.</td>

</tr>

<tr>

<td>`user`</td>

<td>View your personal information.</td>

</tr>

<tr>

<td>`patients:summary`</td>

<td>View summary information about your patients. This includes patients' name, chart ID, age, and gender.</td>

</tr>

<tr>

<td>`patients`</td>

<td>View and edit detailed patient information.</td>

</tr>

</tbody>

</table>

#### `/api/transactions`

Transaction records are read-only.

##### Result object format

<table>

<thead>

<tr>

<th>Key</th>

<th>Type</th>

<th>Notes</th>

</tr>

</thead>

<tbody>

<tr>

<td>`appointment`</td>

<td>integer</td>

<td>appointment id</td>

</tr>

<tr>

<td>`doctor`</td>

<td>integer</td>

<td>doctor id</td>

</tr>

<tr>

<td>`doctor_name`</td>

<td>string</td>

<td>doctor name</td>

</tr>

<tr>

<td>`billing_status`</td>

<td>string</td>

<td>one of `""`, `"Incomplete Information"`, `"In Process Emdeon"`, `"In Process iHCFA"`, `"Rejected Emdeon"`, `"Rejected iHCFA"`, `"In Process Payer"`, `"Payer Acknowledged"`, `"Rejected Payer"`, `"Paid in Full"`, `"Partially Paid"`, `"Coordination of Benefits"`, `"ERA Received"`, `"ERA Denied"`,</td>

</tr>

<tr>

<td>`denied_flag`</td>

<td>boolean</td>

</tr>

<tr>

<td>`code`</td>

<td>string</td>

</tr>

<tr>

<td>`description`</td>

<td>string</td>

</tr>

<tr>

<td>`procedure_type`</td>

<td>string</td>

<td>one of `"CPT"`, `"HCPCS"`, `"Custom"`</td>

</tr>

<tr>

<td>`quantity`</td>

<td>number</td>

</tr>

<tr>

<td>`units`</td>

<td>string</td>

<td>default is 'UN'</td>

</tr>

<tr>

<td>`edi_note`</td>

<td>string</td>

</tr>

<tr>

<td>`modifiers`</td>

<td>list</td>

<td>list of 4 code modifiers</td>

</tr>

<tr>

<td>`diagnosis_pointers`</td>

<td>list</td>

<td>list of 4 diagnosis pointers</td>

</tr>

<tr>

<td>`price`</td>

<td>string</td>

<td>price of procedure</td>

</tr>

<tr>

<td>`billed`</td>

<td>string</td>

<td>total billed</td>

</tr>

<tr>

<td>`allowed`</td>

<td>string</td>

<td>amount allowed by insurance</td>

</tr>

<tr>

<td>`adjustment`</td>

<td>string</td>

<td>adjustment from total billed</td>

</tr>

<tr>

<td>`ins1_paid`</td>

<td>string</td>

<td>amount paid by patient's primary insurer</td>

</tr>

<tr>

<td>`ins2_paid`</td>

<td>string</td>

<td>amount paid by patient's secondary insurer</td>

</tr>

<tr>

<td>`ins3_paid`</td>

<td>string</td>

<td>amount paid by patient's tertiary insurer</td>

</tr>

<tr>

<td>`ins_total`</td>

<td>string</td>

<td>amount paid by patient's insurers</td>

</tr>

<tr>

<td>`pt_paid`</td>

<td>string</td>

<td>amount paid by patient</td>

</tr>

<tr>

<td>`paid_total`</td>

<td>string</td>

<td>total amount paid</td>

</tr>

<tr>

<td>`balance_ins`</td>

<td>string</td>

<td>insurance balance</td>

</tr>

<tr>

<td>`balance_pt`</td>

<td>string</td>

<td>patient balance</td>

</tr>

<tr>

<td>`balance_total`</td>

<td>string</td>

<td>total balance</td>

</tr>

<tr>

<td>`date`</td>

<td>timestamp</td>

<td>The starting time of the appointment</td>

</tr>

<tr>

<td>`location`</td>

<td>integer</td>

<td>office id</td>

</tr>

</tbody>

</table>

##### `GET` parameters for filtering:

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

<td>`doctor`</td>

<td>integer</td>

<td>List transactions for a doctor, required</td>

</tr>

<tr>

<td>`since`</td>

<td>timestamp</td>

<td>Only retrieve transactions that have been updated since a given date, optional</td>

</tr>

</tbody>

</table>

</article>

</div>

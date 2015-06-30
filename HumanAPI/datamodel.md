## 数据资源目录
### 医疗类
医疗机构列表信息
医疗机构详情
生命体征度量值列表(详情对象的数组)
生命体征度量值详情
病历文本列表(详情对象的数组)
病历文本条目详情
检验结果列表(详情对象的数组)
检验项目详情
recipient详情
检验结果详情
就诊记录列表(详情对象的数组)
过敏信息列表(详情对象的数组)
过敏信息详情
用药信息列表(详情对象的数组)
用药信息详情
药房信息详情
免疫接种信息列表(详情对象的数组)
免疫接种信息详情
疾病问题信息列表(详情对象的数组)
疾病问题信息详情
患者当前的基本信息(medical health profile)信息
患者既往的病历记录(包含就诊记录、检验结果、免疫接种、病历记录、生命体征)
患者所有的CCD临床文档列表信息
### 健康类
日常运动信息列表(详情对象的数组)
日常运动信息详情
日常运动概述/摘要列表(详情对象的数组)
日常运动概述/摘要详情
血糖测量值详情
血氧测量值详情
血压测量值详情
BMI测量值详情
体脂测量值详情
基因数据genetic traits
心率测量值详情
身高测量值详情
所有健康指标的概述
指标测量位置详情
膳食信息列表
膳食信息详情
平台账号信息
睡眠信息列表
睡眠信息详情
睡眠概述信息列表
睡眠概述信息详情
体重测量值详情
### 工具类
数据来源详情
本地app用户信息列表
本地APP用户信息详情
批量查询所有用户的某个指标数据

## 医疗类数据资源详情

* 医疗机构列表信息
Property 	Type 	Description
id 	String 	The Id of the organization
name 	String 	The name of the organization
imageUrl 	String 	The image URL for the organization

* 医疗机构详情
Property 	Type 	Descriptionid 	String 	The Id of the organization
name 	String 	The name of the organization
href 	String 	Human API organizations endpoint URL to retrieve full details

* 生命体征度量值列表

Property 	Type 	Description
id 	String 	The Id of the vitals reading
source 	String 	The name of the originating service
updatedAt 	Date 	The time the record was updated on the Human API server
createdAt 	Date 	The time the record was created on the Human API server
dateTime 	String 	The date of the vitals reading
author 	String 	The name of the vitals reading author (e.g. doctor)
results 	Array[Object] 	A list of all test results (see results object below)
organization 	Object 	Hospital information (See organizations)

* 生命体征度量值详情

Property 	Type 	Description
name 	String 	The name of the test component (e.g. ‘HEIGHT’, 'WEIGHT’, 'BODY TEMPERATURE’)
value 	String 	The test result value
unit 	String 	The unit of the value - if provided (e.g. 'kg’, ’%’, 'Cel’)

* 病历文本列表

Response Properties
Property 	Type 	Description
id 	String 	The Id of the narrative
source 	String 	The name of the originating service
updatedAt 	Date 	The time the record was updated on the Human API server
createdAt 	Date 	The time the record was created on the Human API server
dateTime 	String 	The date of the narrative
author 	String 	The name of the author
entries 	Array[Object] 	Array of entry objects (See below)
organization 	Object 	Hospital information (See organizations)

* 病历文本条目详情

Entry Object
Property 	Type 	Description
title 	String 	The title of the entry
text 	String 	The description text of the entry

* 检验结果列表
Property 	Type 	Description
id 	String 	The Id of the test result record
source 	String 	The name of the originating service
updatedAt 	Date 	The time the record was updated on the Human API server
createdAt 	Date 	The time the record was created on the Human API server
components 	Array[Object] 	A list of components of the test - if provided (See component below)
name 	String 	The name of the test preformed (e.g. ‘COMPREHENSIVE METABOLIC PANEL’, 'LIPID PANEL’, 'URINE CULTURE’)
patient 	Object 	Patient information (“name” and other optional attributes)
orderedBy 	String 	The name of the ordering physician or entity
recipients 	Array[Object] 	A list of recipients of the test - if provided (see recipient object below)
resultDateTime 	Date 	The date of the test result
status 	String 	The status of the test result (e.g. 'Final result’, 'Edited’, 'Edited Result - FINAL’)
allResults 	Array[Object] 	A list of all test results (see results object below)
comments 	String 	Optional comments for the test result
narrative 	String 	Optional narrative for the test result)
impression 	String 	Optional impression for the test result
transcriptions 	String 	Optional transcriptions for the test result
organization 	Object 	Hospital information (See organizations)

* 检验项目详情
Property 	Type 	Description
name 	String 	The name of the test component (e.g. 'Potassium’, 'MCHC’, 'MCV’)
value 	String 	The test result value
unit 	String 	The unit of the value - if provided (e.g. 'mg/dL’, ’%’, 'mmol/L’)
low 	String 	The low value for the component - if provided
high 	String 	The high value for the component - if provided
refRange 	String 	The reference range for the value - if provided (e.g. 'Negative’, ’>60’, ’<150’)
componentComments 	String 	Optional comments provided for the component
flag 	String 	Optional flag for the component (e.g. 'L’, 'H’, 'A’)


* recipient详情
Recipient Object
Property 	Type 	Description
objectID 	String 	The id of the recipient
name 	String 	The name of the recipient
isPCP 	Boolean 	Indicates if the provider is the Primary Care Provider of the user
recipTemplate 	String 	The recipient template (e.g. 'WPMessageRecipientTemplateUnknown’)

* 检验结果详情

Results Object (allResults)
Property 	Type 	Description
name 	String 	The name of the test component (e.g. 'Potassium’, 'MCHC’, 'MCV’)
resultDateTime 	Date 	The date of the test result
value 	String 	The test result value
unit 	String 	The unit of the value - if provided (e.g. 'mg/dL’, ’%’, 'mmol/L’)


* 就诊记录列表
Property 	Type 	Description
id 	String 	The Id of the encounter record
source 	String 	The name of the originating service
updatedAt 	Date 	The time the record was updated on the Human API server
createdAt 	Date 	The time the record was created on the Human API server
dateTime 	Date 	The date of the encounter
visitType 	String 	The type of visit
provider 	Object 	The provider for the encounter (see provider object below)
diagnoses 	Array[Object] 	A list of diagnoses for the encounter (“name” and other optional attributes)
reasons 	Array[String] 	A list of reasons for the encounter (e.g. ‘Follow-up’, 'Consult’, 'Back Pain’)
vitals 	Object 	Vitals captured during the encounter (See vitals)
orders 	Array[Object] 	A list of orders prescribed for the patient (see orders below)
medications 	Array[Object] 	A list of prescribed medications (See medications)
followUpInstructions 	String 	Optional follow-up instructions
patientInstruction 	String 	Optional patient instructions
patient 	Object 	Patient information (“name” and other optional attributes)
organization 	Object 	Hospital information (See organizations)

* 医嘱信息
Property 	Type 	Description
name 	String 	The name of the order
codeType 	String 	The code type of the order (e.g. “CPT( R )”, “Custom”)
expectedDate 	Date 	The date the order is expected
expireDate 	Date 	The date the order expires
procedureCode 	String 	The procedure code of the order
type 	String 	The type of the order (e.g. “Lab”, “Procedures”)

* 医务人员信息
Property 	Type 	Description
name 	String 	Name of the provider
departmentName 	String 	Name of the provider department

* 过敏信息列表

Property 	Type 	Description
id 	String 	The Id of the resource
source 	String 	The name of the originating service
updatedAt 	Date 	The time the record was updated on the Human API server
createdAt 	Date 	The time the record was created on the Human API server
name 	String 	The name of the allergy (e.g. ‘Etomidate’, 'Fluconazole’, 'Metaxalone’)
patient 	Object 	Patient information (“name” and other optional attributes)
organization 	Object 	Hospital information (See organizations)
* 过敏信息详情

Property 	Type 	Description
id 	String 	The Id of the resource
source 	String 	The name of the originating service
updatedAt 	Date 	The time the record was updated on the Human API server
createdAt 	Date 	The time the record was created on the Human API server
name 	String 	The name of the allergy (e.g. ‘Etomidate’, 'Fluconazole’, 'Metaxalone’)
patient 	Object 	Patient information (“name” and other optional attributes)
organization 	Object 	Hospital information (See organizations)

* 用药信息详情
Response Properties
Property 	Type 	Description
id 	String 	The Id of the medication record
source 	String 	The name of the originating service
updatedAt 	Date 	The time the record was updated on the Human API server
createdAt 	Date 	The time the record was created on the Human API server
instructions 	String 	The instructions provided for the medication (e.g. ‘Take 1 tablet by mouth daily.’)
genericProductIndicator 	String 	The Generic Product Identifier of the medication
patient 	Object 	Patient information (“name” and other optional attributes)
name 	String 	The name of the medication (e.g. 'Cyanocobalamin (VITAMIN B12 PO)’)
provider 	String 	The name of the provider - if available (e.g. 'Jacob Smith, MD’, 'Outside Provider’, 'Unknown’)
providerID 	String 	The id of the provider
ndcCode 	String 	The National Drug Code of the medication (if provided)
productName 	String 	The product name of the medication - if provided (e.g. 'zolpidem’, 'gabapentin’, 'aspirin’)
commonBrandName 	String 	The common brand name of the medication - if provided (e.g. 'NORCO’, 'XANAX’, 'NEURONTIN’)
dosageInfo 	String 	The dosage information - if provided (e.g. '10 MG tablet’, '100 UNIT/ML injection’, '5mg Tab’)
pharmacy 	Object 	Pharmacy information - if provided (see pharmacy below)
organization 	Object 	Hospital information (See organizations)
expiration 	Date 	The expiration date of the medication - if provided
refillsRemaining 	Number 	The number of refills remaining - if available
refillsTotal 	Number 	The total number of refills - if available
* 药房信息详情

Property 	Type 	Description
iD 	String 	The Id of the pharmacy
name 	String 	The name of the pharmacy
address 	String 	The address of the pharmacy
phone 	String 	The phone number of the pharmacy
hours 	String 	The opening hours of the pharmacy (if provided)


* 免疫接种详情

Property 	Type 	Description
id 	String 	The Id of the immunization record
name 	String 	The name of the immunization (e.g. ‘Tetanus+Dip ADULT (Td)’, 'Varicella’, 'Influenza Virus Vaccine’)
source 	String 	The name of the originating service
dates 	Array[Date] 	The dates immunization was givin
patient 	Object 	Patient information (“name” and other optional attributes)
updatedAt 	Date 	The time the record was updated on the Human API server
createdAt 	Date 	The time the record was created on the Human API server
organization 	Object 	Hospital information (See organizations)


* 疾病问题详情

Property 	Type 	Description
id 	String 	The Id of the problem
source 	String 	The name of the originating service
updatedAt 	Date 	The time the record was updated on the Human API server
createdAt 	Date 	The time the record was created on the Human API server
name 	String 	The name of the problem
patient 	Object 	Patient information (“name” and other optional attributes)
organization 	Object 	Hospital information (See organizations)

* 患者当前的基本信息 medical health profile(包含那些不会改变的信息 诸如人口学、吸烟状态、酗酒状态等等)
Response Properties
Property 	Type 	Description
id 	String 	The Id of the profile
updatedAt 	Date 	The time the record was updated on the Human API server
createdAt 	Date 	The time the record was created on the Human API server
demographics 	Object 	The demographics of the user (see below)
alcohol 	Object 	The user’s alcohol usage
smoking 	Object 	The user’s smoking habits

* 患者人口学信息

Property 	Type 	Description
address 	Object 	The address information of the user
ethnicity 	String 	The ethnicity of the user
gender 	String 	The user’s gender
language 	String 	The user’s primary language
race 	String 	the users’s race
dob 	String 	The user’s date of birth
name 	Object 	The user’s name

* 酗酒信息

Property 	Type 	Description
use 	String 	The user’s alcohol usage (e.g. “yes”, “no”)

* 患者既往的病历记录(包含就诊记录、检验结果、免疫接种、病历记录、生命体征)

Property 	Type 	Description 	Object Types
id 	String 	The Id of the object 	All
source 	String 	The name of the originating service 	All
updatedAt 	Date 	The time the record was updated on the Human API server 	All
createdAt 	Date 	The time the record was created on the Human API server 	All
type 	String 	The type of the object 	All
dateTime 	String 	The timestamp of the object 	All
author 	String 	The author (e.g. doctor) name ­ 	Encounters, Vitals, Narratives
name 	String 	The object name 	Encounters, Test Results, Immunizations
href 	String 	Object URL for details retrieval 	All
department 	String 	Source department details­ 	Encounters, Vitals, Narratives
organization 	Object 	Source organization information (See organizations) 	All

* CCD文档的列表信息

Response Properties (list)
Property 	Type 	Description
id 	String 	The Id of the resource
source 	String 	The name of the originating service
updatedAt 	Date 	The time the record was updated on the Human API server
createdAt 	Date 	The time the record was created on the Human API server
dateTime 	String 	The date that the CCD was created
author 	String 	CCD author’s name
organization 	Object 	Hospital information (See organizations)

## 健康类数据资源详情

* 日常运动信息列表
* 日常运动信息详情
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

* 日常运动信息概述/摘要列表
* 日常运动信息概述/摘要详情

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

* 血糖测量值信息

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

* 血氧测量值信息
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

* 血压测量值详情
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

* BMI测量值详情
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

* 体脂测量值详情
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


* 基因数据详情

Response properties
Property 	Type 	Description
userId 	String 	The global Id of the user
humanId 	String 	Unique user identifier
trait 	Date 	The most likely present trait
possibleTraits 	String 	A list of all the possible values for a specific trait, for easy comparison
description 	Number 	A description/name of the trait

* 心率测量值详情
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

* 身高测量值详情

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

* 健康指标的概述/摘要 包括日常运动和睡眠
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

* 测量位置详情

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

*  膳食信息列表
*  膳食信息详情
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

* 平台账号信息(用于本地APP和humanAPI平台用户关联的字段)

Response properties
Property 	Type 	Description
userId 	String 	The global Id of the user
humanId 	String 	Unique user identifier
email 	String 	The user’s email address, if known
createdAt 	Date 	The time the measurement was created on the Human API server
updatedAt 	Date 	The time the measurement was updated on the Human API server

* 睡眠信息列表
* 睡眠信息详情
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

* 睡眠Time Series数据


Add the query parameter time_series=true to retrieve the time series data if available.

The timeSeries object can have multiple embedded objects. In the example below there is one object called “quality”, some services might provide other properties such as “heartRate” or “breathing” etc.


Property 	Description
type 	Type of data, indexed with fixed time interval or timestamped.
intervalInMillis 	If the type is indexed this indicates the number of milliseconds between each value.
values 	The array of values, single values (indexed) or key-value pairs (timestamped).

* 睡眠概述信息列表
* 睡眠概述信息详情

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

* 体重测量值详情

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


## 工具类数据资源详情

* 数据来源(用户已连接的数据源)详情(同步状态)

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

* 本地APP用户信息列表
* 本地APP用户信息详情
Response properties
Property 	Type 	Description
humanId 	String 	The id of the user in Human API
externalId 	String 	The id of the user provided from your application
appId 	String 	Unique app identifier found in the Developer Portal.
createdAt 	Date 	The time the measurement was created on the Human API server
updatedAt 	Date 	The time the measurement was updated on the Human API server


## 同步通知

POST Payload Structure

[
  {
    humanId: '69020790587bd5bf4707f1085ff3e801',
    updatedAt: '2015-03-23T19:09:26+00:00',
    type: 'bike',
    model: 'activitysegment',
    action: 'created',
    objectId: '551064e6a2819c000089a001',
    endpoint: 'https://api.humanapi.co/v1/human/activities'
  },
  { humanId: '69020790587bd5bf4707f1085ff3e801',
    updatedAt: '2015-03-23T19:09:29+00:00',
    type: 'activitysummary',
    model: 'activitysummary',
    action: 'updated',
    objectId: '551064e8a2819c000089a002' ,
    endpoint: 'https://api.humanapi.co/v1/human/activities/summaries'
  },
  ...
]

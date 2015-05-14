1、挂号信息字段说明

```
{
    "RegistrationId": "挂号流水号",
    "RegistrationDate": "挂号日期时间",
    "CardId": "卡号",
    "CardType": "卡类型 记账代码",
    "ScheduleType": "<挂号类型 专家、专病、普通>",
    "ScheduleKind": "<排班类型 上午 下午 晚上>",
    "SlotSerialNumber": "挂号序号",
    "DepartmentIdentifier": "科室代码",
    "DepartmentName": "科室名称",
    "PatientId": "<患者的系统id 主键>",
    "PatientIdentifier": "<患者的身份标识>",
    "PatientName": "<患者姓名>",
    "PractitionerId": "<接诊医生的系统id>",
    "PractitionerIdentifier": "<接诊医生的工号>",
    "PractitionerName": "<接诊医生姓名>",
    "AuthorId": "<挂号人员的系统id>",
    "AuthorIdentifier": "<挂号人员的工号>",
    "AuthorName": "<挂号人员姓名>"
}

```


* 挂号类型, 取值范围在值域 `http://moh.gov/fhir/appointmenttype` 中:

| 编码       | 描述 |
|:-----------|:-----|
| expert     | 专家 |
| department | 专科 |
| disease    | 专病 |
| special    | 特需 |
| normal     | 普通 |



2、 Encounter example/挂号就诊记录信息字段说明

```
{
    "RegistrationId": "挂号流水号",
    "RegistrationDate": "挂号日期时间",
    "CardId": "卡号",
    "CardType": "卡类型 记账代码",
    "EncounterId": "就诊流水号",
    "EncounterStatus": "就诊状态(0 未就诊 1 已就诊)",
    "EncounterStatusText": "就诊状态文本(挂号，分诊，医生站)",
    "ScheduleType": "<挂号类型 专家、专病、普通>",
    "ScheduleKind": "<排班类型 上午 下午 晚上>",
    "SlotSerialNumber": "挂号序号",
    "DepartmentIdentifier": "科室代码",
    "DepartmentName": "科室名称",
    "PatientId": "<患者的系统id 主键>",
    "PatientIdentifier": "<患者的身份标识>",
    "PatientName": "<患者姓名>",
    "PractitionerId": "<接诊医生的系统id>",
    "PractitionerIdentifier": "<接诊医生的工号>",
    "PractitionerName": "<接诊医生姓名>",
    "AuthorId": "<挂号人员的系统id>",
    "AuthorIdentifier": "<挂号人员的工号>",
    "AuthorName": "<挂号人员姓名>"
}

```
curl -k -H 'Accept: application/json' \
	   -H 'Accept-Language: en_US' \
	   -u "EOJ2S-Z6OoN_le_KS1d75wsZ6y0SFdVsY9183IvxFyZp:EClusMEUk8e9ihI7ZdVLF5cZ6y0SFdVsY9183IvxFyZp" \
	   -d "response_type=token" \
	   -d "grant_type=client_credentials" \

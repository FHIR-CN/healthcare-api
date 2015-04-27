1、预约信息字段说明

```
{
    "AppointmentId": "id",
    "AppointmentType": "预约方式编码",
    "AppointmentReason": {
        "code": "预约原因-疾病编码",
        "text": "疾病编码的非结构化描述"
    },
    "AppointmentPriority": "本次预约的优先级 0-9 0最低 9最高",
    "AppointmentStart": "门诊时间开始时间",
    "AppointmentEnd": "门诊时间结束时间",
    "AppointmentStatus": "预约状态 pending 等待确认中、booked 已完成预约、arrived 患者在预约日期时间内出现、fulfilled 患者完成预约、cancelled 预约被取消、noshow 患者爽约)",
    "AppointmentDescription": "预约描述",
    "SlotId": "<本次预约选择的可选时间段的系统id>",
    "PatientId": "<患者的系统id>",
    "PatientIdentifier": "<患者的身份标识>",
    "PatientName": "<患者姓名>",
    "PatientConfirmStatus": "患者是否确认预约的状态 初始值 needs-action",
    "PractitionerId": "<医生的系统id>",
    "PractitionerIdentifier": "<医生的工号>",
    "PractitionerName": "<医生姓名>",
    "PractitionerConfirmStatus": "医生是否确认预约的状态 初始值 needs-action"
}

```

-	预约方式, 取值范围在值域 `http://moh.gov/fhir/appointmentmethod` 中:

| 编码               | 描述                 |
|--------------------|:---------------------|
| hospital-tel       | 医院电话预约         |
| hospital-web       | 医院门户网站预约     |
| hospital-msg       | 医院短信预约         |
| hospital-his       | 医生工作站预约       |
| hospital-bianmin   | 便民服务中心预约     |
| hospital-zizhuji   | 自助机预约           |
| hospital-other     | 医院内部其他方式预约 |
| hospital-yilian    | 医联预约             |
| hospital-thirdpart | 第三方预约           |

-	预约状态, 取值范围在值域 `http://hl7.org/fhir/appointmentstatus` 中:

| 编码      | 描述                     |
|-----------|:-------------------------|
| pending   | 等待确认中               |
| booked    | 已完成预约               |
| arrived   | 患者在预约日期时间内出现 |
| fulfilled | 患者完成预约             |
| cancelled | 预约被取消               |
| noshow    | 患者爽约                 |

-	预约类型, 取值范围在值域 `http://moh.gov/fhir/appointmenttype` 中:

| 编码       | 描述 |
|------------|:-----|
| expert     | 专家 |
| department | 专科 |
| disease    | 专病 |
| special    | 特需 |
| normal     | 普通 |

-	参与者是否确认预约的状态, 取值范围在值域 `http://moh.gov/fhir/appointmenttype` 中:

| 编码         | 描述                 |
|--------------|:---------------------|
| needs-action | 需要患者确认本次预约 |
| tentative    | 待定                 |
| declined     | 拒绝本次预约         |
| accepted     | 接受本次预约         |

2、 Appointment example/预约信息例子

```
{
    "AppointmentId": "2docs",
    "AppointmentType": "thirdpart 第三方预约平台",
    "AppointmentReason": {
        "code": "甲亢",
        "text": "甲亢"
    },
    "AppointmentPriority": "0",
    "AppointmentStart": "门诊时间开始时间",
    "AppointmentEnd": "门诊时间结束时间",
    "AppointmentStatus": "pending 等待确认中",
    "AppointmentDescription": "甲亢初诊",
    "SlotId": "id",
    "PatientId": "id",
    "PatientIdentifier": "身份证件号码",
    "PatientName": "Peter James Chalmers",
    "PatientConfirmStatus": "needs-action",
    "PractitionerId": "example",
    "PractitionerIdentifier": "0051",
    "PractitionerName": "Dr Adam Careful",
    "PractitionerConfirmStatus": "accepted"
}
```

curl -k -H 'Accept: application/json' \\ -H 'Accept-Language: en_US' \\ -u "EOJ2S-Z6OoN_le_KS1d75wsZ6y0SFdVsY9183IvxFyZp:EClusMEUk8e9ihI7ZdVLF5cZ6y0SFdVsY9183IvxFyZp" \\ -d "response_type=token" \\ -d "grant_type=client_credentials" \\ https://api.sandbox.paypal.com/v1/oauth2/token

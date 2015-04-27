1、提交的预约申请字段说明

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

2、预约挂号申请请求：

POST base /Appointment{&_format=[mime-type]}

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

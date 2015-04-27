1、预约结果确认信息字段说明

```
{
    "AppointmentResponseId": "id",
    "AppointmentResponseId": "待确认的预约申请的id",
    "AppointmentDescription": "手机短信验证码",
    "PatientId": "<患者的系统id>",
    "PatientIdentifier": "<患者的身份标识>",
    "PatientName": "<患者姓名>",
    "PatientParticipantStatus": "固定值：accepted"
}

```

2、 AppointmentResponse example/预约结果确认信息例子

```
{
    "AppointmentResponseId": "id",
    "AppointmentResponseId": "example",
    "AppointmentDescription": "440011",
    "PatientId": "example",
    "PatientIdentifier": "<患者的身份标识>",
    "PatientName": "患者姓名：王二小",
    "PatientParticipantStatus": "accepted"
}

```

curl -k -H 'Accept: application/json' \\ -H 'Accept-Language: en_US' \\ -u "EOJ2S-Z6OoN_le_KS1d75wsZ6y0SFdVsY9183IvxFyZp:EClusMEUk8e9ihI7ZdVLF5cZ6y0SFdVsY9183IvxFyZp" \\ -d "response_type=token" \\ -d "grant_type=client_credentials" \\ https://api.sandbox.paypal.com/v1/oauth2/token

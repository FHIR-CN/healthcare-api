1、预约结果确认信息字段说明

```
{
  "resourceType": "AppointmentResponse",
  "id": "example",
  "extension": {
      "url": "http://hl7.org/fhir/StructureDefinition/手机验证码",
      "valueString": "手机短信验证码"
  },  
  "appointment": {
    "reference": "预约申请的id Appointment/example"
      },
  "actor": {
    "reference": "确认预约结果信息的患者id Patient/example",
    "display": "患者姓名：王二小"
  },
  "participantStatus": "固定值：needs-action"
}
```



2、 AppointmentResponse example/预约结果确认信息例子

```
{
  "resourceType": "AppointmentResponse",
  "id": "example",
  "extension": {
      "url": "http://hl7.org/fhir/StructureDefinition/手机验证码",
      "valueString": "手机短信验证码"
  },  
  "appointment": {
    "reference": "预约申请的id Appointment/example"
      },
  "actor": {
    "reference": "确认预约结果信息的患者id Patient/example",
    "display": "患者姓名：王二小"
  },
  "participantStatus": "needs-action"
}
```
curl -k -H 'Accept: application/json' \
	   -H 'Accept-Language: en_US' \
	   -u "EOJ2S-Z6OoN_le_KS1d75wsZ6y0SFdVsY9183IvxFyZp:EClusMEUk8e9ihI7ZdVLF5cZ6y0SFdVsY9183IvxFyZp" \
	   -d "response_type=token" \
	   -d "grant_type=client_credentials" \
	https://api.sandbox.paypal.com/v1/oauth2/token

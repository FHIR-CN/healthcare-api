1、预约信息字段说明

```
{
    "resourceType": "Appointment",
    "id": "2docs",
    "status": "预约状态 pending 等待确认中、booked 已完成预约、arrived 患者在预约日期时间内出现、fulfilled 患者完成预约、cancelled 预约被取消、noshow 患者爽约)",
    "type": {
        "coding": [
            {
                "code": "预约方式编码 ",
                "display": "如thirdpart"
            }
        ]
    },
    "reason": {
        "coding": [
            {
                "system": "ICD-10",
                "code": "预约原因-疾病编码",
                "display": "疾病名称"
            }
        ],
        "text": "疾病编码的非结构化描述"
    },
    "priority": "本次预约的优先级 0-9 0最低 9最高",
    "description": "预约的简介",
    "start": "预约的开始日期时间2013-12-09T09:00:00Z",
    "end": "预约的结束日期时间2013-12-09T11:00:00Z",
    "comment": "备注",
    "participant": [
        {
            "actor": {
                "reference": "Patient/example",
                "display": "预约的患者姓名Peter James Chalmers"
            },
            "required": "固定值：required",
            "status": "参与者是否确认预约的状态 needs-action"
        },
        {
            "actor": {
                "reference": "Practitioner/example",
                "display": "预约的医生姓名 Dr Adam Careful"
            },
            "required": "required",
            "status": "参与者是否确认预约的状态 needs-action"
        }
    ]
}
```


* 预约方式, 取值范围在值域 `http://moh.gov/fhir/appointmentmethod` 中:

| 编码 | 描述 |
| --- |:--- |
|hospital-tel  |医院电话预约|
|hospital-web  |医院门户网站预约|
|hospital-msg  |医院短信预约|
|hospital-his  |医生工作站预约|
|hospital-bianmin  |便民服务中心预约|
|hospital-zizhuji   |自助机预约|
|hospital-other |医院内部其他方式预约|
|hospital-yilian        |医联预约|
|hospital-thirdpart     |第三方预约|


* 预约状态, 取值范围在值域 `http://hl7.org/fhir/appointmentstatus` 中:

| 编码 | 描述 |
| --- |:--- |
|pending|等待确认中|
|booked|已完成预约|
|arrived|患者在预约日期时间内出现|
|fulfilled|患者完成预约|
|cancelled|预约被取消|
|noshow|患者爽约|

* 预约类型, 取值范围在值域 `http://moh.gov/fhir/appointmenttype` 中:

| 编码 | 描述 |
| --- |:--- |
|expert     |专家|
|department |专科|
|disease    |专病|
|special    |特需|
|normal     |普通|

* 参与者是否确认预约的状态, 取值范围在值域 `http://moh.gov/fhir/appointmenttype` 中:

| 编码 | 描述 |
| --- |:--- |
|needs-action     |需要患者确认本次预约|
|tentative    |待定|
|declined    |拒绝本次预约|
|accepted     |接受本次预约|



2、 Appointment example/预约信息例子

```
{
    "resourceType": "Appointment",
    "id": "2docs",
    "status": "pending",
    "type": {
        "coding": [
            {
                "code": "thirdpart ",
                "display": "第三方预约平台"
            }
        ]
    },
    "reason": {
        "coding": [
            {
                "system": "ICD-10",
                "code": "预约原因-疾病编码",
                "display": "甲亢"
            }
        ],
        "text": "甲亢"
    },
    "priority": "0",
    "description": "甲亢初诊",
    "start": "2013-12-09T09:00:00Z",
    "end": "2013-12-09T11:00:00Z",
    "comment": "备注",
    "participant": [
        {
            "actor": {
                "reference": "Patient/example",
                "display": "Peter James Chalmers"
            },
            "required": "required",
            "status": "needs-action"
        },
        {
            "actor": {
                "reference": "Practitioner/example",
                "display": "Dr Adam Careful"
            },
            "required": "required",
            "status": "accepted"
        }
    ]
}
```
curl -k -H 'Accept: application/json' \
	   -H 'Accept-Language: en_US' \
	   -u "EOJ2S-Z6OoN_le_KS1d75wsZ6y0SFdVsY9183IvxFyZp:EClusMEUk8e9ihI7ZdVLF5cZ6y0SFdVsY9183IvxFyZp" \
	   -d "response_type=token" \
	   -d "grant_type=client_credentials" \
	https://api.sandbox.paypal.com/v1/oauth2/token

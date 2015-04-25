1、可选时间段字段说明

```
{
    "resourceType": "Slot",
    "id": "example",
    "type": {
        "coding": [
            {
                "code": "类型编码",
                "display": "专家、普通、专病"
            }
        ]
    },
    "schedule": {
        "reference": "排班信息的系统id Schedule/example"
    },
    "freeBusyType": "该时间段的空闲状态 ",
    "start": "2013-12-25T07:30:00Z",
    "end": "2013-12-25T08:59:00Z",
    "overbooked": "能否加号true false",
    "comment": "备注."
}
```

* 时间段的状态(freeBusyType)类型，取值范围在值域 `http://hl7.org/fhir/slotstatus` 中。

| 编码 | 描述 |
|:--- |:--- |
| FREE             | 表示该时间段还没有预约。|
| BUSY             | 表示该时间段已经有一到多个预约。|
| BUSY-UNAVAILABLE | 表示该时间段已经满额，不能够再接受预约。|
| BUSY-TENTATIVE   | 表示该时间段已经满额，但是存在未确定的预约。|


2、 Slot example/可选时间段例子

```
{
    "resourceType": "Slot",
    "id": "example",
    "type": {
        "coding": [
            {
                "code": "类型编码",
                "display": "专家、普通、专病"
            }
        ]
    },
    "schedule": {
        "reference": "排班信息的系统id Schedule/example"
    },
    "freeBusyType": "BUSY",
    "start": "2013-12-25T07:30:00Z",
    "end": "2013-12-25T08:59:00Z",
    "overbooked": "false",
    "comment": "备注."
}
```


curl -k -H 'Accept: application/json' \
	   -H 'Accept-Language: en_US' \
	   -u "EOJ2S-Z6OoN_le_KS1d75wsZ6y0SFdVsY9183IvxFyZp:EClusMEUk8e9ihI7ZdVLF5cZ6y0SFdVsY9183IvxFyZp" \
	   -d "response_type=token" \
	   -d "grant_type=client_credentials" \
	https://api.sandbox.paypal.com/v1/oauth2/token

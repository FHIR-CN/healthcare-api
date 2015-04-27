1、可选时间段字段说明

```
{
    "SlotId": "id",
    "SlotMaxnumber": "最大预约人次",
    "SlotSerialNumber": "预约序号列表",
    "ScheduleType": "<预约类型 专家、专病、普通>",
    "SlotStart": "门诊时间开始时间",
    "SlotEnd": "门诊时间结束时间",
    "SlotStatus": "该时间段的空闲状态 可约、约满、停诊",
    "SlotOverbooked": "能否加号true false",
    "SlotComment": "备注信息",
    "ScheduleId": "排班信息的系统id"

}

```

2、 Slot example/可选时间段例子

```
{
    "SlotId": "id",
    "SlotMaxnumber": "10",
    "SlotSerialNumber": "2",
    "ScheduleType": "专家",
    "SlotStart": "2013-12-25T07:30:00Z",
    "SlotEnd": "2013-12-25T08:59:00Z",
    "SlotStatus": "BUSY",
    "SlotOverbooked": "false",
    "SlotComment": "备注信息",
    "ScheduleId": "example"
}

```

curl -k -H 'Accept: application/json' \\ -H 'Accept-Language: en_US' \\ -u "EOJ2S-Z6OoN_le_KS1d75wsZ6y0SFdVsY9183IvxFyZp:EClusMEUk8e9ihI7ZdVLF5cZ6y0SFdVsY9183IvxFyZp" \\ -d "response_type=token" \\ -d "grant_type=client_credentials" \\ https://api.sandbox.paypal.com/v1/oauth2/token

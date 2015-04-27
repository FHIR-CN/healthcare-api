1、排班信息字段说明

```
{
    "ScheduleId": "id",
    "ScheduleType": "<预约类型 专家、专病、普通>",
    "ScheduleKind": "<排班类型 上午 下午 晚上>",
    "ScheduleStart": "门诊时间开始时间",
    "ScheduleEnd": "门诊时间结束时间",
    "ScheduleStatus": "排班状态",
    "SchedulePrice": "价格",
    "ScheduleComment": "备注信息",
    "DepartmentId": "<所属二级科室的系统id>",
    "DepartmentIdentifier": "KS03.07",
    "DepartmentName": "内分泌科",
    "PractitionerId": "<医生的系统id>",
    "PractitionerIdentifier": "<医生的工号>",
    "PractitionerName": "<医生姓名>",
    "PractitionerProfession": "<职称编码>"
}

```

2、 Schedule example/排班例子

```
{
    "ScheduleId": "id",
    "ScheduleType": "专家",
    "ScheduleKind": "上午",
    "ScheduleStart": "2013-12-25T07:30:00Z",
    "ScheduleEnd": "2013-12-25T10:00:00Z",
    "ScheduleStatus": "可约",
    "SchedulePrice": "300",
    "ScheduleComment": "备注信息",
    "DepartmentId": "xxxxxx",
    "DepartmentIdentifier": "KS03.07",
    "DepartmentName": "内分泌科",
    "PractitionerId": "1",
    "PractitionerIdentifier": "0051",
    "PractitionerName": "罗飞宏",
    "PractitionerProfession": "主任医师"
}

```

curl -k -H 'Accept: application/json' \\ -H 'Accept-Language: en_US' \\ -u "EOJ2S-Z6OoN_le_KS1d75wsZ6y0SFdVsY9183IvxFyZp:EClusMEUk8e9ihI7ZdVLF5cZ6y0SFdVsY9183IvxFyZp" \\ -d "response_type=token" \\ -d "grant_type=client_credentials" \\ https://api.sandbox.paypal.com/v1/oauth2/token

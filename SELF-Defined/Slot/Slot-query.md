1、查询结果字段说明

```
{
    "code": "<成功/失败/错误等状态码>",
    "msg": "<登录成功/失败/错误时的额外信息>",
    "result": {
        "total": "<获取的可选时间段信息数目>",
        "data": [
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
        ]
    }
}
```

2、获取某次排班内的可选时间段信息列表：

GET base /Slot??schedule=Schedule/f001{&_format=[mime-type]}

```
{
    "code": 200,
    "msg": "成功",
    "result": {
        "total": 1,
        "data": [
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

        ]
    }
}
```

获取某次排班内的可选时间段信息列表：

GET base /Slot?schedule=Schedule/f001{&_format=[mime-type]}

根据医生id获取某次排班内的可选时间段信息列表：

GET base /Slot?schedule.actor=[医生的系统id]{&_format=[mime-type]}

根据能否加号获取的可选时间段信息列表：

GET base /Slot?overbooked=true{&_format=[mime-type]}

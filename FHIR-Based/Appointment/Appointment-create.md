1、提交的预约申请字段说明
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

2、预约挂号申请请求：

POST  base /Appointment{&_format=[mime-type]}

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

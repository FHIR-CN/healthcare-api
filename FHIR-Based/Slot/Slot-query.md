1、查询结果字段说明
```
{
    "resourceType": "Bundle",
    "id": "查询结果列表的id",
    "type": "固定值：searchset",
    "base": "服务器地址",
    "total": "获取到的可选时间段数目",
    "entry": [
        {
            "resource": {
                "resourceType": "Slot",
                "id": "example",
                "extension": [
                    {
                        "url": "http://moh.gov/fhir/schedule/maxnumber最大预约人次",
                        "valueString": "[int]"
                    },
                    {
                        "url": "http://hl7.org/fhir/StructureDefinition/serialNumber预约序号列表",
                        "valueString": "2,4,6,8,10,12,14"
                    }
                ],
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
        }
    ]
}
```

2、获取某次排班内的可选时间段信息列表：

GET  base /Slot??schedule=Schedule/f001{&_format=[mime-type]}

```
{
    "resourceType": "Bundle",
    "id": "b39f5600-0861-4aaf-acb6-96f9e8f649f1",
    "type": "searchset",
    "base": "http://fhirtest.uhn.ca/baseDstu2 ",
    "total": "1",
    "entry": {
        "resource": {
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
    }
}
```

获取某次排班内的可选时间段信息列表：

GET  base /Slot?schedule=Schedule/f001{&_format=[mime-type]}

根据医生id获取某次排班内的可选时间段信息列表：

GET  base /Slot?schedule.actor=[医生的系统id]{&_format=[mime-type]}

根据能否加号获取的可选时间段信息列表：

GET  base /Slot?overbooked=true{&_format=[mime-type]}

1、排班信息字段说明

```
{
    "resourceType": "Schedule",
    "id": "example",
    "extension": [
        {
            "url": "http://hl7.org/fhir/StructureDefinition/status排班状态",
            "valueCode": "可约、已满、停诊"
        },
        {
            "url": "http://hl7.org/fhir/StructureDefinition/price",
            "valueString": "300"
        },
        {
            "url": "http://hl7.org/fhir/StructureDefinition/医务人员所属二级科室的编码",
            "valueIdentifier": {
                "use": "official",
                "system": "二级科室编码的分配机构",
                "value": "二级科室编码如KS50.15"
            }
        },
        {
            "url": "http://hl7.org/fhir/StructureDefinition/医务人员所属二级科室的名称",
            "valueString": "二级科室名称如康复医学科"
        },
        {
            "url": "http://hl7.org/fhir/StructureDefinition/医务人员所属二级科室的系统id",
            "valueString": "医务人员所属二级科室的系统id"
        }
    ],
    "type": [
        {
            "coding": [
                {
                    "code": "类型编码",
                    "display": "专家、普通、专病"
                }
            ]
        },
        {
            "coding": [
                {
                    "code": "类型编码",
                    "display": "早中晚班"
                }
            ]
        }
    ],
    "actor": {
        "reference": "医生的系统id Practitioner/1",
        "extension": [
            {
                "url": "http://hl7.org/fhir/StructureDefinition/医生工号",
                "valueIdentifier": {
                    "use": "official",
                    "system": "医生工号分配机构编码",
                    "value": "医生工号"
                }
            },
            {
                "url": "http://moh.gov/fhir/schedule/医生姓名",
                "valueString": "要预约的医生姓名"
            },
            {
                "url": "http://moh.gov/fhir/schedule/医生职称",
                "valueString": {
                    "system": "http://hl7.org/fhir/StructureDefinition/职级编码",
                    "code": "职级编码",
                    "display": "副教授"
                }
            }
        ]
    },
    "planningHorizon": {
        "start": "门诊时间开始时间2013-12-25T07:30:00Z",
        "end": "门诊时间结束时间2013-12-25T10:00:00Z"
    },
    "comment": "备注信息"
}
```
2、 Schedule example/排班例子

```
{
    "resourceType": "Schedule",
    "id": "example",
    "extension": [

        {
            "url": "http://hl7.org/fhir/StructureDefinition/status排班状态",
            "valueCode": "可约"
        },
        {
            "url": "http://hl7.org/fhir/StructureDefinition/price",
            "valueString": "300"
        },
        {
            "url": "http://hl7.org/fhir/StructureDefinition/医务人员所属二级科室的编码",
            "valueIdentifier": {
                "use": "official",
                "system": "二级科室编码的分配机构",
                "value": "KS03.07"
            }
        },
        {
            "url": "http://hl7.org/fhir/StructureDefinition/医务人员所属二级科室的名称",
            "valueString": "内分泌科"
        },
        {
            "url": "http://hl7.org/fhir/StructureDefinition/医务人员所属二级科室的系统id",
            "valueString": "Practitioner/xxxxxx"
        }
    ],
    "type": [
        {
            "coding": [
                {
                    "code": "类型编码-专家编码",
                    "display": "专家"
                }
            ]
        },
        {
            "coding": [
                {
                    "code": "类型编码-早班编码",
                    "display": "上午"
                }
            ]
        }
    ],
    "actor": {
        "reference": "医生的系统id Practitioner/1",
        "extension": [
            {
                "url": "http://hl7.org/fhir/StructureDefinition/医生工号",
                "valueIdentifier": {
                    "use": "official",
                    "system": "医生工号分配机构编码",
                    "value": "0051"
                }
            },
            {
                "url": "http://moh.gov/fhir/schedule/医生姓名",
                "valueString": "罗飞宏 "
            },
            {
                "url": "http://moh.gov/fhir/schedule/医生职称",
                "valueString": {
                    "system": "http://hl7.org/fhir/StructureDefinition/职级编码",
                    "code": "职级编码",
                    "display": "主任医师"
                }
            }
        ]
    },
    "planningHorizon": {
        "start": "门诊时间开始时间2013-12-25T07:30:00Z",
        "end": "门诊时间结束时间2013-12-25T10:00:00Z"
    },
    "comment": "备注信息"
}
```


curl -k -H 'Accept: application/json' \
	   -H 'Accept-Language: en_US' \
	   -u "EOJ2S-Z6OoN_le_KS1d75wsZ6y0SFdVsY9183IvxFyZp:EClusMEUk8e9ihI7ZdVLF5cZ6y0SFdVsY9183IvxFyZp" \
	   -d "response_type=token" \
	   -d "grant_type=client_credentials" \
	https://api.sandbox.paypal.com/v1/oauth2/token

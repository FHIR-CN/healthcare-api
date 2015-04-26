1、查询结果字段说明
```
{
    "resourceType": "Bundle",
    "id": "查询结果列表的id",
    "type": "固定值：searchset",
    "base": "服务器地址",
    "total": "获取到的医院数目",
    "entry": [
        {
            "resource": {
                "resourceType": "Practitioner",
                "id": "医生资源的系统id",
                "text": {
                    "status": "generated",
                    "div": "<div>医生简介</div>"
                },
                "extension": {
                    "url": "http://hl7.org/fhir/StructureDefinition/医生图片",
                    "valueString": "医生图片地址"
                },
                "identifier": {
                    "use": "official",
                    "system": "医生工号分配机构编码",
                    "value": "医生工号"
                },
                "name": {
                    "text": "医生姓名"
                },
                "telecom": [
                    {
                        "system": "phone",
                        "value": "工作联系电话",
                        "use": "work"
                    },
                    {
                        "system": "email",
                        "value": "工作联系邮箱",
                        "use": "work"
                    },
                    {
                        "system": "url",
                        "value": "个人主页",
                        "use": "work"
                    },
                    {
                        "system": "微信",
                        "value": "0205664440",
                        "use": "work"
                    }
                ],
                "address": [
                    {
                        "use": "固定值：work",
                        "text": "工作地点",
                        "city": "城市",
                        "postalCode": "邮编",
                        "country": "国家"
                    }
                ],
                "gender": "性别",
                "birthDate": "出生日期",
                "practitionerRole": [
                    {
                        "managingOrganization": {
                            "reference": "医务人员所属医院的系统idOrganization/f001"
                        },
                        "extension": {
                            "url": "http://hl7.org/fhir/StructureDefinition/医务人员所属医院的医疗机构代码",
                            "valueIdentifier": {
                                "use": "official",
                                "system": "医疗机构代码",
                                "value": "42500049500 "
                            }
                        },
                        "role": {
                            "coding": [
                                {
                                    "system": "http://hl7.org/fhir/StructureDefinition/医务人员类别代码",
                                    "code": "执业医师编码",
                                    "display": "执业医师、护士、药剂师"
                                }
                            ],
                            "text": "Care role"
                        },
                        "specialty": [
                            {
                                "coding": [
                                    {
                                        "system": "http://hl7.org/fhir/StructureDefinition/疾病编码",
                                        "code": "专治疾病",
                                        "display": "专治疾病 糖原贮积病 骨质疏松 糖尿病 甲减 甲亢 "
                                    }
                                ],
                                "text": "擅长领域：1型和2型糖尿病的综合治疗，包括饮食、运动和用药的有机配合，主张因人、因时、因地采用方便实用而又经济的治疗方法；甲状腺病、垂体和肾上腺疾病的诊治"
                            }
                        ]
                    },
                    {
                        "managingOrganization": {
                            "reference": "医务人员所属一级科室的系统idOrganization/f001"
                        },
                        "extension": [
                            {
                                "url": "http://hl7.org/fhir/StructureDefinition/医务人员所属一级科室的编码",
                                "valueIdentifier": {
                                    "use": "official",
                                    "system": "一级科室的编码分配机构",
                                    "value": "一级科室的编码如KS50 "
                                }
                            },
                            {
                                "url": "http://hl7.org/fhir/StructureDefinition/医务人员所属二级科室的名称",
                                "valueString": "医务人员所属二级科室的名称如康复医学科"
                            }
                        ]
                    },
                    {
                        "managingOrganization": {
                            "reference": "Organization/427a6f50-f526-4388-a187-d5ede65d8efe"
                        },
                        "extension": [
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
                                "valueString": "康复医学科"
                            }
                        ]
                    }
                ],
                "qualification": [
                    {
                        "code": {
                            "coding": [
                                {
                                    "system": "http://hl7.org/fhir/StructureDefinition/职称编码",
                                    "code": "职称编码",
                                    "display": "副主任医师"
                                }
                            ]
                        }
                    },
                    {
                        "code": {
                            "coding": [
                                {
                                    "system": "http://hl7.org/fhir/StructureDefinition/职级编码",
                                    "code": "职级编码",
                                    "display": "副教授"
                                }
                            ]
                        }
                    }

                ]
            }
        }
    ]
}
```

2、获取某个医生的排班信息：

GET  base /Schedule?doctor-identifier=[工号]{&_format=[mime-type]}
```
{
    "resourceType": "Bundle",
    "id": "b39f5600-0861-4aaf-acb6-96f9e8f649f1",
    "type": "searchset",
    "base": "http://fhirtest.uhn.ca/baseDstu2 ",
    "total": "1",
    "entry": {
        "resource": {
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
    }
}
```
根据工号获取医生排班信息：
GET  base /Schedule?doctor-identifier=[工号]{&_format=[mime-type]}
根据医生系统id获取医生排班信息：
GET  base /Schedule?actor=[医生的系统id]{&_format=[mime-type]}
根据指定日期获取医生排班信息：
GET  base /Schedule?date=>2013-12-20&<2013-12-26{&_format=[mime-type]}
根据排班类型获取医生排班信息：
GET  base /Schedule?type=专家{&_format=[mime-type]}
根据停诊标志获取医生排班信息：
GET  base /Schedule?status=true{&_format=[mime-type]}

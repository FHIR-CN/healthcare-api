1、医生字段说明
```
{
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
```
2、Doctor example/医生例子

```
{
    "resourceType": "Practitioner",
    "id": "a4263470-b27b-4096-bb14-56e0ea800a7e",
    "text": {
        "status": "generated",
        "div": "<div>医生简介</div>"
    },
    "extension": {
        "url": "http://hl7.org/fhir/StructureDefinition/医生图片",
        "valueString": "20150330113732312.jpg"
    },
    "identifier": {
        "use": "official",
        "system": "医生工号分配机构编码",
        "value": "0051"
    },
    "name": {
        "text": "李靖婕"
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
            "use": "work",
            "text": "工作地点",
            "city": "城市",
            "postalCode": "邮编",
            "country": "国家"
        }
    ],
    "gender": "male",
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
                        "code": "4",
                        "display": "执业医师"
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
                    "text": "长期从事小儿脑瘫早期、超早期诊断及脑病后遗症康复方法的研究，擅长小儿脑性瘫痪、小儿智力低下、语言发育迟缓、周围神经损伤、脑炎后遗症、遗传代谢病等小儿神经伤残疾病的康复评定工作"
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
                        "value": "KS50 "
                    }
                },
                {
                    "url": "http://hl7.org/fhir/StructureDefinition/医务人员所属一级科室的名称",
                    "valueString": "中医科"
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
                        "value": "KS50.15"
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
                        "code": "4",
                        "display": "主任医师"
                    }
                ]
            }
        },
        {
            "code": {
                "coding": [
                    {
                        "system": "http://hl7.org/fhir/StructureDefinition/职级编码",
                        "code": "4",
                        "display": "教授"
                    }
                ]
            }
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

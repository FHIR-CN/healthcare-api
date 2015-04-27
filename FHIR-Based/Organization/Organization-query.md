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
                "resourceType": "Organization",
                "id": "医院类型的资源内容的系统ID",
                "extension": [
                    {
                        "url": "医院的等级 医院类别",
                        "valueCode": "三甲 三乙"
                    },
                    {
                        "url": "医院的图片",
                        " valueUri ": "图片地址"
                    },
                    {
                        "url": "医院的评分",
                        "valueString": "评分值"
                    },
                    {
                        "url": "上级机构的业务id",
                        "valueString": "上级机构的业务id"
                    },
                    {
                        "url": "上级机构的名称",
                        "valueString": "上级机构的名称"
                    }
                ],
                "text": {
                    "status": "additional",
                    "div": "<div>医院简介</div>"
                },
                "identifier": [
                    {
                        "use": "official",
                        "system": "医院id的分配机构",
                        "value": "医院的业务id 医疗机构代码"
                    }
                ],
                "name": "医院全称",
                "type": {
                    "coding": [
                        {
                            "system": "http://hl7.org/fhir/organization-type",
                            "code": "医疗机构类型编码",
                            "display": "医疗机构类型 医院 一级科室 二级科室"
                        }
                    ]
                },
                "telecom": [
                    {
                        "system": "phone",
                        "value": "医疗机构的联系电话"
                    },
                    {
                        "system": "url",
                        "value": "医疗机构的网站"
                    }
                ],
                "partOf": {
                    "reference": "上级医疗机构的系统id"
                },
                "address": [
                    {
                        "text": "医疗机构的地址",
                        "city": "医疗机构所属市县",
                        "state": "医疗机构所属省份"
                    }
                ]
            }
        },
        {
            "resource": {
                "resourceType": "Organization",
                "id": "一级科室的资源内容的系统id",
                "extension": [
                    {
                        "url": "http://hl7.org/fhir/StructureDefinition/一级科室所属医院的医疗机构代码",
                        "valueString": "42500049500"
                    }
                ],
                "text": {
                    "status": "additional",
                    "div": "<div>一级科室简介</div>"
                },
                "identifier": [
                    {
                        "use": "official",
                        "system": "一级科室的编码",
                        "value": "KS03(内科)"
                    }
                ],
                "name": "一级科室全称例如内科外科妇产科",
                "type": {
                    "coding": [
                        {
                            "system": "http://hl7.org/fhir/organization-type",
                            "code": "一级科室"
                        }
                    ]
                },
                "partOf": {
                    "reference": "一级科室所属医院的系统id"
                }
            }
        },
        {
            "resource": {
                "resourceType": "Organization",
                "id": "二级科室的资源内容的系统id",
                "extension": [
                    {
                        "url": "http://hl7.org/fhir/StructureDefinition/二级科室所属医院的医疗机构代码",
                        "valueIdentifier": {
                            "use": "official",
                            "system": "医疗机构代码",
                            "value": "42500049500 "
                        }
                    },
                    {
                        "url": "http://hl7.org/fhir/StructureDefinition/二级科室所属一级科室的编码",
                        "valueIdentifier": [
                            {
                                "use": "official",
                                "system": "一级科室的编码分配机构",
                                "value": "KS03(内科)"
                            }
                        ]
                    }
                ],
                "text": {
                    "status": "additional",
                    "div": "<div>二级科室简介</div>"
                },
                "identifier": [
                    {
                        "use": "official",
                        "system": "二级科室编码的分配机构",
                        "value": "二级科室的编码KS03.07"
                    }
                ],
                "name": "二级科室全称 内分泌内科",
                "type": {
                    "coding": [
                        {
                            "system": "http://hl7.org/fhir/organization-type",
                            "code": "固定值：二级科室"
                        }
                    ]
                },
                "partOf": {
                    "reference": "二级科室所属医院的系统id"
                }
            }
        }
    ]
}

```

2、获取所有省市的所有医院列表：

GET  base /Organization?type=prov {&_format=[mime-type]}


```
{
    "resourceType": "Bundle",
    "id": "b39f5600-0861-4aaf-acb6-96f9e8f649f1",
    "type": "searchset",
    "base": "http://fhirtest.uhn.ca/baseDstu2 ",
    "total": "1",
    "entry": {
        "resource": {
            "resourceType": "Organization",
            "id": "2.16.840.1.113883.3.239.23.1.100.1",
            "extension": [
                {
                    "url": "http://hl7.org/fhir/StructureDefinition/医院的类别和等级编码",
                    "valueCode": "三甲"
                },
                {
                    "url": "http://hl7.org/fhir/StructureDefinition/医院的图片",
                    "valueUri": "http://img.guahao.cn/img/character/hos-l.png?_=20121223"
                },
                {
                    "url": "http://hl7.org/fhir/StructureDefinition/对医院的评分",
                    "valueString ": "8.5"
                }
            ],
            "text": {
                "status": "additional",
                "div": "<div>浙江大学附属第一医院系三级甲等医院，是浙江省医疗、教学、科研指导中心之一。医院创建于1947年11月1日，1952年被命名为浙江医学院附属第一医院，1960年被命名为浙江医科大学附属第一医院，1999年9月改称现名，又名浙江省第一医院。医院占地面积73亩，总建筑面积10.5万㎡ , 在建7.1万㎡ ,智能化综合楼现已投入使用，现有在职职工1775人，其中高级职称医护专家300余人。床位1400张，有临床科室35个，医技科室23个。年门诊量约110万人次，年住院病人3万余人次，年手术病人1.5万余人次</div>"
            },
            "identifier": [
                {
                    "use": "official",
                    "system": "医疗机构代码",
                    "value": "42500049500 "
                }
            ],
            "name": "浙江大学医学院附属第一医院",
            "type": {
                "coding": [
                    {
                        "system": "http://hl7.org/fhir/organization-type",
                        "code": "固定值：prov"
                    }
                ]
            },
            "telecom": [
                {
                    "system": "phone",
                    "value": "(0571)87236666 8723611"
                },
                {
                    "system": "url",
                    "value": "http://huashan.org.cn/"
                }
            ],
            "address": [
                {
                    "text": "浙江省杭州市庆春路79号(310003)",
                    "city": "杭州",
                    "state": "浙江"
                }
            ]
        }
    }
}
```
获取所有所有医疗机构及其下属的一级、二级科室的列表：
GET  base /Organization{?_format=[mime-type]}
获取所有省市的所有医院列表：
GET  base /Organization?type=prov {&_format=[mime-type]}
获取某省某市所有医院列表：
GET  base /Organization?type=prov&?address=上海 and 闵行{&_format=[mime-type]}
获取某家医院的所有一级科室：
GET  base /Organization?type=L1-depart&hospital-identifier=[id] {&_format=[mime-type]}
获取某家医院的所有二级科室：
GET  base /Organization?type=L2-depart&hospital-identifier=[id]&L1-depart-identifier=[id]{&_format=[mime-type]}
获取名称为”上海儿童医院”的所有医院：
GET  base /Organization?type=prov&name=上海儿童医院

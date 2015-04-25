1、医院字段说明
```
{
    "resourceType": "Organization",
    "id": "医院类型的资源内容的系统ID",
    "extension": [
        {
            "url": "http://hl7.org/fhir/StructureDefinition/医院的类别和等级编码",
            "valueCode": "三甲 三乙"
        },
        {
            "url": "http://hl7.org/fhir/StructureDefinition/医院的图片",
            " valueUri ": "图片地址"
        },
        {
            "url": "http://hl7.org/fhir/StructureDefinition/对医院的评分",
            "valueString": "评分值"
        },
        {
            "url": "http://hl7.org/fhir/StructureDefinition/总院的业务id机构编码",
            "valueString": "总院的业务id机构编码"
        },
        {
            "url": "http://hl7.org/fhir/StructureDefinition/总院的名称",
            "valueString": "总院的名称"
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
```


2、L3 hospital example/三甲医院例子

```
{
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
```


curl -k -H 'Accept: application/json' \
	   -H 'Accept-Language: en_US' \
	   -u "EOJ2S-Z6OoN_le_KS1d75wsZ6y0SFdVsY9183IvxFyZp:EClusMEUk8e9ihI7ZdVLF5cZ6y0SFdVsY9183IvxFyZp" \
	   -d "response_type=token" \
	   -d "grant_type=client_credentials" \
	https://api.sandbox.paypal.com/v1/oauth2/token

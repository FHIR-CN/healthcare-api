1、一级科室字段说明
```
{
    "resourceType": "Organization",
    "id": "一级科室的资源内容的系统id",
    "extension": {
        "url": "http://hl7.org/fhir/StructureDefinition/一级科室所属医院的医疗机构代码",
        "valueIdentifier": {
            "use": "official",
            "system": "医疗机构代码",
            "value": "42500049500 "
        }
    },
    "text": {
        "status": "additional",
        "div": "<div>一级科室简介</div>"
    },
    "identifier": [
        {
            "use": "official",
            "system": "一级科室的编码分配机构",
            "value": "KS03(内科)"
        }
    ],
    "name": "一级科室全称例如内科外科妇产科",
    "type": {
        "coding": [
            {
                "system": "http://hl7.org/fhir/organization-type",
                "code": "固定值：一级科室"
            }
        ]
    },
    "partOf": {
        "reference": "一级科室所属医院的系统id"
    }
}
```
2、L1 department example/一级科室例子

```
{
    "resourceType": "Organization",
    "id": "ED25EA3F3F67A102E040A8C00F01221B",
    "extension": {
        "url": "http://hl7.org/fhir/StructureDefinition/一级科室所属医院的医疗机构代码",
        "valueIdentifier": {
            "use": "official",
            "system": "医疗机构代码",
            "value": "42500049500 "
        }
    },
    "text": {
        "status": "additional",
        "div": "<div>一级科室简介</div>"
    },
    "identifier": [
        {
            "use": "official",
            "system": "一级科室的编码分配机构",
            "value": "KS03(内科)"
        }
    ],
    "name": "内科",
    "type": {
        "coding": [
            {
                "system": "http://hl7.org/fhir/organization-type",
                "code": "一级科室"
            }
        ]
    },
    "partOf": {
        "reference": "Organization/2.16.840.1.113883.3.239.23.1.100.1"
    }
}
```


curl -k -H 'Accept: application/json' \
	   -H 'Accept-Language: en_US' \
	   -u "EOJ2S-Z6OoN_le_KS1d75wsZ6y0SFdVsY9183IvxFyZp:EClusMEUk8e9ihI7ZdVLF5cZ6y0SFdVsY9183IvxFyZp" \
	   -d "response_type=token" \
	   -d "grant_type=client_credentials" \
	https://api.sandbox.paypal.com/v1/oauth2/token

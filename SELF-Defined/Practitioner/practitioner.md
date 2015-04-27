1、医生字段说明

```
{
    "ParentOrganzationId": "<所属医院的id>",
    "ParentOrganzationIdentifier": "<所属医院的机构代码>",
    "ParentOrganzationName": "<所属医院的名称>",
    "ParentDepartmentId": "<所属一级科室的系统id>",
    "ParentDepartmentIdentifier": "<所属一级科室的编码>",
    "ParentDepartmentName": "<所属一级科室的名称>",
    "DepartmentId": "<所属二级科室的系统id>",
    "DepartmentIdentifier": "<所属二级科室的编码>",
    "DepartmentName": "<所属二级科室的名称>",
    "PractitionerId": "<医生的系统id>",
    "PractitionerIdentifier": "<医生的工号>",
    "PractitionerName": "<医生姓名>",
    "PractitionerTel": "<医生电话>",
    "PractitionerMail": "<医生邮箱>",
    "PractitionerWebsite": "<医生个人主页>",
    "PractitionerWorkplace": "<医生工作地点>",
    "PractitionerPhoto": "<医生图片>",
    "PractitionerType": "<医务人员类别代码>",
    "PractitionerSpecialty": [
        "专治疾病编码"
    ],
    "PractitionerGoodat": "<医务人员擅长领域>",
    "PractitionerLevel": "<职级编码>",
    "PractitionerProfession": "<职称编码>",
    "PractitionerDescription": "<医生简介>"
}

```

2、Doctor example/医生例子

```
{
    "ParentOrganzationId": "f001",
    "ParentOrganzationIdentifier": "42500049500",
    "ParentOrganzationName": "<所属医院的名称>",
    "ParentDepartmentIdentifier": "KS50",
    "ParentDepartmentName": "中医科",
    "DepartmentId": "427a6f50-f526-4388-a187-d5ede65d8efe",
    "DepartmentIdentifier": "KS50.15",
    "DepartmentName": "康复医学科",
    "PractitionerId": "a4263470-b27b-4096-bb14-56e0ea800a7e",
    "PractitionerIdentifier": "0051",
    "PractitionerName": "李靖婕",
    "PractitionerTel": "工作联系电话",
    "PractitionerMail": "工作联系邮箱",
    "PractitionerWebsite": "个人主页",
    "PractitionerWorkplace": "<医生工作地点>",
    "PractitionerPhoto": "20150330113732312.jpg",
    "PractitionerType": "<医务人员类别代码>",
    "PractitionerSpecialty": [
        "骨质疏松",
        "糖原贮积病",
        "糖尿病 甲减 甲亢 "
    ],
    "PractitionerGoodat": "长期从事小儿脑瘫早期、超早期诊断及脑病后遗症康复方法的研究，擅长小儿脑性瘫痪、小儿智力低下、语言发育迟缓、周围神经损伤、脑炎后遗症、遗传代谢病等小儿神经伤残疾病的康复评定工作",
    "PractitionerLevel": "4 教授",
    "PractitionerProfession": "4 主任医师",
    "PractitionerDescription": "医生简介"
}

```

curl -k -H 'Accept: application/json' \\ -H 'Accept-Language: en_US' \\ -u "EOJ2S-Z6OoN_le_KS1d75wsZ6y0SFdVsY9183IvxFyZp:EClusMEUk8e9ihI7ZdVLF5cZ6y0SFdVsY9183IvxFyZp" \\ -d "response_type=token" \\ -d "grant_type=client_credentials" \\ https://api.sandbox.paypal.com/v1/oauth2/token

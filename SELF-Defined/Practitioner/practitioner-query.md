1、查询结果字段说明

```
{
    "code": "<成功/失败/错误等状态码>",
    "msg": "<登录成功/失败/错误时的额外信息>",
    "result": {
        "total": "<获取的医生数目>",
        "data": [
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
        ]
    }
}
```

2、获取某家的所有医生列表：

GET base /Practitioner?hospital-identifier=[id]{&_format=[mime-type]}

```
{
    "code": 200,
    "msg": "成功",
    "result": {
        "total": 1,
        "data": [
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
        ]
    }
}
```

获取所有所有医疗机构及其下属的一级、二级科室的医生列表： GET base /Practitioner{?_format=[mime-type]} 获取某省某市所有医院的医生列表： GET base /Practitioner?address=上海 and 闵行{&_format=[mime-type]} 获取某家医院的所有医生列表： GET base /Practitioner?hospital-identifier=[id]{&_format=[mime-type]} 获取某家医院的某个一级科室的所有医生列表： GET base /Practitioner?L1-depart-identifier=[id]&hospital-identifier=[id] {&_format=[mime-type]} 获取某家医院的某个一级科室下的某个二级科室的所有医生列表： GET base /Practitioner?L2-depart-identifier=[id]&hospital-identifier=[id]&L1-depart-identifier=[id]{&_format=[mime-type]} 获取专治某种疾病的所有医生列表： GET base /Practitioner?L2-depart-identifier=[id]&hospital-identifier=[id]&L1-depart-identifier=[id]{&_format=[mime-type]} 获取姓名为“李翼”的所有医生： GET base /Practitioner?name=李翼

1、查询结果字段说明

```
{
    "code": "<成功/失败/错误等状态码>",
    "msg": "<登录成功/失败/错误时的额外信息>",
    "result": {
        "data": [
            {
                "RegistrationId": "挂号流水号",
                "RegistrationDate": "挂号日期时间",
                "CardId": "卡号",
                "CardType": "卡类型 记账代码",
                "EncounterId": "就诊流水号",
                "EncounterStatus": "就诊状态(0 未就诊 1 已就诊)",
                "EncounterStatusText": "就诊状态文本(挂号，分诊，医生站)",
                "ScheduleType": "<挂号类型 专家、专病、普通>",
                "ScheduleKind": "<排班类型 上午 下午 晚上>",
                "SlotSerialNumber": "挂号序号",
                "DepartmentIdentifier": "科室代码",
                "DepartmentName": "科室名称",
                "PatientId": "<患者的系统id 主键>",
                "PatientIdentifier": "<患者的身份标识>",
                "PatientName": "<患者姓名>",
                "PractitionerId": "<接诊医生的系统id>",
                "PractitionerIdentifier": "<接诊医生的工号>",
                "PractitionerName": "<接诊医生姓名>",
                "AuthorId": "<挂号人员的系统id>",
                "AuthorIdentifier": "<挂号人员的工号>",
                "AuthorName": "<挂号人员姓名>"
            }
        ]
    }
}
```

2、获取所有省市的所有医院列表：

GET base /Organizations{&_format=[mime-type]}

```
{
    "code": 200,
    "msg": "成功",
    "result": {
        "total": 2,
        "data": [
            {
                "DepartmentType": "一级科室1",
                "ParentOrganzationId": "2.16.840.1.113883.3.239.23.1.100.1",
                "ParentOrganzationIdentifier": "42500049500",
                "DepartmentId": "ED25EA3F3F67A102E040A8C00F01221B",
                "DepartmentIdentifier": "KS03(内科)",
                "DepartmentName": "内科",
                "DepartmentDescription": "一级科室简介"
            },
            {
                "DepartmentType": "二级科室2",
                "ParentOrganzationId": "2.16.840.1.113883.3.239.23.1.100.1",
                "ParentOrganzationIdentifier": "42500049500",
                "ParentDepartmentIdentifier": "KS03(内科)",
                "DepartmentId": "ED25EA3F3EF5A102E040A8C00F01221B",
                "DepartmentIdentifier": "KS03.07",
                "DepartmentName": "内分泌内科",
                "DepartmentDescription": "二级科室简介"
            }
        ]
    }
}
```

获取所有所有医疗机构及其下属的一级、二级科室的列表： GET base /Organization{?_format=[mime-type]}

获取所有省市的所有医院列表： GET base /Organization?type=prov {&_format=[mime-type]}

获取某省某市所有医院列表： GET base /Organization?type=prov&?address=上海 and 闵行{&_format=[mime-type]} 获取某家医院的所有一级科室： GET base /Organization?type=L1-depart&hospital-identifier=[id] {&_format=[mime-type]} 获取某家医院的所有二级科室： GET base /Organization?type=L2-depart&hospital-identifier=[id]&L1-depart-identifier=[id]{&_format=[mime-type]} 获取名称为”上海儿童医院”的所有医院： GET base /Organization?type=prov&name=上海儿童医院

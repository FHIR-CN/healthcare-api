1、查询结果字段说明

```
{
    "code": "<成功/失败/错误等状态码>",
    "msg": "<登录成功/失败/错误时的额外信息>",
    "result": {
        "total": "<获取的排班信息数目>",
        "data": [
            {
                "ScheduleId": "id",
                "ScheduleType": "<预约类型 专家、专病、普通>",
                "ScheduleKind": "<排班类型 上午 下午 晚上>",
                "ScheduleStart": "门诊时间开始时间",
                "ScheduleEnd": "门诊时间结束时间",
                "ScheduleStatus": "排班状态",
                "SchedulePrice": "价格",
                "ScheduleComment": "备注信息",
                "DepartmentId": "<所属二级科室的系统id>",
                "DepartmentIdentifier": "KS03.07",
                "DepartmentName": "内分泌科",
                "PractitionerId": "<医生的系统id>",
                "PractitionerIdentifier": "<医生的工号>",
                "PractitionerName": "<医生姓名>",
                "PractitionerProfession": "<职称编码>"
            }
        ]
    }
}
```

2、获取某个医生的排班信息：

GET base /Schedule?doctor-identifier=[工号]{&_format=[mime-type]}

```
{
    "code": 200,
    "msg": "成功",
    "result": {
        "total": 1,
        "data": [
            {
                "ScheduleId": "id",
                "ScheduleType": "专家",
                "ScheduleKind": "上午",
                "ScheduleStart": "2013-12-25T07:30:00Z",
                "ScheduleEnd": "2013-12-25T10:00:00Z",
                "ScheduleStatus": "可约",
                "SchedulePrice": "300",
                "ScheduleComment": "备注信息",
                "DepartmentId": "xxxxxx",
                "DepartmentIdentifier": "KS03.07",
                "DepartmentName": "内分泌科",
                "PractitionerId": "1",
                "PractitionerIdentifier": "0051",
                "PractitionerName": "罗飞宏",
                "PractitionerProfession": "主任医师"
            }
        ]
    }
}
```

根据工号获取医生排班信息：
GET base /Schedule?doctor-identifier=[工号]{&_format=[mime-type]}     
根据医生系统id获取医生排班信息： GET base /Schedule?actor=[医生的系统id]{&_format=[mime-type]} 根据指定日期获取医生排班信息： GET base /Schedule?date=>2013-12-20&<2013-12-26{&_format=[mime-type]} 根据排班类型获取医生排班信息： GET base /Schedule?type=专家{&_format=[mime-type]} 根据停诊标志获取医生排班信息： GET base /Schedule?status=true{&_format=[mime-type]}

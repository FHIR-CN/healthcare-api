1、二级科室字段说明

```
{
    "DepartmentType": "<科室类型 一级科室1 二级科室2>",
    "ParentDepartmentIdentifier": "<上级科室的机构代码>",
    "ParentOrganzationId": "<所属医院的id>",
    "ParentOrganzationIdentifier": "<所属医院的机构代码>",
    "DepartmentId": "<科室的id>",
    "DepartmentIdentifier": "<科室的编码>",
    "DepartmentName": "<科室名称>",
    "DepartmentDescription": "<科室简介>"
}
```

2、L2 department example/二级科室例子

```
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
```

curl -k -H 'Accept: application/json' \\ -H 'Accept-Language: en_US' \\ -u "EOJ2S-Z6OoN_le_KS1d75wsZ6y0SFdVsY9183IvxFyZp:EClusMEUk8e9ihI7ZdVLF5cZ6y0SFdVsY9183IvxFyZp" \\ -d "response_type=token" \\ -d "grant_type=client_credentials" \\ https://api.sandbox.paypal.com/v1/oauth2/token

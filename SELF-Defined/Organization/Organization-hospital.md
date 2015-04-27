1、医院字段说明

```
{
    "OrganizationId": "<医疗机构的id>",
    "OrganizationIdentifier": "<医疗机构的机构编码>",
    "ParentOrganizationId": "<总院的id>",
    "ParentOrganizationIdentifier": "<总院的医疗机构编码>",
    "ParentOrganizationName": "<总院的全称>",
    "OrganizationName": "<医疗机构的全称>",
    "OrganizationLevel": "<医疗机构的等级>",
    "OrganizationAddress": "<医疗机构的地址>",
    "OrganizationPhone": "<医疗机构的联系电话>",
    "OrganizationProvince": "<医疗机构的所属省份>",
    "OrganizationCity": "<医疗机构的所属市县>",
    "OrganizationWebsite": "<医疗机构的网站>",
    "OrganizationPicture": "<医疗机构的图片>",
    "OrganizationScore": "<医疗机构的评分>",
    "OrganizationVisitNO": "<医疗机构的就医经验>",
    "OrganizationDescription": "<医疗机构的简介>"
}
```

2、L3 hospital example/三甲医院例子

```
{
    "OrganizationId": "2.16.840.1.113883.3.239.23.1.100.1",
    "OrganizationIdentifier": "42500049500",
    "ParentOrganizationId": "<总院的id>",
    "ParentOrganizationIdentifier": "<总院的医疗机构编码>",
    "ParentOrganizationName": "<总院的全称>",
    "OrganizationName": "浙江大学医学院附属第一医院",
    "OrganizationLevel": "三甲",
    "OrganizationAddress": "浙江省杭州市庆春路79号(310003)",
    "OrganizationPhone": "(0571)87236666 8723611",
    "OrganizationProvince": "浙江",
    "OrganizationCity": "杭州",
    "OrganizationWebsite": "http://huashan.org.cn/",
    "OrganizationPicture": "http://img.guahao.cn/img/character/hos-l.png?_=20121223",
    "OrganizationScore": "8.5",
    "OrganizationVisitNO": "14356",
    "OrganizationDescription": "浙江大学附属第一医院系三级甲等医院，是浙江省医疗、教学、科研指导中心之一。医院创建于1947年11月1日，1952年被命名为浙江医学院附属第一医院，1960年被命名为浙江医科大学附属第一医院，1999年9月改称现名，又名浙江省第一医院。医院占地面积73亩，总建筑面积10.5万㎡ , 在建7.1万㎡ ,智能化综合楼现已投入使用，现有在职职工1775人，其中高级职称医护专家300余人。床位1400张，有临床科室35个，医技科室23个。年门诊量约110万人次，年住院病人3万余人次，年手术病人1.5万余人次"
}
```

curl -k -H 'Accept: application/json' \\ -H 'Accept-Language: en_US' \\ -u "EOJ2S-Z6OoN_le_KS1d75wsZ6y0SFdVsY9183IvxFyZp:EClusMEUk8e9ihI7ZdVLF5cZ6y0SFdVsY9183IvxFyZp" \\ -d "response_type=token" \\ -d "grant_type=client_credentials" \\ https://api.sandbox.paypal.com/v1/oauth2/token

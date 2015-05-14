1、医嘱信息字段说明

```
{
    "OrderID": "医嘱流水号 主键 医嘱ID是医院内部唯一标识一条医嘱的编号，必须保证在本医院内不重复。如医院内的序号是循环使用的，可以采用在前面加日期的方式避免重复（格式：YYYYMMDD）",
    "OrderWithdrawalSign": "撤销标志 1：正常；2：撤销该条医嘱",
    "OrderNote": "<医嘱说明信息>",
    "OrderTypeCode": "医嘱类别代码 ",
    "OrderTypeName": "医嘱类别名称如：长期（在院）；临时（在院）；出院带药……",
    "OrderGiven": {
        "OrderDate": "医嘱下达日期时间",
        "DepartmentIdentifier": "医嘱下达科室代码",
        "DepartmentName": "医嘱下达科室名称",
        "PractitionerIdentifier": "<医嘱下达医生的工号>",
        "PractitionerName": "<医嘱下达医生姓名>"
    },
    "OrderStop": {
        "OrderDate": "医嘱停止日期时间",
        "DepartmentIdentifier": "医嘱停止科室代码",
        "DepartmentName": "医嘱停止科室名称",
        "PractitionerIdentifier": "<医嘱停止医生的工号>",
        "PractitionerName": "<医嘱停止医生姓名>"
    },
    "EncounterId": "就诊流水号",
    "EncounterTypeCode": "就诊类别代码",
    "EncounterTypeText": "就诊类别代码名称 如：普通门诊；专科门诊；专家门诊；特需门诊；急诊……",
    "EncounterWardCode": "患者当前所在病区的代码",
    "EncounterWardName": "患者当前所在病区的名称",
    "DiseaseDiagnosisCode": "诊断代码",
    "DiseaseDiagnosisCodeDic": "诊断代码字典  1.ICD-10 2.GB/T 15657-1995 3.GB/T 16751.1-1997",
    "DiseaseDiagnosis": "诊断名称",
    "DiseaseDiagnosisText": "诊断说明",
    "CardId": "卡号",
    "CardType": "卡类型 记账代码",
    "PatientId": "<患者的系统id 主键>",
    "PatientIdentifier": "<患者的身份标识>",
    "PatientName": "<患者姓名>",
    "OrderItemDetails": [
        {
            "GroupNumber": "<医嘱组号>",
            "SortNumber": "<排序号 数据展现时按此序号进行排列。>",
            "OrderItemTypeCode": "医嘱明细项目类别代码",
            "OrderItemTypeName": "医嘱明细项目类别名称 如：西药、中草药、中成药、检验、医用材料、诊疗",
            "OrderNote": "<对下达医嘱项目的补充说明和注意事项提示>",
            "OrderCheck": {
                "OrderDate": "医嘱核对日期时间",
                "PractitionerIdentifier": "<医嘱核对护士的工号>",
                "PractitionerName": "<医嘱核对护士的姓名>"
            },
            "OrderAuth": {
                "OrderDate": "医嘱审核日期时间 主要针对用药类医嘱 在药房配药环节进行审核",
                "PractitionerIdentifier": "<医嘱审核医生的工号>",
                "PractitionerName": "<医嘱审核医生姓名>"
            },
            "OrderPerf": {
                "OrderDate": "医嘱执行日期时间",
                "DepartmentIdentifier": "医嘱执行科室代码",
                "DepartmentName": "医嘱执行科室名称",
                "PractitionerIdentifier": "<医嘱执行者的工号>",
                "PractitionerName": "<医嘱执行者的姓名>"
            },
            "Service": {
                "ServiceItemName": "<项目名称 非药品医嘱应填>",
                "HospitalServiceItemCode": "<项目代码（医院内部编码） 非药品医嘱应填>",
                "ServiceItemCodeOfPriceBureau": "<项目代码（物价局编码）>",
                "ServiceItemCodeOfMedicalInsurance": "<项目代码（医保编码）>",
                "ApplicationFormNo": "<电子申请单编号非药品医嘱应填 >"
            },
            "Drug": {
                "DrugFlag": "<是否药品 0：否；1：是。 >",
                "DrugName": "<药物名称 药品医嘱应填 >",
                "HospitalDrugCode": "<药物代码（医院内部编码） >",
                "DrugCodeOfMedicalInsurance": "<药物代码（医保编码） >",
                "DrugCodeOfFoodAndDrugAdministration": "<药物代码（药监局编码） >",
                "DrugCodeOfProvincialDrugPurchase": "<药物代码（省药品采购编码） >",
                "DrugType": "<药物剂型 参见CV08.50.002 药物剂型代码表 >",
                "DrugStrength": "<药品规格 如果是药品，则“药品规格”必填，如果是非药品，则“药品规格”必须为空 >",
                "DrugRouteCode": "<药物使用途径代码  参见CV06.00.102用药途径代码>",
                "DrugRouteText": "<药物使用途径名称 >",
                "DrugUseDays": "<用药天数 >",
                "DrugUseFrequency": "<药物使用频率代码 如bid、tid >",
                "DrugUseFrequencyName": "<药物使用频率名称 >",
                "DrugFormCode": "<药物剂型代码  参见CV08.50.002   药物剂型代码表>",
                "DrugFormName": "<药物剂型名称 >",
                "SingleDose": "<单次使用药物的剂量 >",
                "DosageUnit": "<“片、支、粒、毫升”等单位 >",
                "TotalDosage": "<药物使用的总剂量，不必与次剂量和频度形成数额计算的逻辑一致性关系 >",
                "TotalDosageUnit": "<“片、支、粒、毫升、包、盒”等单位 >",
                "TCMTypeCode": "<中药使用类别代码 根据CV06.00.101 中药使用类别代码表>",
                "TCMSign": "<是否中药饮片 0 否 1 是>",
                "TCMOrderDetail": "<中药饮片处方>",
                "TCMCount": "<中药饮片剂数(剂)>",
                "TCMDecoctionMethod": "<中药饮片煎煮法>",
                "TCMRouteCode": "<中药用药方法>",
                "TCMDosageNo": "<中药剂数>"
            }
        }
    ]
}
```


* 挂号类型, 取值范围在值域 `http://moh.gov/fhir/appointmenttype` 中:

| 编码       | 描述 |
|:-----------|:-----|
| expert     | 专家 |
| department | 专科 |
| disease    | 专病 |
| special    | 特需 |
| normal     | 普通 |



2、 Order example/处方信息字段说明

```

```
curl -k -H 'Accept: application/json' \
	   -H 'Accept-Language: en_US' \
	   -u "EOJ2S-Z6OoN_le_KS1d75wsZ6y0SFdVsY9183IvxFyZp:EClusMEUk8e9ihI7ZdVLF5cZ6y0SFdVsY9183IvxFyZp" \
	   -d "response_type=token" \
	   -d "grant_type=client_credentials" \

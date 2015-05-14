1、处方信息字段说明

```
{
    "PrescriptionID": "处方流水号 主键",
    "PrescriptionNumber": "处方编号",
    "PrescriptionDate": "开方日期时间",
    "PrescriptionType": "处方类型 1.普通处方 2.急诊处方 3.儿科处方 4.毒麻处方 5.精神药品处方 9.其他",
    "PrescriptionNote": "<处方备注信息>",
    "PrescriptionDays": "<处方有效天数>",
    "PrescriptionMoney": "<处方金额>",
    "EncounterId": "就诊流水号",
    "EncounterTypeCode": "就诊类别代码",
    "EncounterTypeText": "就诊类别代码名称 如：普通门诊；专科门诊；专家门诊；特需门诊；急诊……",
    "DiseaseDiagnosisCode": "诊断代码",
    "DiseaseDiagnosisCodeDic": "诊断代码字典  1.ICD-10 2.GB/T 15657-1995 3.GB/T 16751.1-1997",
    "DiseaseDiagnosis": "诊断名称",
    "DiseaseDiagnosisText": "诊断说明",
    "CardId": "卡号",
    "CardType": "卡类型 记账代码",
    "PatientId": "<患者的系统id 主键>",
    "PatientIdentifier": "<患者的身份标识>",
    "PatientName": "<患者姓名>",
    "PrescriptionGiven": {
        "OrderDate": "处方下达日期时间",
        "DepartmentIdentifier": "处方下达科室代码",
        "DepartmentName": "处方下达科室名称",
        "PractitionerIdentifier": "<处方下达医生的工号>",
        "PractitionerName": "<处方下达医生姓名>"
    },
    "PrescriptionStop": {
        "OrderDate": "处方停止日期时间",
        "DepartmentIdentifier": "处方停止科室代码",
        "DepartmentName": "处方停止科室名称",
        "PractitionerIdentifier": "<处方停止医生的工号>",
        "PractitionerName": "<处方停止医生姓名>"
    },
    "PrescriptionDetails": [
        {
            "SortNumber": "<顺序号>",
            "GroupNo": "<处方组号 由系统从1开始根据自然递增的原则赋予每条新增医嘱的顺序号，用于标识大输液配液分组>",
            "Note": "<对下达医嘱的补充说明和注意事项提示>",
            "ItemTypeCode": "处方明细项目类别代码",
            "ItemTypeName": "处方明细项目类别名称 如：西药、中草药、中成药",
            "PresCheck": {
                "Date": "处方核对日期时间",
                "PractitionerIdentifier": "<处方核对护士的工号>",
                "PractitionerName": "<处方核对护士的姓名>"
            },
            "PresAuth": {
                "Date": "处方审核日期时间 主要针对用药类医嘱 在药房配药环节进行审核",
                "PractitionerIdentifier": "<处方审核医生的工号>",
                "PractitionerName": "<处方审核医生姓名>"
            },
            "PresPerf": {
                "Date": "处方执行日期时间",
                "DepartmentIdentifier": "处方执行科室代码",
                "DepartmentName": "处方执行科室名称",
                "PractitionerIdentifier": "<处方执行者的工号>",
                "PractitionerName": "<处方执行者的姓名>"
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



2、 Prescription example/处方信息字段说明

```
{
    "PrescriptionID": "处方流水号 主键",
    "PrescriptionNumber": "处方编号",
    "PrescriptionDate": "开方日期时间",
    "PrescriptionType": "处方类型 1.普通处方 2.急诊处方 3.儿科处方 4.毒麻处方 5.精神药品处方 9.其他",
    "PrescriptionNote": "<处方备注信息>",
    "PrescriptionDays": "<处方有效天数>",
    "PrescriptionMoney": "<处方金额>",
    "EncounterId": "就诊流水号",
    "EncounterTypeCode": "就诊类别代码",
    "EncounterTypeText": "就诊类别代码名称 如：普通门诊；专科门诊；专家门诊；特需门诊；急诊……",
    "DiseaseDiagnosisCode": "诊断代码",
    "DiseaseDiagnosisCodeDic": "诊断代码字典  1.ICD-10 2.GB/T 15657-1995 3.GB/T 16751.1-1997",
    "DiseaseDiagnosis": "诊断名称",
    "DiseaseDiagnosisText": "诊断说明",
    "CardId": "卡号",
    "CardType": "卡类型 记账代码",
    "PatientId": "<患者的系统id 主键>",
    "PatientIdentifier": "<患者的身份标识>",
    "PatientName": "<患者姓名>",
    "PrescriptionGiven": {
        "OrderDate": "处方下达日期时间",
        "DepartmentIdentifier": "处方下达科室代码",
        "DepartmentName": "处方下达科室名称",
        "PractitionerIdentifier": "<处方下达医生的工号>",
        "PractitionerName": "<处方下达医生姓名>"
    },
    "PrescriptionStop": {
        "OrderDate": "处方停止日期时间",
        "DepartmentIdentifier": "处方停止科室代码",
        "DepartmentName": "处方停止科室名称",
        "PractitionerIdentifier": "<处方停止医生的工号>",
        "PractitionerName": "<处方停止医生姓名>"
    },
    "PrescriptionDetails": [
        {
            "SortNumber": "<顺序号>",
            "GroupNo": "<处方组号 由系统从1开始根据自然递增的原则赋予每条新增医嘱的顺序号，用于标识大输液配液分组>",
            "Note": "<对下达医嘱的补充说明和注意事项提示>",
            "ItemTypeCode": "处方明细项目类别代码",
            "ItemTypeName": "处方明细项目类别名称 如：西药、中草药、中成药",
            "PresCheck": {
                "Date": "处方核对日期时间",
                "PractitionerIdentifier": "<处方核对护士的工号>",
                "PractitionerName": "<处方核对护士的姓名>"
            },
            "PresAuth": {
                "Date": "处方审核日期时间 主要针对用药类医嘱 在药房配药环节进行审核",
                "PractitionerIdentifier": "<处方审核医生的工号>",
                "PractitionerName": "<处方审核医生姓名>"
            },
            "PresPerf": {
                "Date": "处方执行日期时间",
                "DepartmentIdentifier": "处方执行科室代码",
                "DepartmentName": "处方执行科室名称",
                "PractitionerIdentifier": "<处方执行者的工号>",
                "PractitionerName": "<处方执行者的姓名>"
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
curl -k -H 'Accept: application/json' \
	   -H 'Accept-Language: en_US' \
	   -u "EOJ2S-Z6OoN_le_KS1d75wsZ6y0SFdVsY9183IvxFyZp:EClusMEUk8e9ihI7ZdVLF5cZ6y0SFdVsY9183IvxFyZp" \
	   -d "response_type=token" \
	   -d "grant_type=client_credentials" \

# 病人病历信息查询

## 需求描述

### 需求介绍

病人在查看自己的历史病历、具体病案。

### API列表

| 请求类型 | PATH                                                         | 描述             |
| -------- | ----------------------------------------------------------- | ----------------|
| GET      | /api/healthrecord/case/{user_phone}                          | 查看病人病历（病案集合) |
| GET | /api/healthrecord/case/detail | 查询病人某一具体病案 |



## API文档

### GET   /api/healthrecord/case/{userPhone}    查询病人病历表

#### Request

路由参数

| Key          | Value | Required | Description    |
| ------------ | ----- | -------- | -------------- |
| user_phone    | string  | y        | 病人id          |


#### Response

```json
{
	"st": 0,
	"msg": "",
	"data":[ 
        ["病人姓名1","病案id1","诊断信息1"],
        ["病人姓名1","病案id2","诊断信息2"]     
        ......
     ]
}
```
建议数据库操作：

~~~sql
select patient_name, case_id, info from healthguide_healthrecord_case_record ;
~~~


### GET   /api/healthrecord/case/detail   查询病人某一具体病案

#### Request

查询参数

| Key          | Value | Required | Description    |
| ------------ | ----- | -------- | -------------- |
| userPhone    | string  | y        | 病人id              |
| caseId    | long | y        | 病案id              |


#### Response

```json
{
	"st": 0,
	"msg": "",
	"data":[ 
        ["病人姓名","病案id","病案创造时间","所在医院信息",
"负责医生姓名","负责医生联系方式"......"诊断信息"]
     ]
}
```
建议数据库操作：

~~~sql
select * from healthguide_healthrecord_case_record  
where case_id=xxx
;
~~~

## 数据表

**healthguide_healthrecord_case_record**
存储所有病案信息

~~~sql
create table `healthguide_healthrecord_case_record` (
    `case_id`       bigint      not null    comment '病案ID' primary key auto_increment,
    `patient_phone` varchar(40) not null    comment '用户ID',
    `hospital`      varchar(60) not null    comment '所在医院信息',
    `doctor_name`   varchar(40) not null    comment '负责医生姓名',
    `doctor_phone`  varchar(40) not null    comment '负责医生联系方式',
    `case_result`   varchar(80) not null    comment '病案诊断信息',
    `cost`          decimal(10,2) not null  comment '诊断费用',
    `medicine`      varchar(40) not null    comment '推荐药品名称',
    `create_time`   datetime  default current_timestamp   not null    comment '病案创建时间'
)  engine=InnoDB default charset=utf8mb4 comment='所有病人的所有病案信息';
insert into healthguide_healthrecord_case (patient_phone, hospital, doctor_name, doctor_phone, case_result, cost, medicine) values 
('18888888888', '浙一', '赵嘉成','18888988888','儿科记录', 20.00, '阿莫西林'),
('18888888888', '浙一', '李右','18888988889','外科记录', 20.00, '阿莫西林');

select * from healthguide_healthrecord_case;
drop table healthguide_healthrecord_case;
~~~



## 测试样例（by后端）

### **GET**/api/healthrecord/case/{userPhone}

查询成功：

```json
userphone = '18888888888'

Response body
{
  "st": 0,
  "msg": "",
  "data": {
    "caseList": [
      {
        "patientName": "卢本伟",
        "caseId": 1,
        "caseResult": "儿科记录"
      },
      {
        "patientName": "卢本伟",
        "caseId": 2,
        "caseResult": "外科记录"
      }
    ]
  }
}
```

未查询到，数据不一致：

```json
userphone = '？'
	
Response body
{
  "st": 1,
  "msg": "数据不一致",
  "data": null
}
```



### **GET**/api/healthrecord/case/detail

查询成功：

```json
userphone = '18888888888'
caseId = 1

Response body
{
  "st": 0,
  "msg": "",
  "data": {
    "caseId": 1,
    "patientPhone": "18888888888",
    "hospital": "浙一",
    "doctorName": "赵嘉成",
    "doctorPhone": "18888988888",
    "caseResult": "儿科记录",
    "cost": 20,
    "medicine": "阿莫西林",
    "createTime": 1619167737
  }
}
```

未查询到，数据不一致：

```json
userphone = '？'
caseId = 1

Response body
{
  "st": 1,
  "msg": "数据不一致",
  "data": null
}
```


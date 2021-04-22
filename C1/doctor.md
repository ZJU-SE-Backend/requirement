# 医生查看挂号列表

## 需求描述

### 需求介绍

医生查看病人预约挂的号，展示挂号列表，列表里面显示病人的详细信息。

### API列表

| 请求类型 | PATH                                                         | 描述             |
| -------- | ------------------------------------------------------------ | ---------------- |
| GET      | /api/appoinment/doctor/{doctor_id}register_list              | 查看病人挂号列表 |
| GET      | /api/appoinment/doctor/{patient_id}/infomation?appoint_date=xxxx | 查看病人详细信息 |

## API文档

### GET  /api/appoinment/doctor/register_list 查看病人挂号列表  

#### Request

URL参数

| Key          | Value | Required | Description    |
| ------------ | ----- | -------- | -------------- |
| doctor_id    | long  | y        | 医生id         |
| appoint_date | int   | y        | 预约时间段编号 |

#### Response

```json
{
	"st": 0,
	"msg": "",
	"data":[
        ["病人id","病人电话","挂科室","预约时间"]
        #如：["123456","18888888888","口腔科","2021/4/21"]
     ]
}
```

#### 

### GET  /api/appoinment/doctor/patient_info  查看病人详细信息      

#### Request

查询参数

| Key        | Value | Required | Description        |
| ---------- | ----- | -------- | ------------------ |
| patient_id | long  | y        | 病人id，即就诊卡id |

#### Response

```json
{
	"st": 0,
	"msg": "",
	"data":[
        ["病人id","病人电话","挂科室","预约时间""历史看病记录"]
        #如：["123456","18888888888","口腔科","2021/4/21","一个看病列表"]
     ]
}
```

## 数据表

**复用预约模块的预约记录表healthguide_appointment_register_record来筛选医生对应时间的预约记录**

# 医生查看挂号列表

## 需求描述

### 需求介绍

医生查看病人预约挂的号，展示挂号列表，列表里面显示病人的详细信息。

### API列表

| 请求类型 | PATH                                             | 描述                   |
| -------- | ------------------------------------------------ | ---------------------- |
| GET      | /api/appoinment/doctor/appointList/{doctorPhone} | 查看病人挂号列表       |
| GET      | /api/appoinment/doctor/{patientPhone}            | 查看病人历史病案       |
| GET      | /api/appoinment/doctor/department/{patientPhone} | 查看病人在某科室的病案 |

## API文档

### GET  /api/appoinment/{doctorId}/appointList 查看病人挂号列表  

#### Request

路由参数

| Key         | Value  | Required | Description |
| ----------- | ------ | -------- | ----------- |
| doctorPhone | string | y        | 医生id      |

查询参数

| Key         | Value | Required | Description |
| ----------- | ----- | -------- | ----------- |
| appointDate | long  | y        | 预约日期    |

#### Response 

```json
{
	"st": 0,
	"msg": "",
	"data":[
        ["病人姓名","病人电话","挂科室","预约时间","预约时间段"]
    ]
}
```



### GET  /api/appoinment/doctor/{patientPhone} 查看病人历史病案

#### Request

URL参数
| Key          | Value  | Required | Description |
| ------------ | ------ | -------- | ----------- |
| patientPhone | string | y        | 病人id      |


#### Response

```
{
	"st": 0,
	"msg": "",
	"data":{
        "病人姓名","病人电话"，
        "历史病案记录"：
        [{病案id，病案时间，医生姓名，科室，医生电话，诊断信息}]
    }
}
```



### GET  /api/appoinment/doctor/department/{patientPhone} 查看病人历史病案

#### Request

URL参数

| Key          | Value  | Required | Description |
| ------------ | ------ | -------- | ----------- |
| patientPhone | string | y        | 病人id      |

查询参数

| Key        | Value  | Required | Description |
| ---------- | ------ | -------- | ----------- |
| department | string | n        | 科室        |


#### Response

```
{
	"st": 0,
	"msg": "",
	"data":{
        "病人姓名","病人电话"，"科室",
        "历史病案记录"：
        [{病案id，病案时间，医生姓名，医生电话，诊断信息}]
    }
}
```





## 数据表

**复用预约模块的预约记录表healthguide_appointment_register_record来筛选医生对应时间的预约记录**



## 测试样例（by后端）

### **GET**/api/appointment/doctor/{doctorPhone}/appointList

查询成功：

```json
doctorPhone = '18888988888'
appointDate = 1618934400

esponse body
{
  "st": 0,
  "msg": "",
  "data": {
    "appointList": [
      {
        "patientName": "卢本伟",
        "patientPhone": "18888888888",
        "department": "儿科",
        "appointDate": 1618934400,
        "section": 1
      },
      {
        "patientName": "卢本伟",
        "patientPhone": "18888888888",
        "department": "儿科",
        "appointDate": 1618934400,
        "section": 3
      }
    ]
  }
}
```

未查询到数据：

```json
doctorPhone = ''
appointDate = 1618934400

esponse body
{
  "st": 0,
  "msg": "",
  "data": {
    "appointList": []
  }
}
```


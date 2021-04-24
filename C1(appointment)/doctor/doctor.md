# 医生查看挂号列表

## 需求描述

### 需求介绍

医生查看病人预约挂的号，展示挂号列表，列表里面显示病人的详细信息。

### API列表

| 请求类型 | PATH                                             | 描述             |
| -------- | ------------------------------------------------ | ---------------- |
| GET      | /api/appoinment/doctor/{doctorPhone}/appointList | 查看病人挂号列表 |

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


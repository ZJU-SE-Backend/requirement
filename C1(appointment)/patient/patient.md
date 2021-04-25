## 需求描述


### 需求介绍

病人预约挂号界面，包括用户个人信息表的查询，预约记录表的查询和增改。

### API列表

| 请求类型  | PATH            | 描述               |
| -------- | ----------------- | -------------------- |
| GET      | /api/appoinment/register/hospital | 查询医院列表  |
| GET      | /api/appoinment/register/department/{hospital} | 查询某医院科室列表	|
| GET      | /api/appoinment/register/doctorList | 查询某科室的医生列表   |
| GET      | /api/appoinment/register/appoint/doctor | 查询某医生预约数量  |
| POST     | /api/appoinment/register/appoint | 新增预约     |
| GET      | /api/appoinment/register/appoint/{userPhone} | 查询预约信息  |
| DELETE | /api/appoinment/withdraw/appoint | 删除具体某条预约记录 |


## API文档

### GET  /api/appoinment/register/hospital  查询医院列表

#### Request

无参数

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data":[
		"浙江大学医学院附属第一医院",
		"浙江大学医学院附属第二医院",
		"浙江大学医学院附属邵逸夫医院",
		"杭州市第一人民医院"
	]
}
~~~

; healthguide_appointment_register_hospital 表存放医院、科室、医生信息

### GET  /api/appoinment/register/department/{hospital}  查询某医院科室列表

#### Request
**路由参数**

| Key | Value | Required | Description |
| ----- | ------- | ---------- | ------------- |
| hospital     | string    | y       | 医院名   |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data":[
		"儿科",
		"妇科",
		"外科",
		"精神科",
		"..."
	]
}
~~~

### GET  /api/appoinment/register/doctorList  查询某科室的医生列表 

#### Request
**查询参数 Query Parames**

| Key | Value | Required | Description |
| ----- | ------- | ---------- | ------------- |
| hospital | string | y | 医院 |
| department | string | y | 科室 |


#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data":
    "doctorList":[
		{"医生姓名":"赵嘉成","医生id":"18888988888","医生相关描述":"主任医师，擅长儿科的诊治……"},
		...
	]
}
~~~



### POST  /api/appoinment/register/appoint  新增预约

**数据 Data**

| Key | Value | Required | Description |
| ----- | ------- | ---------- | ------------- |
| patientPhone  | string    | y/n     | 病人id（如果后台能根据header自动确定则这个不用）   |
| appointDate | long | y		| 预约日期	|
| section | int   | y        | 预约时间段编号 |
| doctorPhone    | string  | y       | 医生id |
(目前暂定预约记录展示这些信息，根据后续开发确定是否需要添加新的字段)
（预约时间段编号对应了不同的时间段，在前端会进行针对性的解释翻译）

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data":
    {
        "result":true
    }
}
~~~

~~~json
{
	"st": 0,
	"msg": "",
	"data":
    {
        "result":false
    }
}
~~~


默认在执行此操作的时候，前端已经通过前面获取的后台数据确定了操作的合法性。



### GET   /api/appoinment/register/appoint/{userPhone}   查询预约信息

#### Request
**路由参数 ：**

| Key | Value | Required | Description |
| ----- | ------- | ---------- | ------------- |
| userPhone   | string    | y/n     | 用户id （如果后台能根据header自动确定则这个不用）   |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data":[
		["杭州市第一人民医院","1618934400","1","儿科","妙手回春哥","18888888888"],
		["..."],
		["医院","日期","时间段"，"科室","预约医生姓名","医生电话"]
	]
}
~~~



### GET  /api/appoinment/register/appoint/doctor  查询某医生预约数量

#### Request
**查询参数 Query Parames**

| Key | Value | Required | Description |
| ----- | ------- | ---------- | ------------- |
| doctorPhone | string    | y/n     | 医生ID  |
| appointDate | long   | y       |预约日期  |
| section | int | y |预约时间段 |
#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data":{
        "appointNum":
    }
}
~~~

### DELETE  /api/appoinment/withdraw/appoint  删除具体某条预约记录

**查询参数Query Params**

| Key          | Value  | Required | Description                                      |
| ------------ | ------ | -------- | ------------------------------------------------ |
| appointDate  | long   | y        | 预约日期                                         |
| section      | int    | y        | 预约时段                                         |
| patientPhone | string | y/n      | 病人id（如果后台能根据header自动确定则这个不用） |

（预约时间段编号对应了不同的时间段，在前端会进行针对性的解释翻译）

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": null
}
~~~

~~~json
{
	"st": 1,
	"msg": "数据不一致",
	"data": null
}
~~~



## 数据表

**healthguide_appointment_register_hospital**

```sql
create table `healthguide_appointment_register_hospital` (
	`hospital` varchar(60) not null comment '医院名',
	`department` varchar(40) not null comment '医院科室',
    primary key (`hospital`,`department`)
) engine=InnoDB default charset=utf8mb4 comment='医院科室信息';

insert into healthguide_appointment_register_hospital (hospital, department) values 
('浙一','儿科'),
('浙一','妇科'),
('浙一','外科'),
('浙二','儿科'),
('浙二','内科');

drop table healthguide_appointment_register_hospital;
select * from healthguide_appointment_register_hospital;
```

**healthguide_appointment_register_doctor**
存储医院、每一个科室，科室中医生id，医生的信息这些内容

~~~sql
create table `healthguide_appointment_register_doctor` (
    `doctor_phone` varchar(40) not null comment '医生id' primary key,
    `doctor_name` varchar(40) not null comment '医生姓名',
	`hospital` varchar(60) not null comment '医院名',
	`department` varchar(40) not null comment '医院科室',
	`doctor_info` varchar(255) not null comment '医生信息'
) engine=InnoDB default charset=utf8mb4 comment='医生信息';

insert into healthguide_appointment_register_doctor (doctor_phone, doctor_name, hospital, department, doctor_info)values
('18888988888', '赵嘉成', '浙一', '儿科', '主任医师，擅长儿科'),
('18888988887', 'xxx', '浙一', '儿科', '副主任医师，擅长儿科'),
('18888988889', '李右', '浙一','外科', '副主任医师');

drop table healthguide_appointment_register_doctor;
select  * from healthguide_appointment_register_doctor;
~~~

**healthguide_appointment_register_appoint**
每一个医生不同时间段都有预约余量

~~~sql
create table `healthguide_appointment_register_appoint` (
	`appoint_id` bigint not null comment '序列号' primary key auto_increment,
	`patient_phone` varchar(40) not null comment '病人ID',
	`doctor_phone` varchar(40) not null comment '医生ID',
    `doctor_name` varchar(40) not null comment '医生姓名',
	`department` varchar(40) not null comment '科室',
	`hospital` varchar(40) not null comment '医院',
    `appoint_date` date not null comment '预约的日期',
	`section` int not null comment '预约的时段'
) engine=InnoDB default charset=utf8mb4 comment='医院预约记录';

insert into healthguide_appointment_register_appoint (patient_phone, doctor_phone, doctor_name, department, hospital, appoint_date, section) values
('18888888888', '18888988888', '赵嘉成', '儿科', '浙一','2021-4-21',1),
('18888888888', '18888988888', '赵嘉成', '儿科', '浙一','2021-4-21',3),
('10000000000', '18888988889', '李右', '外科', '浙一','2021-4-21',2);

select * from healthguide_appointment_register_appoint;
drop table healthguide_appointment_register_appoint;
~~~



## 测试样例（by后端）

### **GET**/api/appointment/register/hospital

```json
Response body
{
  "st": 0,
  "msg": "",
  "data": {
    "hospitalList": [
      "浙一",
      "浙二"
    ]
  }
}
```



### **GET**/api/appointment/register/department/{hospital}

获取信息：

```json
hospital = '浙一'
	
Response body
{
  "st": 0,
  "msg": "",
  "data": {
    "departmentList": [
      "儿科",
      "外科",
      "妇科"
    ]
  }
}
```

没有相关数据：

```json
hospital = '浙?'

Response body
{
  "st": 0,
  "msg": "",
  "data": {
    "departmentList": []
  }
}
```



### **GET**/api/appointment/register/doctorList

获取信息：

```json
hospital = '浙一'
department = '外科'
	
Response body
{
  "st": 0,
  "msg": "",
  "data": {
    "doctorList": [
      {
        "doctorName": "xxx",
        "doctorPhone": "18888988887",
        "doctorInfo": "副主任医师，擅长儿科"
      },
      {
        "doctorName": "赵嘉成",
        "doctorPhone": "18888988888",
        "doctorInfo": "主任医师，擅长儿科"
      }
    ]
  }
}
```

没有相关数据：

```json
hospital = '浙一'
department = '妇科'

	
Response body
{
  "st": 0,
  "msg": "",
  "data": {
    "departmentList": []
  }
}
```



### **POST**/api/appointment/register/appoint

预约成功，数据库更新成功：

```json
Request body
{
  "patientPhone": "10000000000",
  "doctorPhone": "18888988888",
  "appointDate": 1618934400,
  "section": 2
}
	
Response body
{
  "st": 0,
  "msg": "",
  "data": {
    "result": true
  }
}
```

不存在该医生：

```json
Request body
{
  "patientPhone": "10000000000",
  "doctorPhone": "18888988808",
  "appointDate": 1618934400,
  "section": 2
}
	
Response body
{
  "st": 1,
  "msg": "数据不一致",
  "data": null
}
```



### **GET**/api/appointment/register/appoint/{userPhone}

获取信息：

```json
userPhone = '18888888888'
	
Response body
{
  "st": 0,
  "msg": "",
  "data": {
    "appointList": [
      {
        "hospital": "浙一",
        "appointDate": 1618934400,
        "section": 1,
        "department": "儿科",
        "doctorName": "赵嘉成",
        "doctorPhone": "18888988888"
      },
      {
        "hospital": "浙一",
        "appointDate": 1618934400,
        "section": 3,
        "department": "儿科",
        "doctorName": "赵嘉成",
        "doctorPhone": "18888988888"
      }
    ]
  }
}
```

没有相关数据：

```json
hospital = '?'

Response body
{
  "st": 0,
  "msg": "",
  "data": {
    "appointList": []
  }
}
```



### **GET**/api/appointment/register/appoint/doctor

获取信息：

```json
doctorPhone = '18888988888'
appointDate = 1618934400
section = 1
	
Response body
{
  "st": 0,
  "msg": "",
  "data": {
    "appointNum": 1
  }
}
```

没有相关数据：

```json
doctorPhone = '18888988888'
appointDate = 1618934400
section = 0

Response body
{
  "st": 0,
  "msg": "",
  "data": {
    "appointNum": 0
  }
}
```

### 测试样例（by后端）

### **DELETE**/api/appointment/withdraw/appoint

删除成功，数据库更新成功

```json
Request body
{
  "patientPhone": "18888888888",
  "appointDate": 1618934400,
  "section": 3
}

Response body
{
  "st": 0,
  "msg": "",
  "data": null
}
```

未有相关信息：

```json
Request body
{
  "patientPhone": "18888888888",
  "appointDate": 1618934400,
  "section": 0
}

Response body
{
  "st": 1,
  "msg": "数据不一致",
  "data": null
}
```


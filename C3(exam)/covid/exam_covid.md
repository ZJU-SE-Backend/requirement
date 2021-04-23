## 需求描述


### 需求介绍

核酸检测预约的即时余量和预约信息。

### API列表

| 请求类型  | PATH            | 描述               |
| -------- | ----------------- | -------------------- |
| GET      | /api/exam/covid/hospital | 查询医院列表  |
| GET | /api/exam/covid/remainder | 查询预约余量   |
| POST     | /api/exam/covid/appointment | 新增预约     |
| GET      | /api/exam/covid/appointment/{userPhone} | 查询预约信息  |
| GET      | /api/exam/covid/report/{appointId}    | 获取体检报告（实现形式待定）  |
| GET | /api/exam/covid/setting   | 查询余量设置（可迟点实现）  |
| PUT      | /api/exam/covid/setting   | 修改余量设置（可迟点实现）  |



## API文档

### GET  /api/exam/covid/hospital  查询医院列表

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

建议数据库操作：
~~~sql
select distinct hospital from healthguide_exam_covid_capacity;
~~~


### GET  /api/exam/covid/remainder  查询预约余量 

#### Request
**查询参数 Query Parames**

| Key | Value | Required | Description |
| ----- | ------- | ---------- | ------------- |
| hospital     | string    | y       | 医院名   |
| appointDate | long   | y       | 期望预约的日期 |


#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data":{
		"sections": [150,150,0,0,0,150,0,0],
	}
}
~~~

建议数据库操作：
~~~sql
select section, remainder from healthguide_exam_covid_remainder
where hospital=XXX, appoint_date=XXX;
~~~


### POST  /api/exam/covid/appointment  新增预约

**数据 Data**

| Key | Value | Required | Description |
| ----- | ------- | ---------- | ------------- |
| userPhone   | string    | y/n     | 用户id（如果后台能根据header自动确定则这个不用）   |
| hospital     | string    | y       | 医院名   |
| appointDate | long   | y       | 期望预约的日期 |
| section      | int       | y       | 预约时段 |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data":
    {
        "result": true
    }
}
~~~

~~~json
{
	"st": 0,
	"msg": "",
	"data":
    {
        "result": false
    }
}
~~~

数据库操作：
检验余量是否大于0，若是则插入预约信息，并更新余量

### GET   /api/exam/covid/appointment/{userPhone}  查询预约信息

#### Request
**路由参数**

| Key | Value | Required | Description |
| ----- | ------- | ---------- | ------------- |
| userPhone   | string    | y/n     | 用户id （如果后台能根据header自动确定则这个不用）   |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data":[
		{1234, "杭州市第一人民医院","2020-4-20", 2},
		{"..."},
		{"预约号","医院","日期","时段}
	]
}
~~~

建议数据库操作：
~~~sql
select appoint_id, hospital, appoint_date, section
from healthguide_exam_covid_appointment
where user_phone=XXX;
~~~
（最好能按appoint_id做排序，大的在前面）


### GET  /api/exam/covid/report/{appointId}  获取核酸检测报告（实现形式待定）

#### Request
**路由参数**

| Key | Value | Required | Description |
| ----- | ------- | ---------- | ------------- |
| appointId  | int    | y     | 预约号  |

### Response
实现形式待定


### GET  /api/exam/covid/setting  查询余量设置（管理端）

#### Request
**查询参数 Query Parames**

| Key | Value | Required | Description |
| ----- | ------- | ---------- | ------------- |
| hospital   | string    | y/n     | 查询的医院（如果可以自动确定则不用）  |
| appointDate | int     | y       | 查询的日期 |


#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data":{
		"defaultCapacity": 35,
		"remainder":[0,1,...,20]
	}
}
~~~


建议数据库操作：
~~~sql
select default_capacity from healthguide_exam_covid_capacity;
select section, remainder from healthguide_exam_covid_remainder
where hospital=XXX, appoint_date=XXX;
~~~

### PUT  /api/exam/covid/setting  修改余量设置（管理端）

**数据 Data**

| Key | Value | Required | Description |
| ----- | ------- | ---------- | ------------- |
| hospital   | string    | y/n     | 医院（如果可以自动确定则不用）  |
| type | int | y | 修改方式（0为方式一，其他为方式二） |
| defaultCapacity | int | n       | 修改默认容量（方式一）   |
| appointDate | int       | n       | 修改特定特定日期时段的余量（方式二） |
| section      | int       | n       | 修改特定特定日期时段的余量（方式二） |
| remainder    | int       | n       | 修改特定特定日期时段的余量（方式二） |


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

建议数据库操作：
方式一是修改healthguide_exam_covid_capacity中的default_capacity。
~~~sql
update healthguide_exam_covid_capacity set default_capacity=XXX where hospital=XXX;
~~~
但是修改后，healthguide_exam_covid_remainder中的remainder也要做修改，因为最大容量改变了，实时余量也要改变（比如容量从35变成30，则每个时段的余量都要-5。负数没有关系）

方式二是修改ealthguide_exam_covid_remainder中的某一项。
~~~sql
update healthguide_exam_covid_remainder set remainder=XXX
where hospital=XXX, appoint_date=XXX, section=XXX;
~~~



## 数据表相关（by参考之后）

**healthguide_exam_covid_capacity**
设置每个医院的时段容量默认值。一个医院对应一个值（如杭州市第一人民医院-35表示该医院每个时段默认容纳35个预约）

~~~sql
create table `healthguide_exam_covid_capacity` (
	`hospital` varchar(60) not null comment '医院名' primary key,
	`default_capacity` int not null comment '医院各时段默认容量',
) engine=InnoDB default charset=utf8mb4 comment='各医院各时段默认容量';

insert into healthguide_exam_covid_capacity value
("浙江大学医学院附属第一医院", 30),
("浙江大学医学院附属第二医院", 30),
("浙江大学医学院附属邵逸夫医院", 30),
("杭州市第一人民医院", 30);

select * from healthguide_exam_covid_capacity;
drop table `healthguide_exam_covid_capacity`;
~~~

**healthguide_exam_covid_remainder**
设置每个医院每天各个时段的实时余量。一个医院一天提供8个时段，用整数1到8对应。系统提供15天内的预约，故一个医院对应的完整项数是120项，且需要每天更新（把前一天的丢弃，然后插入新的一天的数据）。或许也可以采用on demand的策略，即等到前端获取表项的时候，发现没有对应表项，这时才插入所需表项。插入的新表项的余量应该等于healthguide_exam_covid_capacity的默认容量。

~~~sql
create table `healthguide_exam_covid_remainder` (
	`hospital` varchar(60) not null comment '医院名',
	`appoint_date` date not null comment '余量对应的日期',
	`section` int not null comment '一天当中的时段（从1到8）',
	`remainder` int not null comment '实时余量',
	primary key (`hospital`,`appoint_date`,`section`)
) engine=InnoDB default charset=utf8mb4 comment='各医院各时段实时余量';

insert into healthguide_exam_covid_remainder values
('浙江大学医学院附属第一医院','2021-4-21',1,0),
('浙江大学医学院附属第一医院','2021-4-21',2,3),
('浙江大学医学院附属第一医院','2021-4-21',3,6),
('浙江大学医学院附属第一医院','2021-4-21',4,9),
('浙江大学医学院附属第一医院','2021-4-21',5,12),
('浙江大学医学院附属第一医院','2021-4-21',6,15),
('浙江大学医学院附属第一医院','2021-4-21',7,18),
('浙江大学医学院附属第一医院','2021-4-21',8,21);

select * from healthguide_exam_covid_remainder;
drop table `healthguide_exam_covid_remainder`;
~~~

**healthguide_exam_covid_appointment**
预约信息。

~~~sql
create table `healthguide_exam_covid_appointment` (
	`appoint_id` bigint not null comment '序列号' primary key auto_increment,
	`user_phone` varchar(40) not null comment '用户电话',
	`hospital` varchar(60) not null comment '医院名',
	`appoint_date` date not null comment '预约的日期',
	`section` int not null comment '预约的时段（从1到8）',
	`create_time` datetime not null default current_timestamp comment '记录创建时间',
	key `idx_phone` (`user_phone`)
) engine=InnoDB default charset=utf8mb4 comment='预约信息';

insert into healthguide_exam_covid_appointment (user_phone, hospital, appoint_date, section) values
('18888888888', '浙江大学医学院附属第一医院','2021-4-21',1),
('18888888888', '浙江大学医学院附属第一医院','2021-4-21',3),
('10000000000', '浙江大学医学院附属第一医院','2021-4-21',2);

select * from healthguide_exam_covid_appointment;
drop table `healthguide_exam_covid_appointment`;
~~~

**healthguide_exam_covid_report**（暂未完成）

储存体检报告。

~~~sql
create table `healthguide_exam_covid_report` (
	`appoint_id` bigint not null comment '预约序号' primary key,
	`user_phone` varchar(40) not null comment '用户电话',
	`report` MediumBlob default null comment '报告',
	key `idx_phone` (`user_phone`)
) engine=InnoDB default charset=utf8mb4 comment='检测报告';
~~~



## 测试样例（by后端）

### **GET**/api/exam/covid/hospital

```json
Response body
{
  "st": 0,
  "msg": "",
  "data": {
    "hospitalList": [
      "杭州市第一人民医院",
      "浙江大学医学院附属第一医院",
      "浙江大学医学院附属第二医院",
      "浙江大学医学院附属邵逸夫医院"
    ]
  }
}
```



### **GET**/api/exam/covid/remainder

查询成功：

```json
hospital= "浙江大学医学院附属第一医院",
appointDate= 1618934400
	
Response body
{
  "st": 0,
  "msg": "",
  "data": {
    "sections": [
      0,
      3,
      6,
      9,
      12,
      15,
      18,
      21
    ]
  }
}
```

未查询到：

```json
hospital= "浙江大学医学院附属第一医院",
appointDate= 1618934400
	
Response body
{
  "st": 0,
  "msg": "",
  "data": {
    "sections": []
  }
}
```



### **GET**/api/exam/covid/appointment/{userPhone}

查询成功：

```json
Parameters
userPhone: 18888888888
	
Response body
{
  "st": 0,
  "msg": "",
  "data": {
    "appointments": [
      {
        "appointId": 2,
        "hospital": "浙江大学医学院附属第一医院",
        "appointDate": 1618934400,
        "section": 3
      },
      {
        "appointId": 1,
        "hospital": "浙江大学医学院附属第一医院",
        "appointDate": 1618934400,
        "section": 1
      }
    ]
  }
}
```

未查询到：

```json
Parameters
userPhone: 18888888880
	
Response body
{
  "st": 0,
  "msg": "",
  "data": {
    "appointments": []
  }
}
```



### **POST**/api/exam/covid/appointment

预约成功，数据库新增预约信息，容量-1成功：

```json
Request body
{
  "hospital": "浙江大学医学院附属第一医院",
  "appointDate": 1618934400,
  "section": 6,
  "userPhone": "18888888888"
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

预约失败，容量不足：

```json
Request body
{
  "hospital": "浙江大学医学院附属第一医院",
  "appointDate": 1618934400,
  "section": 1,
  "userPhone": "18888888888"
}

Response body
{
  "st": 0,
  "msg": "",
  "data": {
    "result": false
  }
}
```



### **POST**/api/exam/covid/setting

查询完整：

```json
hospital= "浙江大学医学院附属第一医院",
appointDate= 1618934400

Response body
{
  "st": 0,
  "msg": "",
  "data": {
    "defaultCapacty": 30,
    "sections": [
      0,
      3,
      6,
      9,
      12,
      14,
      18,
      21
    ]
  }
}
```

医院存在，查询不到日期：

```json
hospital= "浙江大学医学院附属第一医院",
appointDate= 161893440

Response body
{
  "st": 0,
  "msg": "",
  "data": {
    "defaultCapacty": 30,
    "sections": []
  }
}
```

医院不存在：

```json
hospital= "浙江大学医学院附属第一医",
appointDate= 1618934400

Response body
{
  "st": 0,
  "msg": "",
  "data": null
}
```



### **PUT**/api/exam/covid/setting

修改容量，容量及余量数据库更新正确：

```json
Request body
{
  "hospital": "浙江大学医学院附属第一医院",
  "type": 0,
  "defaultCapacty": 35,
  "appointDate": 0,
  "section": 0,
  "remainder": 0
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

修改余量，余量数据库更新正确：

```json
Request body
{
  "hospital": "浙江大学医学院附属第一医院",
  "type": 1,
  "defaultCapacty": 35,
  "appointDate": 1618934400,
  "section": 2,
  "remainder": 50
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




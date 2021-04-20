本需求和exam_covid.md的内容是一样的，但模块名不同

## 需求描述


### 需求介绍

体检预约的即时余量和预约信息。

### API列表

| 请求类型  | PATH            | 描述               |
| -------- | ----------------- | -------------------- |
| GET      | /api/exam/physical/hospital | 查询医院列表  |
| GET      | /api/exam/physical/remainder | 查询预约余量   |
| POST     | /api/exam/physical/appointment | 新增预约     |
| GET      | /api/exam/physical/appointment | 查询预约信息  |
| GET      | /api/exam/physical/report    | 获取体检报告（实现形式待定）  |
| GET      | /api/exam/physical/setting   | 查询余量设置（可迟点实现）  |
| PUT      | /api/exam/physical/setting   | 修改余量设置（可迟点实现）  |


## API文档

### GET  /api/exam/physical/hospital  查询医院列表

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
select distinct hospital from healthguide_exam_physical_capacity;
~~~


### GET  /api/exam/physical/remainder  查询预约余量 

#### Request
**查询参数 Query Parames**

| Key | Value | Required | Description |
| ----- | ------- | ---------- | ------------- |
| hospital     | string    | y       | 医院名   |
| appoint_date | date      | y       | 期望预约的日期 |
（date一般用什么样的格式？）

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data":{
		1: 150,
		2: 150,
		3: 150,
		4: 150,
		...
	}
}
~~~

建议数据库操作：
~~~sql
select section, remainder from healthguide_exam_physical_remainder
where hospital=XXX, appoint_date=XXX;
~~~


### POST  /api/exam/physical/appointment  新增预约

**数据 Data**

| Key | Value | Required | Description |
| ----- | ------- | ---------- | ------------- |
| user_phone   | string    | y/n     | 用户id（如果后台能根据header自动确定则这个不用）   | 
| hospital     | string    | y       | 医院名   |
| appoint_date | date      | y       | 期望预约的日期 |
| section      | int       | y       | 预约时段 |

#### Response

~~~json
{
	"st": 0,
	"msg": "成功",
	"data":null
}
~~~

~~~json
{
	"st": 1,
	"msg": "失败",
	"data":null
}
~~~

建议数据库操作：
先看healthguide_exam_physical_remainder中对应的时段余量是否大于0，然后往healthguide_exam_physical_appointment中插入数据（还要加入create_time字段）
~~~sql
insert into healthguide_exam_physical_appointment
(user_phone, hospital, appoint_date, section)
values (XXX);
~~~



### GET   /api/exam/physical/appointment   查询预约信息

#### Request
**查询参数 Query Parames**

| Key | Value | Required | Description |
| ----- | ------- | ---------- | ------------- |
| user_phone   | string    | y/n     | 用户id （如果后台能根据header自动确定则这个不用）   |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data":{
		[1234，"杭州市第一人民医院","2020-4-20", 2, true],
		...,
		[预约号,医院,日期,时段,状态]
	}
}
~~~

建议数据库操作：
~~~sql
select appoint_id, hospital, appoint_date, section, report_status
from healthguide_exam_physical_appointment
where user_phone=XXX;
~~~
（最好能按appoint_id做排序，大的在前面）


### GET  /api/exam/physical/report  获取体检报告（实现形式待定）

#### Request
**查询参数 Query Parames**

| Key | Value | Required | Description |
| ----- | ------- | ---------- | ------------- |
| appoint_id   | int    | y     | 预约号  |

### Response
实现形式待定


### GET  /api/exam/physical/setting  查询余量设置（管理端）

#### Request
**查询参数 Query Parames**

| Key | Value | Required | Description |
| ----- | ------- | ---------- | ------------- |
| hospital   | string    | y/n     | 查询的医院（如果可以自动确定则不用）  |
| appoint_date | date    | y       | 查询的日期 |


#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data":{
		default_capacity: 35,
		remainder:{
			1:30,
			2:28,
			...,
			时段: 余量
		}
	}
}
~~~


建议数据库操作：
~~~sql
select default_capacity from healthguide_exam_physical_capacity;
select section, remainder from healthguide_exam_physical_remainder
where hospital=XXX, appoint_date=XXX;
~~~

### PUT  /api/exam/physical/setting  修改余量设置（管理端）

**数据 Data**

| Key | Value | Required | Description |
| ----- | ------- | ---------- | ------------- |
| hospital   | string    | y/n     | 医院（如果可以自动确定则不用）  |
| default_capacity   | int | n       | 修改默认容量（方式一）   |
| appoint_date | date      | n       | 修改特定特定日期时段的余量（方式二） |
| section      | int       | n       | 修改特定特定日期时段的余量（方式二） |
| remainder    | int       | n       | 修改特定特定日期时段的余量（方式二） |


#### Response

~~~json
{
	"st": 0,
	"msg": "成功",
	"data":null
}
~~~

~~~json
{
	"st": 1,
	"msg": "失败",
	"data":null
}
~~~

建议数据库操作：
方式一是修改healthguide_exam_physical_capacity中的default_capacity。
~~~sql
update healthguide_exam_physical_capacity set default_capacity=XXX where hospital=XXX;
~~~
但是修改后，healthguide_exam_physical_remainder中的remainder也要做修改，因为最大容量改变了，实时余量也要改变（比如容量从35变成30，则每个时段的余量都要-5。负数没有关系）

方式二是修改ealthguide_exam_physical_remainder中的某一项。
~~~sql
update healthguide_exam_physical_remainder set remainder=XXX
where hospital=XXX, appoint_date=XXX, section=XXX;
~~~


## 数据表

**healthguide_exam_physical_capacity**
设置每个医院的时段容量默认值。一个医院对应一个值（如杭州市第一人民医院-35表示该医院每个时段默认容纳35个预约）

~~~sql
create table `healthguide_exam_physical_capacity` (
	`hospital` varchar(60) not null comment '医院名' primary key,
	`default_capacity` int not null comment '医院各时段默认容量',
) engine=InnoDB default charset=utf8mb4 comment='各医院各时段默认容量';

insert into healthguide_exam_physical_capacity value
("浙江大学医学院附属第一医院", 30),
("浙江大学医学院附属第二医院", 30),
("浙江大学医学院附属邵逸夫医院", 30),
("杭州市第一人民医院", 30);
~~~

**healthguide_exam_physical_remainder**
设置每个医院每天各个时段的实时余量。一个医院一天提供8个时段，用整数1到8对应。系统提供15天内的预约，故一个医院对应的完整项数是120项，且需要每天更新（把前一天的丢弃，然后插入新的一天的数据）。或许也可以采用on demand的策略，即等到前端获取表项的时候，发现没有对应表项，这时才插入所需表项。插入的新表项的余量应该等于healthguide_exam_physical_capacity的默认容量。

~~~sql
create table `healthguide_exam_physical_remainder` (
	`hospital` varchar(60) not null comment '医院名',
	`appoint_date` date not null comment '余量对应的日期',
	`section` int not null comment '一天当中的时段（从1到8）',
	`remainder` int not null comment '实时余量',
	primary key (`hospital`,`appoint_date`,`section`)
) engine=InnoDB default charset=utf8mb4 comment='各医院各时段实时余量';
~~~

**healthguide_exam_physical_appointment**
预约信息。表内还要存报告的pdf。
~~~sql
create table `healthguide_exam_physical_appointment` (
	`appoint_id` bigint not null comment '序列号' primary key auto_increment,
	`user_phone` varchar(40) not null comment '用户电话',
	`hospital` varchar(60) not null comment '医院名',
	`appoint_date` date not null comment '预约的日期',
	`section` int not null comment '预约的时段（从1到8）',
	`create_time` datetime not null default current_timestamp comment '记录创建时间',

	`report_status` boolean not null default 0 comment '报告是否生成',
	`report` MediumBlob default null comment '报告',
	key idx_phone ('user_phone')
) engine=InnoDB default charset=utf8mb4 comment='预约信息';
~~~
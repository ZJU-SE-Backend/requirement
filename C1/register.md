## 需求描述


### 需求介绍

病人预约挂号界面，包括用户个人信息表的查询，预约记录表的查询和增改

### API列表

| 请求类型  | PATH            | 描述               |
| -------- | ----------------- | -------------------- |
| GET      | /api/appoinment/register/hospital | 查询医院列表  |
| GET      | /api/appoinment/register/room/{hospital} | 查询某医院科室列表	|
| GET      | /api/appoinment/register/doctor_list | 查询某科室的医生列表   |
| GET      | /api/appoinment/register/doctor_remainder | 查询某医生预约数量  |
| POST     | /api/appoinment/register/record | 新增预约     |
| GET      | /api/appoinment/register/record/user_phone | 查询预约信息  |


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

### GET  /api/appoinment/register/room/{hospital}  查询某医院科室列表

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

建议数据库操作：
~~~sql
select distinct room from healthguide_appointment_register_hospital 
					where hospital = XXX;
~~~
; healthguide_appointment_register_room 表存放医院的科室，每个医院一张科室的表

### GET  /api/appoinment/register/doctor_list  查询某科室的医生列表 

#### Request
**查询参数 Query Parames**

| Key | Value | Required | Description |
| ----- | ------- | ---------- | ------------- |
| room  | string    | y   | 科室   |
| hospital | string | y | 医院 |


#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data":[
		["赵嘉成","18888988888","主任医师，擅长儿科的诊治……"],
		["李右", "18888988889","副主任医师"],
		["阿伟","18888988881","医生，彬彬的好朋友"],
		["杰哥", "18888912321","家里蛮大的"],
		["..."],
		["医生姓名","医生id","医生相关描述"]
	]
}
~~~



### POST  /api/appoinment/register/record  新增预约

**数据 Data**

| Key | Value | Required | Description |
| ----- | ------- | ---------- | ------------- |
| patient_phone   | string    | y/n     | 病人id（如果后台能根据header自动确定则这个不用）   |
| hospital     | string    | y       | 医院名   |
| room 			| string 	| y		|  科室名	|
| appoint_id  | int   | y        | 期望预约时间段编号 |
| doctor_phone    | int       | y       | 医生id |
(目前暂定预约记录展示这些信息，根据后续开发确定是否需要添加新的字段)
（预约时间段编号对应了不同的时间段，在前端会进行针对性的解释翻译）

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data":null
}
~~~

~~~json
{
	"st": 1,
	"msg": "",
	"data":null
}
~~~


默认在执行此操作的时候，前端已经通过前面获取的后台数据确定了操作的合法性。



### GET   /api/appoinment/register/record/{user_phone}   查询预约信息

#### Request
**路由参数 ：**

| Key | Value | Required | Description |
| ----- | ------- | ---------- | ------------- |
| user_phone   | string    | y/n     | 用户id （如果后台能根据header自动确定则这个不用）   |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data":[
		["杭州市第一人民医院","2020-4-20 13:13:13","儿科","妙手回春哥"],
		["..."],
		["医院","日期","科室","预约医生姓名"]
	]
}
~~~



### GET  /api/appoinment/register/doctor_remainder  查询某医生预约数量

#### Request
**查询参数 Query Parames**

| Key | Value | Required | Description |
| ----- | ------- | ---------- | ------------- |
| patient_phone   | string    | y/n     | 医生ID  |
| appoint_date | int     | y       |预约日期  |
#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data":{
		"default_number": 0,
		"remainder":{
			"1":5,
			"2":4,
			"...":"...",
			"时段编号": "预约数量"
		}
	}
}
~~~




## 数据表

**healthguide_appointment_register_hospital**
存储医院、每一个科室，科室中医生id，医生的信息这些内容

~~~sql
create table `healthguide_appointment_register_hospital` (
	`hospital` varchar(60) not null comment '医院名' primary key,
	`room` int not null comment '医院科室',
	`doctor_phone` varchar(40) not null comment '医生id'，
	`doctor_info` varchar(255) not null comment '医生信息'
) engine=InnoDB default charset=utf8mb4 comment='各医院科室和医生信息';

insert into healthguide_appointment_register_hospital value
("浙江大学医学院附属第一医院", "儿科", "赵嘉成","主任医师");
...
~~~

**healthguide_appointment_register_record**
每一个医生不同时间段都有预约余量

~~~sql
create table `healthguide_appointment_register_record` (
	`appoint_id` int not null comment '预约时间段编号',
	`patient_phone` int not null comment '病人ID',
	`doctor_phone` int not null comment '医生ID',
	`room` varchar(40) not null comment '科室',
	`hospital` varchar(40) not null comment '医院'
	primary key (`doctor_phone`,`appoint_id`,`patient_phone`)
) engine=InnoDB default charset=utf8mb4 comment='医院预约记录';
~~~


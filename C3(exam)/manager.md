##

### 需求介绍

把一个用户绑定为某个医院的管理员

### API列表
|请求类型|PATH|描述|
|-|-|-|
|GET | /api/exam/manager/{userPhone}| 获取该用户是否是某家医院的管理员|

## API文档

### GET  /api/exam/manager/{userPhone}  获取该用户是否是某家医院的管理员

#### Request

无参数

#### Response

如果是某家医院的管理员，返回医院名称，否则返回空串。

~~~json
{
	"st": 0,
	"msg": "",
	"data":{
		"hospital": "杭州市第一人民医院"
	}
}
~~~


## 数据表

**healthguide_exam_manager**
医院对应管理员

~~~sql
create table `healthguide_exam_manager` (
	`user_phone` varchar(40) not null comment '用户电话' primary key,
	`hospital` varchar(60) not null comment '医院名',
) engine=InnoDB default charset=utf8mb4 comment='各医院管理员';

insert into healthguide_exam_manager(user_phone, hospital) values
('10000000000','杭州市第一人民医院');
~~~
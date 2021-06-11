## 需求描述

### 需求介绍

用户个人信息界面，功能包括：注册；查看基本信息；修改基本信息；修改用户密码

### API列表

| 请求类型  | PATH                                                          | 描述               |
| -------- | ------------------------------------------------------------  | ----------------  |
| GET      | /api/healthrecord/personInfo/{userPhone}                      | 查看用户基本信息     |
| POST     | /api/join                                                     | 注册新用户          |
| POST     | /api/cgpswd                                                   | 修改用户密码        |
| POST     | /api/cginfo                                                   | 修改用户信息        |



## API文档

### GET  /api/healthrecord/personInfo/{userPhone}   查询用户基本信息

#### Request

查询参数

| Key          | Value | Required | Description    |
| ------------ | ----- | -------- | -------------- |
| userPhone |string | y        | 用户id          |


#### Response

```json
{
	"st": 0,
	"msg": "",
	"data":[ 
        ["用户姓名","性别","手机号","身份证",.....]
        ......
     ]
}
```

建议数据库操作：
~~~sql
select user_phone, auth_type, user_name, user_email, user_gender, user_height, user_weight, user_ID_number
from person_info
where user_phone = XXX;
~~~
返回所有用户个人信息。(权限等级用于判断是否显示病历信息：病人 -> 显示病历，医生/游客 -> 不显示病历)


### POST /api/join  注册新用户
**数据 Data**
| Key | Value | Required | Description |
| ----- | ------- | ---------- | ------------- |
| userPhone   | string    | y     | 用户id     |
| userName     | string   | y     | 用户姓名    |
| password     | string   | y     | 用户密码    |
| authType      | tinyint | y     | 用户权限    |
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
#### 数据库操作
就是在user_info表里添加一项，其余未出现的字段均先默认为null，`user_gender`默认为`male`，`auth_type`默认为1

### POST /api/cgpswd  修改用户密码
**数据 Data**
| Key | Value | Required | Description |
| ----- | ------- | ---------- | ------------- |
| userPhone   | string    | y     | 用户id     |
| password     | string   | y     | 用户密码    |
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
#### 数据库操作
操作就是在user_info中，将手机号为userPhone的一项对应的密码修改为password
（因为用户已登录，所以无需再次输入原来的密码）

### POST /api/cginfo  修改用户信息
**数据 Data**
| Key | Value | Required | Description |
| ----- | ------- | ---------- | ------------- |
| userPhone     | string    | y     | 用户id     |
| userName      | string    | y     | 用户姓名    |
| userEmail     | string    | y     | 用户邮箱    |
| userGender    | string    | y     | 用户性别    |
| userHeight    | string    | y     | 用户身高    |
| userWeight    | string    | y     | 用户体重    |
| userAge       | string    | y     | 用户年龄    |
| userIDnumber  | string    | y     | 用户身份证号 |
| userSocnumber | string    | y     | 用户社保卡号 |

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

#### 数据库操作
在user_info将手机号为userPhone的一项，将个人信息重置，健康码等级与auth_type不允许用户个人修改因此没有加入


## 数据表
**person_info**
存储用于显示的用户信息内容

~~~sql
create table `person_info` (
  `user_phone`          varchar(40)             NOT NULL        COMMENT '电话' primary key,
  `auth_type`         	tinyint                 DEFAULT 0	      COMMENT '权限等级',
  `user_name`   	      varchar(40)             DEFAULT ''      COMMENT '姓名',
  `user_email`  	      varchar(40)             DEFAULT ''      COMMENT '电子邮箱',
  `user_gender`         enum('male', 'female')  NOT NULL        COMMENT '性别',        
  `user_height`         integer                 DEFAULT 165     COMMENT '身高',
  `user_weight`         integer                 DEFAULT 55      COMMENT '体重',
  `user_ID_number`      varchar(40)             NOT NULL        COMMENT '身份证号',
  `healthcode_type`     tinyint                 DEFAULT 0       COMMENT '健康码等级',
  `soc_insure_number`    varchar(40)             NOT NULL        COMMENT '社保账号'
) engine=InnoDB default charset=utf8mb4 comment='用户的个人信息（用于显示）';
~~~
authType目前规定的枚举如下：

~~~c#
enum AuthType
{
    AuthGuest   = 0,        // 未登录/无权限/访客				
    AuthPatient = 1,        // 病人 
    AuthDoctor  = 2,        // 医生
    AuthManager = 3,        // 管理					预留	
    AuthAdmin   = 4,        // 高管					预留
}

enum HealthcodeType
{
  Green   = 0,    // 绿码
  Yellow  = 1,    // 黄码
  Red     = 2,    // 红码
}
~~~



## 测试样例（by后端）

### **GET**/api/healthrecord/personInfo/{userPhone}

查询成功：

```json
userPhone = '18888888888'

	
Response body
{
  "st": 0,
  "msg": "",
  "data": {
    "userPhone": "18888888888",
    "authType": 1,
    "userName": "卢本伟",
    "userEmail": "@",
    "userGender": "male",
    "userHeight": "170",
    "userWeight": "70",
    "userIDNumber": "ID1",
    "healcodeType": 0,
    "socInsureNumber": "ID2"
  }
}
```

查询失败，用户不存在：

```json
userPhone = '?'

	
Response body
{
  "st": 1,
  "msg": "数据不一致",
  "data": null
}
```






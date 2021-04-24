# 用户个人信息查询

## 需求描述

### 需求介绍

用户查看自己的个人信息。

### API列表

| 请求类型  | PATH                                                          | 描述               |
| -------- | ------------------------------------------------------------  | ----------------  |
| GET      | /api/healthrecord/personInfo/{userPhone}          | 查看用户基本信息     |



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
  `user_height`         varchar(10)             NOT NULL        COMMENT '身高',
  `user_weight`         varchar(10)             NOT NULL        COMMENT '体重',
  `user_ID_number`      varchar(40)             NOT NULL        COMMENT '身份证号'
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
    "userIDNumber": "ID1"
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






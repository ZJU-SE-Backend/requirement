## 需求描述

### 需求介绍

用户发布问题，等待其他用户进行回答。

### API列表

| 请求类型 | PATH                                           | 描述               |
| -------- | ---------------------------------------------- | ------------------ |
| GET      | /api/forum/qa/question                         | 查询问题列表       |
| GET      | /api/forum/qa/question/{questionId}            | 查询单个问题的信息 |
| GET      | /api/forum/qa/question/user/{userPhone}        | 查询用户提出的问题 |
| POST     | /api/forum/qa/question                         | 新增问题           |
| PUT      | /api/forum/qa/question/{questionId}            | 问题内容编辑       |
| PUT      | /api/forum/qa/question/addViewCnt/{questionId} | 增加问题浏览数     |
| DELETE   | /api/forum/qa/question/{questionId}            | 删除问题           |



## API文档

### GET  /api/forum/qa/question  查询问题列表

#### Request

**查询参数 Query Params**


| Key      | Value | Required | Description      |
| -------- | ----- | -------- | ---------------- |
| pageSize | int   | 是       | 分页中一页的容量 |
| pageNo   | int   | 是       | 需要获取页的序数 |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": {
        "total": 2,
		"posts": [
		{
            "questionId": 1,
			"title": "为什么",
			"userPhone": "18888888888",
			"userName": "卢本伟",
			"answerCnt": 1,
			"viewCnt": 12,
			"lastEditTime": timestamp
		},
		{
            "questionId": 2,
			"title": "不懂就问",
			"userPhone": "18888888888",
			"userName": "卢本伟",
			"answerCnt": 12,
			"viewCnt": 13,
			"lastEditTime": timestamp
        },
    	]
    }
}
~~~



### GET  /api/forum/qa/question/{questionId}  查询单个问题的信息

#### Request

**路由参数 URL Params**


| Key        | Value  | Required | Description |
| ---------- | ------ | -------- | ----------- |
| questionId | bigint | 是       | 贴子id      |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data":
    {
        "title": "为什么",
        "userPhone": "18888888888",
        "userName": "卢本伟",
        "answerCnt": 1,
        "viewCnt": 12,
        "lastEditTime": timestamp
    }
}
~~~



### GET  /api/forum/qa/question/user/{userPhone}  查询用户提出的问题

#### Request

**路由参数 URL Params**


| Key       | Value  | Required | Description  |
| --------- | ------ | -------- | ------------ |
| userPhone | bigint | 是       | 用户电话号码 |

**查询参数 Query Params**


| Key      | Value | Required | Description      |
| -------- | ----- | -------- | ---------------- |
| pageSize | int   | 是       | 分页中一页的容量 |
| pageNo   | int   | 是       | 需要获取页的序数 |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": {
        "total": 2,
		"posts": [
		{
            "questionId": 1,
			"title": "为什么",
			"userPhone": "18888888888",
			"userName": "卢本伟",
			"answerCnt": 1,
			"viewCnt": 12,
			"lastEditTime": timestamp
		},
		{
            "questionId": 2,
			"title": "不懂就问",
			"userPhone": "18888888888",
			"userName": "卢本伟",
			"answerCnt": 12,
			"viewCnt": 13,
			"lastEditTime": timestamp
        },
    	]
    }
}
~~~



### POST  /api/forum/qa/question  新增问题

#### Request

**请求头部 Header**

| Key       | Value       | Required | Description |
| --------- | ----------- | -------- | ----------- |
| title     | varchar(40) | 是       | 问题标题    |
| userName  | varchar(40) | 是       | 用户        |
| userPhone | varchar(40) | 是       | 用户ID      |
| content   | text        | 是       | 问题内容    |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": null
}
~~~



### PUT  /api/forum/qa/question/{questionId}  问题内容编辑

#### Request

**路由参数 URL Params**


| Key        | Value  | Required | Description |
| ---------- | ------ | -------- | ----------- |
| questionId | bigint | 是       | 问题ID      |

**请求主体 Body**

| Key     | Value       | Required | Description |
| ------- | ----------- | -------- | ----------- |
| title   | varchar(40) | 是       | 文章标题    |
| content | text        | 是       | 内容        |

~~~json
{
	"title" : "xxx",
	"content" : "xxxx",
}
~~~

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": null
}
~~~



### DELETE  /api/forum/qa/question/{questionId}  删除问题

#### Request

**路由参数 URL Params**


| Key        | Value  | Required | Description |
| ---------- | ------ | -------- | ----------- |
| questionId | bigint | 是       | 问题ID      |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": null
}
~~~



### PUT  /api/forum/qa/question/addViewCnt/{questionId}  增加问题浏览数

#### Request

**路由参数 URL Params**


| Key        | Value  | Required | Description |
| ---------- | ------ | -------- | ----------- |
| questionId | bigint | 是       | 问题ID      |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": null
}
~~~



## 数据表定义

### healthguide_forum_qa_question

```mysql
create table `healthguide_forum_qa_question` (
  `question_id` bigint not null auto_increment comment '问题ID',
  `title` varchar(40) not null comment '标题',
  `user_name` varchar(40) not null comment '用户姓名',
  `user_phone` varchar(40) not null comment '用户ID',
  `content` text not null comment '问题内容',
  `answer_cnt` int default 0 not null comment '回答数',
  `view_cnt` int default 0 not null comment '浏览数',
  `create_time` datetime not null DEFAULT CURRENT_TIMESTAMP comment '创建时间',
  `update_time` datetime not null DEFAULT CURRENT_TIMESTAMP comment '最后编辑时间' ON UPDATE CURRENT_TIMESTAMP,
  primary key (`question_id`),
  key `idx_userphone` (`user_phone`)
) engine=InnoDB default charset=utf8mb4 comment='健康论坛提问';

insert into healthguide_forum_qa_question (title, user_name, user_phone, content, answer_cnt, view_cnt) values 
('为什么','卢本伟','18888888888','大家好','1','12'),
('不懂就问','卢本伟','18888888888','大家好2','12','13');                                                                                                           
select * from healthguide_forum_qa_question;
drop table `healthguide_forum_qa_question`;
```
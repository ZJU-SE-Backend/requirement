## 需求描述

### 需求介绍

用户发布问题，等待其他用户进行回答。

### API列表

| 请求类型 | PATH                                             | 描述                     |
| -------- | ------------------------------------------------ | ------------------------ |
| GET      | /api/forum/qa/question/{session}                 | 查询问题列表             |
| GET      | /api/forum/qa/question/{questionId}              | 查询单个问题的信息       |
| GET      | /api/forum/qa/question/user/{userPhone}          | 查询用户提出的问题       |
| POST     | /api/forum/qa/question                           | 新增问题                 |
| PUT      | /api/forum/qa/question/{questionId}              | 问题内容编辑             |
| PUT      | /api/forum/qa/question/addViewCnt/{questionId}   | 增加问题浏览数           |
| DELETE   | /api/forum/qa/question/{questionId}              | 删除问题                 |
| GET      | /api/forum/qa/question/favorite/{questionId}     | 返回用户是否收藏了该问题 |
| GET      | /api/forum/qa/question/favorite/user/{userPhone} | 返回用户收藏的问题       |
| POST     | /api/forum/qa/question/favorite/{questionId}     | 用户新增收藏             |
| DELETE   | /api/forum/qa/question/favorite/{questionId}     | 用户取消收藏             |



## API文档

### GET  /api/forum/qa/question/{session}  查询问题列表

#### Request

**路由参数 URL Params**


| Key     | Value   | Required | Description |
| ------- | ------- | -------- | ----------- |
| session | tinyint | 是       | 问题分区    |

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
| session   | tinyint     | 是       | 问题分区    |
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



### GET  /api/forum/qa/question/favorite/{questionId}  返回用户是否收藏了该问题

#### Request

**路由参数 URL Params**


| Key        | Value  | Required | Description |
| ---------- | ------ | -------- | ----------- |
| questionId | bigint | 是       | 问题id      |

**查询参数 Query Params**


| Key       | Value  | Required | Description  |
| --------- | ------ | -------- | ------------ |
| userPhone | bigint | 是       | 用户电话号码 |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": true // or false
}
~~~



### GET  /api/forum/qa/question/favorite/user/{userPhone}  返回用户收藏的问题

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
		"questions": 
        [
            {"questionId": 1, "title":"为什么", "answerCnt": 12, "favoriteTime": timestamp},
            {"questionId": 2, "title":"不懂就问", "answerCnt": 13, "favoriteTime": timestamp}
        ]
    }
}
~~~



### POST  /api/forum/qa/question/favorite/{questionId}  用户新增收藏

#### Request

**路由参数 URL Params**


| Key        | Value  | Required | Description |
| ---------- | ------ | -------- | ----------- |
| questionId | bigint | 是       | 问题id      |

**请求头部 Header**


| Key       | Value  | Required | Description  |
| --------- | ------ | -------- | ------------ |
| userPhone | bigint | 是       | 用户电话号码 |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": null
}
~~~



### DELETE  /api/forum/qa/question/favorite/{questionId}  用户取消收藏

#### Request

**路由参数 URL Params**


| Key        | Value  | Required | Description |
| ---------- | ------ | -------- | ----------- |
| questionId | bigint | 是       | 问题id      |

**请求主体 Body**


| Key       | Value  | Required | Description  |
| --------- | ------ | -------- | ------------ |
| userPhone | bigint | 是       | 用户电话号码 |

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
  `session` tinyint not null comment '问题分区',
  `title` varchar(40) not null comment '标题',
  `user_name` varchar(40) not null comment '用户姓名',
  `user_phone` varchar(40) not null comment '用户ID',
  `content` text not null comment '问题内容',
  `answer_cnt` int default 0 not null comment '回答数',
  `view_cnt` int default 0 not null comment '浏览数',
  `create_time` datetime not null DEFAULT CURRENT_TIMESTAMP comment '创建时间',
  `update_time` datetime not null DEFAULT CURRENT_TIMESTAMP comment '最后编辑时间' ON UPDATE CURRENT_TIMESTAMP,
  primary key (`question_id`),
  key `idx_userphone` (`user_phone`),
  key `idx_session` (`session`)
) engine=InnoDB default charset=utf8mb4 comment='健康论坛提问';

insert into healthguide_forum_qa_question (title, `session`, user_name, user_phone, content, answer_cnt, view_cnt) values 
('为什么',1,'卢本伟','18888888888','大家好','1','12'),
('不懂就问',2,'卢本伟','18888888888','大家好2','12','13');                                                                                                           
select * from healthguide_forum_qa_question;
drop table `healthguide_forum_qa_question`;
```

#### 备注

新增的分区仅在返回列表和新增问题时有用，其他地方都不做使用。



### healthguide_forum_qa_question_favorite

```mysql
create table `healthguide_forum_qa_question_favorite` (
   	`question_id` bigint not null comment '问题ID',
    `user_phone` varchar(40) not null comment '收藏者ID',
    `title` varchar(40) not null comment '问题标题',
    `answer_cnt` int not null comment '在收藏问题时的问题回答数',
    `favorite_time` datetime not null DEFAULT CURRENT_TIMESTAMP comment '收藏时间',
    primary key (`question_id`, `user_phone`),
    key `idx_userphone` (`user_phone`)
) engine=InnoDB default charset=utf8mb4 comment='健康论坛问题收藏';

insert into healthguide_forum_qa_question_favorite (question_id, user_phone, title, answer_cnt) values
('1','18888888888', '为什么', '12'),
('2','18888888888', '不懂就问', '13');

select * from healthguide_forum_qa_question_favorite;
drop table `healthguide_forum_qa_question_favorite`;
```

#### 备注

储存问题收藏时的回答数是为了判断哪些问题有新回答（前端来判断hhh，就不给后端搞复杂东西了）。

如果用户进去看有新回答的问题，前端会把收藏删除再新增，这样 `answer_cnt` 就和问题表那边一致，下次前端查询就知道问题没有新回答。


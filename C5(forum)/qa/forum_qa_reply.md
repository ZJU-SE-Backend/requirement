## 需求描述

### 需求介绍

用户对问题下的回答进行回复。

### API列表

| 请求类型 | PATH                                       | 描述                               |
| -------- | ------------------------------------------ | ---------------------------------- |
| GET      | /api/forum/qa/answer/reply/{answerId}      | 查询某回答下的回复                 |
| POST     | /api/forum/qa/answer/reply/{answerId}      | 在某回答下新增回复                 |
| PUT      | /api/forum/qa/answer/reply/{replyId}       | 回复内容编辑                       |
| DELETE   | /api/forum/qa/answer/reply/{replyId}       | 删除回复                           |
| GET      | /api/forum/qa/answer/reply/like/{answerId} | 获取用户对回答下某页回复的赞踩情况 |
| POST     | /api/forum/qa/answer/reply/like/{replyId}  | 新增赞/踩                          |
| PUT      | /api/forum/qa/answer/reply/like/{replyId}  | 赞/踩修改                          |
| DELETE   | /api/forum/qa/answer/reply/like/{replyId}  | 取消赞踩                           |



## API文档

### GET  /api/forum/qa/answer/reply/{answerId}  查询某回答下的回复

#### Request

**路由参数 URL Params**


| Key      | Value  | Required | Description |
| -------- | ------ | -------- | ----------- |
| answerId | bigint | 是       | 回答ID      |

**查询参数 Query Params**


| Key      | Value | Required | Description      |
| -------- | ----- | -------- | ---------------- |
| pageSize | int   | 是       | 分页中一页的容量 |
| pageNo   | int   | 是       | 需要获取页的序数 |

#### Response

```json
{
	"st": 0,
	"msg": "",
	"data": {
        "total": 2,
		"posts": [
		{
			"replyId": 1,
            "answerId": 1,
			"userPhone": "18888888888",
			"userName": "卢本伟",
            "content": "这个回答不错",
            "likeCnt": 7,
        	"dislikeCnt": 6,
			"lastEditTime": timestamp
		},
		{
			"replyId": 2,
            "answerId": 2,
			"userPhone": "18888888888",
			"userName": "卢本伟",
            "content": "这个回答8行",
            "likeCnt": 8,
        	"dislikeCnt": 9,
			"lastEditTime": timestamp
	    	},
    	]
    }
}
```



### POST  /api/forum/qa/answer/reply/{answerId}  在某回答下新增回复

#### Request

**路由参数 URL Params**


| Key      | Value  | Required | Description |
| -------- | ------ | -------- | ----------- |
| answerId | bigint | 是       | 回答ID      |

**请求头部 Header**

| Key       | Value       | Required | Description  |
| --------- | ----------- | -------- | ------------ |
| userName  | varchar(40) | 是       | 用户         |
| userPhone | varchar(40) | 是       | 用户电话号码 |
| content   | text        | 是       | 内容         |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": null
}
~~~

#### 备注

除在 `healthguide_forum_qa_answer_reply` 新增一个记录之外，还需要修改`healthguide_forum_qa_answer` 表中对应的回复计数。



### PUT  /api/forum/qa/answer/reply/{replyId}  回复内容编辑

#### Request

**路由参数 URL Params**


| Key     | Value  | Required | Description |
| ------- | ------ | -------- | ----------- |
| replyId | bigint | 是       | 回复id      |

**请求主体 Body**

| Key     | Value | Required | Description |
| ------- | ----- | -------- | ----------- |
| content | text  | 是       | 内容        |

~~~json
{
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



### DELETE  /api/forum/qa/answer/reply/{replyId}  删除回复

#### Request

**路由参数 URL Params**


| Key     | Value  | Required | Description |
| ------- | ------ | -------- | ----------- |
| replyId | bigint | 是       | 回复id      |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": null
}
~~~

除在 `healthguide_forum_qa_answer_reply` 删除一个记录之外，还需要修改`healthguide_forum_qa_answer` 表中对应的回复计数。



### GET  /api/forum/qa/answer/reply/like/{answerId}  获取用户对回答下某页回复的赞踩情况

**路由参数 URL Params**


| Key      | Value  | Required | Description |
| -------- | ------ | -------- | ----------- |
| answerId | bigint | 是       | 回答ID      |

**查询参数 Query Params**


| Key       | Value  | Required | Description      |
| --------- | ------ | -------- | ---------------- |
| userPhone | bigint | 是       | 用户电话号码     |
| pageSize  | int    | 是       | 分页中一页的容量 |
| pageNo    | int    | 是       | 需要获取页的序数 |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": 
    {
        likes: [11， 13],	// or [] if no like exists
        dislikes: [12， 14]	// or []
    }
}
~~~

#### 备注

**需要联表查询，先通过 `healthguide_forum_qa_answer` 获得这一页的所有replyId，再获得赞踩状态**

返回的是回答下这一页的回复中用户赞/踩过的回复ID。



### POST  /api/forum/qa/answer/reply/like/{replyId}  新增赞/踩

**路由参数 URL Params**


| Key     | Value  | Required | Description |
| ------- | ------ | -------- | ----------- |
| replyId | bigint | 是       | 回复id      |

**请求头部 Header**


| Key       | Value      | Required | Description      |
| --------- | ---------- | -------- | ---------------- |
| userPhone | bigint     | 是       | 用户电话号码     |
| like      | tinyint(1) | 是       | 1表示赞，0表示踩 |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": null
}
~~~

#### 备注

除在 `healthguide_forum_qa_answer_reply_like` 新增一个记录之外，还需要修改`healthguide_forum_qa_answer_reply` 表中对应主题贴的赞踩计数。



### PUT  /api/forum/qa/answer/reply/like/{replyId}  赞/踩修改

**路由参数 URL Params**


| Key     | Value  | Required | Description |
| ------- | ------ | -------- | ----------- |
| replyId | bigint | 是       | 回复id      |

**请求主体 Body**


| Key       | Value      | Required | Description      |
| --------- | ---------- | -------- | ---------------- |
| userPhone | bigint     | 是       | 用户电话号码     |
| like      | tinyint(1) | 是       | 1表示赞，0表示踩 |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": null
}
~~~

#### 备注

同时需要修改 `healthguide_forum_qa_answer_reply` 表中对应主题贴的赞踩计数。



### DELETE  /api/forum/qa/answer/reply/like/{replyId}  取消赞踩

**路由参数 URL Params**


| Key     | Value  | Required | Description |
| ------- | ------ | -------- | ----------- |
| replyId | bigint | 是       | 回复id      |

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

#### 备注

除在 `healthguide_forum_qa_answer_reply_like` 删除一个记录之外，还需要修改`healthguide_forum_qa_answer_reply` 表中对应主题贴的赞踩计数。



## 数据表定义

### healthguide_forum_qa_answer_reply

```mysql
create table `healthguide_forum_qa_answer_reply` (
  `reply_id` bigint not null auto_increment comment '回复ID',
  `answer_id` bigint not null comment '回答ID',
  `user_name` varchar(40) not null comment '用户姓名',
  `user_phone` varchar(40) not null comment '用户ID',
  `content` text not null comment '回答内容',
  `like_cnt` int default 0 not null comment '点赞数',
  `dislike_cnt` int default 0 not null comment '点踩数',
  `create_time` datetime not null DEFAULT CURRENT_TIMESTAMP comment '创建时间',
  `update_time` datetime not null DEFAULT CURRENT_TIMESTAMP comment '最后编辑时间' ON UPDATE CURRENT_TIMESTAMP,
  primary key (`reply_id`),
  key `idx_questionid` (`answer_id`)
) engine=InnoDB default charset=utf8mb4 comment='健康论坛回答的回复';

insert into healthguide_forum_qa_answer_reply (answer_id, user_name, user_phone, content, like_cnt, dislike_cnt) values 
('1','卢本伟','18888888888','我来回答1','7','6'),
('2','卢本伟','18888888888','我就不回答2','8','9');                                                                                                           
select * from healthguide_forum_qa_answer_reply;
drop table `healthguide_forum_qa_answer_reply`;
```



### healthguide_forum_qa_answer_reply_like

```mysql
create table `healthguide_forum_qa_answer_reply_like` (
   	`reply_id` bigint not null comment '回复ID',
    `user_phone` varchar(40) not null comment '赞踩者ID',
    `like` tinyint(1) not null comment '1表示赞 0表示踩',
    primary key (`reply_id`, `user_phone`)
) engine=InnoDB default charset=utf8mb4 comment='健康论坛回答回复赞踩情况';

insert into healthguide_forum_qa_answer_reply_like (reply_id, user_phone, `like`) values
('1','18888888888', '1'),
('2','18888888888', '0');

select * from healthguide_forum_qa_answer_reply_like;
drop table `healthguide_forum_qa_answer_reply_like`;
```


## 需求描述

### 需求介绍

用户对问题作出回答。

### API列表

| 请求类型 | PATH                                           | 描述                               |
| -------- | ---------------------------------------------- | ---------------------------------- |
| GET      | /api/forum/qa/answer/{questionId}              | 查询某问题下的回答                 |
| GET      | /api/forum/qa/answer/content/{answerId}        | 获取回答详情                       |
| GET      | /api/forum/qa/answer/user/{userPhone}          | 查询用户作出的所有回答             |
| POST     | /api/forum/qa/answer/{questionId}              | 在某问题下新增回答                 |
| PUT      | /api/forum/qa/answer/{answerId}                | 编辑回答                           |
| PUT      | /api/forum/qa/answer/addViewCnt/{answerId}     | 增加回答浏览数                     |
| DELETE   | /api/forum/qa/answer/{answerId}                | 删除回答                           |
| GET      | /api/forum/qa/answer/like/{questionId}         | 获取用户对问题下某页回答的赞踩情况 |
| POST     | /api/forum/qa/answer/like/{answerId}           | 新增赞/踩                          |
| PUT      | /api/forum/qa/answer/like/{answerId}           | 赞/踩修改                          |
| DELETE   | /api/forum/qa/answer/like/{answerId}           | 取消赞踩                           |
| GET      | /api/forum/qa/answer/favorite/{answerId}       | 返回用户是否收藏了该回答           |
| GET      | /api/forum/qa/answer/favorite/user/{userPhone} | 返回用户收藏的回答                 |
| POST     | /api/forum/qa/answer/favorite/{answerId}       | 用户新增收藏                       |
| DELETE   | /api/forum/qa/answer/favorite/{answerId}       | 用户取消收藏                       |





## API文档

### GET  /api/forum/qa/answer/{questionId}  查询某问题下的回答

#### Request

**路由参数 URL Params**


| Key        | Value  | Required | Description |
| ---------- | ------ | -------- | ----------- |
| questionId | bigint | 是       | 问题ID      |

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
			"answerId": 1,
            "questionId": 1,
			"userPhone": "18888888888",
			"userName": "卢本伟",
            "content": "我来回答1",
			"replyCnt": 1,
			"viewCnt": 12,
            "likeCnt": 7,
        	"dislikeCnt": 6,
			"lastEditTime": timestamp
		},
		{
			"answerId": 2,
            "questionId": 2,
			"userPhone": "18888888888",
			"userName": "卢本伟",
            "content": "我就不回答2",
			"replyCnt": 12,
			"viewCnt": 13,
            "likeCnt": 8,
        	"dislikeCnt": 9,
			"lastEditTime": timestamp
        },
    	]
    }
}
```

#### 备注

此处返回的 `content` 截取前100字即可。



### GET  /api/forum/qa/answer/content/{answerId}  获取回答详情

#### Request

**路由参数 URL Params**


| Key      | Value  | Required | Description |
| -------- | ------ | -------- | ----------- |
| answerId | bigint | 是       | 回答ID      |

#### Response

```json
{
	"st": 0,
	"msg": "",
	"data":
    {
            "questionId": 1,
			"userPhone": "18888888888",
			"userName": "卢本伟",
            "content": "我来回答1",
			"replyCnt": 1,
			"viewCnt": 12,
            "likeCnt": 7,
        	"dislikeCnt": 6,
			"lastEditTime": timestamp
    }
}
```

#### 备注

返回完整的 `content` 。



### GET  /api/forum/qa/answer/user/{userPhone}  查询用户作出的所有回答

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

```json
{
	"st": 0,
	"msg": "",
	"data": {
        "total": 2,
		"posts": [
		{
			"answerId": 1,
            "questionId": 1,
			"userPhone": "18888888888",
			"userName": "卢本伟",
            "content": "我来回答1",
			"replyCnt": 1,
			"viewCnt": 12,
            "likeCnt": 7,
        	"dislikeCnt": 6,
			"lastEditTime": timestamp
		},
		{
			"answerId": 2,
            "questionId": 2,
			"userPhone": "18888888888",
			"userName": "卢本伟",
            "content": "我就不回答2",
			"replyCnt": 12,
			"viewCnt": 13,
            "likeCnt": 8,
        	"dislikeCnt": 9,
			"lastEditTime": timestamp
        },
    	]
    }
}
```

#### 备注

此处返回的 `content` 截取前100字即可。



### POST  /api/forum/qa/answer/{questionId}  在某问题下新增回答

#### Request

**路由参数 URL Params**


| Key        | Value  | Required | Description |
| ---------- | ------ | -------- | ----------- |
| questionId | bigint | 是       | 问题ID      |

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

除在 `healthguide_forum_qa_answer` 新增一个记录之外，还需要修改`healthguide_forum_qa_question` 表中对应的回复计数。



### PUT  /api/forum/qa/answer/{answerId}  编辑回答

#### Request

**路由参数 URL Params**


| Key      | Value  | Required | Description |
| -------- | ------ | -------- | ----------- |
| answerId | bigint | 是       | 回答id      |

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



### PUT  /api/forum/qa/answer/addViewCnt/{answerId}  增加回答浏览数

#### Request

**路由参数 URL Params**


| Key      | Value  | Required | Description |
| -------- | ------ | -------- | ----------- |
| answerId | bigint | 是       | 回答ID      |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": null
}
~~~





### DELETE  /api/forum/qa/answer/{answerId}  删除回答

#### Request

**路由参数 URL Params**


| Key      | Value  | Required | Description |
| -------- | ------ | -------- | ----------- |
| answerId | bigint | 是       | 回答id      |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": null
}
~~~

除在 `healthguide_forum_qa_answer` 删除一个记录之外，还需要修改`healthguide_forum_qa_question` 表中对应的回复计数。



### GET  /api/forum/qa/answer/like/{questionId}  获取用户对问题下某页回答的赞踩情况

**路由参数 URL Params**


| Key        | Value  | Required | Description |
| ---------- | ------ | -------- | ----------- |
| questionId | bigint | 是       | 问题id      |

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

**需要联表查询，先通过 `healthguide_forum_qa_answer` 获得这一页的所有answerId，再获得赞踩状态**

返回的是问题这一页的回答中用户赞/踩过的回答ID。



### POST  /api/forum/qa/answer/like/{answerId}  新增赞/踩

**路由参数 URL Params**


| Key      | Value  | Required | Description |
| -------- | ------ | -------- | ----------- |
| answerId | bigint | 是       | 回答id      |

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

除在 `healthguide_forum_qa_answer_like` 新增一个记录之外，还需要修改`healthguide_forum_qa_answer` 表中对应回答的赞踩计数。



### PUT  /api/forum/qa/answer/like/{answerId}  赞/踩修改

**路由参数 URL Params**


| Key      | Value  | Required | Description |
| -------- | ------ | -------- | ----------- |
| answerId | bigint | 是       | 回答id      |

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

同时需要修改 `healthguide_forum_qa_answer` 表中对应主题贴的回答计数。



### DELETE  /api/forum/qa/answer/like/{answerId}  取消赞踩

**路由参数 URL Params**


| Key      | Value  | Required | Description |
| -------- | ------ | -------- | ----------- |
| answerId | bigint | 是       | 回答id      |

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

除在 `healthguide_forum_qa_answer_like` 删除一个记录之外，同时需要修改 `healthguide_forum_qa_answer` 表中对应主题贴的赞踩计数。



### GET  /api/forum/qa/answer/favorite/{answerId}  返回用户是否收藏了该回答

**路由参数 URL Params**


| Key      | Value  | Required | Description |
| -------- | ------ | -------- | ----------- |
| answerId | bigint | 是       | 回答id      |

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

#### 备注

数据库中有记录则返回true，否则返回false



### GET  /api/forum/qa/answer/favorite/user/{userPhone}  返回用户收藏的回答

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
		"posts": 
        [
            {"answerId": 1, "title":"为什么", "content": "我来回答1", "favoriteTime": timestamp},
            {"answerId": 2, "title":"不懂就问", "content": "我就不回答2", "favoriteTime": timestamp}
        ]
    }
}
~~~

#### 备注

按收藏日期倒序排序、分页。

此处的 `content` 在新增收藏时由前端控制字数，返回时不用特殊处理。



### POST  /api/forum/qa/answer/favorite/{answerId}  用户新增收藏

**路由参数 URL Params**


| Key      | Value  | Required | Description |
| -------- | ------ | -------- | ----------- |
| answerId | bigint | 是       | 回答id      |

**请求头部 Header**


| Key       | Value        | Required | Description  |
| --------- | ------------ | -------- | ------------ |
| userPhone | bigint       | 是       | 用户电话号码 |
| title     | varchar(40)  | 是       | 问题标题     |
| content   | varchar(100) | 是       | 回答内容     |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": null
}
~~~



### DELETE  /api/forum/qa/answer/favorite/{answerId}  用户取消收藏

**路由参数 URL Params**


| Key      | Value  | Required | Description |
| -------- | ------ | -------- | ----------- |
| answerId | bigint | 是       | 回答id      |

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

### healthguide_forum_qa_answer

```mysql
create table `healthguide_forum_qa_answer` (
  `answer_id` bigint not null auto_increment comment '回答ID',
  `question_id` bigint not null comment '问题ID',
  `user_name` varchar(40) not null comment '用户姓名',
  `user_phone` varchar(40) not null comment '用户ID',
  `content` text not null comment '回答内容',
  `reply_cnt` int default 0 not null comment '回复数',
  `view_cnt` int default 0 not null comment '浏览数',
  `like_cnt` int default 0 not null comment '点赞数',
  `dislike_cnt` int default 0 not null comment '点踩数',
  `create_time` datetime not null DEFAULT CURRENT_TIMESTAMP comment '创建时间',
  `update_time` datetime not null DEFAULT CURRENT_TIMESTAMP comment '最后编辑时间' ON UPDATE CURRENT_TIMESTAMP,
  primary key (`answer_id`),
  key `idx_userphone` (`user_phone`),
  key `idx_questionid` (`question_id`)
) engine=InnoDB default charset=utf8mb4 comment='健康论坛回答';

insert into healthguide_forum_qa_answer (question_id, user_name, user_phone, content, reply_cnt, view_cnt, like_cnt, dislike_cnt) values 
('1','卢本伟','18888888888','我来回答1','1','12','7','6'),
('2','卢本伟','18888888888','我就不回答2','12','13','8','9');                                                                                                           
select * from healthguide_forum_qa_answer;
drop table `healthguide_forum_qa_answer`;
```



### healthguide_forum_qa_answer_like

```mysql
create table `healthguide_forum_qa_answer_like` (
   	`answer_id` bigint not null comment '回答ID',
    `user_phone` varchar(40) not null comment '赞踩者ID',
    `like` tinyint(1) not null comment '1表示赞 0表示踩',
    primary key (`answer_id`, `user_phone`)
) engine=InnoDB default charset=utf8mb4 comment='健康论坛回答赞踩情况';

insert into healthguide_forum_qa_answer_like (answer_id, user_phone, `like`) values
('1','18888888888', '1'),
('2','18888888888', '0');

select * from healthguide_forum_qa_answer_like;
drop table `healthguide_forum_qa_answer_like`;
```



### healthguide_forum_qa_answer_favorite

```mysql
create table `healthguide_forum_qa_answer_favorite` (
   	`answer_id` bigint not null comment '回答ID',
    `user_phone` varchar(40) not null comment '收藏者ID',
    `title` varchar(40) not null comment '问题标题',
    `content` varchar(100) not null comment '回答内容',
    `favorite_time` datetime not null DEFAULT CURRENT_TIMESTAMP comment '收藏时间',
    primary key (`answer_id`, `user_phone`),
    key `idx_userphone` (`user_phone`)
) engine=InnoDB default charset=utf8mb4 comment='健康论坛回答收藏情况';

insert into healthguide_forum_qa_answer_favorite (answer_id, user_phone, title, content) values
('1','18888888888', '为什么', '我来回答1'),
('2','18888888888', '不懂就问', '我就不回答2');

select * from healthguide_forum_qa_answer_favorite;
drop table `healthguide_forum_qa_answer_favorite`;
```


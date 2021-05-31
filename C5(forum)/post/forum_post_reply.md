## 需求描述


### 需求介绍

主题贴下的回复。

### API列表

| 请求类型 | PATH                                 | 描述                               |
| -------- | ------------------------------------ | ---------------------------------- |
| GET      | /api/forum/post/reply/{topicId}      | 查询某贴子下的回复                 |
| POST     | /api/forum/post/reply/{topicId}      | 在某贴子下新增回复                 |
| PUT      | /api/forum/post/reply/{replyId}      | 回复内容编辑                       |
| DELETE   | /api/forum/post/reply/{replyId}      | 删除回复                           |
| GET      | /api/forum/post/reply/like/{topicId} | 获取用户对主题贴某页回复的赞踩情况 |
| POST     | /api/forum/post/reply/like/{replyId} | 新增赞/踩                          |
| PUT      | /api/forum/post/reply/like/{replyId} | 赞/踩修改                          |
| DELETE   | /api/forum/post/reply/like/{replyId} | 取消赞踩                           |



## API文档

### GET /api/forum/post/reply/{topicId} 查询某贴子下的回复

#### Request

**路由参数 URL Params**


| Key     | Value  | Required | Description |
| ------- | ------ | -------- | ----------- |
| topicId | bigint | 是       | 主题贴ID    |

**查询参数 Query Params**


| Key      | Value | Required | Description      |
| -------- | ----- | -------- | ---------------- |
| pageSize | int   | 是       | 分页中一页的容量 |
| pageNo   | int   | 是       | 需要获取页的序数 |

```json
{
	"st": 0,
	"msg": "",
	"data": {
        "total": 2,
		"posts": [
		{
			"replyId": 11,
			"userPhone": "13912345678",
			"userName": "我是谁",
			"floor": 1,
            "content": "试试回复。",
            "likeCnt": 12,
        	"dislikeCnt": 0,
			"lastEditTime": timestamp
		},
		{
			"replyId": 12,
			"userPhone": "13912345678",
			"userName": "我是谁",
			"floor": 2,
            "content": "试试回复2。",
            "likeCnt": 12,
        	"dislikeCnt": 32,
			"lastEditTime": timestamp
	    	},
    	]
    }
}
```



### POST /api/forum/post/reply/{topicId} 在某贴子下新增回复

#### Request

**路由参数 URL Params**


| Key     | Value  | Required | Description |
| ------- | ------ | -------- | ----------- |
| topicId | bigint | 是       | 主题贴ID    |

**请求主体 Body**

| Key         | Value       | Required | Description  |
| ----------- | ----------- | -------- | ------------ |
| userName      | varchar(40) | 是       | 用户         |
| userPhone | varchar(40) | 是       | 用户电话号码 |
| content     | text        | 是       | 内容         |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": null
}
~~~

#### 备注

插入回复时，`healthguide_forum_post_topic_reply` 中的floor需要根据情况进行设置（=同topic下最大楼层数+1）

除在 `healthguide_forum_post_topic_reply` 新增一个记录之外，还需要修改`healthguide_forum_post_topic` 表中对应的回复计数。



### PUT /api/forum/post/reply/{replyId} 回复内容编辑

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



### DELETE /api/forum/post/reply/{replyId} 删除回复

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

除在 `healthguide_forum_post_topic_reply` 删除一个记录之外，还需要修改`healthguide_forum_post_topic` 表中对应的回复计数。



### GET  /api/forum/post/reply/like/{topicId}  获取用户对主题贴某页下回复的赞踩情况

**路由参数 URL Params**


| Key     | Value  | Required | Description |
| ------- | ------ | -------- | ----------- |
| topicId | bigint | 是       | 贴子id      |

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

**需要联表查询，先通过 `healthguide_forum_post_topic_reply` 获得这一页的所有replyId，再获得赞踩状态**

返回的是主题贴这一页的回复中用户赞/踩过的回复ID。



### POST  /api/forum/post/reply/like/{replyId}  新增赞/踩

**路由参数 URL Params**


| Key     | Value  | Required | Description |
| ------- | ------ | -------- | ----------- |
| replyId | bigint | 是       | 回复id      |

**查询参数 Query Params**


| Key         | Value      | Required | Description      |
| ----------- | ---------- | -------- | ---------------- |
| userPhone | bigint     | 是       | 用户电话号码     |
| like        | tinyint(1) | 是       | 1表示赞，0表示踩 |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": null
}
~~~

#### 备注

除在 `healthguide_forum_post_topic_reply_like` 新增一个记录之外，还需要修改`healthguide_forum_post_topic_reply` 表中对应主题贴的赞踩计数。



### PUT  /api/forum/post/reply/like/{replyId}  赞/踩修改

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

同时需要修改 `healthguide_forum_post_topic_reply` 表中对应主题贴的赞踩计数。



### DELETE  /api/forum/post/reply/like/{replyId}  取消赞踩

**路由参数 URL Params**


| Key     | Value  | Required | Description |
| ------- | ------ | -------- | ----------- |
| replyId | bigint | 是       | 贴子id      |

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

除在 `healthguide_forum_post_topic_reply_like` 删除一个记录之外，同时需要修改 `healthguide_forum_post_topic_reply` 表中对应主题贴的赞踩计数。



## 数据表定义

### healthguide_forum_post_topic_reply

```mysql
create table `healthguide_forum_post_topic_reply` (
  `reply_id` bigint not null auto_increment comment '回复ID',
  `topic_id` bigint not null comment '主题贴ID',
  `user_name` varchar(40) not null comment '用户姓名',
  `user_phone` varchar(40) not null comment '用户ID',
  `floor` int not null comment '楼层',
  `content` text not null comment '内容',
  `like_cnt` int default 0 not null comment '点赞数',
  `dislike_cnt` int default 0 not null comment '点踩数',
  `create_time` datetime not null DEFAULT CURRENT_TIMESTAMP comment '创建时间',
  `update_time` datetime not null DEFAULT CURRENT_TIMESTAMP comment '最后编辑时间' ON UPDATE CURRENT_TIMESTAMP,
  primary key (`reply_id`),
  key `idx_topicid` (`topic_id`)
) engine=InnoDB default charset=utf8mb4 comment='健康论坛贴子回复';

insert into healthguide_forum_post_topic_reply (reply_id, topic_id, user_name, user_phone, floor, content, like_cnt, dislike_cnt) values 
('11','1','卢本伟','18888888888','1','大家好1L','2','1'),
('12','1','卢本伟','18888888888','2','大家好2L','1','3');

select * from healthguide_forum_post_topic_reply;
drop table `healthguide_forum_post_topic_reply`;
```



### healthguide_forum_post_topic_reply_like

```mysql
create table `healthguide_forum_post_topic_reply_like` (
   	`reply_id` bigint not null comment '被收藏的回复ID',
    `user_phone` varchar(40) not null comment '赞踩者ID',
    `like` tinyint(1) not null comment '1表示赞 0表示踩',
    primary key (`reply_id`, `user_phone`)
) engine=InnoDB default charset=utf8mb4 comment='健康论坛主题贴回复赞踩情况';

insert into healthguide_forum_post_topic_reply_like (reply_id, user_phone, `like`) values
('1','18888888888', '1'),
('2','18888888888', '0');

select * from healthguide_forum_post_topic_reply_like;
drop table `healthguide_forum_post_topic_reply_like`;
```


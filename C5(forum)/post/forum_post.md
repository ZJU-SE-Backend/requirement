## 需求描述


### 需求介绍

论坛主题贴的发布与浏览。

### API列表

| 请求类型 | PATH                                      | 描述                         |
| -------- | ----------------------------------------- | ---------------------------- |
| GET      | /api/forum/post                           | 查询主题贴列表               |
| GET      | /api/forum/post/{topicId}                 | 查询单个主题贴的信息         |
| GET      | /api/forum/post/user/{userPhone}          | 查询用户发出的贴子           |
| POST     | /api/forum/post                           | 新增主题贴                   |
| PUT      | /api/forum/post/{topicId}                 | 贴子内容编辑                 |
| PUT      | /api/forum/post/addViewCnt/{topicId}      | 增加贴子浏览数               |
| DELETE   | /api/forum/post/{topicId}                 | 删除贴子                     |
| GET      | /api/forum/post/like/{topicId}            | 返回用户对该主题贴的赞踩信息 |
| POST     | /api/forum/post/like/{topicId}            | 新增赞/踩                    |
| PUT      | /api/forum/post/like/{topicId}            | 赞/踩修改                    |
| DELETE   | /api/forum/post/like/{topicId}            | 取消赞踩                     |
| GET      | /api/forum/post/favorite/{topicId}        | 返回用户是否收藏了该主题贴   |
| GET      | /api/forum/post/favorite/user/{userPhone} | 返回用户收藏的贴子           |
| POST     | /api/forum/post/favorite/{topicId}        | 用户新增收藏                 |
| DELETE   | /api/forum/post/favorite/{topicId}        | 用户取消收藏                 |



## API文档

### GET /api/forum/post 查询主题贴列表

#### Request

**查询参数 Query Params**


| Key         | Value | Required | Description      |
| ----------- | ----- | -------- | ---------------- |
| pageSize    | int   | 是       | 分页中一页的容量 |
| pageNo  	  | int   | 是       | 需要获取页的序数 |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": {
        "total": 2,
		"posts": [
		{
            "topicId": 1,
			"title": "今天天气不错",
			"userPhone": "13912345678",
			"userName": "我是谁",
			"viewCnt": 123,
			"replyCnt": 4,
            "likeCnt": 12,
        	"dislikeCnt": 0,
			"lastEditTime": timestamp
		},
		{
            "topicId": 2,
			"title": "想吃好吃的",
			"userPhone": "13312345678",
			"userName": "皮卡丘",
			"viewCnt": 456,
			"replyCnt": 7,
            "likeCnt": 1,
        	"dislikeCnt": 12,
			"lastEditTime": timestamp
	    	},
    	]
    }
}
~~~



### GET /api/forum/post/{topicId} 查询单个主题贴的信息

#### Request

**路由参数 URL Params**


| Key     | Value  | Required | Description |
| ------- | ------ | -------- | ----------- |
| topicId | bigint | 是       | 贴子id      |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data":
    {
        "title": "想吃好吃的",
        "userPhone": "13312345678",
        "userName": "皮卡丘",
        "content": "上课饿了，想吃麦斯威的薯条！",
        "lastEditTime": timestamp,
        "viewCnt": 456,
        "replyCnt": 7,
        "likeCnt": 12,
        "dislikeCnt": 0
    }
}
~~~



### GET /api/forum/post/user/{userPhone} 查询用户发出的贴子

#### Request

**路由参数 URL Params**


| Key         | Value  | Required | Description  |
| ----------- | ------ | -------- | ------------ |
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
            "topicId": 1,
			"title": "今天天气不错",
			"viewCnt": 123,
			"replyCnt": 4,
            "likeCnt": 1,
        	"dislikeCnt": 12,
			"lastEditTime": timestamp
		},
		{
            "topicId": 2,
			"title": "想吃好吃的",
			"viewCnt": 456,
			"replyCnt": 7,
            "likeCnt": 1,
        	"dislikeCnt": 12,
			"lastEditTime": timestamp
	    	},
    	]
    }
}
~~~



### POST /api/forum/post 新增主题贴

#### Request

**请求头部 Header**

| Key       | Value       | Required | Description |
| --------- | ----------- | -------- | ----------- |
| title     | varchar(40) | 是       | 文章标题    |
| userName    | varchar(40) | 是       | 用户        |
| userPhone | varchar(40) | 是       | 用户ID      |
| content   | text        | 是       | 内容        |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": null
}
~~~



### PUT /api/forum/post/{topicId} 贴子内容编辑

#### Request

**路由参数 URL Params**


| Key     | Value  | Required | Description |
| ------- | ------ | -------- | ----------- |
| topicId | bigint | 是       | 贴子id      |

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



### PUT /api/forum/post/addViewCnt/{topicId} 增加贴子浏览数

#### Request

**路由参数 URL Params**


| Key     | Value  | Required | Description |
| ------- | ------ | -------- | ----------- |
| topicId | bigint | 是       | 贴子id      |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": null
}
~~~



### DELETE /api/forum/post/{topicId} 删除贴子

#### Request

**路由参数 URL Params**


| Key     | Value  | Required | Description |
| ------- | ------ | -------- | ----------- |
| topicId | bigint | 是       | 贴子id      |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": null
}
~~~



### GET  /api/forum/post/like/{topicId}  返回对该主题贴的赞踩信息

**路由参数 URL Params**


| Key     | Value  | Required | Description |
| ------- | ------ | -------- | ----------- |
| topicId | bigint | 是       | 贴子id      |

**查询参数 Query Params**


| Key       | Value  | Required | Description  |
| --------- | ------ | -------- | ------------ |
| userPhone | bigint | 是       | 用户电话号码 |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": true // or false or null
}
~~~

#### 备注

数据库记录为1返回true，为0返回false，无记录返回null



### POST  /api/forum/post/like/{topicId}  新增赞/踩

**路由参数 URL Params**


| Key     | Value  | Required | Description |
| ------- | ------ | -------- | ----------- |
| topicId | bigint | 是       | 贴子id      |

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

前端会通过GET的结果控制不重复点赞，这样后端就不用特别处理了（应该是吧x）。PUT同理。

除在 `healthguide_forum_post_topic_like` 新增一个记录之外，还需要修改`healthguide_forum_post_topic` 表中对应主题贴的赞踩计数。



### DELETE  /api/forum/post/like/{topicId}  取消赞踩

**路由参数 URL Params**


| Key     | Value  | Required | Description |
| ------- | ------ | -------- | ----------- |
| topicId | bigint | 是       | 贴子id      |

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

除在 `healthguide_forum_post_topic_like` 删除一个记录之外，同时需要修改 `healthguide_forum_post_topic` 表中对应主题贴的赞踩计数。



### GET  /api/forum/post/favorite/{topicId}  返回用户是否收藏了该主题贴

**路由参数 URL Params**


| Key     | Value  | Required | Description |
| ------- | ------ | -------- | ----------- |
| topicId | bigint | 是       | 贴子id      |

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



### GET  /api/forum/post/favorite/user/{userPhone}  返回用户收藏的贴子

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
            {"topicId": 1, "title": "测试", "favoriteTime": timestamp},
            {"topicId": 2, "title": "你好2", "favoriteTime": timestamp}
        ]
    }
}
~~~

#### 备注

按收藏日期倒序排序、分页。



### POST  /api/forum/post/favorite/{topicId}  用户新增收藏

**路由参数 URL Params**


| Key     | Value  | Required | Description |
| ------- | ------ | -------- | ----------- |
| topicId | bigint | 是       | 贴子id      |

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



### DELETE  /api/forum/post/favorite/{topicId}  用户取消收藏

**路由参数 URL Params**


| Key     | Value  | Required | Description |
| ------- | ------ | -------- | ----------- |
| topicId | bigint | 是       | 贴子id      |

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

### healthguide_forum_post_topic

```sql
create table `healthguide_forum_post_topic` (
  `topic_id` bigint not null auto_increment comment '主题贴ID',
  `title` varchar(40) not null comment '标题',
  `user_name` varchar(40) not null comment '用户姓名',
  `user_phone` varchar(40) not null comment '用户ID',
  `content` text not null comment '内容',
  `reply_cnt` int default 0 not null comment '回复数',
  `view_cnt` int default 0 not null comment '浏览数',
  `like_cnt` int default 0 not null comment '点赞数',
  `dislike_cnt` int default 0 not null comment '点踩数',
  `create_time` datetime not null DEFAULT CURRENT_TIMESTAMP comment '创建时间',
  `update_time` datetime not null DEFAULT CURRENT_TIMESTAMP comment '最后编辑时间' ON UPDATE CURRENT_TIMESTAMP,
  primary key (`topic_id`),
  key `idx_userphone` (`user_phone`)
) engine=InnoDB default charset=utf8mb4 comment='健康论坛主题贴';

insert into healthguide_forum_post_topic (title, user_name, user_phone, content, reply_cnt, view_cnt, like_cnt, dislike_cnt) values 
('你好','卢本伟','18888888888','大家好','1','1','1','0'),
('你好2','卢本伟','18888888888','大家好2','1','1','0','1');                                                                                                           
select * from healthguide_forum_post_topic;
drop table `healthguide_forum_post_topic`;
```



### healthguide_forum_post_topic_like

```mysql
create table `healthguide_forum_post_topic_like` (
   	`topic_id` bigint not null comment '主题贴ID',
    `user_phone` varchar(40) not null comment '赞踩者ID',
    `like` tinyint(1) not null comment '1表示赞 0表示踩',
    primary key (`topic_id`, `user_phone`)
) engine=InnoDB default charset=utf8mb4 comment='健康论坛主题贴赞踩情况';

insert into healthguide_forum_post_topic_like (topic_id, user_phone, `like`) values
('1','18888888888', '1'),
('2','18888888888', '0');

select * from healthguide_forum_post_topic_like;
drop table `healthguide_forum_post_topic_like`;
```



### healthguide_forum_post_topic_favorite

```mysql
create table `healthguide_forum_post_topic_favorite` (
   	`topic_id` bigint not null comment '主题贴ID',
    `user_phone` varchar(40) not null comment '收藏者ID',
    `title` varchar(40) not null comment '标题',
    `favorite_time` datetime not null DEFAULT CURRENT_TIMESTAMP comment '收藏时间',
    primary key (`topic_id`, `user_phone`),
    key `idx_userphone` (`user_phone`)
) engine=InnoDB default charset=utf8mb4 comment='健康论坛主题贴收藏情况';

insert into healthguide_forum_post_topic_favorite (topic_id, user_phone, title) values
('1','18888888888', '修改'),
('2','18888888888', '你好2');

select * from healthguide_forum_post_topic_favorite;
drop table `healthguide_forum_post_topic_favorite`;
```



## 测试样例（by后端）

### **GET**/api/forum/post

每页显示1条记录，取出第1页：

```json
pageSize = 1
pageNo = 1

Response body
{
  "st": 0,
  "msg": "",
  "data": {
    "total": 2,
    "posts": [
      {
        "title": "你好",
        "userPhone": "18888888888",
        "userName": "卢本伟",
        "viewCnt": 1,
        "replyCnt": 1,
        "lastEditTime": 1619266204
      }
    ]
  }
}
```

每页显示1条记录，取出第2页：

```json
pageSize = 1
pageNo = 2

Response body
{
  "st": 0,
  "msg": "",
  "data": {
    "total": 2,
    "posts": [
      {
        "title": "你好2",
        "userPhone": "18888888888",
        "userName": "卢本伟",
        "viewCnt": 1,
        "replyCnt": 1,
        "lastEditTime": 1619266204
      }
    ]
  }
}
```

每页显示2条记录，取出第1页：

```json
pageSize = 1
pageNo = 2

Response body
{
  "st": 0,
  "msg": "",
  "data": {
    "total": 2,
    "posts": [
      {
        "title": "你好",
        "userPhone": "18888888888",
        "userName": "卢本伟",
        "viewCnt": 1,
        "replyCnt": 1,
        "lastEditTime": 1619266204
      },
      {
        "title": "你好2",
        "userPhone": "18888888888",
        "userName": "卢本伟",
        "viewCnt": 1,
        "replyCnt": 1,
        "lastEditTime": 1619266204
      }
    ]
  }
}
```

每页显示2条记录，取出第3页（超出页面范围）：

```json
pageSize = 1
pageNo = 2

Response body
{
  "st": 0,
  "msg": "",
  "data": {
    "total": 2,
    "posts": []
  }
}
```



### **GET**/api/forum/post/{id}

成功查询：

```json
id = 1

Response body
{
  "st": 0,
  "msg": "",
  "data": {
    "title": "你好",
    "userName": "卢本伟",
    "userPhone": "18888888888",
    "content": "大家好",
    "replyCnt": 1,
    "viewCnt": 1,
    "likeCnt": 1,
    "dislikeCnt": 0,
    "updateTime": 1619266204
  }
}
```

未找到：

```json
id = 100

Response body
{
  "st": 1,
  "msg": "数据不一致",
  "data": null
}
```



### **POST**/api/forum/post

成功发布：

```json
Request body
{
  "session": "string",
  "title": "string",
  "userName": "string",
  "userPhone": "string",
  "content": "string"
}
	
Response body
{
  "st": 0,
  "msg": "",
  "data": null
}
```



### **PUT**/api/forum/post/{id}

修改成功，数据库验证正确：

```json
id = 1

Request body
{
  "title": "修改",
  "content": "string"
}

Response body
{
  "st": 0,
  "msg": "",
  "data": null
}
```

修改失败，未找到id：

```json
id = 0

Request body
{
  "title": "修改",
  "content": "string"
}

Response body
{
  "st": 1,
  "msg": "数据不一致",
  "data": null
}
```



### **DELETE**/api/forum/post/{id}

删除成功，数据库验证正确：

```json
id = 1

Response body
{
  "st": 0,
  "msg": "",
  "data": null
}
```

删除失败，未找到id：

```json
id = 0

Response body
{
  "st": 1,
  "msg": "数据不一致",
  "data": null
}
```




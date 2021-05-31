## 需求描述


### 需求介绍

论坛主题贴的发布与浏览。

### API列表

| 请求类型 | PATH                               | 描述                   |
| -------- | ---------------------------------- | ---------------------- |
| GET      | /api/forum/post                    | 查询主题贴列表         |
| GET      | /api/forum/post/{id}               | 查询单个主题贴的信息   |
| GET      | /api/forum/post/user/{authorPhone} | 查询某用户发的所有贴子 |
| POST     | /api/forum/post                    | 新增主题贴             |
| PUT      | /api/forum/post/{id}               | 贴子内容编辑           |
| PUT      | /api/forum/post/addViewCnt/{id}    | 增加贴子浏览数         |
| DELETE   | /api/forum/post/{id}               | 删除贴子               |



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
			"authorId": "13912345678",
			"author": "我是谁",
			"viewCnt": 123,
			"replyCnt": 4,
            "likeCnt": 12,
        	"dislikeCnt": 0,
			"lastEditTime": timestamp
		},
		{
            "topicId": 2,
			"title": "想吃好吃的",
			"authorId": "13312345678",
			"author": "皮卡丘",
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



### GET /api/forum/post/{id} 查询单个主题贴的信息

#### Request

**路由参数 URL Params**


| Key  | Value  | Required | Description |
| ---- | ------ | -------- | ----------- |
| id   | bigint | 是       | 贴子id      |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data":
    {
        "title": "想吃好吃的",
        "authorId": "13312345678",
        "author": "皮卡丘",
        "content": "上课饿了，想吃麦斯威的薯条！",
        "lastEditTime": timestamp,
        "viewCnt": 456,
        "replyCnt": 7,
        "likeCnt": 12,
        "dislikeCnt": 0
    }
}
~~~



### GET /api/forum/post/user/{authorPhone} 查询某用户的所有贴子

#### Request

**路由参数 URL Params**


| Key         | Value  | Required | Description  |
| ----------- | ------ | -------- | ------------ |
| authorPhone | bigint | 是       | 作者电话号码 |

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
| session   | varchar(10) | 是       | 发布板块    |
| title     | varchar(40) | 是       | 文章标题    |
| author    | varchar(40) | 是       | 作者        |
| authorId | varchar(40) | 是       | 作者ID      |
| content   | text        | 是       | 内容        |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": null
}
~~~



### PUT /api/forum/post/{id} 贴子内容编辑

#### Request

**路由参数 URL Params**


| Key  | Value  | Required | Description |
| ---- | ------ | -------- | ----------- |
| id   | bigint | 是       | 贴子id      |

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



### PUT /api/forum/post/addViewCnt/{id} 增加贴子浏览数

#### Request

**路由参数 URL Params**


| Key  | Value  | Required | Description |
| ---- | ------ | -------- | ----------- |
| id   | bigint | 是       | 贴子id      |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": null
}
~~~



### DELETE /api/forum/post/{id} 删除贴子

#### Request

**路由参数 URL Params**


| Key  | Value  | Required | Description |
| ---- | ------ | -------- | ----------- |
| id   | bigint | 是       | 贴子id      |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": null
}
~~~



## 数据表定义

```sql
create table `forum_post_topic` (
  `topic_id` bigint not null auto_increment comment '主题贴ID',
  `session` varchar(10) not null comment '所属版块',
  `title` varchar(40) not null comment '标题',
  `author_name` varchar(40) not null comment '作者姓名',
  `author_phone` varchar(40) not null comment '作者ID',
  `content` text not null comment '内容',
  `reply_cnt` int default 0 not null comment '回复数',
  `view_cnt` int default 0 not null comment '浏览数',
  `like_cnt` int default 0 not null comment '点赞数',
  `dislike_cnt` int default 0 not null comment '点踩数',
  `create_time` datetime not null DEFAULT CURRENT_TIMESTAMP comment '创建时间',
  `update_time` datetime not null DEFAULT CURRENT_TIMESTAMP comment '最后编辑时间' ON UPDATE CURRENT_TIMESTAMP,
  primary key (`topic_id`)
) engine=InnoDB default charset=utf8mb4 comment='健康论坛主题贴';

insert into healthguide_forum_post_topic (session, title, author_name, author_phone, content, reply_cnt, view_cnt, like_cnt, dislike_cnt) values 
('病情讨论','你好','卢本伟','18888888888','大家好','1','1','1','0'),
('病情讨论','你好2','卢本伟','18888888888','大家好2','1','1','0','1');                                                                                                           
select * from healthguide_forum_post_topic;
drop table `healthguide_forum_post_topic`;
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
        "authorId": "18888888888",
        "authorName": "卢本伟",
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
        "authorId": "18888888888",
        "authorName": "卢本伟",
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
        "authorId": "18888888888",
        "authorName": "卢本伟",
        "viewCnt": 1,
        "replyCnt": 1,
        "lastEditTime": 1619266204
      },
      {
        "title": "你好2",
        "authorId": "18888888888",
        "authorName": "卢本伟",
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
    "authorName": "卢本伟",
    "authorPhone": "18888888888",
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
  "authorName": "string",
  "authorPhone": "string",
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




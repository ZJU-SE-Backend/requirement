## 需求描述


### 需求介绍

论坛主题贴的发布与浏览。

### API列表

| 请求类型 | PATH                 | 描述                       |
| -------- | -------------------- | -------------------------- |
| GET      | /api/forum/post      | 查询主题贴列表             |
| GET      | /api/forum/post/{id} | 查询单个主题贴的信息       |
| POST     | /api/forum/post      | 新增主题贴                 |
| PUT      | /api/forum/post/{id} | 贴子内容编辑（可晚点实现） |
| DELETE   | /api/forum/post/{id} | 删除贴子（可晚点实现）     |



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
        	"post_cnt": 2,
		"posts": [
		{
			"title": "今天天气不错",
			"authorId": "13912345678",
			"author": "我是谁",
			"viewCnt": 123,
			"replyCnt": 4,
			"lastEditTime": timestamp
		},
		{
			"title": "想吃好吃的",
			"authorId": "13312345678",
			"author": "皮卡丘",
			"viewCnt": 456,
			"replyCnt": 7,
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
	"data": {
        "result": true
    }
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
	"data": {
        "result": true
    }
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
	"data": {
        "result": true
    }
}
~~~



## 数据表定义

```sql
create table `forum_post_topic` (
                                    `topic_id` bigint not null auto_increment comment '主题贴ID',
                                    `session` varchar(10) not null comment '所属版块',
                                    `title` varchar(40) not null comment '标题',
                                    `author_name` varchar(40) not null comment '作者姓名',
                                    `author_id` varchar(40) not null comment '作者ID',
                                    `content` text not null comment '内容',
                                    `reply_cnt` int not null comment '回复数',
                                    `view_cnt` int not null comment '浏览数',
                                    `like_cnt` int not null comment '点赞数',
                                    `dislike_cnt` int not null comment '点踩数',
                                    `create_time` datetime not null DEFAULT CURRENT_TIMESTAMP comment '创建时间',
                                    `update_time` datetime not null DEFAULT CURRENT_TIMESTAMP comment '最后编辑时间' ON UPDATE CURRENT_TIMESTAMP,
                                    primary key (`topic_id`)
) engine=InnoDB default charset=utf8mb4 comment='健康论坛主题贴';
```



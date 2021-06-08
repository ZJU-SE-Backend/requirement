## 需求描述

### 需求介绍

管理员对**回答**进行推荐。

操作者是否是管理员由前端检查。

### API列表

| 请求类型 | PATH                                 | 描述             |
| -------- | ------------------------------------ | ---------------- |
| GET      | /api/forum/qa/recommended            | 获取推荐回答列表 |
| POST     | /api/forum/qa/recommended/{answerId} | 新增推荐         |
| DELETE   | /api/forum/qa/recommended/{answerId} | 删除推荐         |



## API文档

### GET  /api/forum/qa/recommended  获取推荐回答列表

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
		"qas": [
		{
			"answerId": 1,
            "title": "不懂就问",
            "content": "问得好"
		},
		{
			"answerId": 2,
            "title": "就是想问",
            "content": "问得妙"
        },
    	]
    }
}
~~~



### POST  /api/forum/qa/recommended/{answerId}  新增推荐

**路由参数 URL Params**


| Key      | Value  | Required | Description |
| -------- | ------ | -------- | ----------- |
| answerId | bigint | 是       | 回答的ID    |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": null
}
~~~



### DELETE  /api/forum/qa/recommended/{answerId}  删除推荐

**路由参数 URL Params**


| Key      | Value  | Required | Description |
| -------- | ------ | -------- | ----------- |
| answerId | bigint | 是       | 回答的ID    |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": null
}
~~~



## 数据表定义

### healthguide_forum_qa_recommended

```mysql
create table `healthguide_forum_qa_recommended` (
    `answer_id` bigint not null comment '被推荐回答ID',
    `title` varchar(40) not null comment '回答对应的问题标题',
    `content` varchar(40) not null comment '回答内容简介',
    primary key (`answer_id`)
) engine=InnoDB default charset=utf8mb4 comment='健康论坛回答推荐';

insert into healthguide_forum_qa_recommended (answer_id, title, content) values
('1', '不懂就问', '问得好'),
('2', '就是想问', '问得妙');

select * from healthguide_forum_qa_recommended;
drop table `healthguide_forum_qa_recommended`;
```


## 需求描述

### 需求介绍

论坛回答、回复的举报功能



### API列表

| 请求类型 | PATH                                        | 描述                     |
| -------- | ------------------------------------------- | ------------------------ |
| GET      | /api/forum/report/post/reply                | 查看被举报的贴文回复列表 |
| POST     | /api/forum/report/post/reply/{replyId}      | 举报贴文回复             |
| DELETE   | /api/forum/report/post/reply/{replyId}      | 删除举报                 |
| GET      | /api/forum/report/qa/answer                 | 查看被举报的回答列表     |
| POST     | /api/forum/report/qa/answer/{answerId}      | 举报回答                 |
| DELETE   | /api/forum/report/qa/answer/{answerId}      | 删除举报                 |
| GET      | /api/forum/report/qa/answer/reply           | 查看被举报的回答回复列表 |
| POST     | /api/forum/report/qa/answer/reply/{replyId} | 举报回答回复             |
| DELETE   | /api/forum/report/qa/answer/reply/{replyId} | 删除举报                 |



## API文档

### GET  /api/forum/report/post/reply  查看被举报的贴文回复列表

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
		"posts": 
        [
            {"replyId": 1, "content": "哈哈哈"},
            {"replyId": 2, "content": "嘿嘿嘿"}
        ]
    }
}
~~~

#### 

### POST  /api/forum/report/post/reply/{replyId}  举报贴文回复

**路由参数 URL Params**


| Key     | Value  | Required | Description  |
| ------- | ------ | -------- | ------------ |
| replyId | bigint | 是       | 贴文回复的ID |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": null
}
~~~

#### 

### DELETE  /api/forum/report/post/reply/{replyId}  删除举报

**路由参数 URL Params**


| Key     | Value  | Required | Description  |
| ------- | ------ | -------- | ------------ |
| replyId | bigint | 是       | 贴文回复的ID |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": null
}
~~~

#### 

### GET  /api/forum/report/qa/answer  查看被举报的回答列表

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
		"posts": 
        [
            {"answerId": 1, "content": "哈哈哈"},
            {"answerId": 2, "content": "嘿嘿嘿"}
        ]
    }
}
~~~

#### 

### POST  /api/forum/report/qa/answer/{answerId}  举报回答

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

#### 

### DELETE  /api/forum/report/qa/answer/{answerId}  删除举报

**路由参数 URL Params**


| Key     | Value  | Required | Description |
| ------- | ------ | -------- | ----------- |
| replyId | bigint | 是       | 回答的ID    |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": null
}
~~~

#### 

### GET  /api/forum/report/qa/answer/reply  查看被举报的回答回复列表

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
		"posts": 
        [
            {"replyId": 1, "content": "哈哈哈"},
            {"replyId": 2, "content": "嘿嘿嘿"}
        ]
    }
}
~~~

#### 

### POST  /api/forum/report/qa/answer/reply/{replyId}  举报回答回复

**路由参数 URL Params**


| Key     | Value  | Required | Description  |
| ------- | ------ | -------- | ------------ |
| replyId | bigint | 是       | 回答回复的ID |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": null
}
~~~

#### 

### DELETE  /api/forum/report/qa/answer/reply/{replyId}  删除举报

**路由参数 URL Params**


| Key     | Value  | Required | Description  |
| ------- | ------ | -------- | ------------ |
| replyId | bigint | 是       | 贴文回复的ID |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": null
}
~~~

#### 

## 数据表定义

### healthguide_forum_report_post_reply

```mysql
create table `healthguide_forum_report_post_reply` (
    `reply_id` bigint not null comment '被举报贴子回复ID',
    `content` varchar(40) not null comment '被举报回复内容简介',
    primary key (`reply_id`)
) engine=InnoDB default charset=utf8mb4 comment='健康论坛举报的贴文回复';

insert into healthguide_forum_report_post_reply (reply_id, content) values ('1', '哈哈哈'), ('2', '嘿嘿嘿');

select * from healthguide_forum_report_post_reply;
drop table `healthguide_forum_report_post_reply`;
```



### healthguide_forum_report_qa_answer

```mysql
create table `healthguide_forum_report_qa_answer` (
    `answer_id` bigint not null comment '被举报回答ID',
    `content` varchar(40) not null comment '被举报回答内容简介',
    primary key (`answer_id`)
) engine=InnoDB default charset=utf8mb4 comment='健康论坛举报的回答';

insert into healthguide_forum_report_qa_answer (answer_id, content) values ('1', '哈哈哈'), ('2', '嘿嘿嘿');

select * from healthguide_forum_report_qa_answer;
drop table `healthguide_forum_report_qa_answer`;
```



### healthguide_forum_report_qa_answer_reply

```mysql
create table `healthguide_forum_report_qa_answer_reply` (
    `reply_id` bigint not null comment '被举报回答回复ID',
    `content` varchar(40) not null comment '被举报回复内容简介',
    primary key (`reply_id`)
) engine=InnoDB default charset=utf8mb4 comment='健康论坛举报的回答回复';

insert into healthguide_forum_report_qa_answer_reply (reply_id, content) values ('1', '哈哈哈'), ('2', '嘿嘿嘿');

select * from healthguide_forum_report_qa_answer_reply;
drop table `healthguide_forum_report_qa_answer_reply`;
```


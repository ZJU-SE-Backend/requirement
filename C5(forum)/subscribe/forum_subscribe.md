## 需求描述

### 需求介绍

用户关注其他用户。

user - 订阅者，author - 被订阅者。

### API列表

| 请求类型 | PATH                                  | 描述               |
| -------- | ------------------------------------- | ------------------ |
| GET      | /api/forum/subscribe/user/{userPhone} | 获取某用户关注列表 |
| POST     | /api/forum/subscribe/{authorPhone}    | 新增关注           |
| DELETE   | /api/forum/subscribe/{authorPhone}    | 取消关注           |



## API文档

### GET  /api/forum/subscribe/user/{userPhone}  获取某用户关注列表

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
		"subList": [
		{
			"authorPhone": "18888888888",
            "answerCnt": 12,
		},
		{
			"authorPhone": "18888988888",
            "answerCnt": 13,
        },
    	]
    }
}
~~~





### POST  /api/forum/subscribe/{authorPhone}  新增关注

#### Request

**路由参数 URL Params**


| Key         | Value  | Required | Description      |
| ----------- | ------ | -------- | ---------------- |
| ahthorPhone | bigint | 是       | 被订阅者电话号码 |

**请求头部 Header**


| Key       | Value  | Required | Description  |
| --------- | ------ | -------- | ------------ |
| userPhone | bigint | 是       | 用户电话号码 |

#### Response

```json
{
	"st": 0,
	"msg": "",
	"data": null
}
```



### DELETE  /api/forum/subscribe/{authorPhone}  取消关注

#### Request

**路由参数 URL Params**


| Key         | Value  | Required | Description      |
| ----------- | ------ | -------- | ---------------- |
| ahthorPhone | bigint | 是       | 被订阅者电话号码 |

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

### healthguide_forum_subscribe

```mysql
create table `healthguide_forum_subscribe` (
    `author_phone` varchar(40) not null comment '被订阅者ID',
	`user_phone` varchar(40) not null comment '订阅者ID',
    `answer_cnt` int not null comment '在订阅时被订阅者的回答数',
    primary key (`user_phone`, `author_phone`),
    key `idx_userphone` (`user_phone`)
) engine=InnoDB default charset=utf8mb4 comment='健康论坛关注用户';

insert into healthguide_forum_subscribe (author_phone, user_phone, answer_cnt) values
('18888888888', '18888988888', '12'),
('18888988888', '18888888888', '13');

select * from healthguide_forum_subscribe;
drop table `healthguide_forum_subscribe`;
```

#### 备注

和收藏问题类似的逻辑，也是前端需要比较回答数判断是否有新回答。


## 需求描述

### 需求介绍

进行一些简单的数据统计，如用户发帖量等。

### API列表

| 请求类型 | PATH                                              | 描述             |
| -------- | ------------------------------------------------- | ---------------- |
| GET      | /api/forum/statistic/user/postCnt/{userPhone}     | 查询用户发帖量   |
| GET      | /api/forum/statistic/user/questionCnt/{userPhone} | 查询用户提问数量 |
| GET      | /api/forum/statistic/user/answerCnt/{userPhone}   | 查询用户回答数量 |



## API文档

### GET  /api/forum/statistic/user/postCnt/{userPhone}  查询用户发帖量

#### Request

**路由参数 URL Params**


| Key       | Value  | Required | Description  |
| --------- | ------ | -------- | ------------ |
| userPhone | bigint | 是       | 用户电话号码 |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": 2	// x >= 0
}
~~~



### GET  /api/forum/statistic/user/questionCnt/{userPhone}  查询用户提问数量

#### Request

**路由参数 URL Params**


| Key       | Value  | Required | Description  |
| --------- | ------ | -------- | ------------ |
| userPhone | bigint | 是       | 用户电话号码 |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": 2	// x >= 0
}
~~~



### GET  /api/forum/statistic/user/answerCnt/{userPhone}  查询用户回答数量

#### Request

**路由参数 URL Params**


| Key       | Value  | Required | Description  |
| --------- | ------ | -------- | ------------ |
| userPhone | bigint | 是       | 用户电话号码 |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": 2	// x >= 0
}
~~~




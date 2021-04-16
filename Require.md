# 需求规范

每个小组提出自己的新需求时，应在对应小组文件夹下的添加需求文件，并按照下列规则描述需求、API、数据表。以需求为单位单独形成文档。

命名空间介绍——P.S.M规范：

P代表product，总体工作内容视为一个product

S代表subsys，每个小组分到的工作视为一个subsys，

M代表module，工作中设计的不同模块视为一个module。

因此一个完整的P.S.M命名空间示例：dingtalk.im.history

代表**钉钉产品**的**即时通信子系统**的**历史记录模块**

P.S.M是后端命名的规范

## 一、需求描述

需求描述分为两个部分：需求介绍、API列表。样例如下：

### 需求介绍：

P.S.M:	product.subsys.module

一个商家数据管理界面，主要功能包括商家信息的增删查改。

### API列表：


| 请求类型 | PATH            | 描述               |
| ---------- | ----------------- | -------------------- |
| GET      | /api/p/s/m/{id} | 查询单个商家的信息 |
| GET      | /api/p/s/m/     | 查询商家列表       |
| POST     | /api/p/s/m/{id} | 新增商家信息       |
| PUT      | /api/p/s/m/{id} | 修改商家信息       |
| DELETE   | /api/p/s/m/{id} | 删除商家信息       |

## 二、API文档

提出需求时，小组需要以需求为单位建立完整的RESTful API文档。

样例如下:

### GET	/api/product/subsys/module/ \{id\}	根据Shop ID查询商家数据

---

#### Request

**请求头部 Header**


| Key | Value | Required | Description |
| ----- | ------- | ---------- | ------------- |
| 无  | 无    | 无       | 无          |

**路由参数 URL Params**


| Key | Value | Required | Description |
| ----- | ------- | ---------- | ------------- |
| Id  | int   | 是       | 商家Id      |

**查询参数 Query Parames**


| Key | Value | Required | Description |
| ----- | ------- | ---------- | ------------- |
| 无  | 无    | 无       | 无          |

---

#### Response

**状态码 Status Code: 200**

描述 Description：请求成功

~~~json
{
	"st": 0,
	"msg": "",
	"data":{
		"shopId": "3100100000",
		"onwerId": "10086",
    "shopName": "苹果旗舰店"
	}
}
~~~

**状态码 Status Code: 200**

描述 Description：暂无数据

~~~json
{
	"st": 1,
	"msg": "暂无数据",
	"data": null
}
~~~

**状态码 Status Code: 200**

描述 Description：请求参数格式错误

~~~json
{
	"st": 2,
	"msg": "请求参数格式错误",
	"data": null
}
~~~

---

**注意：**

1. 每个response格式都一定是st、msg、data的形式.
2. 状态码通常使用200，错误状态判别通过st进行判断，msg用于标示错误信息，data用于标示返回数据
3. st的具体定义，参考枚举文档，请不要自己定义st的值，如需新增，请联系后端。
4. api样例中response一般给出正确相应的返回就可以了，找不到数据或者请求格式错误等response格式不需要前端给出。

## 三、数据表定义

如果你的需求需要新的数据表，请给出对应的建表SQL（要求MySQL能正常执行）。



| 规范      | 内容                                                         |
| --------- | ------------------------------------------------------------ |
| table命名 | 按照P.S.M的规范进行，P代表产品名，S代表子服务名称，M代表模块名，表名使用下划线分割，比如dingtalk\_im\_history，代表钉钉这个**产品**下面的**即时通信子系统**用到的**历史记录模块**。 |
| Id        | 不能直接使用简单的id，而是要写的更具体，比如shop\_id，类型一律bigint |
| name      | 不能直接使用简单的name，而是要写的更具体，比如shop\_name，类型一律varchar |
| 数量      | 与数量有关的命名，一般使用cnt，例如：user_cnt                |
| 时间      | 时间参考样表的定义方式，常用时间主要是create\_time与update\_time |
| 索引      | 自主评估并按照样表格式建立索引，索引命名一律以idx_开头，用下划线链接索引项 |
| 外键      | 自主声明，声明后一定要单独与后端联系一下。                   |
| 引擎      | 一般使用InnoDB，可以保证完整的事务性                         |
| 字符集    | 一般使用utf8mb4                                              |
| 注释      | 所有表项与表后都必须有COMMENT                                |

样表：

~~~sql
CREATE TABLE `product_subsys_module` (
  `shop_id` bigint NOT NULL COMMENT '店铺ID',
  `owner_id` bigint NOT NULL COMMENT '商家ID',
  `shop_name` varchar(40) DEFAULT '' COMMENT '店铺中文名',
  `create_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `update_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
  PRIMARY KEY (`shop_id`),
  KEY `idx_ownerid_shopname` (`owner_id`,`shop_name`),
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='商家信息表'
~~~

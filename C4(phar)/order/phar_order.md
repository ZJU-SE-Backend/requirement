## 需求描述


### 需求介绍

获取药品相关信息

### API列表

| 请求类型 | PATH                             | 描述         |
| -------- | -------------------------------- | ------------ |
| GET      | /api/phar/order/list/{userPhone} | 获取订单列表 |
| GET      | /api/phar/order/detail           | 获取订单详情 |
| PUT      | /api/phar/order/changestate      | 更改订单状况 |
| POST     | /api/phar/order/add              | 添加订单     |

## API文档

### GET    /api/phar/order/list/{userPhone}   获取订单列表

#### Request

**路由参数**

| Key       | Value  | Required | Description |
| --------- | ------ | -------- | ----------- |
| userPhone | string | y        | 用户id      |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data":[{
        "orderId": 202106041452,
        "state":"已完成",
        "totalPrice": 47.98
    },{
        "orderId": 202106041444,
        "state":"配送中",
        "totalPrice": 17.99
    }]
}
~~~

### GET /api/phar/order/detail 获取订单详情

#### Request

**请求参数**

```json
{
  
  "orderId": 202106041452
}
```

#### Response

```json
{
	"st": 0,
	"msg": "",
	"data":[
        {
            "medicine_id": 2,
            "amount": 1,
            "price": 13.99
        },
        {
            "medicine_id": 3,
            "amount": 2,
            "price": 23.98
        },
        {
            "medicine_id": 6,
            "amount": 1,
            "price": 44.99
        }
    ]
}
```
### PUT /api/phar/order/changestate 更改订单状况

#### Request

**请求参数**

```json
{
 
  "orderId": 202106041452,
  "state": "已完成"
}
```

#### Response

```json
{
	"st": 0,
	"msg": "",
	"data":{}
}
```
### POST /api/phar/order/add 添加订单

#### Request

**请求参数**

```json
{
  "userPhone": "13777777777",
  "state": "已完成",
  "list": [
      {
          "medicineId": 1, 
          "amount": 2, 
      },
      {
          "medicineId": 6, 
          "amount": 3, 
      },
      {
          "medicineId": 7, 
          "amount": 1, 
      }
  ]
}
```

#### Response

```json
{
	"st": 0,
	"msg": "",
	"data":{}
}
```

## 数据表

**healthguide_phar_order_orders**
存储所有订单信息的总表

~~~sql
drop table `healthguide_phar_order_orders`;
create table `healthguide_phar_order_orders` (
                                               `order_id` bigint not null comment '药品id' primary key auto_increment,
                                               `user_phone` varchar(40) not null comment '电话',
                                               `state` enum('待支付', '配送中', '已完成', '售后处理中') not null comment '订单状态',
                                               `total_price` float not null comment '总价' ,
                                               `create_time` datetime not null DEFAULT CURRENT_TIMESTAMP comment '创建时间',
                                               `update_time` datetime not null DEFAULT CURRENT_TIMESTAMP comment '最后编辑时间' ON UPDATE CURRENT_TIMESTAMP
) engine=InnoDB default charset=utf8mb4 comment='存储所有订单信息的总表';
insert into healthguide_phar_order_orders(user_phone, state, total_price) values
('13777777777', '已完成', 79.10);
select * from healthguide_phar_order_orders;
~~~

**healthguide_phar_order_details**

存放订单详细信息的总表

~~~sql
drop table `healthguide_phar_order_details`;
create table healthguide_phar_order_details (
                                                `order_id` bigint not null comment '订单id',
                                                `medicine_id` bigint not null comment '药品id',
                                                `amount` int not null comment '药品数量',
                                                `price` float not null comment '价格' ,
                                                primary key(order_id, medicine_id),
                                                foreign key(medicine_id) references healthguide_phar_booth_medicine(id)
) engine=InnoDB default charset=utf8mb4 comment='存放订单详细信息的总表';
insert into healthguide_phar_order_details(order_id, medicine_id, amount, price)values
(1, 1, 2, 49.10),
(1, 2, 1, 30.00);
select * from healthguide_phar_order_details;
~~~
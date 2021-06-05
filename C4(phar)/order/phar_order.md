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
  "userPhone": "13777777777",
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
  "userPhone": "13777777777",
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
  "orderId": 202106041452,
  "state": "已完成",
  "list": [
      {
          "medicineId": 1, 
          "amount": 2, 
          "price": 13.99
      },
      {
          "medicineId": 6, 
          "amount": 3, 
          "price": 22.33
      },
      {
          "medicineId": 7, 
          "amount": 1, 
          "price": 55.23
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
create table healthguide_phar_order_orders (
    user_phone varchar(40) not null comment '电话',
	order_id int not null comment '订单id,同时也是时间戳',
    state enum("待支付", "配送中", "已完成", "售后处理中") not null comment '订单状态',
	total_price float not null comment '总价' ,
    primary key(user_phone, order_id),
    foreign key(user_phone) references user_info(user_phone)
) engine=InnoDB default charset=utf8mb4 comment='存储所有订单信息的总表';

insert into healthguide_phar_order_orders(user_phone, order_id, state, total_price)
("13777777777", 202106041452, '已完成', 47.98);
~~~

**healthguide_phar_order_details**

存放订单详细信息的总表

~~~sql
create table healthguide_phar_order_details (
    user_phone varchar(40) not null comment '电话',
	order_id int not null comment '订单id',
	medicine_id int not null comment '药品id',
    amount int not null comment '药品数量',
    price float not null comment '价格' ,
    primary key(user_phone, order_id, medicine_id),
    foreign key(medicine_id) references healthguide_phar_booth_medicine(id)
) engine=InnoDB default charset=utf8mb4 comment='存放订单详细信息的总表';

insert into healthguide_phar_order_details(user_phone, order_id, medicine_id, amount, price)values
("13777777777", 202106041452, 1, 2, 13.99),
("13777777777", 202106041452, 3, 4, 23.88);
~~~
## 需求描述


### 需求介绍

获取药品相关信息

### API列表

| 请求类型 | PATH             | 描述                                     |
| -------- | ---------------- | ---------------------------------------- |
| POST     | /api/phar/list   | 获取药品列表                             |
| GET      | /api/phar/detail | 获取药品详情                             |
| GET      | /api/phar/search | 根据药品名搜索药品（暂时不用做模糊匹配） |

## API文档

### POST  /api/phar/list  获取药品列表

#### Request

**查询参数 Query Parames**

| Key   | Value  | Required | Description |
| ----- | ------ | -------- | ----------- |
| cata  | string | y        | 种类名      |
| count | int    | n        | 查询数      |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data":{
        "length": 20,
        "list":[
            {
            	"id": 1,
        		"name": "阿司匹林",
                "type": "抗生素",
        		"price": 21.99,
                "sIndication": "用于发热、疼痛及类风湿关节炎等"
        	},
            {
            	"id": 2
        		"name": "阿莫西林",
                "type": "抗生素",
        		"price": 24.99,
                "sIndication": "细菌感染"
        	}
    	]
    }
}
~~~

建议数据库操作：

~~~sql
select id, name, type, price, sIndication from healthguide_phar_medicine_list where cata=XXX;
~~~

### GET /api/phar/detail 获取药品详情

#### Request

**URL参数*

| Key  | Value | Required | Description |
| ---- | ----- | -------- | ----------- |
| id   | int   | y        | 药品id      |

#### Response

```json
{
	"st": 0,
	"msg": "",
	"data":{
        "name": "阿莫西林",
        "type": "抗生素",
        "price": 24.99,
        "lIndication": "阿莫西林用以治疗伤寒、其他沙门菌感染和伤寒带菌者可获得满意疗效。",
        "usageAndDosage": "口服：①成人1次0.5g，每6～8小时1次，每日剂量不超过4g；",
        "adr": "过敏反应症状：可出现药物热、荨麻疹、皮疹和哮喘等，尤易发生于传染性单核细胞增多症患者，少见过敏性休克",
        "contraindications": "国家卫生部门规定，使用阿莫西林前必须进行青霉素皮肤试验，阳性反应者禁用。"
    }
}
```

建议数据库操作：

```sql
select name, type, price, lIndication, usageAndDosage, adr, contraindications from healthguide_phar_medicine_list where id=XXX;
```

### GET  /api/phar/search 搜索药品（暂时不做模糊匹配）

#### Request

**URL参数** 

| Key  | Value  | Required | Description |
| ---- | ------ | -------- | ----------- |
| name | string | y        | 药品名称    |

#### Response

```json
{
	"st": 0,
	"msg": "",
	"data":{
        "id": 1,
        "name": "阿司匹林",
        "type": "抗生素",
        "price": 21.99,
        "sIndication": "用于发热、疼痛及类风湿关节炎等"
    }
}
```

建议数据库操作:

```sql
select id, name, type, price, sIndication from healthguide_phar_medicine_list where name=XXX;
```

## 数据表

**healthguide_exam_medicine_list**
存储所有药品信息的总表

~~~sql
create table healthguide_phar_medicine_list (
	id int not null comment '药品id' primary key auto_increment,
	name varchar(40) not null comment '药品名',
    cata varchar(40) not null comment '商品分类',
    type varchar(40) not null comment '药品类型',
    price decimal(10,2) not null comment '药品价格',
    sIndication tinytext comment '简要适用症',
    lIndication text comment '适用症',
    usageAndDosage text comment '用法用量',
    adr text comment '不良反应',
    contraindications text comment '禁忌'
) engine=InnoDB default charset=utf8mb4 comment='存储所有药品信息的总表';
insert into healthguide_phar_medicine_list values
(null, "阿莫西林", "感冒头痛", "抗生素", 24.55, "季节性头痛",
 "季节性头痛", "用法用量", "不良反应", "禁忌");
~~~
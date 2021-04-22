### **一、需求描述**

### **需求介绍**

病人退号界面，即预约记录表的查询与具体某条记录的删除

### **API列表**

| 请求类型 | PATH                                       | 描述                 |
| -------- | ------------------------------------------ | -------------------- |
| GET      | /api/appoinment/register/record/user_phone | 查询预约信息         |
| DELETE   | /api/appoinment/withdraw/record/user_phone | 删除具体某条预约记录 |

### **二、API文档**

### GET   /api/appoinment/register/record/{user_phone}   查询预约信息

#### Request

**路由参数**：

| Key        | Value  | Required | Description                                       |
| ---------- | ------ | -------- | ------------------------------------------------- |
| user_phone | string | y/n      | 用户id （如果后台能根据header自动确定则这个不用） |

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data":[
		["杭州市第一人民医院","2020-4-20 13:13:13","儿科","妙手回春哥"],
		["..."],
		["医院","日期","科室","预约医生姓名"]
	]
}
~~~



### DELETE  /api/appoinment/withdraw/record/user_phone  删除具体某条预约记录

**查询参数Query Params**

| Key        | Value  | Required | Description                                      |
| ---------- | ------ | -------- | ------------------------------------------------ |
| appoint_id | int    | y        | 期望预约时间段编号                               |
| user_phone | string | y/n      | 病人id（如果后台能根据header自动确定则这个不用） |

（预约时间段编号对应了不同的时间段，在前端会进行针对性的解释翻译）

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data":null
}
~~~

~~~json
{
	"st": 1,
	"msg": "暂无数据",
	"data":null
}
~~~

### **三、数据表定义**

暂无


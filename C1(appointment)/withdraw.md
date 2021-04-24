### **一、需求描述**

### **需求介绍**

病人退号界面，即预约记录表的查询与具体某条记录的删除

### **API列表**

| 请求类型 | PATH                                         | 描述                 |
| -------- | -------------------------------------------- | -------------------- |
| GET      | /api/appoinment/register/appoint/{userPhone} | 查询预约信息         |
| DELETE   | /api/appoinment/withdraw/appoint             | 删除具体某条预约记录 |

### **二、API文档**

### DELETE  /api/appoinment/withdraw/appoint  删除具体某条预约记录

**查询参数Query Params**

| Key          | Value  | Required | Description                                      |
| ------------ | ------ | -------- | ------------------------------------------------ |
| appointDate  | long   | y        | 预约日期                                         |
| section      | int    | y        | 预约时段                                         |
| patientPhone | string | y/n      | 病人id（如果后台能根据header自动确定则这个不用） |

（预约时间段编号对应了不同的时间段，在前端会进行针对性的解释翻译）

#### Response

~~~json
{
	"st": 0,
	"msg": "",
	"data": null
}
~~~

~~~json
{
	"st": 1,
	"msg": "数据不一致",
	"data": null
}
~~~



### **三、数据表定义**

暂无



### 测试样例（by后端）

### **DELETE**/api/appointment/withdraw/appoint

删除成功，数据库更新成功

```json
Request body
{
  "patientPhone": "18888888888",
  "appointDate": 1618934400,
  "section": 3
}

Response body
{
  "st": 0,
  "msg": "",
  "data": null
}
```

未有相关信息：

```json
Request body
{
  "patientPhone": "18888888888",
  "appointDate": 1618934400,
  "section": 0
}

Response body
{
  "st": 1,
  "msg": "数据不一致",
  "data": null
}
```


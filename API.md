<center style='font-size:22px;'>COVID19 CT 在线辅助诊断平台  API 文档</center>

API 规定：

1. 分页参数page从0开始
2. 超链接中含有/multiple表示返回的对象可能有多个，返回时用rows参数来封装对象，totals表示总数；
3. 超链接中含有/single表示返回的对象有且为一个，返回时用row参数来封装对象；
4. 返回数据中必须含有状态status和msg。当且仅当status=0时，表示成功（成功由业务自己控制，可以使不出BUG表示成功，也可以使成功查询到数据表示成功）；其它状态均表示失败；
5. 数据库相关API的超链接规则：/数据库表/操作名称（查select、增add、删del、修modify）/条件1/条件2/.../数据对象。

公共规定：

1. 增加：所有增加方法，前台如果不传isdelele字段参数，后台默认赋值为0；插入数据时，自动赋值gmt_create、gmt_modified为当前时间；
2. 删除：所有删除方法，仅修改isdelete字段，切勿真删除；
3. 修改：使用修改方法后，更新数据库字段时间：gmt_modified；
4. **重要：所有数据中的字段，前台在传递参数时，必须小写。比如数据库中的is_delete方法，传参数时使用isdelete。**

API包含内容：

超链接[必须]、方法描述[可选]、输入数据[必须]、返回数据[必须]

# 1. 数据库相关

## 1.1 医院表（hospital）  

### 1.1.1 查询单个医院信息（ID）

超链接：/hospital/select/single/hospitalByID

输入数据：

```
{
	id:
}
```

返回数据：

```
{
	row: {医院对象}
	status: 0
	msg: ""
}
```

### 1.1.2 查询医院信息（分页+条件）

超链接：/hospital/select/multiple/paging/hospitalInfo

输入数据：

```
{
	// 分页参数
	page:
	pagesize:
	// 下面为数据库中的条件参数
	name:
	address:
	remark:
	isdelete:
}
```

返回数据：

```
{
	rows: [
		{医院对象1},
		{医院对象2},
		...
	]
	status: 0
	msg: ""
}
```

### 1.1.3 增加新医院

超链接：/hospital/add/hospitalInfo

输入数据：

```
{
	// 和数据库表中字段一致
	name: // 必须
	address: // 必须
	remark: // 非必须
	isdelete: // 非必须，默认为0
}
```

返回数据：

```
{
	status: 0
	msg: ""
}
```

### 1.1.4 删除医院

超链接：/hospital/del/hospitalByIDs

方法描述：根据ID删除医院信息，仅修改is_delete状态。可删除1个，也可删除多个。

输入数据：

```
{
	ids: // 必须，输入的id是以英文逗号分隔的id字符串，例如："1111,2222,3333"
}
```

返回数据：

```
{
	status: 0
	msg: ""
}
```

## 1.2 科室表（department）

### 1.2.1 查询单个科室信息（ID）

超链接：/department/select/single/departmentByID

输入数据：

```
{
	id:
}
```

返回数据：

```
{
	row: {科室对象}
	status: 0
	msg: ""
}
```

### 1.2.2 查询科室信息（分页+条件）

超链接：/department/select/multiple/paging/departmentInfo

输入数据：

```
{
	// 分页参数
	page:
	pagesize:
	// 下面为数据库中的条件参数
	name:
	address:
	remark:
	isdelete:
	hospitalid:
}
```

返回数据：

```
{
	rows: [
		{科室对象1},
		{科室对象2},
		...
	]
	status: 0
	msg: ""
}
```

### 1.2.3 增加新科室

超链接：/department/add/departmentInfo

输入数据：

```
{
	// 和数据库表中字段一致
	name: // 必须
	address: // 必须
	remark: // 非必须
	isdelete: // 非必须，默认为0
	hospitalid: // 必须，创建科室必须指定所示医院
}
```

返回数据：

```
{
	status: 0
	msg: ""
}
```

### 1.2.4 删除科室

超链接：/department/del/departmentByIDs

方法描述：根据ID删除科室信息，仅修改is_delete状态。可删除1个，也可删除多个。

输入数据：

```
{
	ids: // 必须，输入的id是以英文逗号分隔的id字符串，例如："1111,2222,3333"
}
```

返回数据：

```
{
	status: 0
	msg: ""
}
```





## 1.3 医生表（doctor）





## 1.4 文件树（filetree）  





## 1.5 群聊表（chat）   



## 1.6 消息日志表（informationlog）  





## 1.7 消息表（information）



## ==== 以下是关联表 ====

## 1.8 医院-科室表 （hospital_department）



# 2. 业务相关


<center style='font-size:22px;'>云南大学软件学院导师双选系统  API 文档</center>

API 规定：

1. 分页参数page从0开始
2. 超链接中含有/multiple表示返回的对象可能有多个，返回时用rows参数来封装对象，totals表示总数；
3. 超链接中含有/single表示返回的对象有且为一个，返回时用row参数来封装对象，row中封装的对象格式为：

```
row: {
		id:
		name:
		remark
		isdelete:
		gmtcreate:
		gmtmodified:
		object1: { // 如果还有对象，继续json封装，此内部对象需不需要读取出来，视情况而定
			...
		}
		objects: [{医生1}, {医生2}]  // 如果还有对象数组，使用数组形式的json封装
	}
```

4. 返回数据中必须含有状态status和msg。当且仅当status=0（整型）时，表示成功（成功由业务自己控制，可以使不出BUG表示成功，也可以使成功查询到数据表示成功）；其它状态均表示失败；
5. 数据库相关API的超链接规则：/数据库表/操作名称（查select、增add、删del、修modify）/条件1/条件2/.../数据对象；
6. 返回的时间gmtcreate和gmtmodified为时间戳形式；
7. 所有的1.*.2的方法，一定是**按时间倒序**;
8. 所有的后端方法均在前端使用POST方式来请求。

公共规定：

1. 增加：所有增加方法，前台如果不传isdelele字段参数，后台默认赋值为0；插入数据时，自动赋值gmt_create、gmt_modified为当前时间；
2. 删除：所有删除方法，仅修改isdelete字段，切勿真删除；
3. 修改：使用修改方法后，更新数据库字段时间：gmt_modified；
4. **重要：所有数据中的字段，前台在传递参数时，必须采用驼峰命名的方式。比如数据库中的is_delete方法，传参数时使用isDelete（'_'后面的首字母必须大写）。**

API包含内容：

超链接[必须]、方法描述[可选]、输入数据[必须]、返回数据[必须]

# 1. 数据库相关

## 1.1 学生（student）  

### 1.1.1 查询单个学生信息（ID）

- [ ] 开发完成
- [ ] 测试完成

超链接：/student/select/single/studentByID

输入数据：

```
{
	id:
}
```

返回数据：

```
{
	row: {学生对象}
	status: 0
	msg: ""
}
```

### 1.1.2 查询学生信息（分页+条件）

- [ ] 开发完成
- [ ] 测试完成

超链接：/student/select/multiple/paging/studentInfo

输入数据：

```
{
	// 分页参数
	page:
	pagesize:
	// 下面为数据库中的条件参数，模糊查询（username）
	id:
	username:
	password:
	openid:
	remark:
	isDelete:
	gmtCreate:
	gmtModified:
	enrollmentYear:
	major:
	direction:
	currentGrade:
	sclass:
	idcard:
	phone:
	sex:
}
```

返回数据：

```
{
	rows: [
		{学生对象1},
		{学生对象2},
		...
	]
	totals: 20
	status: 0
	msg: ""
}
```

### 1.1.3 增加新学生

- [ ] 开发完成
- [ ] 测试完成

超链接：/student/add/studentInfo

输入数据：

```
{
	// 和数据库表中字段一致
	username: // 必须
	password: // 必须
	openid: 
	remark:
	isDelete: // 非必须，默认为0
	gmtCreate:
	gmtModified:
	enrollmentYear:
	major:
	direction:
	currentGrade:
	sclass:
	idcard:
	phone:
	sex:
}
```

返回数据：

```
{
	status: 0
	msg: ""
}
```

### 1.1.4 删除学生

- [ ] 开发完成
- [ ] 测试完成

超链接：/student/del/studentByIDs

方法描述：根据ID删除学生信息，仅修改is_delete状态。可删除1个，也可删除多个。

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

### 1.1.5 修改学生

- [ ] 开发完成
- [ ] 测试完成

超链接：/student/modify/studentInfo

方法描述：修改学生，修改后更新gmt_modified值

输入数据：

```
{
	id: // 必须，医院ID
	// 要修改的字段，传了就修改，没传不修改
	username:
	password:
	openid: 
	remark:
	isDelete: 
	gmtCreate:
	gmtModified:
	enrollmentYear:
	major:
	direction:
	currentGrade:
	sclass:
	idcard:
	phone:
	sex:
}
```

返回数据：

```
{
	status: 0
	msg: ""
}
```




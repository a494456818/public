<center style='font-size:22px;'>COVID19 CT 在线辅助诊断平台  API 文档</center>

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
		share: { // 如果还有对象，继续json封装，此内部对象需不需要读取出来，视情况而定
			...
		}
		doctors: [{医生1}, {医生2}]  // 如果还有对象数组，使用数组形式的json封装
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
4. **重要：所有数据中的字段，前台在传递参数时，必须小写。比如数据库中的is_delete方法，传参数时使用isdelete。**

API包含内容：

超链接[必须]、方法描述[可选]、输入数据[必须]、返回数据[必须]

# 1. 数据库相关

## 1.1 医院表（hospital）  

### 1.1.1 查询单个医院信息（ID）

- [ ] 开发完成
- [ ] 测试完成

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

- [ ] 开发完成
- [ ] 测试完成

超链接：/hospital/select/multiple/paging/hospitalInfo

输入数据：

```
{
	// 分页参数
	page:
	pagesize:
	// 下面为数据库中的条件参数，模糊查询（address、remark）
	id:
	name:
	address:
	remark:
	isDelete:
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
	totals: 20
	status: 0
	msg: ""
}
```

### 1.1.3 增加新医院

- [ ] 开发完成
- [ ] 测试完成

超链接：/hospital/add/hospitalInfo

输入数据：

```
{
	// 和数据库表中字段一致
	name: // 必须
	address: // 必须
	remark: // 非必须
	isDelete: // 非必须，默认为0
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

- [ ] 开发完成
- [ ] 测试完成

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

### 1.1.5 修改医院

- [ ] 开发完成
- [ ] 测试完成

超链接：/hospital/modify/hospitalInfo

方法描述：修改医院，修改后更新gmt_modified值

输入数据：

```
{
	id: // 必须，医院ID
	// 要修改的字段，传了就修改，没传不修改
	name: // 非必须
	address: // 非必须
	remark: // 非必须
	isDelete: // 非必须
}
```

返回数据：

```
{
	status: 0
	msg: ""
}
```

### 1.1.6 根据医院查询所有科室

- [ ] 开发完成
- [ ] 测试完成

超链接：/hospital/select/multiple/departmentsByHospitalID

方法描述：根据医院ID查询所属医院的所有科室。

输入数据：

```
{
	id: // 必须, 医院id
}
```

返回数据：

```
{
	rows: [{
		科室1,
		科室2,
		...
	}]
	status: 0
	msg: ""
}
```

### 1.1.7 根据医院查询所有医生

- [ ] 开发完成
- [ ] 测试完成

超链接：/hospital/select/multiple/doctorsByHospitalID

方法描述：根据医院ID查询所属的所有医生

输入数据：

```
{
	id: // 医院id
}
```

返回数据：

```
{
	rows: [{
		医生1,
		医生2,
		...
	}]
	status: 0
	msg: ""
}
```

## 1.2 科室表（department）

### 1.2.1 查询单个科室信息（ID）

- [ ] 开发完成
- [ ] 测试完成

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

- [ ] 开发完成
- [ ] 测试完成

超链接：/department/select/multiple/paging/departmentInfo

输入数据：

```
{
	// 分页参数
	page:
	pagesize:
	// 下面为数据库中的条件参数，模糊查询（address、remark）
	id:
	name:
	address:
	remark:
	isDelete:
	hospitalId:
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
	totals: 12
	status: 0
	msg: ""
}
```

### 1.2.3 增加新科室

- [ ] 开发完成
- [ ] 测试完成

超链接：/department/add/departmentInfo

输入数据：

```
{
	// 和数据库表中字段一致
	name: // 必须
	address: // 必须
	remark: // 非必须
	isDelete: // 非必须，默认为0
	hospitalId: // 必须，创建科室必须指定所示医院
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

- [ ] 开发完成
- [ ] 测试完成

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

### 1.2.5 修改科室

- [ ] 开发完成
- [ ] 测试完成

超链接：/department/modify/departmentInfo

方法描述：修改科室，修改后更新gmt_modified值

输入数据：

```
{
	id: // 必须，科室id
	// 要修改的字段，传了就修改，没传不修改
	name: // 非必须
	address: // 非必须
	remark: // 非必须
	isDelete: // 非必须
}
```

返回数据：

```
{
	status: 0
	msg: ""
}
```

### 1.2.6 根据科室查询所属医院

- [ ] 开发完成
- [ ] 测试完成

超链接：/department/select/single/hospitalByDepartmentID

方法描述：根据ID删除科室信息，仅修改is_delete状态。可删除1个，也可删除多个。

输入数据：

```
{
	id: // 科室id
}
```

返回数据：

```
{
	row: {
		医院信息
	}
	status: 0
	msg: ""
}
```

### 1.2.7 根据科室查询科室下所有医生

- [ ] 开发完成
- [ ] 测试完成

超链接：/department/select/multiple/doctorsByDepartmentID

输入数据：

```
{
	id: // 科室id
}
```

返回数据：

```
{
	rows: [{医生1}, {医生2},...]
	status: 0
	msg: ""
}
```

## 1.3 医生表（doctor）

### 1.3.1 查询单个医生信息（ID）

- [ ] 开发完成
- [ ] 测试完成

超链接：/doctor/select/single/doctorByID

输入数据：

```
{
	id:
}
```

返回数据：

```
{
	row: {医生对象}
	status: 0
	msg: ""
}
```

### 1.3.2 查询医生信息（分页+条件）

- [ ] 开发完成
- [ ] 测试完成

超链接：/doctor/select/multiple/paging/doctorInfo

输入数据：

```
{
	// 分页参数
	page:
	pagesize:
	// 下面为数据库中的条件参数, 模糊查询字段（address、remark）
	id:
	username:
	password:
	email:
	level:
	phone:
	address:
	remark:
	type:
	isDelete:
	departmentId:
}
```

返回数据：

```
{
	rows: [
		{医生对象1},
		{医生对象2},
		...
	]
	totals: 12
	status: 0
	msg: ""
}
```

### 1.3.3 增加新医生

- [ ] 开发完成
- [ ] 测试完成

超链接：/doctor/add/doctorInfo

方法描述：添加新医生，在添加新医生后，检查该是否有文件树，如果没有文件树将自动创建带有根节点的文件树。

初始化的文件树各个字段为：

```
{
	pid: -1,
	isdir: 1,
	name: "root",
	path: "",
	attributes: "",
	remark: "",
	isDelete: 0
}
```

输入数据：

```
{
	// 和数据库表中字段一致
	id:
	username:
	password:
	email:
	level:
	phone:
	address:
	remark:
	type:
	isDelete:
	departmentId: // 必须，医生必须在科室下
}
```

返回数据：

```
{
	status: 0
	msg: ""
}
```

### 1.3.4 删除医生

- [ ] 开发完成
- [ ] 测试完成

超链接：/doctor/del/doctorByIDs

方法描述：根据ID删除医生信息，仅修改is_delete状态。可删除1个，也可删除多个。

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

### 1.3.5 修改医生

- [ ] 开发完成
- [ ] 测试完成

超链接：/doctor/modify/doctorInfo

方法描述：修改医生，修改后更新gmt_modified值。

输入数据：

```
{
	id: // 必须，医生ID
	// 要修改的字段，传了就修改，没传不修改
	username:
	password:
	email:
	level:
	phone:
	address:
	remark:
	type:
	isDelete:
}
```

返回数据：

```
{
	status: 0
	msg: ""
}
```

### 1.3.6 根据医生查询所属科室

- [ ] 开发完成
- [ ] 测试完成

超链接：/doctor/select/single/departmentByDoctorID

输入数据：

```
{
	id: // 必须，医生ID
}
```

返回数据：

```
{
	row: {
		科室信息
	}
	status: 0
	msg: ""
}
```

### 1.3.7 根据医生查询所属医院

- [ ] 开发完成
- [ ] 测试完成

超链接：/doctor/select/single/hospitalByDoctorID

输入数据：

```
{
	id: // 必须，医生ID
}
```

返回数据：

```
{
	row: {
		医院信息
	}
	status: 0
	msg: ""
}
```

## 1.4 文件树（filetree）  

### 1.4.1 查询文件树中单个node信息（ID）

- [ ] 开发完成
- [ ] 测试完成

超链接：/filetree/select/single/nodeByID

输入数据：

```
{
	id:
}
```

返回数据：

```
{
	row: {医生对象}
	status: 0
	msg: ""
}
```

### 1.4.2 * 查询文件树信息（分页+条件）

- [ ] 开发完成
- [ ] 测试完成

超链接：/filetree/select/multiple/paging/nodeInfo

输入数据：

```
{
	// 分页参数
	page:
	pagesize:
	// 下面为数据库中的条件参数, 模糊查询字段（name、attributes）
	id:
	pid:  // * 如果传递该参数，查询处于该pid下的节点
	isDir: 
	name:
	path:
	attributes:
	remark:
	isDelete:
	doctorId: 
}
```

返回数据：

```
{
	// 需在每个节点对象中加入该节点在文件树中的路径（nodepath），例如：/a/b/c/1.dcm
	rows: [
		{节点对象1},
		{key1: value1, "nodepath": "/a/b/c/1.dcm"}, // 节点对象2
		...
	]
	totals: 5
	status: 0
	msg: ""
}
```

### 1.4.3 增加新节点

- [ ] 开发完成
- [ ] 测试完成

超链接：/filetree/add/nodeInfo

方法描述：某个医生在文件夹中创建一个文件或者文件夹。如果用户添加的是文件节点，后台关于文件的具体操作：

1. 获取文件流和文件名称；
2. 文件名称赋值给数据库中的name字段
3. 保存文件流到服务器：为了避免文件名出现重复和文件夹中文件过多导致的问题，保存文件的路径规则为 根目录/医院ID/科室ID/医生id/年/月/日/UUID.dcm，并将新的路径赋值给数据库的path字段保存。

输入数据：

```
{
	// 和数据库表中字段一致
	id:
	pid:  // 必须
	isDir:  // 必须
	name:  // 必须
	path:  
	attributes:
	remark:
	isDelete: 0  // 非必须，不传后台默认给0
	doctorId:  // 必须
}
```

返回数据：

```
{
	status: 0
	msg: ""
}
```

### 1.4.4 删除文件树中的节点

- [ ] 开发完成
- [ ] 测试完成

超链接：/filetree/del/nodeByIDs

方法描述：根据ID删除节点信息，仅修改is_delete状态。可删除1个，也可删除多个。**注意：如果删除的是文件夹，需要递归把文件夹下的所有文件夹和文件删除。**

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

### 1.4.5 修改文件树中的节点

- [ ] 开发完成
- [ ] 测试完成

超链接：/filetree/modify/nodeInfo

方法描述：修改文件树中的节点。修改后更新gmt_modified值。

输入数据：

```
{
	id: // 必须，节点ID
	// 要修改的字段，传了就修改，没传不修改
	pid:  // 非必须
	name:  // 非必须
	attributes: // 非必须
	remark: // 非必须
	isDelete: 0  // 非必须
}
```

返回数据：

```
{
	status: 0
	msg: ""
}
```

### 1.4.6 根据结点ID查询所属医生

- [ ] 开发完成
- [ ] 测试完成

超链接：/node/select/single/doctorByNodeID

输入数据：

```
{
	id: // 必须，结点ID
}
```

返回数据：

```
{
	row: {
		医生信息
	}
	status: 0
	msg: ""
}
```

## 1.5 群聊表（chat）   

### 1.5.1 查询单个群聊信息（ID）

- [ ] 开发完成
- [ ] 测试完成

超链接：/chat/select/single/chatByID

输入数据：

```
{
	id:
}
```

返回数据：

```
{
	row: {群聊对象}
	status: 0
	msg: ""
}
```

### 1.5.2 查询群聊信息（分页+条件）

- [ ] 开发完成
- [ ] 测试完成

超链接：/chat/select/multiple/paging/chatInfo

输入数据：

```
{
	// 分页参数
	page:
	pagesize:
	// 下面为数据库中的条件参数, 模糊查询字段（address、remark）
	id:
	name:
	remark:
	isDelete:
	shareId:
	doctorIds: // 本条件使用英文逗号分隔，如: "111,222,333", 后台查询包含这些医生id的群聊, 这些id是无需的, 不要硬匹配或使用"=="或LIKE
}
```

返回数据：

```
{
	rows: [
		{群聊对象1},
		{群聊对象2},
		...
	]
	totals: 12
	status: 0
	msg: ""
}
```

### 1.5.3 增加新群聊

- [ ] 开发完成
- [ ] 测试完成

超链接：/chat/add/chatInfo

方法描述：增加新群聊，注意分享发起人的doctorids必须在第一个。

输入数据：

```
{
	name: // 必须
	remark:
	isDelete:
	shareId:
	doctorIds: // 如: "111,222,333"
}
```

返回数据：

```
{
	status: 0
	msg: ""
}
```

### 1.5.4 删除群聊

- [ ] 开发完成
- [ ] 测试完成

超链接：/chat/del/chatByIDs

方法描述：根据ID删除chat信息，仅修改is_delete状态。可删除1个，也可删除多个。

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

### 1.5.5 修改群聊信息

- [ ] 开发完成
- [ ] 测试完成

超链接：/chat/modify/chatInfo

方法描述：修改群聊信息。修改后更新gmt_modified值。仅修改基本的群聊信息，如果有要修改群聊中的人的需求，另起方法。

输入数据：

```
{
	id: // 必须，群聊ID
	// 要修改的字段，传了就修改，没传不修改
	name:
	remark:
	isDelete:
}
```

返回数据：

```
{
	status: 0
	msg: ""
}
```

### 1.5.6 根据群聊ID查询所有参与的医生信息

- [ ] 开发完成
- [ ] 测试完成

超链接：/chat/select/multiple/doctorsBychatID

输入数据：

```
{
	id: // 必须，群聊ID
}
```

返回数据：

```
{
	rows: [{医生1}, {医生2}, ...]
	status: 0
	msg: ""
}
```

### 1.5.7 根据群聊ID查询分享（会诊）信息

- [ ] 开发完成
- [ ] 测试完成

超链接：/chat/select/single/shareBychatID

输入数据：

```
{
	id: // 必须，群聊ID
}
```

返回数据：

```
{
	row: {分享信息}
	status: 0
	msg: ""
}
```

## 1.6 消息日志表（informationlog）  

### 1.6.1 查询单个信息日志（ID）

- [ ] 开发完成
- [ ] 测试完成

超链接：/informationlog/select/single/informationlogByID

输入数据：

```
{
	id:
}
```

返回数据：

```
{
	row: {信息对象}
	status: 0
	msg: ""
}
```

### 1.6.2 查询信息日志（分页+条件）

- [ ] 开发完成
- [ ] 测试完成

超链接：/informationlog/select/multiple/paging/informationlogInfo

输入数据：

```
{
	// 分页参数
	page:
	pagesize:
	// 下面为数据库中的条件参数, 模糊查询字段（address、remark）
	id:
	type:
	remark:
	isDelete:
	chatId:
	informationId: :
	doctorId:
}
```

返回数据：

```
{
	rows: [
		{信息对象1},
		{信息对象2},
		...
	]
	totals: 12
	status: 0
	msg: ""
}
```

### 1.6.3 增加新信息日志（发送消息）

- [ ] 开发完成
- [ ] 测试完成

超链接：/informationlog/add/informationlogInfo

方法描述：增加新信息日志。

输入数据：

```
{
	type:
	remark:
	isDelete: 0
	chatId:
	informationId: :
	doctorId:
}
```

返回数据：

```
{
	status: 0
	msg: ""
}
```

### 1.6.4 删除信息日志

- [ ] 开发完成
- [ ] 测试完成

超链接：/informationlog/del/informationlogByIDs

方法描述：根据ID删除信息，仅修改is_delete状态。可删除1个，也可删除多个。

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

### 1.6.5 修改消息日志信息

- [ ] 开发完成
- [ ] 测试完成

超链接：/chat/modify/informationlogInfo

方法描述：修改消息日志信息。修改后更新gmt_modified值。

输入数据：

```
{
	id: // 必须，消息日志ID
	// 要修改的字段，传了就修改，没传不修改
	type:
	remark:
	isDelete: 0
}
```

返回数据：

```
{
	status: 0
	msg: ""
}
```

### 1.6.6 根据消息日志ID查询对应的信息

- [ ] 开发完成
- [ ] 测试完成

超链接：/chat/select/single/informationBylogID

输入数据：

```
{
	id: // 必须，日志ID
}
```

返回数据：

```
{
	rows: {信息}
	status: 0
	msg: ""
}
```

### 1.6.7 根据消息日志查询发送信息的医生

- [ ] 开发完成
- [ ] 测试完成

超链接：/chat/select/single/doctorBylogID

输入数据：

```
{
	id: // 必须，日志ID
}
```

返回数据：

```
{
	row: {医生信息}
	status: 0
	msg: ""
}
```

### 1.6.8 根据消息日志查询所属的群聊

- [ ] 开发完成
- [ ] 测试完成

超链接：/chat/select/single/chatBylogID

输入数据：

```
{
	id: // 必须，日志ID
}
```

返回数据：

```
{
	row: {群聊信息}
	status: 0
	msg: ""
}
```

## 1.7 消息表（information）

### 1.7.1 查询单个信息（ID）

- [ ] 开发完成
- [ ] 测试完成

超链接：/information/select/single/informationByID

输入数据：

```
{
	id:
}
```

返回数据：

```
{
	row: {信息对象}
	status: 0
	msg: ""
}
```

### 1.7.2 查询信息（分页+条件）

- [ ] 开发完成
- [ ] 测试完成

超链接：/information/select/multiple/paging/informationInfo

输入数据：

```
{
	// 分页参数
	page:
	pagesize:
	// 下面为数据库中的条件参数, 模糊查询字段（address、remark）
	id:
	content:
	remark:
	isDelete:
}
```

返回数据：

```
{
	rows: [
		{信息对象1},
		{信息对象2},
		...
	]
	totals: 12
	status: 0
	msg: ""
}
```

### 1.7.3 增加新信息（发送消息）

- [ ] 开发完成
- [ ] 测试完成

超链接：/information/add/informationInfo

方法描述：增加新信息，和1.6一起使用。

输入数据：

```
{
	content:
	remark:
	isDelete:
}
```

返回数据：

```
{
	status: 0
	msg: ""
}
```

### 1.7.4 删除信息

- [ ] 开发完成
- [ ] 测试完成

超链接：/information/del/informationByIDs

方法描述：根据ID删除信息，仅修改is_delete状态。可删除1个，也可删除多个。

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

### 1.7.5 修改消息信息

- [ ] 开发完成
- [ ] 测试完成

超链接：/chat/modify/informationInfo

方法描述：修改消息信息。修改后更新gmt_modified值。

输入数据：

```
{
	id: // 必须，消息ID
	// 要修改的字段，传了就修改，没传不修改
	content:
	remark:
	isDelete:
}
```

返回数据：

```
{
	status: 0
	msg: ""
}
```

## ==== 以下是关联表 ====

## 1.8 分享表 （share）

因为是关联表，所以在查询时，需要把关联的外键对象查询出来。

### 1.8.1 查询单个分享（ID）

- [ ] 开发完成
- [ ] 测试完成

超链接：/share/select/single/shareByID

输入数据：

```
{
	id:
}
```

返回数据：

```
{
	row: {
		id:
		remark:
		isdelete:
		gmtCreate:
		gmtModified:
		sendDoctorId:
		receiveDoctorId:
		filetreeId:
		// 三个外键对应的对象
		sendDoctor: {
		
		}
		receiveDoctor: {
		
		}
		filetree: {
		
		}
	}
	status: 0
	msg: ""
}
```

### 1.8.2 查询分享（分页+条件）

- [ ] 开发完成
- [ ] 测试完成

超链接：/share/select/multiple/paging/shareInfo

输入数据：

```
{
	// 分页参数
	page:
	pagesize:
	// 下面为数据库中的条件参数, 模糊查询字段（address、remark）
	id:
	remark:
	isdelete:
}
```

返回数据：

```
{
	rows: [
		{分享对象1，这些对象包含的内容和1.8.1一样},
		{分享对象2},
		...
	]
	totals: 12
	status: 0
	msg: ""
}
```

### 1.8.3 增加新分享（医生发起会诊）

- [ ] 开发完成
- [ ] 测试完成

超链接：/share/add/shareInfo

方法描述：增加新信息。

输入数据：

```
{
	remark:
	isDelete:
	send_doctor_id:
	received_doctor_id:
	filetree_id:
}
```

返回数据：

```
{
	status: 0
	msg: ""
}
```

### 1.8.4 删除分享

- [ ] 开发完成
- [ ] 测试完成

超链接：/share/del/shareByIDs

方法描述：根据ID删除共享信息，仅修改is_delete状态。可删除1个，也可删除多个。

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

## 1.9 医生-图像 诊断表（doctor_filetree）

### 1.9.1 查询单个诊断信息（ID）

- [ ] 开发完成
- [ ] 测试完成

超链接：/doctor_filetree/select/single/doctorFiletreeByID

输入数据：

```
{
	id:
}
```

返回数据：

```
{
	row: {医生-图像 诊断信息}
	status: 0
	msg: ""
}
```

### 1.9.2 查询诊断信息（分页+条件）

- [ ] 开发完成
- [ ] 测试完成

超链接：/doctor_filetree/select/multiple/paging/doctorFiletreeInfo

输入数据：

```
{
	// 分页参数
	page:
	pagesize:
	// 下面为数据库中的条件参数, 模糊查询字段（address、remark）
	id:
	anno_result:
	cls
	remark:
	isDelete:
	doctorId:
	filetreeId:
}
```

返回数据：

```
{
	rows: [
		{医生-图像 诊断对象1, 医生-图像 诊断对象2, ...},
	]
	totals: 12
	status: 0
	msg: ""
}
```

### 1.9.3 增加新诊断信息

- [ ] 开发完成
- [ ] 测试完成

超链接：/doctor_filetree/add/doctorFiletreeInfo

方法描述：增加新诊断信息

输入数据：

```
{
	anno_result:
	cls:
	remark:
	isDelete:
	doctorId:
	filetreeId:
}
```

返回数据：

```
{
	status: 0
	msg: ""
}
```

### 1.9.4 删除诊断信息

- [ ] 开发完成
- [ ] 测试完成

超链接：/doctor_filetree/del/doctorFiletreeByIDs

方法描述：根据ID删除诊断信息，仅修改is_delete状态。可删除1个，也可删除多个。

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

### 1.9.5 查询未下载的图片数量

- [ ] 开发完成
- [ ] 测试完成

超链接：/doctor_filetree/select/single/unDownloadNum

方法描述：查询未下载的图像数量，也就是在该表中查询当天status=1的图像。

输入数据：

```
{
	
}
```

返回数据：

```
{
	status: 0
	msg: ""
}
```

### 1.9.6 下载未下载的图片数量（有疑问，讨论）

- [ ] 开发完成
- [ ] 测试完成

超链接：/doctor_filetree/select/multiple/downloadUnDLDcmAndAnno

方法描述：查找status=1的数据，将对应的图像通过网络传输出去（传送dcm和标注文件），并修改status=2。

输入数据：

```
{
	
}
```

返回数据：

```
{
	status: 0
	msg: ""
}
```



## 1.10 提醒表（warning）

### 1.10.1 查询单个提醒信息（ID）

- [ ] 开发完成
- [ ] 测试完成

超链接：/warning/select/single/warningByID

输入数据：

```
{
	id:
}
```

返回数据：

```
{
	row: {提醒信息}
	status: 0
	msg: ""
}
```

### 1.10.2 查询提醒信息（分页+条件）

- [ ] 开发完成
- [ ] 测试完成

超链接：/warning/select/multiple/paging/warningInfo

输入数据：

```
{
	// 分页参数
	page:
	pagesize:
	// 下面为数据库中的条件参数, 模糊查询字段（address、remark）
	id:
	content:
	remark:
	isRead:
	isDelete:
	sendDoctorId:
	receiveDoctorId:
}
```

返回数据：

```
{
	rows: [
		{提醒1,提醒2, ...},
	]
	totals: 12
	status: 0
	msg: ""
}
```

### 1.10.3 增加新提醒信息

- [ ] 开发完成
- [ ] 测试完成

超链接：/warning/add/warningInfo

方法描述：增加新诊断信息

输入数据：

```
{
	content:
	remark:
	isRead:
	isDelete:
	sendDoctorId:
	receiveDoctorId:
}
```

返回数据：

```
{
	status: 0
	msg: ""
}
```

### 1.10.4 删除提醒信息

- [ ] 开发完成
- [ ] 测试完成

超链接：/warning/del/warningByIDs

方法描述：根据ID删除提醒信息，仅修改is_delete状态。可删除1个，也可删除多个。

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

# 2. 业务相关


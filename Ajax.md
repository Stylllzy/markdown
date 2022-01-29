[toc]

# Ajax

> 浏览器提供的一套方法，实现页面无刷新更新数据

## 应用场景

1. 页面上拉加载更多数据
2. 列表数据无刷新分页
3. 表单项离开焦点数据验证
4. 搜索框提示文字下拉列表

## Ajax状态码

0：请求未初始化（未调用open()）

1：请求已经建立，但是还未发送（未调用send()）

2：请求已经发送

3：请求正在处理中，有部分数据可以用

4：响应完成，可以获取并使用服务器的响应

	xhr.readyState		// 获取Ajax状态码

`onreadystatechange`，在Ajax状态码发生变化时触发

两种获取服务器端响应方式

- onload事件
- onreadystatechange事件

`xhr.status：`获取http状态码

## Ajax错误处理

1. 网络畅通，服务器端能接收到请求，服务器端返回的结果不是预期结果（`400`）

   ==可以判断服务器端返回的状态码，分别进行处理，xhr.status获取http状态码==

2. 网络畅通，服务器端没有接收到请求，返回`404`状态码

   ==检查请求地址是否错误==

3. 网络畅通，服务器端能接收到请求，服务器端返回`500`状态码

   ==服务器端错误，找后端程序员进行沟通==

4. 网络中断，请求无法发送到服务器端

   ==会触发xhr对象下面的onerror事件，在onerron事件处理函数中对错误进行处理==

## $.ajax()

	$.ajax({
		type: "GET",
		url: 'http://www.example.com',
		data: {	username:'zy',age:'20'},											contentType:'application/x-www-form-urlencoded',(或application/json)
		//JSON.stringify 转换成JSON=
		beforeSend: function () {
			return false
		},
		success: function (response) {},
		error: function (xhr) {}
	}


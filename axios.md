### axios

是一个专注于网络请求的库

基本语法：

```
axios({
	method:'请求的类型'，
	url: '请求的URL地址'	
}).then((result)=>{
	//.then用来指定请求成功后的回调函数
	//形参中的result是请求成功之后的结果
})
```

调用axios的返回值是一个Promise对象


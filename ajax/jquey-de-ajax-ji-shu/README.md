# Jquey的Ajax技术

jquery是一个优秀的js框架，自然对js原生的ajax进行了封装，封装后的ajax的操 作方法更简洁，功能更强大，与ajax操作相关的jquery方法有如下几种，但开发中 经常使用的有三种

1）$.get\(url, \[data\], \[callback\], \[type\]\) 

2）$.post\(url, \[data\], \[callback\], \[type\]\)

其中： 

url：代表请求的服务器端地址 

data：代表请求服务器端的数据（可以是key=value形式也可以是json格式） 

callback：表示服务器端成功响应所触发的函数（只有正常成功返回才执行）

 type：表示服务器端返回的数据类型（jquery会根据指定的类型自动类型转换） 常用的返回类型：text、json、html等


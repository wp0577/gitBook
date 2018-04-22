![](/ajax/import.png)传统的方式：

一个http请求对应一个http响应，他们是同步机制，即你发送请求后需要等到服务器端的回应才能进行下面的操作。

![](/ajax2/import.png)

---



```
使用ajax与服务器通信的的步骤  
① 创建一个XMLHttpRequest对象 
② 创建url,data,通过 xmlHttpRequest.send() 
③ 服务器端接收 ajax的请求,做相应处理(操作数据库)，然后返回结果(echo 语句) 
④ 客户端通过xmlHttpRequest的属性 reponseText , responseXML 取的数据，然后就完成局部刷新当前页面任务
```










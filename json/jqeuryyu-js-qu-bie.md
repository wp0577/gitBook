//JQ的加载比JS快，并且没有覆盖问题

![](/assets/import.png)

```js
function JSWrite(){
	//document.getElementById("span1").innerHTML="美美哒！";
	var spanEle = document.getElementById("span1");
	$(spanEle).html("美美哒！");
}
			
$(function(){
	/*document.getElementById("btn1").onclick = function(){
					document.getElementById("span1").innerHTML="帅帅哒！";
	}*/
	$("#btn1").click(function(){
	//JQ对象转换成DOM对象的第一种方式
	//$("#span1")[0].innerHTML="呵呵哒！";
	//JQ对象转换成DOM对象的第二种方式
	$("#span1").get(0).innerHTML="呵呵哒！";
	});
  });

```




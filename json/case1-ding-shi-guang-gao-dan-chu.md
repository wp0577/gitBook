```js
3.实现步骤
第一步：引入jQuery相关的文件
第二步：书写页面加载函数
第三步：在页面加载函数中，获取显示广告图片的元素。
第四步：设置定时操作(显示广告图片的函数)
第五步：在显示广告图片的函数中，使用jQuery的方法让广告图片显示(show())
第六步：清除显示广告图片的定时操作
第七步：设置定时操作(隐藏广告图片的函数)
第八步：在隐藏广告图片的函数中，使用jQuery的方法让广告图片隐藏(hide())
第九步：清除隐藏广告图片的定时操作
4.代码实现
<script type="text/javascript">
	var time;
	$(function(){
		time=setInterval("showAd()",3000);
	});
	
	function showAd(){
		//$("#img1").show();
		//$("#img1").slideDown(3000);
		$("#img1").fadeIn(3000);
		clearInterval(time);
		time = setInterval("hideAd()",5000);
	}
	
	function hideAd(){
		//$("#img1").hide();
		//$("#img1").slideUp(3000);
		$("#img1").slideUp(3000);
		clearInterval(time);
	}
	
</script>


```




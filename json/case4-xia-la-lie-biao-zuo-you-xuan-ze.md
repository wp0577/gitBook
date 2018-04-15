```js
$(function () {
				$("#toRightOne").click(function () {
					$("#left option:selected").appendTo($("#right"));
				})
				$("#toRight").click(function () {
					$("#left option").appendTo($("#right"));
				})
				$("option").dblclick(function () {
					$("#left option:selected").appendTo("#right");
				})
			})
```




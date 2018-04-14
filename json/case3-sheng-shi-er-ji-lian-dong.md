```js
$(function () {
				
			var cities = new Array(3);
			cities[0] = new Array("武汉市","黄冈市","襄阳市","荆州市");
			cities[1] = new Array("长沙市","郴州市","株洲市","岳阳市");
			cities[2] = new Array("石家庄市","邯郸市","廊坊市","保定市");
			cities[3] = new Array("郑州市","洛阳市","开封市","安阳市");

				$("#province").change(function () {
					$("#city").empty();
					var city = this.value;
					$.each(cities, function(i, n) {
						if (i == city) {	
							$.each(cities[i], function(j, m) {
								// Document 和document不一样, 应该使用document
								var textNode = document.createTextNode(m);
								var opt = document.createElement("option");
								//append是将textnode添加到opt内部
								$(opt).append(textNode);
								$(opt).appendTo($("#city"));
							});
						}
					});
				});
			});
		
```




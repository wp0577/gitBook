# Case4: 下拉列表左右选择

```javascript
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


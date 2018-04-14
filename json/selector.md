```
jquery的选择器
基本选择器
id选择器：$(“#id名称”);
元素选择器：$(“元素名称”);
类选择器：$(“.类名”);
通配符：*
多个选择器共用(并集)

案例代码：
<html>
    <head>
        <meta charset="UTF-8">
        <title>基本选择器</title>
        <link rel="stylesheet" href="../../css/style.css" type="text/css"/>
        <script type="text/javascript" src="../../js/jquery-1.8.3.js" ></script>
        <script>
            $(function(){
                $("#btn1").click(function(){
                    $("#one").css("background-color","pink");
                });
                $("#btn2").click(function(){
                    $(".mini").css("background-color","pink");
                });
                $("#btn3").click(function(){
                    $("div").css("background-color","pink");
                });
                $("#btn4").click(function(){
                    $("*").css("background-color","pink");
                });
                $("#btn5").click(function(){
                    $("#two .mini").css("background-color","pink");
                });
            });
        </script>        
    </head>
    <body>
        <input type="button" id="btn1" value="选择为one的元素"/>
        <input type="button" id="btn2" value="选择样式为mini的元素"/>
        <input type="button" id="btn3" value="选择所有的div元素"/>
        <input type="button" id="btn4" value="选择所有元素"/>
        <input type="button" id="btn5" value="选择id为two并且样式为mini的元素"/>
        <hr/>
        <div id="one">
            <div class="mini">
                111
            </div>
        </div>

        <div id="two">
            <div class="mini">
                222
            </div>
            <div class="mini">
                333
            </div>
        </div>

        <div id="three">
            <div class="mini">
                444
            </div>
            <div class="mini">
                555
            </div>
            <div class="mini">
                666
            </div>
        </div>

        <span id="four">

        </span>
    </body>
</html>




    层级选择器

ancestor descendant: 在给定的祖先元素下匹配所有的后代元素(儿子、孙子、重孙子)
parent > child : 在给定的父元素下匹配所有的子元素(儿子)
prev + next: 匹配所有紧接在 prev 元素后的 next 元素(紧挨着的，同桌)
prev ~ siblings: 匹配 prev 元素之后的所有 siblings 元素(兄弟)

代码演示：
<html>
    <head>
        <meta charset="UTF-8">
        <title>层级选择器</title>
        <link rel="stylesheet" href="../../css/style.css" />
        <script type="text/javascript" src="../../js/jquery-1.8.3.js" ></script>
        <script>
            $(function(){
                $("#btn1").click(function(){
                    $("body div").css("background-color","pink");
                });
                $("#btn2").click(function(){
                    $("body>div").css("background-color","pink");
                });
                $("#btn3").click(function(){
                    $("#two+div").css("background-color","pink");
                });
                $("#btn4").click(function(){
                    $("#one~div").css("background-color","pink");
                });
            });

        </script>


    </head>
    <body>
        <input type="button" id="btn1" value="选择body中的所有的div元素"/>
        <input type="button" id="btn2" value="选择body中的第一级的孩子"/>
        <input type="button" id="btn3" value="选择id为two的元素的下一个元素"/>
        <input type="button" id="btn4" value="选择id为one的所有的兄弟元素"/>

        <hr/>
        <div id="one">
            <div class="mini">
                111
            </div>
        </div>

        <div id="two">
            <div class="mini">
                222
            </div>
            <div class="mini">
                333
            </div>
        </div>

        <div id="three">
            <div class="mini">
                444
            </div>
            <div class="mini">
                555
            </div>
            <div class="mini">
                666
            </div>
        </div>

        <span id="four">

        </span>
    </body>
</html>


    基本过滤选择器
$('li').first() 等价于：$(“li:first”)

代码案例演示：

<html>
    <head>
        <meta charset="UTF-8">
        <title>基本过滤选择器</title>
        <link rel="stylesheet" href="../../css/style.css" type="text/css"/>
        <script type="text/javascript" src="../../js/jquery-1.8.3.js" ></script>
        <script>
            $(function(){
                $("#btn1").click(function(){
                    $("div:first").css("background-color","pink");
                });
                $("#btn2").click(function(){
                    $("div:last").css("background-color","pink");
                });
                $("#btn3").click(function(){
                    $("div:odd").css("background-color","pink");
                });
                $("#btn4").click(function(){
                    $("div:even").css("background-color","pink");
                });
            });
        </script>

    </head>
    <body>
        <input type="button" id="btn1" value="body中的第一个div元素"/>
        <input type="button" id="btn2" value="body中的最后一个div元素"/>
        <input type="button" id="btn3" value="选择body中的奇数的div"/>
        <input type="button" id="btn4" value="选择body中的偶数的div"/>

        <hr/>
        <div id="one">
            <div class="mini">
                111
            </div>
        </div>

        <div id="two">
            <div class="mini">
                222
            </div>
            <div class="mini">
                333
            </div>
        </div>

        <div id="three">
            <div class="mini">
                444
            </div>
            <div class="mini">
                555
            </div>
            <div class="mini">
                666
            </div>
        </div>

        <span id="four">

        </span>
    </body>
</html>



属性选择器
```




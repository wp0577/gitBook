### 1．jsp脚本和注释

jsp脚本：

1）&lt;%java代码%&gt; ----- 内部的java代码翻译到service方法的内部

2）&lt;%=java变量或表达式&gt; ----- 会被翻译成service方法内部out.print\(\)

3）&lt;%!java代码%&gt; ---- 会被翻译成servlet的成员的内容

jsp注释：不同的注释可见范围是不同

1）Html注释：&lt;!--注释内容--&gt; ---可见范围 jsp源码、翻译后的servlet、页面显示html源码

2）java注释：//单行注释 /\*多行注释\*/ --可见范围 jsp源码 翻译后的servlet

3）jsp注释：&lt;%--注释内容--%&gt; ----- 可见范围 jsp源码可见

### 2．jsp指令（3个）

jsp的指令是指导jsp翻译和运行的命令，jsp包括三大指令：

1）page指令 --- 属性最多的指令（实际开发中page指令默认）

属性最多的一个指令，根据不同的属性，指导整个页面特性

格式：&lt;%@ page 属性名1= "属性值1"属性名2= "属性值2" ...%&gt;

常用属性如下：

language：jsp脚本中可以嵌入的语言种类

pageEncoding：当前jsp文件的本身编码---内部可以包含contentType

contentType：response.setContentType\(text/html;charset=UTF-8\)

session：是否jsp在翻译时自动创建session

import：导入java的包

errorPage：当当前页面出错后跳转到哪个页面

isErrorPage：当前页面是一个处理错误的页面

2）include指令

页面包含（静态包含）指令，可以将一个jsp页面包含到另一个jsp页面中

格式：&lt;%@ include file="被包含的文件地址"%&gt;

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

**通过静态引入，可以将页面的头尾部分单独做成jsp文件之后只需要改中间内容即可。**

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

3）taglib指令

在jsp页面中引入标签库（jstl标签库、struts2标签库）

格式：&lt;%@ taglib uri="标签库地址" prefix="前缀"%&gt;

1. JSP动作标签

![](/jsp11/import.png)


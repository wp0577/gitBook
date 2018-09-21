# @RequestMapping

通过@RequestMapping注解可以定义不同的处理器映射规则。

### 1.1. URL路径映射

@RequestMapping\(value="item"\)或@RequestMapping\("/item"）

value的值是数组，可以将多个url映射到同一个方法

/\*\*

 \* 查询商品列表

 \* **@return**

 \*/

@RequestMapping\(value = { "itemList", "itemListAll" }\)

**public** ModelAndView queryItemList\(\) {

      // 查询商品数据

      List&lt;Item&gt; list = **this**.itemService.queryItemList\(\);

      // 创建ModelAndView,设置逻辑视图名

      ModelAndView mv = **new** ModelAndView\("itemList"\);

      // 把商品数据放到模型中

      mv.addObject\("itemList", list\);

      **return** mv;

}

### 1.2. 添加在类上面

在class上添加@RequestMapping\(url\)指定通用请求前缀， 限制此类下的所有方法请求url必须以请求前缀开头

可以使用此方法对url进行分类管理，如下图：

![](../../.gitbook/assets/image%20%28139%29.png)

此时需要进入queryItemList\(\)方法的请求url为：

http://127.0.0.1:8080/springmvc-web2/item/itemList.action

或者

http://127.0.0.1:8080/springmvc-web2/item/itemListAll.action

注意：学员练习此项后，把类上的@RequestMapping注释掉，如下：

//@RequestMapping\("item"\)

**public** **class** ItemController {

以方便后面的练习

### 1.3. 请求方法限定

除了可以对url进行设置，还可以限定请求进来的方法

u  限定GET方法

@RequestMapping\(method = RequestMethod.**GET**\)

如果通过POST访问则报错：

HTTP Status 405 - Request method 'POST' not supported

例如：

@RequestMapping\(value = "itemList",method = RequestMethod.**POST**\)

u  限定POST方法

@RequestMapping\(method = RequestMethod.**POST**\)

如果通过GET访问则报错：

HTTP Status 405 - Request method 'GET' not supported

u  GET和POST都可以

@RequestMapping\(method = {RequestMethod.**GET**,RequestMethod.**POST**}\)

## 2.  Controller方法返回值

### 2.1. 返回ModelAndView

controller方法中定义ModelAndView对象并返回，对象中可添加model数据、指定view。

参考第一天的内容

### 2.2. 返回void

            在Controller方法形参上可以定义request和response，使用request或response指定响应结果：

1、使用request转发页面，如下：

request.getRequestDispatcher\("页面路径"\).forward\(request, response\);

request.getRequestDispatcher\("/WEB-INF/jsp/success.jsp"\).forward\(request, response\);

2、可以通过response页面重定向：

response.sendRedirect\("url"\)

response.sendRedirect\("/springmvc-web2/itemEdit.action"\);

3、可以通过response指定响应结果，例如响应json数据如下：

response.getWriter\(\).print\("{\"abc\":123}"\);

#### 2.2.1. 代码演示

以下代码一次测试，演示上面的效果

/\*\*

 \* 返回void测试

 \*

 \* **@param** request

 \* **@param** response

 \* **@throws** Exception

 \*/

@RequestMapping\("queryItem"\)

**public** **void** queryItem\(HttpServletRequest request, HttpServletResponse response\) **throws** Exception {

      // 1 使用request进行转发

      // request.getRequestDispatcher\("/WEB-INF/jsp/success.jsp"\).forward\(request,

      // response\);

      // 2 使用response进行重定向到编辑页面

      // response.sendRedirect\("/springmvc-web2/itemEdit.action"\);

      // 3 使用response直接显示

      response.getWriter\(\).print\("{\"abc\":123}"\);

}

### 2.3. 返回字符串

#### 2.3.1. 逻辑视图名

controller方法返回字符串可以指定逻辑视图名，通过视图解析器解析为物理视图地址。

//指定逻辑视图名，经过视图解析器解析为jsp物理路径：/WEB-INF/jsp/itemList.jsp

**return** "itemList";

参考第一天内容

#### 2.3.2. Redirect重定向

Contrller方法返回字符串可以重定向到一个url地址

如下商品修改提交后重定向到商品编辑页面。

/\*\*

 \* 更新商品

 \*

 \* **@param** item

 \* **@return**

 \*/

@RequestMapping\("updateItem"\)

**public** String updateItemById\(Item item\) {

      // 更新商品

      **this**.itemService.updateItemById\(item\);

      // 修改商品成功后，重定向到商品编辑页面

      // 重定向后浏览器地址栏变更为重定向的地址，

      // 重定向相当于执行了新的request和response，所以之前的请求参数都会丢失

      // 如果要指定请求参数，需要在重定向的url后面添加 ?itemId=1 这样的请求参数

      **return** "redirect:/itemEdit.action?itemId=" + item.getId\(\);

}

#### 2.3.3. forward转发

Controller方法执行后继续执行另一个Controller方法

如下商品修改提交后转向到商品修改页面，修改商品的id参数可以带到商品修改方法中。

/\*\*

 \* 更新商品

 \*

 \* **@param** item

 \* **@return**

 \*/

@RequestMapping\("updateItem"\)

**public** String updateItemById\(Item item\) {

      // 更新商品

      **this**.itemService.updateItemById\(item\);

      // 修改商品成功后，重定向到商品编辑页面

      // 重定向后浏览器地址栏变更为重定向的地址，

      // 重定向相当于执行了新的request和response，所以之前的请求参数都会丢失

      // 如果要指定请求参数，需要在重定向的url后面添加 ?itemId=1 这样的请求参数

      // return "redirect:/itemEdit.action?itemId=" + item.getId\(\);

      // 修改商品成功后，继续执行另一个方法

      // 使用转发的方式实现。转发后浏览器地址栏还是原来的请求地址，

      // 转发并没有执行新的request和response，所以之前的请求参数都存在

      **return** "forward:/itemEdit.action";

}

//结果转发到editItem.action，request可以带过去

**return** "forward: /itemEdit.action";

需要修改之前编写的根据id查询商品方法

因为请求进行修改商品时，请求参数里面只有id属性，没有itemId属性

修改，如下图：：

![](../../.gitbook/assets/image%20%2846%29.png)


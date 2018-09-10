# RESTful支持

### 1.1. 什么是restful？

Restful就是一个资源定位及资源操作的风格。不是标准也不是协议，只是一种风格。基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存等机制。

资源：互联网所有的事物都可以被抽象为资源

资源操作：使用POST、DELETE、PUT、GET，使用不同方法对资源进行操作。

      分别对应 添加、 删除、修改、查询。

传统方式操作资源

http://127.0.0.1/item/queryItem.action?id=1          查询,GET

http://127.0.0.1/item/saveItem.action                                 新增,POST

http://127.0.0.1/item/updateItem.action                 更新,POST

http://127.0.0.1/item/deleteItem.action?id=1         删除,GET或POST

使用RESTful操作资源

http://127.0.0.1/item/1                     查询,GET

http://127.0.0.1/item             新增,POST

http://127.0.0.1/item             更新,PUT

http://127.0.0.1/item/1                     删除,DELETE

### 1.2. 需求

RESTful方式实现商品信息查询，返回json数据

### 1.3. 从URL上获取参数

使用RESTful风格开发的接口，根据id查询商品，接口地址是：

http://127.0.0.1/item/1

我们需要从url上获取商品id，步骤如下：

1. 使用注解@RequestMapping\("item/{id}"\)声明请求的url

{xxx}叫做占位符，请求的URL可以是“item /1”或“item/2”

2. 使用\(@PathVariable\(\) Integer id\)获取url上的数据

/\*\*

 \* 使用RESTful风格开发接口，实现根据id查询商品

 \*

 \* **@param** id

 \* **@return**

 \*/

@RequestMapping\("item/{id}"\)

@ResponseBody

**public** Item queryItemById\(@PathVariable\(\) Integer id\) {

      Item item = **this**.itemService.queryItemById\(id\);

      **return** item;

}

如果@RequestMapping中表示为"item/{id}"，id和形参名称一致，@PathVariable不用指定名称。如果不一致，例如"item/{ItemId}"则需要指定名称@PathVariable\("itemId"\)。

http://127.0.0.1/item/123?id=1

注意两个区别

1. @PathVariable是获取url上数据的。@RequestParam获取请求参数的（包括post表单提交）

2. 如果加上@ResponseBody注解，就不会走视图解析器，不会返回页面，目前返回的json数据。如果不加，就走视图解析器，返回页面


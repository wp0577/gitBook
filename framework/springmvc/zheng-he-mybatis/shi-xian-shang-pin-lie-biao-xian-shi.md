# 实现商品列表显示

实现商品查询列表，从mysql数据库查询商品信息。

### 1.1. DAO开发

使用逆向工程，生成代码

注意修改逆向工程的配置文件，参考MyBatis第二天

逆向工程生成代码如下图：

![](../../../.gitbook/assets/image%20%28186%29.png)

### 1.2. ItemService接口

**public** **interface** ItemService {

      /\*\*

       \* 查询商品列表

       \*

       \* **@return**

       \*/

      List&lt;Item&gt; queryItemList\(\);

}

### 1.3. ItemServiceImpl实现类

@Service

**public** **class** ItemServiceImpl **implements** ItemService {

      @Autowired

      **private** ItemMapper itemMapper;

      @Override

      **public** List&lt;Item&gt; queryItemList\(\) {

            // 从数据库查询商品数据

            List&lt;Item&gt; list = **this**.itemMapper.selectByExample\(**null**\);

            **return** list;

      }

}

### 1.4. ItemController

@Controller

**public** **class** ItemController {

      @Autowired

      **private** ItemService itemService;

      /\*\*

       \* 显示商品列表

       \*

       \* **@return**

       \*/

      @RequestMapping\("/itemList"\)

      **public** ModelAndView queryItemList\(\) {

            // 获取商品数据

            List&lt;Item&gt; list = **this**.itemService.queryItemList\(\);

            ModelAndView modelAndView = **new** ModelAndView\(\);

            // 把商品数据放到模型中

            modelAndView.addObject\("itemList", list\);

            // 设置逻辑视图

            modelAndView.setViewName\("itemList"\);

            **return** modelAndView;

      }

}


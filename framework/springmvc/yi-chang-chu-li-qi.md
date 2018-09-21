# 异常处理器

springmvc在处理请求过程中出现异常信息交由异常处理器进行处理，自定义异常处理器可以实现一个系统的异常处理逻辑。

### 1.1. 异常处理思路

            系统中异常包括两类：预期异常和运行时异常RuntimeException，前者通过捕获异常从而获取异常信息，后者主要通过规范代码开发、测试通过手段减少运行时异常的发生。

            系统的dao、service、controller出现都通过throws Exception向上抛出，最后由springmvc前端控制器交由异常处理器进行异常处理，如下图：

![](../../.gitbook/assets/image%20%2836%29.png)

Controller客户端ServiceDaoSpringmvcDispatcherServlet请求异常HandlerExceptionResolver异常处理器异常异常

### 1.2. 自定义异常类

            为了区别不同的异常,通常根据异常类型进行区分，这里我们创建一个自定义系统异常。

如果controller、service、dao抛出此类异常说明是系统预期处理的异常信息。

**public** **class** MyException **extends** Exception {

      // 异常信息

      **private** String message;

      **public** MyException\(\) {

            **super**\(\);

      }

      **public** MyException\(String message\) {

            **super**\(\);

            **this**.message = message;

      }

      **public** String getMessage\(\) {

            **return** message;

      }

      **public** **void** setMessage\(String message\) {

            **this**.message = message;

      }

}

### 1.3. 自定义异常处理器

**public** **class** CustomHandleException **implements** HandlerExceptionResolver {

      @Override

      **public** ModelAndView resolveException\(HttpServletRequest request, HttpServletResponse response, Object handler,

                  Exception exception\) {

            // 定义异常信息

            String msg;

            // 判断异常类型

            **if** \(exception **instanceof** MyException\) {

                  // 如果是自定义异常，读取异常信息

                  msg = exception.getMessage\(\);

            } **else** {

                  // 如果是运行时异常，则取错误堆栈，从堆栈中获取异常信息

                  Writer out = **new** StringWriter\(\);

                  PrintWriter s = **new** PrintWriter\(out\);

                  exception.printStackTrace\(s\);

                  msg = out.toString\(\);

            }

            // 把错误信息发给相关人员,邮件,短信等方式

            // **TODO**

            // 返回错误页面，给用户友好页面显示错误信息

            ModelAndView modelAndView = **new** ModelAndView\(\);

            modelAndView.addObject\("msg", msg\);

            modelAndView.setViewName\("error"\);

            **return** modelAndView;

      }

}

### 1.4. 异常处理器配置

在springmvc.xml中添加：

&lt;!-- 配置全局异常处理器 --&gt;

&lt;bean

id="customHandleException"      class="cn.itcast.ssm.exception.CustomHandleException"/&gt;

### 1.5. 错误页面

&lt;%@ page language="java" contentType="text/html; charset=UTF-8"

      pageEncoding="UTF-8"%&gt;

&lt;!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd"&gt;

&lt;html&gt;

&lt;head&gt;

&lt;meta http-equiv="Content-Type" content="text/html; charset=UTF-8"&gt;

&lt;title&gt;Insert title here&lt;/title&gt;

&lt;/head&gt;

&lt;body&gt;

      &lt;h1&gt;系统发生异常了！&lt;/h1&gt;

      &lt;br /&gt;

      &lt;h1&gt;异常信息&lt;/h1&gt;

      &lt;br /&gt;

      &lt;h2&gt;${msg }&lt;/h2&gt;

&lt;/body&gt;

&lt;/html&gt;

### 1.6. 异常测试

修改ItemController方法“queryItemList”，抛出异常：

/\*\*

 \* 查询商品列表

 \*

 \* **@return**

 \* **@throws** Exception

 \*/

@RequestMapping\(value = { "itemList", "itemListAll" }\)

**public** ModelAndView queryItemList\(\) **throws** Exception {

      // 自定义异常

      **if** \(**true**\) {

            **throw** **new** MyException\("自定义异常出现了~"\);

      }

      // 运行时异常

      **int** a = 1 / 0;

      // 查询商品数据

      List&lt;Item&gt; list = **this**.itemService.queryItemList\(\);

      // 创建ModelAndView,设置逻辑视图名

      ModelAndView mv = **new** ModelAndView\("itemList"\);

      // 把商品数据放到模型中

      mv.addObject\("itemList", list\);

      **return** mv;

}


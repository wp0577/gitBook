# 拦截器

### 1.1. 定义

            Spring Web MVC 的处理器拦截器类似于Servlet 开发中的过滤器Filter，用于对处理器进行预处理和后处理。

### 1.2. 拦截器定义

实现HandlerInterceptor接口，如下：

**public** **class** HandlerInterceptor1 **implements** HandlerInterceptor {

      // controller执行后且视图返回后调用此方法

      // 这里可得到执行controller时的异常信息

      // 这里可记录操作日志

      @Override

      **public** **void** afterCompletion\(HttpServletRequest arg0, HttpServletResponse arg1, Object arg2, Exception arg3\)

                  **throws** Exception {

            System.**out**.println\("HandlerInterceptor1....afterCompletion"\);

      }

      // controller执行后但未返回视图前调用此方法

      // 这里可在返回用户前对模型数据进行加工处理，比如这里加入公用信息以便页面显示

      @Override

      **public** **void** postHandle\(HttpServletRequest arg0, HttpServletResponse arg1, Object arg2, ModelAndView arg3\)

                  **throws** Exception {

            System.**out**.println\("HandlerInterceptor1....postHandle"\);

      }

      // Controller执行前调用此方法

      // 返回true表示继续执行，返回false中止执行

      // 这里可以加入登录校验、权限拦截等

      @Override

      **public** **boolean** preHandle\(HttpServletRequest arg0, HttpServletResponse arg1, Object arg2\) **throws** Exception {

            System.**out**.println\("HandlerInterceptor1....preHandle"\);

            // 设置为true，测试使用

            **return** **true**;

      }

}

### 1.3. 拦截器配置

上面定义的拦截器再复制一份HandlerInterceptor2，注意新的拦截器修改代码：

System.**out**.println\("HandlerInterceptor2....preHandle"\);

在springmvc.xml中配置拦截器

&lt;!-- 配置拦截器 --&gt;

&lt;mvc:interceptors&gt;

      &lt;mvc:interceptor&gt;

            &lt;!-- 所有的请求都进入拦截器 --&gt;

            &lt;mvc:mapping path="/\*\*" /&gt;

            &lt;!-- 配置具体的拦截器 --&gt;

            &lt;bean class="cn.itcast.ssm.interceptor.HandlerInterceptor1" /&gt;

      &lt;/mvc:interceptor&gt;

      &lt;mvc:interceptor&gt;

            &lt;!-- 所有的请求都进入拦截器 --&gt;

            &lt;mvc:mapping path="/\*\*" /&gt;

            &lt;!-- 配置具体的拦截器 --&gt;

            &lt;bean class="cn.itcast.ssm.interceptor.HandlerInterceptor2" /&gt;

      &lt;/mvc:interceptor&gt;

&lt;/mvc:interceptors&gt;

### 1.4. 正常流程测试

浏览器访问地址

http://127.0.0.1:8080/springmvc-web2/itemList.action

#### 1.4.1. 运行流程

控制台打印：

HandlerInterceptor1..preHandle..

HandlerInterceptor2..preHandle..

HandlerInterceptor2..postHandle..

HandlerInterceptor1..postHandle..

HandlerInterceptor2..afterCompletion..

HandlerInterceptor1..afterCompletion..

### 1.5. 中断流程测试

浏览器访问地址

http://127.0.0.1:8080/springmvc-web2/itemList.action

#### 1.5.1. 运行流程

HandlerInterceptor1的preHandler方法返回false，HandlerInterceptor2返回true，运行流程如下：

HandlerInterceptor1..preHandle..

从日志看出第一个拦截器的preHandler方法返回false后第一个拦截器只执行了preHandler方法，其它两个方法没有执行，第二个拦截器的所有方法不执行，且Controller也不执行了。

HandlerInterceptor1的preHandler方法返回true，HandlerInterceptor2返回false，运行流程如下：

HandlerInterceptor1..preHandle..

HandlerInterceptor2..preHandle..

HandlerInterceptor1..afterCompletion..

从日志看出第二个拦截器的preHandler方法返回false后第一个拦截器的postHandler没有执行，第二个拦截器的postHandler和afterCompletion没有执行，且controller也不执行了。

总结：

preHandle按拦截器定义顺序调用

postHandler按拦截器定义逆序调用

afterCompletion按拦截器定义逆序调用

postHandler在拦截器链内所有拦截器返成功调用

afterCompletion只有preHandle返回true才调用

### 1.6. 拦截器应用

#### 1.6.1. 处理流程

1、有一个登录页面，需要写一个Controller访问登录页面

2、登录页面有一提交表单的动作。需要在Controller中处理。

a\)      判断用户名密码是否正确（在控制台打印）

b\)      如果正确,向session中写入用户信息（写入用户名username）

c\)      跳转到商品列表

3、拦截器。

a\)      拦截用户请求，判断用户是否登录（登录请求不能拦截）

b\)      如果用户已经登录。放行

c\)      如果用户未登录，跳转到登录页面。

#### 1.6.2. 编写登录jsp

&lt;%@ page language="java" contentType="text/html; charset=UTF-8"

      pageEncoding="UTF-8"%&gt;

&lt;!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd"&gt;

&lt;html&gt;

&lt;head&gt;

&lt;meta http-equiv="Content-Type" content="text/html; charset=UTF-8"&gt;

&lt;title&gt;Insert title here&lt;/title&gt;

&lt;/head&gt;

&lt;body&gt;

&lt;form action="${pageContext.request.contextPath }/user/login.action"&gt;

&lt;label&gt;用户名：&lt;/label&gt;

&lt;br&gt;

&lt;input type="text" name="username"&gt;

&lt;br&gt;

&lt;label&gt;密码：&lt;/label&gt;

&lt;br&gt;

&lt;input type="password" name="password"&gt;

&lt;br&gt;

&lt;input type="submit"&gt;

&lt;/form&gt;

&lt;/body&gt;

&lt;/html&gt;

#### 1.6.3. 用户登陆Controller

@Controller

@RequestMapping\("user"\)

**public** **class** UserController {

      /\*\*

       \* 跳转到登录页面

       \*

       \* **@return**

       \*/

      @RequestMapping\("toLogin"\)

      **public** String toLogin\(\) {

            **return** "login";

      }

      /\*\*

       \* 用户登录

       \*

       \* **@param** username

       \* **@param** password

       \* **@param** session

       \* **@return**

       \*/

      @RequestMapping\("login"\)

      **public** String login\(String username, String password, HttpSession session\) {

            // 校验用户登录

            System.**out**.println\(username\);

            System.**out**.println\(password\);

            // 把用户名放到session中

            session.setAttribute\("username", username\);

            **return** "redirect:/item/itemList.action";

      }

}

#### 1.6.4. 编写拦截器

@Override

**public** **boolean** preHandle\(HttpServletRequest request, HttpServletResponse response, Object arg2\) **throws** Exception {

      // 从request中获取session

      HttpSession session = request.getSession\(\);

      // 从session中获取username

      Object username = session.getAttribute\("username"\);

      // 判断username是否为null

      **if** \(username != **null**\) {

            // 如果不为空则放行

            **return** **true**;

      } **else** {

            // 如果为空则跳转到登录页面

            response.sendRedirect\(request.getContextPath\(\) + "/user/toLogin.action"\);

      }

      **return** **false**;

}

#### 1.6.5. 配置拦截器

只能拦截商品的url，所以需要修改ItemController，让所有的请求都必须以item开头，如下图：

在springmvc.xml配置拦截器

&lt;mvc:interceptor&gt;

      &lt;!-- 配置商品被拦截器拦截 --&gt;

      &lt;mvc:mapping path="/item/\*\*" /&gt;

      &lt;!-- 配置具体的拦截器 --&gt;

      &lt;bean class="cn.itcast.ssm.interceptor.LoginHandlerInterceptor" /&gt;

&lt;/mvc:interceptor&gt;


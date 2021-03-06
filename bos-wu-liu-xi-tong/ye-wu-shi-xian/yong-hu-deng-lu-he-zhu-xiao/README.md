# 用户登陆和注销

## js实现a标签超链接提交form表单的方法

这里a标签可以有以下几种写法：

方法1：  
代码如下:&lt;a class="searchPic h-submitBtn png" id="h-submitBtn"  onclick="document:search\_form.submit\(\);"&gt;提交&lt;/a&gt;

方法2：  
代码如下:&lt;a href="javascript:document:search\_form.submit\(\);"&gt;提交&lt;/a&gt;

方法3:  
代码如下:&lt;a class="searchPic h-submitBtn png" id="h-submitBtn"  onclick="document.getElementById\('search\_form'\).submit\(\);"&gt;提交&lt;/a&gt;&gt;

{% hint style="info" %}
为什么

```text
@Autowired
private IUserService userService;
```

 通过这段代码，使用userService的方法会自动调用其接口实现类的方法？？？

可能原因：

因为在IUserService实现类中用到了

```text
@Service
public class UserServiceImp implements IUserService<User>{
```

 的注解。因此Autowired在进行bean扫描时便会寻找属于IUserService类型的bean，而UserServiceImp因为正好实现了IUserService的接口，所以能够被其直接调用去实现的方法。

有点类似于

```text
IUserService userService = new UserServiceImp();
```
{% endhint %}



## 用户登陆的实现

{% code-tabs %}
{% code-tabs-item title="UserAction" %}
```java
private String checkcode;
    @Autowired
    private IUserService userService;

    public String login() throws Exception {
        //get correct checkcode from session
        String rightCheckcode = (String) ServletActionContext.getRequest().getSession().getAttribute("key");
        if(rightCheckcode.equals(checkcode)) {
            User user = (User) userService.login(model);
            if(user == null) {
                addActionError("username or password wrong");
                return LOGIN;
            }
            ServletActionContext.getRequest().getSession().setAttribute("user", user);
            return SUCCESS;
        }
        else {
            //
            addActionError("the validate code is wrong");
            return LOGIN;
        }
    }

```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="info" %}
addActionError\(""\);方法所保存的信息能够直接在jsp中通过

&lt;s:actionerror/&gt;显示在页面中
{% endhint %}

## 在首页显示用户名字

jsp获取action传来的session值有以下几种方法：

如action中有一个session\("sessionnid","123456"\)

一：用struts标签获取：&lt;s:property value="\#session.sessionid"/&gt;

二：&lt;%=request.getSession.getAttribute\("sessionid"\);&gt;

session也是jsp内置对象之一，可以直接用session,比request.getSession方便很多

也可以写成&lt;%=session.getAttribute\("sessionid"\);&gt;

三：el表达式获取：${sessionScope.sessionid}

如果是对象可以用'${sessionScope\["com.\*\*\*.system.pojo.SessionInfo"\].ck\_loginid}'

&lt;%SessionInfo sessionInfo=\(SessionInfo\)session.getAttribut\("com.\*\*\*.system.pojo.SessionInfo"\);%&gt;



## 用户注销功能

```text
ServletActionContext.getRequest().getSession().invalidate();
```

 


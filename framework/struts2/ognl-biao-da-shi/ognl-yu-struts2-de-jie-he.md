# OGNL与Struts2的结合

## 原理

![](../../../.gitbook/assets/image%20%2811%29.png)

ValueStack就是类似于OGNL中的OGNLContext。

{% hint style="info" %}
什么是值栈
{% endhint %}

![](../../../.gitbook/assets/image%20%2883%29.png)



## 栈原理

![](../../../.gitbook/assets/image.png)

![](../../../.gitbook/assets/image%20%2821%29.png)

## 查看值栈中两部分内容\(使用DEBUG标签\)

标签引入&lt;%@ taglib prefix="s" uri="/struts-tags" %&gt;

调试标签&lt;s:debug&gt;&lt;/s:debug&gt;

![](../../../.gitbook/assets/image%20%2838%29.png)

![](../../../.gitbook/assets/image%20%2852%29.png)

## 参数接收具体流程

![](../../../.gitbook/assets/image%20%286%29.png)

![](../../../.gitbook/assets/image%20%2830%29.png)

## 配置文件中的使用

![](../../../.gitbook/assets/image%20%289%29.png)

## request查找顺序

![](../../../.gitbook/assets/image%20%2824%29.png)


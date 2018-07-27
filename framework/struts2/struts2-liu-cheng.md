# struts2 流程

## 访问流程

![](../../.gitbook/assets/image%20%281%29.png)

## Struts2架构

Interceptor: 拦截器。Struts2相对于1而言最核心的思想就是拦截器，包含了很多架构思想。

![&#x5177;&#x4F53;&#x6D41;&#x7A0B;&#x53EF;&#x4EE5;&#x770B;youtube&#x89C6;&#x9891;Struts15](../../.gitbook/assets/image%20%28110%29.png)

{% hint style="info" %}
Action对象在Interceptor中间是因为Interceptor的方法实现中有前处理和后处理，先执行前处理，再处理action最后是后处理。
{% endhint %}

##  AOP

Aspect oriented programming\(面向切面编程\)

![](../../.gitbook/assets/image%20%28127%29.png)

![](../../.gitbook/assets/image%20%286%29.png)




# Spring和Struts整合

## 单独配置struts

[https://wp0577.gitbook.io/test1/~/edit/drafts/-LIS1NTNgPF7KhT3EUYs/framework/spring/ssh-zheng-he/spring-he-struts-zheng-he](https://wp0577.gitbook.io/test1/~/edit/drafts/-LIS1NTNgPF7KhT3EUYs/framework/spring/ssh-zheng-he/spring-he-struts-zheng-he)

## Spring和Struts整合

### 配置常量

在struts.xml中配置

![](../../../.gitbook/assets/image%20%2899%29.png)

![](../../../.gitbook/assets/image%20%2822%29.png)

## 整合方案

### 1：struts2自己创建action,spring负责组装依赖属性

![&#x4E0D;&#x63A8;&#x8350;&#x7406;&#x7531;:&#x6700;&#x597D;&#x7531;spring&#x5B8C;&#x6574;&#x7BA1;&#x7406;action&#x7684;&#x751F;&#x547D;&#x5468;&#x671F;.spring&#x4E2D;&#x529F;&#x80FD;&#x624D;&#x5E94;&#x7528;&#x5230;Action&#x4E0A;.](../../../.gitbook/assets/image%20%2861%29.png)

### 2：spring负责创建action以及组装.

![ApplicationContext.xml](../../../.gitbook/assets/image%20%28116%29.png)

![struts.xml](../../../.gitbook/assets/image%20%2895%29.png)




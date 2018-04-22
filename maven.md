[https://www.cnblogs.com/hongwz/p/5456578.html](https://www.cnblogs.com/hongwz/p/5456578.html "Maven ")

**Maven的作用**

1. 在开发中，为了保证编译通过，我们会到处去寻找jar包，当编译通过了，运行的时候，却发现"ClassNotFoundException"，我们想到的是，难道还差jar包？
2. 每个Java项目的目录结构都没有一个统一的标准，配置文件到处都是，单元测试代码到底应该放在那里也没有一个权威的规范。
3. 因此，我们就要用到Maven（使用Ant也可以，不过编写Ant的xml脚本比较麻烦）----一个
   **项目管理工具**
   。
4. Maven主要做了两件事：

5. 统一开发规范与工具

6. 统一管理jar包

**pom.xml**，这是Maven的核心配置文件，pom称为Project Object Model（**项目对象模型**），它用于描述整个Maven项目，所以也称为Maven描述文件。

[https://blog.csdn.net/qq\_27093465/article/details/63683873](https://blog.csdn.net/qq_27093465/article/details/63683873 " IntelliJ IDEA 创建 hello world Java web Maven")

[https://blog.csdn.net/qq\_27093465/article/details/68961393](https://blog.csdn.net/qq_27093465/article/details/68961393 "https://blog.csdn.net/qq\_27093465/article/details/68961393")

---

Artifact 是maven中的一个概念，表示某个module要如何打包。这又多个模块的概念，不懂的就先当他是个web项目吧，

例如war exploded、war、jar、ear等等这种打包形式；

一个module有了 Artifacts 就可以部署到应用服务器中了！

[https://blog.csdn.net/qq\_27093465/article/details/76091287](https://blog.csdn.net/qq_27093465/article/details/76091287)

想在哪新建Java class，就给设置为src.

[https://blog.csdn.net/qq\_27093465/article/details/52912444](https://blog.csdn.net/qq_27093465/article/details/52912444 " IntelliJ IDEA 中 右键新建时，选项没有Java class的解决方法和具体解释")

[spring framework体系结构及内部各模块jar之间的maven依赖关系](https://blog.csdn.net/qq_27093465/article/details/65660689)


# Maven



## 1.1   Maven的好处

节省空间 对jar包做了统一管理 依赖管理

一键构建

可跨平台

应用在大型项目可提高开发效率



## 1.2 依赖范围

 5.3.1 Compile struts2-core 编译（compile）时需要 测试时需要，，运行时需要，打包时需要 5.3.2 Provided jsp-api.jar servlet-api.jar 编译（compile）时需要，测试（test）时也需要 ，运行时不需要，打包时不需要

5.3.3 Runtime 数据库驱动包 编译时不需要，测试时需要，，运行时需要，打包时需要 5.3.4 Test junit.jar 编译时不需要，测试时需要，运行时不需要，打包也不需要

添加插件 Maven add plugin



## 1.3依赖传递



![](../.gitbook/assets/image%20%2842%29.png)



## 1.4  依赖版本冲突的解决

1、 第一声明优先原则

&lt;dependencies&gt;

  &lt;!--   spring-beans-4.2.4 --&gt;

                  &lt;dependency&gt;

                                    &lt;groupId&gt;org.springframework&lt;/groupId&gt;

                                    &lt;artifactId&gt;spring-context&lt;/artifactId&gt;

                                    &lt;version&gt;4.2.4.RELEASE&lt;/version&gt;

                  &lt;/dependency&gt;

&lt;!--   spring-beans-3.0.5 --&gt;

                  &lt;dependency&gt;

                                    &lt;groupId&gt;org.apache.struts&lt;/groupId&gt;

                                    &lt;artifactId&gt;struts2-spring-plugin&lt;/artifactId&gt;

                                    &lt;version&gt;2.3.24&lt;/version&gt;

                  &lt;/dependency&gt;

2、 路径近者优先原则

自己添加jar包

                  &lt;dependency&gt;

                                    &lt;groupId&gt;org.springframework&lt;/groupId&gt;

                                    &lt;artifactId&gt;spring-beans&lt;/artifactId&gt;

                                    &lt;version&gt;4.2.4.RELEASE&lt;/version&gt;

                  &lt;/dependency&gt;

3、 排除原则

                  &lt;dependency&gt;

                                    &lt;groupId&gt;org.apache.struts&lt;/groupId&gt;

                                    &lt;artifactId&gt;struts2-spring-plugin&lt;/artifactId&gt;

                                    &lt;version&gt;2.3.24&lt;/version&gt;

                                    &lt;exclusions&gt;

                                      &lt;exclusion&gt;

                                        &lt;groupId&gt;org.springframework&lt;/groupId&gt;

                                        &lt;artifactId&gt;spring-beans&lt;/artifactId&gt;

                                      &lt;/exclusion&gt;

                                    &lt;/exclusions&gt;

                  &lt;/dependency&gt;

4、 版本锁定原则

&lt;properties&gt;

                                    &lt;spring.version&gt;4.2.4.RELEASE&lt;/spring.version&gt;

                                    &lt;hibernate.version&gt;5.0.7.Final&lt;/hibernate.version&gt;

                                    &lt;struts.version&gt;2.3.24&lt;/struts.version&gt;

                  &lt;/properties&gt;

                  &lt;!-- 锁定版本，struts2-2.3.24、spring4.2.4、hibernate5.0.7 --&gt;

                  &lt;dependencyManagement&gt;

                                    &lt;dependencies&gt;

                                                      &lt;dependency&gt;

                                                                        &lt;groupId&gt;org.springframework&lt;/groupId&gt;

                                                                        &lt;artifactId&gt;spring-context&lt;/artifactId&gt;

                                                                        &lt;version&gt;${spring.version}&lt;/version&gt;

                                                      &lt;/dependency&gt;

&lt;/dependencies&gt;

&lt;/dependencyManagement&gt;


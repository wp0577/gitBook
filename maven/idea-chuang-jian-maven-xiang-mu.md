# idea创建maven项目

一：File -&gt;New Project，左边菜单选择maven项目，右边勾选Create from archetype，找到org.apache.maven.archetype:maven-archetype-webapp，这个是Maven项目的一个骨架，就好像住酒店时候，你选标间，还是单间，还是大床，然后里面的配置不一样。

![](https://img-blog.csdn.net/20150920223917733?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

 点击Next按钮

二、 因为是maven项目，所以需要项目的groupid, artifactId version 这是Maven项目的坐标，必填

![](https://img-blog.csdn.net/20150920224122264?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

 继续点击Next

三、

![](https://img-blog.csdn.net/20150920224215942?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

   这个窗口基本上不用修改什么，但是这样会比较慢，有时候如果网速不好，就会卡的比较久，这是因为maven这个骨架会从远程仓库加载archetype元数据，但是archetype又比较多，所以比较卡，这时候可以加个属性 archetypeCatelog = internal，表示仅使用内部元数据，点击右边的蓝色“+”号

![](https://img-blog.csdn.net/20150920224628458?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

  Next ---&gt;

四、输入Project name，输入project name 后，我们会发现下面的Module name跟上面一样，但有时候我们的项目比较大，会分好几个module，这时候可以输入自己的module name，当然也可以不改，则 module name 和 project name一样

![](https://img-blog.csdn.net/20150920225011546?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

五、Finish， 项目会去配置的仓库中下载对应的构件和依赖

![](https://img-blog.csdn.net/20150920225149152?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

  这时候已经完成大部分了，不过我们还需要新建几个目录文件，因为maven项目的文件结构是 src-main-[Java](http://lib.csdn.net/base/javase) / resources，src-test-java/resources，但骨架只自带了resources，所以需要我们手动添加文件目录。

加载完成后，下面控制台会有BUILD SUCCESS的字样，表示加载成功，这时，有个我自己觉得比较关键的一步：maven可能由于缓存或其他原因，需要我们手动在右边的maven project页签上，刷新一下， 最好刷新下 

![](https://img-blog.csdn.net/20150921103139837?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

 疑点1： 如果maven项目新建成功后，他会自动在Project Structure的articfactId上出现两个生成的打包方式，如果没有生成，不要自己手动添加，按上一步刷新下maven project即可，他会自动生成

![](https://img-blog.csdn.net/20150921103415704?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

六、增加文件目录

 在main下面新建java文件夹，这时候还不能直接新建类，需要配置下

![](https://img-blog.csdn.net/20150920225503487?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

配置，点File -&gt; Project  Structure, 选择Modules，右边找到java这层机构，在上面有个“Mask as”, 点下Sources，表示这里面是源代码类![](https://img-blog.csdn.net/20150920225631587?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

同样，test结构，在src下新建test，再test下面建java, resources，跟上面一样，建好后，打开Project Structure，找到对应目录后

![](https://img-blog.csdn.net/20150920230105899?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

 OK， Maven项目已经搭建完成，如何跟tomcat集成呢，这个可以参考上一篇的结束，本篇只做个大概的介绍

配置Tomcat

一、Run -&gt; Edit Configurations![](https://img-blog.csdn.net/20150920230249391?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

![](https://img-blog.csdn.net/20150920230338741?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

出现tomcat配置界面

![](https://img-blog.csdn.net/20150921103735509?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

在name文本框输入自己的服务名称，可随便起。选项卡切到Deploymeng，把打包项目加进去

![](https://img-blog.csdn.net/20150921103918507?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

选择出现的打包方式，![](https://img-blog.csdn.net/20150921104000108?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

![](https://img-blog.csdn.net/20150921104033476?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

选择好了，输入你的名称，保存

再切到Server选项卡

![](https://img-blog.csdn.net/20150921104157404?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

保存，项目搭建完成，你可以在pom.xml文件里加入自己的maven相关配置。 启动服务。。。

![](https://img-blog.csdn.net/20150921104321639?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

![](https://img-blog.csdn.net/20150921104334314?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)


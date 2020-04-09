---
date: 2020-04-09 16:37
status: public
title: 'openTCS 持久化'
---

# 该文件主要记录openTCS二次开发中遇到的问题：数据持久化问题
##介绍：本次采用的框架组合是：guice，Gradle，MyBatis和MySQL。
##首先，本次问题一共需要4份插件的帮助，分别是MyBatis Generator，MYSQL驱动，MyBatis Mapper和iBatis（如有请忽略）
整体文件结构如图1.
![](~/16-53-59.jpg)
                                       图1  文件结构
##（1）配置MyBatis Generator   
在build.gradle 首先实现mybatis Generator配置，图1 的S1文件，这部分由代码单独运行，不需要参与编译。具体操作如图2
![](~/16-59-02.jpg)
          图2  MyBatis Generator 配置
注意：这里，很多blog都有MyBatis Generator 路径等配置信息，如图3所示。经过验证，不需要此操作。
![](~/17-01-33.jpg)
                                                                    图3  不需要的配置操作
##（2） 在openTCS-Kernel中build.gradle文件中实现依赖注入。
本次需要的是四份三方插件，在https://mvnrepository.com 可以寻找到。
本次选择的插件具体信息如下：

![](~/17-09-01.jpg)
                                                                                                图4   插件信息
将图4中红色框中注入语句，添加到build.gradle中，但是需要修改一下，因为前3插件与MyBatis Generator直接链接。而最后以后是在打包后生成的文件中使用到。具体如下：
![](~/17-11-07.jpg)
                                                                                图5  注入依赖操作                                                                                                
##（3）设置数据库配置信息
这里主要进行数据库配置，主要修改（没有就创建）图1 的S2文件。文件具体内容如下：
图6主要配置的mysql数据库的基本信息以及打包后生成的文件路径位置设置。包文件路径可以自己根据需求修改。
![](~/17-17-04.jpg)
                                                                                                        图6  mysql配置
图7 是数据库表配置信息，具体完整的中文配置详解，见 https://www.cnblogs.com/ygjlch/p/6471924.html
![](~/17-21-22.jpg)
                                                                                                图7 配置信息
##（4）Gradle设置Ant Task
因为MyBatis Generator不支持Gradle，需要使用Ant Task进行代替。这里动用的还是build.gradle文件
![](~/17-24-17.jpg)
                                                                                            图8  ant task操作
##至此，持久化部分的操作都已完成。但是还需要将这部分工作加入到内核启动配置中，修改图1的S3文件。如图9
![](~/17-31-13.jpg)
##至此，所有操作完成！



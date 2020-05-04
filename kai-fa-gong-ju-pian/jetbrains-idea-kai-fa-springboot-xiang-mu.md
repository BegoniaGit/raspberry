---
description: JetBrains IDEA 开发 SpringBoot 项目
---

# JetBrains IDEA 开发 SpringBoot 项目

1. 首先要理解的是 JetBrains 系列工具和插件都是由 Java 编写的，所以其运行是需要先配置Java环境，Java环境最好按照 JAVA\_HOME 的方式配置，因为有些开发工具会自动通过 JAVA\_HOME 环境变量去找Java 环境。
2. IDEA 自带 Maven ，但是很多时候会遇到很奇怪的问题，所以建议单独下载 Maven 并配置好 settings.xml 文件，使用阿里源，若到公司实习会替换为公司内部仓库地址。

        

![settings.xml &#x6240;&#x5728;&#x76EE;&#x5F55;](https://shaosim-image.oss-cn-chengdu.aliyuncs.com/20200413114837.png)

1. 在 IDEA 中配置 Maven 环境，.m2/ 目录在你的 ’home‘ 文件夹下 
2. 完成如上的基本配置，就可以创建或导入 Spring Boot 项目了，对于导入的项目，可能需要手动设置 Java 环境和 Maven 环境，请注意项目未下载依赖即手动刷新 Maven 依赖，如下图。

![&#x70B9;&#x51FB;&#x5DE6;&#x4E0A;&#x89D2;&#x7684;&#x5237;&#x65B0;](https://shaosim-image.oss-cn-chengdu.aliyuncs.com/20200413123405.png)

![&#x914D;&#x7F6E;&#x5982;&#x56FE;](https://shaosim-image.oss-cn-chengdu.aliyuncs.com/20200413115322.png)


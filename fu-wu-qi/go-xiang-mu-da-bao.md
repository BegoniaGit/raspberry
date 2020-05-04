# Go 项目打包

1. 静态文件不会打入包中，而是需要手动放到程序所在的同级目录。

* maven项目打包可独立执行的jar包

1. 引入 maven-assembly-plugin ，并修改入口类名。

```text
<build>
       <plugins>
           <plugin>
               <groupId>org.apache.maven.plugins</groupId>
               <artifactId>maven-assembly-plugin</artifactId>
               <configuration>
                   <archive>
                       <manifest>
                           <mainClass>site.yan.gateway.Boot</mainClass>
                       </manifest>
                   </archive>
                   <descriptorRefs>
                       <descriptorRef>
                          jar-with-dependencies
                       </descriptorRef>
                   </descriptorRefs>
               </configuration>
           </plugin>
       </plugins>
</build>
```

1. 点击 maven 的 package 按钮即可完成打包。


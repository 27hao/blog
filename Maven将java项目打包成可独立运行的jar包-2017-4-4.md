---
title: Maven将java项目打包成可独立运行的jar包
categories: 其它
date: 2017-04-04 13:03:26
---

在java项目中，常常需要打包成独立jar包方便发送或者发放。而maven默认的打包插件并没有在项目清单MANIFEST.MF中添加主类和依赖信息，所以运行时就会出错。所以我们需要使用其它的插件来打包。
### 方法一 使用maven-assembly-plugin插件 ###
```xml
<build>  
    <plugins>  
  
        <plugin>  
            <groupId>org.apache.maven.plugins</groupId>  
            <artifactId>maven-assembly-plugin</artifactId>  
            <version>2.5.5</version>  
            <configuration>  
                <archive>  
                    <manifest>  
                        <mainClass>com.xxg.Main</mainClass>  
                    </manifest>  
                </archive>  
                <descriptorRefs>  
                    <descriptorRef>jar-with-dependencies</descriptorRef>  
                </descriptorRefs>  
            </configuration>  
            <executions>  
                <execution>  
                    <id>make-assembly</id>  
                    <phase>package</phase>  
                    <goals>  
                        <goal>single</goal>  
                    </goals>  
                </execution>  
            </executions>  
        </plugin>  
  
    </plugins>  
</build>
```
### 方法二 使用maven-shade-plugin插件 ###
```xml
<build>  
    <plugins>  
  
        <plugin>  
            <groupId>org.apache.maven.plugins</groupId>  
            <artifactId>maven-shade-plugin</artifactId>  
            <version>2.4.1</version>  
            <executions>  
                <execution>  
                    <phase>package</phase>  
                    <goals>  
                        <goal>shade</goal>  
                    </goals>  
                    <configuration>  
                        <transformers>  
                            <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">  
                                <mainClass>com.xxg.Main</mainClass>  
                            </transformer>  
                        </transformers>  
                    </configuration>  
                </execution>  
            </executions>  
        </plugin>  
  
    </plugins>  
</build> 
```
### 如果使用了Spring，Spring Framework的多个jar包中包含相同的文件spring.handlers和spring.schemas，如果生成一个jar包会互相覆盖。为了避免互相影响，可以使用AppendingTransformer来对文件内容追加合并 ###
``` xml
<build>  
    <plugins>  
  
        <plugin>  
            <groupId>org.apache.maven.plugins</groupId>  
            <artifactId>maven-shade-plugin</artifactId>  
            <version>2.4.1</version>  
            <executions>  
                <execution>  
                    <phase>package</phase>  
                    <goals>  
                        <goal>shade</goal>  
                    </goals>  
                    <configuration>  
                        <transformers>  
                            <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">  
                                <mainClass>com.xxg.Main</mainClass>  
                            </transformer>  
                            <transformer implementation="org.apache.maven.plugins.shade.resource.AppendingTransformer">  
                                <resource>META-INF/spring.handlers</resource>  
                            </transformer>  
                            <transformer implementation="org.apache.maven.plugins.shade.resource.AppendingTransformer">  
                                <resource>META-INF/spring.schemas</resource>  
                            </transformer>  
                        </transformers>  
                    </configuration>  
                </execution>  
            </executions>  
        </plugin>  
  
    </plugins>  
</build>  
```
### 引用 ###
> http://blog.csdn.net/daiyutage/article/details/53739452

---
layout:     post
title: maven发布war包插件wagon-maven-plugin
date: '2017-10-20'
description:
categories:
    - java
tags:
    - maven插件
    - wagon-maven-plugin

---

# 使用maven插件将本地war包发布到其它环境
## 使用步骤
- pom.xml文件

```xml
<build>
    <finalName>pay</finalName>
    <extensions>
        <extension>
            <groupId>org.apache.maven.wagon</groupId>
            <artifactId>wagon-ssh</artifactId>
            <version>3.0.0</version>
        </extension>
    </extensions>
    <plugins>
        <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>wagon-maven-plugin</artifactId>
            <version>1.0</version>
            <configuration>
                <url>scp://root:123456@192.168.1.174/root/service_${project.build.finalName}/webapps</url>
                <fromFile>target/${project.build.finalName}.war</fromFile>
                <toFile>pay.war</toFile>
                <commands>
                    <command>...</command>
                </commands>
            </configuration>
        </plugin>
    </plugins>
</build>
```

- 发布命名

```bash
mvn clean install wagon:upload-single
```
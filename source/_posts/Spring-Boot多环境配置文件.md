---
title: Spring Boot多环境配置文件
tags: [Java, SpringBoot, Maven]
categories:
  - 学习笔记
date: 2023-04-28 17:17:54
image:
---





1.在resource目录下新建profiles目录

2.在profiles目录下新建wfli  macli  test  prod四个目录，分别存放四个环境下的配置文件

3.在pom.xml文件添加profiles节点配置

> 如果是多module项目，在需要打包的那个项目的pom里写而不是在父pom写，如果是单体项目，就直接在pom里写

```xml
    <profiles>
        <profile>
            <id>macli</id>
            <properties>
                <package.environment>macli</package.environment>
            </properties>
        </profile>
        <profile>
            <id>wfli</id>
            <properties>
                <package.environment>wfli</package.environment>
            </properties>
        </profile>
        <profile>
            <id>test</id>
            <properties>
                <package.environment>test</package.environment>
            </properties>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
        </profile>
        <profile>
            <id>prod</id>
            <properties>
                <package.environment>prod</package.environment>
            </properties>
        </profile>
    </profiles>
```

这一步添加完成后，在Jetbrains IDEA窗口右侧边上的Maven插件里，就可以看到profiles里面已经多出了几个环境的选项了，并且activation激活的环境上打了勾

4.在pom.xml文件添加build节点配置

> 如果是多module项目，在需要打包的那个项目的pom里写而不是在父pom写，如果是单体项目，就直接在pom里写

```xml
    <build>
        <resources>
            <resource>
                <directory>${basedir}/src/main/resources/profiles/${package.environment}</directory><!--动态配置当前生效配置文件的目录-->
                <filtering>true</filtering><!--这个是过滤什么的呢？-->
            </resource>
            <resource><!--其实这个resource注释掉也没啥影响，不知道为什么写，也不知道为什么不写-->
                <directory>${basedir}/src/main/resources</directory>
                <filtering>true</filtering>
                <excludes><!--配置打包时不要将源文件放到包内-->
                    <exclude>profiles/macli</exclude>
                    <exclude>profiles/wfli</exclude>
                    <exclude>profiles/test</exclude>
                    <exclude>profiles/prod</exclude>
                </excludes>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
            </resource>
            <resource>
                <directory>${basedir}/src/main/java</directory>
                <filtering>true</filtering>
                <includes>
                    <include>**/*.**</include>
                </includes>
            </resource>
        </resources>
        <plugins>
            <!--maven打包插件-->
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <!--<version>2.18.1</version>-->
                <configuration>
                    <skipTests>true</skipTests>
                </configuration>
            </plugin>
        </plugins>
    </build>
```

5.在每个application.properties中添加

```properties
spring.profiles.active=@package.environment@
```

6.启动成功

> 启动时，可以通过maven插件上的profiles属性来选择生效的环境配置

7.打包成功

> 打包时，可以通过maven插件上的profiles属性来选择生效的环境配置
>
> 通过maven命令打包时，可以通过-P参数来指定生效的环境配置








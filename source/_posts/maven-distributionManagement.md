---
title: maven distributionManagement
date: 2018-02-04 18:05:58
tags:
---
maven的pom文件中可以添加distributionManagement标签，用于指定上传RELEASE和SNATSHOT版的jar包的maven私服
地址，例如
```
<distributionManagement>
    <repository>
        <id>nexus-releases</id>
        <name>Nexus Release Repository</name>
        <url>http://xx.xx.xx.xx:8081/repository/maven-releases</url>
    </repository>
    <snapshotRepository>
        <id>nexus-snapshots</id>
        <name>Nexus Snapshot Repository</name>
        <url>http://xx.xx.xx.xx:8081/repository/maven-snapshots</url>
    </snapshotRepository>
</distributionManagement>
```
当然，发布jar是需要认证的，需要在maven的setting.xml文件中添加认证信息
```
<servers>
    <server>
        <id>nexus-releases</id>
        <username>admin</username>
        <password>admin123</password>
    </server>
    <server>
        <id>nexus-snapshots</id>
        <username>admin</username>
        <password>admin123</password>
    </server>
</servers>
```
其中id要与pom文件中的id对应。
此后，如果pom中的version形如`<version>1.0-RELEASE</version>`，会自动发布到maven私服的RELEASE库；
如果形如`<version>1.0-SNAPSHOT</version>`，则会发布到SNAPSHOT库
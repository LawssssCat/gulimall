# 环境搭建
## JDK
1.8+
![20200411191643](https://raw.githubusercontent.com/LawssssCat/piggo-vscode/master/images/20200411191643.png)
## Maven
3.6.3
![20200411200633](https://raw.githubusercontent.com/LawssssCat/piggo-vscode/master/images/20200411200633.png)
配置阿里云镜像
```xml
<mirror>
  <id>alimaven</id>
  <name>aliyun maven</name>
  <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
  <mirrorOf>central</mirrorOf>
</mirror>
```
配置jdk1.8编译项目
```xml
<profile>
  <id>jdk-1.8</id>
  <activation>
    <activeByDefault>true</activeByDefault>
    <jdk>1.8</jdk>
  </activation>
  <properties>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
  </properties>
</profile>
```
## IDEA

配置 Maven

安装插件：
+ lombok
+ MyBatisX

## VS Code
安装

插件
+ Auto Close Tag：标签自闭
+ Auto Rename Tag：方便标签改名
+ Chinese：简体中文包
+ ESLint：语法检查
+ HTML CSS Support：语法支持
+ HTML Snippets：语法提示
+ JavaScript(ES6) code snippets：语法提示
+ Live Server：一键开启前端服务器，简化调试
+ open in browser：浏览器（快捷）打开页面
+ Vetur：语法提示

## git
安装

全局基本配置

```bash
git config --global user.name "替换成你的名字"
git config --global user.email "替换成你的邮箱"
```

配置 ssh 免密连接
生成 ssh 公钥

```bash
ssh-keygen -t rsa -C "替换成你的邮箱"
cat ~/.ssh/id_rsa.pub
```
![20200411202423](https://raw.githubusercontent.com/LawssssCat/piggo-vscode/master/images/20200411202423.png)

设置 ssh 公钥

![20200411202636](https://raw.githubusercontent.com/LawssssCat/piggo-vscode/master/images/20200411202636.png)

测试配置是否成功
```bash
ssh -T 替换成你的邮箱
```

## GitHub
创建项目容器


![20200411204231](https://raw.githubusercontent.com/LawssssCat/piggo-vscode/master/images/20200411204231.png)




# 项目搭建

**创建项目微服务**


商品服务、仓储服务、订单服务、优惠券服务、用户服务

共同：
1. web、openfeign
2. 每一个服务，包名 com.vshop.gullimall.xxx (product/order/ware/coupon/member)
3. 模块名：gulimall-xxx (product/order/ware/coupon/member)

## 子项目：product（商品）

![20200411204104](https://raw.githubusercontent.com/LawssssCat/piggo-vscode/master/images/20200411204104.png)

**导入Web微服务两个必要依赖**

+ Spring Web：web服务
+ OpenFeign：微服务间互相调用

![20200411204720](https://raw.githubusercontent.com/LawssssCat/piggo-vscode/master/images/20200411204720.png)

## 子项目：ware（仓储）


![20200411210823](https://raw.githubusercontent.com/LawssssCat/piggo-vscode/master/images/20200411210823.png)
## 子项目：order（订单）

![20200411211048](https://raw.githubusercontent.com/LawssssCat/piggo-vscode/master/images/20200411211048.png)

## 子项目：member（会员）

![20200411211155](https://raw.githubusercontent.com/LawssssCat/piggo-vscode/master/images/20200411211155.png)


## 子项目：coupon（优惠券）

![20200411211329](https://raw.githubusercontent.com/LawssssCat/piggo-vscode/master/images/20200411211329.png)


## 聚合项目
pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.vshop.guilimall</groupId>
    <artifactId>gulimall</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>gulimall</name>
    <description>谷粒商城-聚合服务</description>
    <packaging>pom</packaging>

    <modules>
        <module>gulimall-coupon</module>
        <module>gulimall-member</module>
        <module>gulimall-order</module>
        <module>gulimall-product</module>
        <module>gulimall-ware</module>
    </modules>

</project>
```
导入聚合 pom.xml

![20200411212944](https://raw.githubusercontent.com/LawssssCat/piggo-vscode/master/images/20200411212944.png)

添加忽略文件 .gitignore 

```bash
target/
pom.xml.tag
pom.xml.releaseBackup
pom.xml.versionsBackup
pom.xml.next
release.properties
dependency-reduced-pom.xml
buildNumber.properties
.mvn/timing.properties
# https://github.com/takari/maven-wrapper#usage-without-binary-jar
.mvn/wrapper/maven-wrapper.jar

# 添加

mvnw
mvnw.cmd
.mvn

.idea
.gitignore
```
> 进行步进行依赖版本统一，请看：[https://blog.csdn.net/LawssssCat/article/details/105080690](https://blog.csdn.net/LawssssCat/article/details/105080690)
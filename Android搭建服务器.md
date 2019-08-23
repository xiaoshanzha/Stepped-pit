## 搭建自己的服务器，Android端实现登陆注册
    本文将介绍如何搭建属于自己的服务器，以供android、web等开发的使用。并不会涉及到各种工具的安装与配置，但会记录所遇到的各种Bug,请开始一起踩坑！

---
### 所需工具：
    工欲善其事必先利其器,先准备好以下工具。
* IntelliJ IDEA：创建web项目 (社区版不可以创建)
* Android Studio : Android 项目开发
* MySql数据库 : 
* Tomcat : 一款java web 的应用服务器
* 开通阿里云服务器ECS
---
### 步骤：
1. web项目连接Mysql数据库
2. 利用IDEA发布web项目到Tomcat
3. android项目进行测试
4. 搭建好服务器运行环境，并上传项目到服务器
---
### Web项目：

![](https://upload-images.jianshu.io/upload_images/10460153-955f43a9a4b5dd4b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

[源码](https://github.com/xiaoshanzha/java-web)
### Android项目：

![](https://upload-images.jianshu.io/upload_images/10460153-ad69af5ff2ba70a6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

[源码](https://github.com/xiaoshanzha/Android--/tree/master/Register_Login)

---
### 踩到的坑:
1. Tomcat的jvm不能对程序进行编译：

![](https://upload-images.jianshu.io/upload_images/10460153-f7b37f3b2d842c33.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

像这样的，Tomcat运行时，依靠java虚拟机进行编译，文件是由高版本的jre编译的，而tomcat选择的jre版本过低，只用下载个高版本的，然后在Tomcat设置里面改下jvm就可以了。

当然，用低版本编译的文件，tomcat依赖高版本的或许也会不成功，我测试的时候，用版本10编译的，tomcat依赖11版本的，也是不可以的。

---

2. 不能进行远程桌面连接：

![](https://upload-images.jianshu.io/upload_images/10460153-9bae19c8b99a9247.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我的是win10家庭版，不支持远程桌面连接，在下几乎尝遍了csdn、百度上的方法，花了好几个小时，全部失败,最后还是乖乖借助远程桌面连接的工具吧，我用的TeamViewer,好用。

---

3. 本地一切正常，服务器连接不上数据库:

![](https://upload-images.jianshu.io/upload_images/10460153-9febf1182909e6d2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

又是好久好久，本地mysql版本5.5的，然后使用的mysql-connector-java 的jar包是5.1的，操作完美，开心，上传到服务器后运行，数据一直显示不出来，最后打印出异常信息，见上图，又是版本问题。

服务器中mysql是8.0的，下了个8.0的m-c-j jar包，改了里面的驱动连接和数据库连接语句。
以前连接驱动：“com.mysql.jdbc.Driver”
更换驱动：“com.mysql.cj.jdbc.Driver”

此时报错如下：

![](https://upload-images.jianshu.io/upload_images/10460153-48528f30ce1185ec.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

解决：
在连接字符串后面加上?serverTimezone=UTC ; (UTC是统一标准世界时间)
eg: jdbc:mysql://localhost:3306/test?serverTimezone=UTC  

此时连接正常但可能出现中文乱码， 继续在后面添加：useUnicode=true&characterEncoding=UTF-8；

完整示例：
jdbc:mysql://127.0.0.1:3306/test?useUnicode=true&characterEncoding=UTF-8&serverTimezone=UTC

---

4.MySql-8.0数据库使用正确密码却无法登录，不输密码却可以

解决：进去后设置密码：alter user 'root'@'localhost' identified with mysql_native_password by '密码';
8.0和之前密码机制有点区别

---
## 所以，工具版本很重要！！！



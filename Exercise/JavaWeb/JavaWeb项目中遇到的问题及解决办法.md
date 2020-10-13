# Java Web项目中遇到的问题

## 1、编写mvc框架时

create new Servlet时一定不要勾选create Java EE 6 annotated class

![newServlet注意](https://github.com/Vinci-Ma/KeepLearning/raw/master/Exercise/JavaWeb/picture/image-20200819010834970.png)

## 2、前后端交互时



## 3、MySQL数据库建表时

![mysql建表](https://github.com/Vinci-Ma/KeepLearning/raw/master/Exercise/JavaWeb/picture/image-20200819012257726.png)

出现问题原因：

- 创建表如果只有一个字段设为TIMESTAMP类型，默认情况下

  - 没有指明null属性，该字段就会自动加上NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP属性
  - 如果给该字段插入null值，会自动给该字段设置为CURRENT_TIMESTAMP（当前时间）值

- 创建表如果有一个以上的字段指定TIMESTAMP类型，如果没有指定null属性或者没有设置默认值，则第二个TIMESTAMP字段报Invalid错误。
  
  这是因为：
       第一个TIMESTAMP字段会自动被加上DEFAULT CURRENT_TIMESTAMP和ON UPDATE CURRENT_TIMESTAMP属性。
       之后的TIMESTAMP字段，会被自动加上DEFAULT ‘0000-00-00 00:00:00’属性。而5.7版本的sql_mode变量中含有NO_ZERO_DATE，表示'0000-00-00 00:00:00'格式非法，这与严格模式有关。

解决办法：修改全局变量explicit_defaults_for_timestamp【注：设定后重新打开MySQL生效】

mysql> set global explicit_defaults_for_timestamp = ON;

## 4、IDEA中server中文显示乱码

首先IDEA的编码是GBK

![idea设置](https://github.com/Vinci-Ma/KeepLearning/raw/master/Exercise/JavaWeb/picture/image-20200822175140106.png)

解决办法：

将tomcat中conf文件夹下的logging.properties中控制台的编码方式改为GBK

![tomcat设置](https://github.com/Vinci-Ma/KeepLearning/raw/master/Exercise/JavaWeb/picture/image-20200822175354583.png)

## 5、mysql执行出现异常

![mysql异常](https://github.com/Vinci-Ma/KeepLearning/raw/master/Exercise/JavaWeb/picture/image-20200826172622891.png)

解决办法：注意在编写MySQL语句时注意添加空格！

## 6、微信端 扫码二维码 api scanQRCode不会回调

手机：iOS系统

解决办法：![微信扫码](https://github.com/Vinci-Ma/KeepLearning/raw/master/Exercise/JavaWeb/picture/image-20200831002227541.png)

![微信扫码](https://github.com/Vinci-Ma/KeepLearning/raw/master/Exercise/JavaWeb/picture/image-20200831002252152.png)

[解决办法的网址](https://developers.weixin.qq.com/community/develop/doc/000e640b670ef09b8419030fa5b400?jumpto=reply&commentid=000426ef3e4d38061a2939d42518&parent_commentid=0006a0a80a08301b8c196149f5b8)


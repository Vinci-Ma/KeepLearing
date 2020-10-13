![大纲](https://github.com/Vinci-Ma/KeepLearning/row/master/Exercise/Java Web项目——快递e栈/picture/快递e栈.png)

- [项目过程](#项目过程)
  * [项目准备](#项目准备)
  * [一、管理员登录](#一、管理员登录)
  * [二、用户管理](#二、用户管理)
  * [三、快递管理管理](#三、快递管理管理)
  * [四、快递员管理](#四、快递员管理)
  * [五、控制台显示](#五、控制台显示)
- [项目成果展示](#项目成果展示)
  * [后台管理](#后台管理)
  * [微信端](#微信端)
  
# 项目过程
## 项目准备

1、框架相关

参考springMVC框架

Servlet -> 映射器 -> 调用方法 -> 将结果返回给用户

2、阿里云

[阿里云短信](https://dysms.console.aliyun.com/dysms.htm?spm=5176.8195934.1283918..67016a7dCZFiWb&accounttraceid=f33cefcaf0f246a9846d67b23a6e9b9ccpka#/overview)

- 短信签名名称
- 短信模板code
- AccessKey ID
- AccessKey Secret

3、[Sunny-Ngrok](http://www.ngrok.cc/user.html)

- 隧道id

- 域名

- 本机IP

- 映射服务器地址

4、微信端（公众号）

- AppID

- AppSecret

## 一、管理员登录

BaseAdminDao：定义eadmin表格的操作规范

AdminDaoMySql：实现BaseAdminDao中的方法，实现数据库操作

AdminService：把dao所具有的方法进行实现，以便于外部调用

web中login.html：编写ajax代码进行数据的传输

AdminController：具体的后端与前端数据的传输代码

将AdminController文件的地址添加到application.properties中

## 二、用户管理

### 1、编写流程

#### 创建mysql数据库表（Users）

```java
Users表
CREATE TABLE users(
 id int PRIMARY KEY auto_increment,
 uname VARCHAR(32),
 uphone VARCHAR(32),
 uidcard VARCHAR(32),
 ucode VARCHAR(32) UNIQUE,
 signuptime TIMESTAMP,
 logintime TIMESTAMP,
 status int
)ENGINE=INNODB DEFAULT CHARSET=utf8;
```

#### 编写Dao

BaseUsersDao：快递管理相关操作的接口

UsersDaoMysql：实现了BaseExpressDao中的方法，实现数据库操作

#### 编写service

UsersService：把dao所具有的方法进行实现，以便于外部调用

#### 编写Controller

UsersController：具体的后端与前端数据的传输代码

#### 前后端交互

在web代码中编写ajax等代码，实现数据在后端与前端之间传输

### 2、用户管理相关操作

#### 1 用户列表

- 分页查询的列表

#### 2 新增用户

- 输入内容，后台接收相应的参数，向数据库存储

#### 3 删除用户

- 输入用户手机号码查询到用户信息
- 浏览用户信息，最后可以点击删除，删除用户信息（使用逻辑删除）

#### 4 修改用户

- 输入用户手机号码查询到用户信息
- 浏览用户信息，可以对用户信息进行修改，点击确认按钮，完成用户信息的修改

## 三、快递管理管理

### 1、编写流程

#### 创建mysql数据库表（Express）

```java
Express表
CREATE TABLE Express(
	id int PRIMARY KEY auto_increment,
	number VARCHAR(64) UNIQUE,
	username VARCHAR(32),
	userphone VARCHAR(32),
	company VARCHAR(32),
	code VARCHAR(32) UNIQUE,
	intime TIMESTAMP,
	outtime TIMESTAMP,
	status int,
	sysPhone VARCHAR(32)
)ENGINE=INNODB DEFAULT CHARSET=utf8;
```

#### 编写Dao

BaseExpressDao：快递管理相关操作的接口

ExpressDaoMysql：实现了BaseExpressDao中的方法，实现数据库操作

#### 编写service

ExpressService：把dao所具有的方法进行实现，以便于外部调用

#### 编写Controller

ExpressController：具体的后端与前端数据的传输代码

#### 前后端交互

在web代码中编写ajax等代码，实现数据在后端与前端之间传输

### 2、快递管理相关操作

#### 1 快递列表

- 分页查询的列表

#### 2 新增快递

- 用户输入内容，后台接收相应的参数，向数据库存储

#### 3 删除快递

- 用户输入快递单号查询到快递信息
- 浏览快递信息，最后可以点击删除，删除快递

#### 4 修改快递

- 用户输入快递单号查询到快递信息
- 浏览快递信息，可以对快递信息进行修改，点击确认按钮，完成快递的修改

## 四、快递员管理

### 1、编写流程

#### 创建MySQL数据库表（Courier）

```java
Courier表
编号、快递员姓名、快递员电话、身份证、密码、注册时间、上次登陆时间
CREATE TABLE Courier(
	id int PRIMARY KEY auto_increment,
	couriername VARCHAR(32),
	courierphone VARCHAR(32),
	idcard VARCHAR(32),
	code VARCHAR(32) UNIQUE,
	signuptime TIMESTAMP,
	logintime TIMESTAMP
)ENGINE=INNODB DEFAULT CHARSET=utf8;
ALTER TABLE Courier AUTO_INCREMENT = 1000;    
```

#### 编写Dao

BaseCourierDao：快递管理相关操作的接口

CourierDaoMysql：实现了CourierExpressDao中的方法，实现数据库操作

#### 编写service

CourierService：把dao所具有的方法进行实现，以便于外部调用

#### 编写Controller

ExpressController：具体的后端与前端数据的传输代码

#### 前后端交互

在web代码中编写ajax等代码，实现数据在后端与前端之间传输

### 2、快递员管理相关操作

#### 1 快递员列表

- 分页查询的列表

#### 2 新增快递员

- 输入内容，后台接收相应的参数，向数据库存储

#### 3 删除快递员

- 输入快递员手机号码查询到快递员信息
- 浏览快递员信息，最后可以点击删除，删除快递员

#### 4 修改快递员

- 输入快递员手机号码查询到快递员信息
- 浏览快递员信息，可以对快递员信息进行修改，点击确认按钮，完成快递员信息的修改

## 五、控制台显示


# 项目成果展示
## 后台管理

### 1、管理员的登录和退出

![后台管理登录页面](https://github.com/Vinci-Ma/KeepLearning/blob/master/Exercise/Java%20Web%E9%A1%B9%E7%9B%AE%E2%80%94%E2%80%94%E5%BF%AB%E9%80%92e%E6%A0%88/picture/后台管理登录页面.png)

### 2、快递的增加、删除、修改、列表查看操作

![后台管理-快件管理相关](https://github.com/Vinci-Ma/KeepLearning/blob/master/Exercise/Java%20Web%E9%A1%B9%E7%9B%AE%E2%80%94%E2%80%94%E5%BF%AB%E9%80%92e%E6%A0%88/picture/后台管理-快件管理相关.png)

### 3、用户的增加、删除、修改、列表查看操作

![后台管理-用户管理相关](https://github.com/Vinci-Ma/KeepLearning/blob/master/Exercise/Java%20Web%E9%A1%B9%E7%9B%AE%E2%80%94%E2%80%94%E5%BF%AB%E9%80%92e%E6%A0%88/picture/后台管理-用户管理相关.png)

### 4、快递员的增加、删除、修改、列表查看操作

![后台管理-快递员管理相关](https://github.com/Vinci-Ma/KeepLearning/blob/master/Exercise/Java%20Web%E9%A1%B9%E7%9B%AE%E2%80%94%E2%80%94%E5%BF%AB%E9%80%92e%E6%A0%88/picture/后台管理-快递员管理相关.png)

### 5、控制台显示用户人数、快递员人数、快递人数、待取件快递数

![后台管理控制台](https://github.com/Vinci-Ma/KeepLearning/blob/master/Exercise/Java%20Web%E9%A1%B9%E7%9B%AE%E2%80%94%E2%80%94%E5%BF%AB%E9%80%92e%E6%A0%88/picture/后台管理控制台.png)

### 6、短信发送

![短信发送](https://github.com/Vinci-Ma/KeepLearning/blob/master/Exercise/Java%20Web%E9%A1%B9%E7%9B%AE%E2%80%94%E2%80%94%E5%BF%AB%E9%80%92e%E6%A0%88/picture/短信发送.jpg)

## 微信端

![微信端](https://github.com/Vinci-Ma/KeepLearning/blob/master/Exercise/Java%20Web%E9%A1%B9%E7%9B%AE%E2%80%94%E2%80%94%E5%BF%AB%E9%80%92e%E6%A0%88/picture/微信端.jpg)

### 1、用户/快递员登录（注册）

快递员页面（有快递助手）

![快递员界面](https://github.com/Vinci-Ma/KeepLearning/blob/master/Exercise/Java%20Web%E9%A1%B9%E7%9B%AE%E2%80%94%E2%80%94%E5%BF%AB%E9%80%92e%E6%A0%88/picture/快递员界面.png)

用户页面

![用户页面](https://github.com/Vinci-Ma/KeepLearning/blob/master/Exercise/Java%20Web%E9%A1%B9%E7%9B%AE%E2%80%94%E2%80%94%E5%BF%AB%E9%80%92e%E6%A0%88/picture/用户页面.png)

### 2、用户操作

取件列表显示快递信息（已取件/未取件）

![用户快递查看页面](https://github.com/Vinci-Ma/KeepLearning/blob/master/Exercise/Java%20Web%E9%A1%B9%E7%9B%AE%E2%80%94%E2%80%94%E5%BF%AB%E9%80%92e%E6%A0%88/picture/用户快递查看页面.png)

快递取件码展示

![二维码显示](https://github.com/Vinci-Ma/KeepLearning/blob/master/Exercise/Java%20Web%E9%A1%B9%E7%9B%AE%E2%80%94%E2%80%94%E5%BF%AB%E9%80%92e%E6%A0%88/picture/二维码显示.png)

### 3、快递员操作

取件页面（扫码取件、取件码取件）

![手机取件页面](https://github.com/Vinci-Ma/KeepLearning/blob/master/Exercise/Java%20Web%E9%A1%B9%E7%9B%AE%E2%80%94%E2%80%94%E5%BF%AB%E9%80%92e%E6%A0%88/picture/手机取件页面.jpg)


# Ajax技术与原理

## 1、Ajax简介

AJAX = Asynchronous JavaScript and XML（异步的 JavaScript 和 XML）。

AJAX 不是新的编程语言，而是一种使用现有标准的新方法。

AJAX 是与服务器交换数据并更新部分网页的艺术，在不重新加载整个页面的情况下。

## 2、Ajax所包含的技术

Ajax并非一种新的技术，而是几种原有技术的结合体。它由下列技术组合而成。

```java
1.使用CSS和XHTML来表示。
2.使用DOM模型来交互和动态显示。
3.使用XMLHttpRequest来和服务器进行异步通信。
4.使用javascript来绑定和调用。
```

AJAX 的核心是 XMLHttpRequest 对象

不同的浏览器创建 XMLHttpRequest 对象的方法是有差异的。

**IE 浏览器使用 ActiveXObject，而其他的浏览器使用名为 XMLHttpRequest 的 JavaScript 内建对象**

## 3、Ajax的工作原理

Ajax的工作原理相当于在用户和服务器之间加了—个**中间层(AJAX引擎)**，使用户操作与服务器响应**异步化**。

并不是所有的用户请求都提交给服务器。像—些数据验证和数据处理等都交给Ajax引擎自己来做,，只有确定需要从服务器读取新数据时再由Ajax引擎代为向服务器提交请求。

**两者区别：**

### 1、模型

1、传统的Web模型

![image-20200814163823140](C:\Users\jasmi\AppData\Roaming\Typora\typora-user-images\image-20200814163823140.png)

2、Ajax模型

![image-20200814163845461](C:\Users\jasmi\AppData\Roaming\Typora\typora-user-images\image-20200814163845461.png)

### 2、交互方式

1、浏览器的普通交互方式

![a2](C:\Users\jasmi\Desktop\C7 JavaWeb\C7S6 AJAX\资料\文档\img\a2.jpg)

2、浏览器的Ajax交互方式

![a3](C:\Users\jasmi\Desktop\C7 JavaWeb\C7S6 AJAX\资料\文档\img\a3.jpg)

![a4](C:\Users\jasmi\Desktop\C7 JavaWeb\C7S6 AJAX\资料\文档\img\a4.png)

## 4、XMLHttpRequest常用属性

### 1、onreadystatechange 属性（回调函数）

onreadystatechange 属性存有处理服务器响应的函数。 下面的代码定义一个空的函数，可同时对onreadystatechange 属性进行设置：

```java
xmlHttp.onreadystatechange = function() { //我们需要在这写一些代码}
```

### 2、readyState 属性

readyState 属性**存有服务器响应的状态信息**。每当 readyState 改变时，onreadystatechange 函数就会被执行。
readyState 属性可能的值：

### 3、responseText 属性

可以通过 responseText 属性来取回由服务器返回的数据。 在我们的代码中，我们将把时间文本框的值设置为等于responseText：

```java
xmlHttp.onreadystatechange = function() {
if (xmlHttp.readyState == 4) {
document.myForm.time.value = xmlHttp.responseText;
}
}
```

## 5、XMLHttpRequest方法

### 1、open() 方法

open() 有三个参数。第一个参数定义发送请求所使用的方法，第二个参数规定服务器端脚本的URL，第三个参数规定应当对请求进行异步地处理。

```java
xmlHttp.open("GET","test.php",true);
```

### 2、send() 方法

send() 方法将请求送往服务器。如果我们假设 HTML 文件和 PHP 文件位于相同的目录，那么代码是这样的：

```java
xmlHttp.send(null);
```

### 3、其他方法

![a6](C:\Users\jasmi\Desktop\C7 JavaWeb\C7S6 AJAX\资料\文档\img\a6.png)

# Ajax编程步骤（基于js实现ajax）

```java
1. 创建XMLHttpRequest对象。
2. 设置请求方式。
3. 调用回调函数。
4. 发送请求。
```

## 1、创建XMLHttpRequest对象

```java
var xmlHttp=new XMLHttpRequest();
如果是IE5或者IE6浏览器，则使用ActiveX对象，创建方法是：
var xmlHttp=new ActiveXObject("Microsoft.XMLHTTP");
一般我们手写AJAX的时候，首先要判断该浏览器是否支持XMLHttpRequest对象，如果支持则创建该对象，如果不支持则创建ActiveX对象。JS代码如下：
    //第一步：创建XMLHttpRequest对象
var xmlHttp;
if (window.XMLHttpRequest) {
    //非IE
    xmlHttp = new XMLHttpRequest();
} else if (window.ActiveXObject) {
    //IE
    xmlHttp = new ActiveXObject("Microsoft.XMLHTTP")
}
```



## 2、设置请求方式

```java
//第二步：设置和服务器端交互的相应参数，向路径http://localhost:8080/JsLearning3/getAjax准备发送数据
var url = "http://localhost:8080/JsLearning3/getAjax";
xmlHttp.open("POST", url, true);
```



## 3、调用回调函数

```java
//第三步：注册回调函数
xmlHttp.onreadystatechange = function() {
    //判断状态
    if (xmlHttp.readyState == 4) {
        //接收返回的内容
        if (xmlHttp.status == 200) {
            var obj = document.getElementById(id);
            obj.innerHTML = xmlHttp.responseText;
        } else {
        	alert("AJAX服务器返回错误！");
        }
    }
}
```

## 4、发送请求

```java
//第四步：设置发送请求的内容和发送报送。然后发送请求
var uname= document.getElementsByName("userName")[0].value;
var upass= document.getElementsByName("userPass")[0].value ;
var params = "userName=" + uname+ "&userPass=" +upass+ "&time=" + Math.random();
// 增加time随机参数，防止读取缓存
xmlHttp.setRequestHeader("Content-type", "application/x-www-form-urlencoded;charset=UTF-8");

// 向请求添加 HTTP 头，POST如果有数据一定加加！！！！
xmlHttp.send(params);
```

# jquery的ajax操作*

### 1、传统方式实现Ajax的不足

步骤繁琐，方法、属性、常用值较多不好记忆

### 2、ajax()方法

可以通过发送 HTTP请求加载远程数据，是 jQuery 最底层的 Ajax 实现，具有较高灵活性。

$.ajax([settings]);//参数settings是方法的参数列表，用于配置Ajax请求的键值对集合；

```java
$.ajax({
    url:请求地址
    type:"get | post | put | delete " 默认是get,
    data:请求参数 {"id":"123","pwd":"123456"},
    dataType:请求数据类型"html | text | json | xml | script | jsonp ",
    success:function(data,dataTextStatus,jqxhr){ },//请求成功时
error:function(jqxhr,textStatus,error)//请求失败时
})
```

### 3、get() 方法通过远程 HTTP GET 请求载入信息

这是一个简单的 GET 请求功能以取代复杂 $.ajax 

```java
$.get(url,data,function(result) {
//省略将服务器返回的数据显示到页面的代码
});
url:请求的路径
data:发送的数据
success:成功函数
datatype 返回的数据
```

### 4、post() 方法通过远程 HTTP GET 请求载入信息

```java
$.post(url,data,function(result) {
//省略将服务器返回的数据显示到页面的代码
});
url:请求的路径
data:发送的数据
success:成功函数
datatype 返回的数据
```

# JSON

## 1、JSON概念

JSON (JavaScript Object Notation) 是一种轻量级的数据交换格式。 易于人阅读和编写。同时也易于机器解析和生成。 

## 2、JSON对象定义和基本使用

### 1、定义

```java
var 变量名 = {
“key” : value , // Number类型
“key2” : “value” , // 字符串类型
“key3” : [] , // 数组类型
“key4” : {}, // json 对象类型
“key5” : [{},{}] // json 数组
};
```

### 2、JSON对象的访问

son对象，顾名思义，就知道它是一个对象。里面的key就是对象的属性。我们要访问一个对象的属性，只需要使用【对象名.属性名】的方式访问即可

```java
<script type="text/javascript">
// json的定义
var jsons = {
"key1":"abc", // 字符串类型
"key2":1234, // Number
"key3":[1234,"21341","53"], // 数组
"key4":{ // json类型
    "key4_1" : 12,
    "key4_2" : "kkk"
    },
"key5":[{ // json数组
    "key5_1_1" : 12,
    "key5_1_2" : "abc"
    },{
        "key5_2_1" : 41,
            "key5_2_2" : "bbj"
        }]
};
// 访问json的属性
alert(jsons.key1); // "abc"
// 访问json的数组属性
alert(jsons.key3[1]); // "21341"
// 访问json的json属性
alert(jsons.key4.key4_1);//12
// 访问json的json数组
alert(jsons.key5[0].key5_1_2);//"abc"
</script>
```

## 3、JSON在java中的使用*

导包：

![包目录](C:\Users\jasmi\Desktop\C7 JavaWeb\C7S6 AJAX\资料\文档\img\包目录.png)

### java对象和json之间的转换

#### 1、单个对象或map集合

```java
java->json：
    Users user2=new Users();
    user2.setUsername("李四");
    user2.setPassword("abc");
    user2.setAge(20);
    JSONObject obj=JSONObject.fromObject(user);//obj就是json格式的

json->java
    String str="{'username':'李四','password':'admin','age':19}";
    JSONObject json=JSONObject.fromObject(str);
    Users user=(Users)JSONObject.toBean(json,Users.class);
```

#### 2、对象集合和json的转换

```java
java集合->json数组:
    List list=new ArrayList();
    list.add("dd");
    list.add("aa");
    JSONArray obj=JSONArray.fromObject(list);//set也是这么转
json数组->java集合:
	Key1:
	String str2="[{'age':20,'password':'abc','username':'李四'},{'age':10,'password':'adb','username':'张三'}]";
	JSONArray json2=JSONArray.fromObject(str2);
	Object[] obj=(Object[])JSONArray.toArray(json2,Users.class);

	Key2:
	String str3="[{'age':20,'password':'abc','username':'李四'},{'age':10,'password':'adb','username':'展示干'}]";
	JSONArray json3=JSONArray.fromObject(str3);
	//默认转换成ArrayList
	List<Users> list=(List<Users>) 				JSONArray.toCollection(json3,Users.class);
```


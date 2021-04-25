# 游标（cursor）

游标，是一个存储在MySQL服务器上的数据库查询，不是一条SELECT语句，而是**被该语句检索出来的结果集**。在存储了游标之后，应用程序可以根据需要滚动或浏览其中的数据。

游标主要用于交互式应用，其中用户需要滚动屏幕上的数据，并对数据进行浏览或作出更改。（游标只能用于存储过程和函数）

**使用游标的过程：**

1、使用游标前，定义它，定义需要使用的SELECT

2、声明后，必须打开游标以供使用，这个过程用前面定义的SELECT语句把数据实际检索出来

3、对于填有数据的游标，根据需要取出（检索）各行

4、在结束游标使用时，必须关闭游标

# 触发器

对触发器的支持是在 MySQL5 中增加的，所以只支持5之后的版本

用于：需要在某个表发生更改时自动进行处理。

触发器时MySQL响应以下任意语句而自动执行的一条MySQL语句【DELETE、INSERT、UPDATE】

创建触发器：

1、唯一的触发器名

2、触发器关联的表

3、触发器应该响应的活动（DELETE、INSERT或UPDATE）

4、触发器何时执行（处理之前或处理之后）

```
EG：CREATE TRIGGER newproduct AFTER INSERT ON products FOR EACH ROW SELECT 'Product added';
```

创建了一个名为newproduct的新触发器，在INSERT语句成功执行后执行，定义了FOR EACH ROW，代码对每个插入行执行，此例中，Product added将对每个插入的行显示一次。

 

每个表每个事件每次只允许一个触发器，因此每个表最多支持6个触发器（增删更新 前、后）。单一触发器不能与多个事件或多个表关联。

 

删除触发器：

```
EG：DROP TRIGGER newproduct；
```

# 管理事务处理

MySQL最常用数据库引擎：

​	MyISAM：不支持事务处理管理

​	InnoDB：支持

事务处理（transaction processing）可以用来维护数据库的完整性，它保证成批的MySQL操作要么完全执行，要么完全不执行。利用事务处理，可以保证一组操作不会中途停止，如果没有错误发生，整句提交到数据库表，如果发生错误，则进行回退以回复数据库到某个已知且安全的状态。

事务（transaction）：指一组SQL语句

回退（rollback）：指撤销指定SQL语句的过程

提交（commit）：指将未存储的SQL语句结果写入数据库表

保留点（savepoint）：指事务处理中设置的临时占位符（placeholder），你可以对它进行发布回退（与回退整个事务处理不同）

**使用ROLLBACK**：

用来回退MySQL语句

```
EG：
START TRANSACTION;

DELETE …

ROLLBACK;
```

【ROLLBACK只能在一个事务处理内使用（在执行一条START TRANSACTION 命令后】

事务处理用来管理INSERT、UPDATE、DELETE语句，不能回退CREATE或者DROP操作

**使用COMMIT**：

一般语句都是直接针对数据库表执行和编写的，这时隐含提交，即提交操作是自动进行的。

在事务管理块中，提交不会隐含地进行，为进行明确提交，使用commit语句

```
EG: 
START TRANSACTION;

DELETE…

COMMIT;
```

当commit或rollback语句执行后，事务会自动关闭

**使用保留点**：

简单的rollback和commit就可以写入或撤销整个事务处理，复杂的事务处理可能需要部分提交或回退。

```
EG: 
SAVEPOINT DELETE1;

ROLLBACK TO DELETE1;
```

保留点在事务完成（执行一条rollback或commit）后自动释放，也可以用RELEASE SAVEPOINT明确地释放保留点

#  安全管理

访问控制：防止无意的错误

创建用户账号：

```
EG: CREATE USER ben IDENTIFIED BY 'P@$$W0RD'(还可以用GRANT、INSERT（不建议）)
```

删除用户账号： DROP USER bforta；

设置访问权限：

使用GRANT设置权限需要的信息：1、要授予的权限；2、被授予访问权限的数据库或表、用户名

```
EG: GRANT SELECT ON crashcourse.* TO bforta;
```

允许用户在crashcourse.*（crashcourse数据库的所有表）上使用SELETE。

撤销权限：

```
EG：REVOKE SELETE ON crashcourse.* FROM bforta;
```

撤销刚赋予用户bforta的SELETE访问权限

GRANT和REVOKE能够在及各层次上控制访问权限：

1、整个服务器，使用 .. ALL

2、整个数据库，使用 ON database.*

3、特定的表，使用 ON database.table

4、特定的列

5、特定的存储过程

**更改口令**：

```
EG: SET PASSWORD FOR bforta = Password（'n3w'）；
```

新口令必须传递到Password（）函数进行加密
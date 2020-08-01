- [SQL练习题](#sqlSQL练习题)
  * [数据表介绍](#数据表介绍)
  * [数据SQL](#数据SQL)
  * [练习题](#练习题)
    + [1、查询“01”课程比“02”课程成绩高的学生的信息及课程分数](#1、查询“01”课程比“02”课程成绩高的学生的信息及课程分数)
    + [11、查询至少有一门课与学号为“01”的同学所学相同的同学的信息](#11、查询至少有一门课与学号为“01”的同学所学相同的同学的信息)
    + [21、查询学生的总成绩，并进行排名，总分重复时不保留名次空缺](#21、查询学生的总成绩，并进行排名，总分重复时不保留名次空缺)
    + [31、查询平均成绩大于等于85的所有学生的学号、姓名、平均成绩](#31、查询平均成绩大于等于85的所有学生的学号、姓名、平均成绩)
    + [41、查询每门课程成绩最好的前两名](#41、查询每门课程成绩最好的前两名)
    + [50、查询下月过生日的学生](#50、查询下月过生日的学生)
   + [50、查询下月过生日的学生](#50-----------)

# SQL练习题

## 数据表介绍

```sql
1.学生表
Student(SId,Sname,Sage,Ssex)
SId 学生编号,Sname 学生姓名,Sage 出生年月,Ssex 学生性别
2.课程表
Course(CId,Cname,TId)
CId 课程编号,Cname 课程名称,TId 教师编号
3.教师表
Teacher(TId,Tname)
TId 教师编号,Tname 教师姓名
4.成绩表
SC(SId,CId,score)
SId 学生编号,CId 课程编号,score 分数
```

## 数据SQL

```sql
1、学生表 student
create table Student(SId varchar(10),Sname varchar(10),Sage datetime,Ssex
varchar(10))engine=innodb default charset=utf8;
相关数据：
01	李雷	1990-01-01 00:00:00	男
02	钱电	1990-12-21 00:00:00	男
03	孙风	1990-12-20 00:00:00	男
04	李云	1990-12-06 00:00:00	男
05	周梅	1991-12-01 00:00:00	女
06	吴兰	1992-01-01 00:00:00	女
07	郑竹	1989-01-01 00:00:00	女
09	张三	2017-12-20 00:00:00	女
10	李四	2017-12-25 00:00:00	女
11	李四	2012-06-06 00:00:00	女
12	赵六	2013-06-13 00:00:00	女
13	孙七	2014-06-01 00:00:00	女
2、科目表 course
create table Course(CId varchar(10),Cname nvarchar(10),TId varchar(10))engine=innodb default charset=utf8;
相关数据：
01	语文	02
02	数学	01
03	英语	03
3、教师表 teacher
create table Teacher(TId varchar(10),Tname varchar(10))engine=innodb default charset=utf8;
01	张三
02	李四
03	王五
4、成绩表 sc
create table SC(SId varchar(10),CId varchar(10),score decimal(18,1))engine=innodb default charset=utf8;
01	01	80
01	02	90
01	03	99
02	01	70
02	02	60
02	03	80
03	01	80
03	02	80
03	03	80
04	01	50
04	02	30
04	03	20
05	01	76
05	02	87
06	01	31
06	03	34
07	02	89
07	03	98
```

## 练习题

### 1、查询“01”课程比“02”课程成绩高的学生的信息及课程分数

```sql
SELECT stu.sid,stu.sname,s.cid1,s.score1,s.cid2,s.score2
FROM student as stu
JOIN(
		SELECT s1.sid,s1.cid as cid1,s1.score as score1,s2.cid as cid2,s2.score as score2 FROM
		(SELECT sid,score,cid FROM sc WHERE cid ='01') as s1
		JOIN
		(SELECT sid,score,cid FROM sc WHERE cid = '02') as s2
		on s1.sid=s2.sid
		WHERE s1.score >s2.score
) as s
on stu.sid = s.sid;
结果：
+------+-------+------+--------+------+--------+
| sid  | sname | cid1 | score1 | cid2 | score2 |
+------+-------+------+--------+------+--------+
| 02   | 钱电  | 01   |   70.0 | 02   |   60.0 |
| 04   | 李云  | 01   |   50.0 | 02   |   30.0 |
+------+-------+------+--------+------+--------+
```

### 2、查询同时存在“01”课程和“02”课程的情况

```sql
SELECT s1.SId,s1.CId as cid1,s1.score as score1,s2.CId as cid2,s2.score as score2
FROM
	(SELECT sid,score,cid FROM sc WHERE cid ='01') as s1
	JOIN
	(SELECT sid,score,cid FROM sc WHERE cid ='02') as s2
	on s1.sid=s2.sid;
结果：
+------+------+--------+------+--------+
| sid  | cid1 | score1 | cid2 | score2 |
+------+------+--------+------+--------+
| 01   | 01   |   80.0 | 02   |   90.0 |
| 02   | 01   |   70.0 | 02   |   60.0 |
| 03   | 01   |   80.0 | 02   |   80.0 |
| 04   | 01   |   50.0 | 02   |   30.0 |
| 05   | 01   |   76.0 | 02   |   87.0 |
+------+------+--------+------+--------+
```

### 3、查询存在“01”课程但可能不存在“02”课程的情况（不存在时显示为null）

```sql
SELECT s1.SId,s1.CId as cid1,s1.score as score1,s2.CId as cid2,s2.score as score2
FROM
	(SELECT sid,score,cid FROM sc WHERE cid ='01') as s1
	LEFT JOIN
	(SELECT sid,score,cid FROM sc WHERE cid ='02') as s2
	on s1.sid=s2.sid;
结果：
+------+------+--------+------+--------+
| sid  | cid1 | score1 | cid2 | score2 |
+------+------+--------+------+--------+
| 01   | 01   |   80.0 | 02   |   90.0 |
| 02   | 01   |   70.0 | 02   |   60.0 |
| 03   | 01   |   80.0 | 02   |   80.0 |
| 04   | 01   |   50.0 | 02   |   30.0 |
| 05   | 01   |   76.0 | 02   |   87.0 |
| 06   | 01   |   31.0 | NULL |   NULL |
+------+------+--------+------+--------+
```

### 4、查询不存在“01”课程但存在“02”课程的情况

```sql
SELECT * FROM sc
WHERE sId NOT in (SELECT sid FROM sc WHERE cid ='01')
AND CId='02';
结果：
+------+------+-------+
| SId  | CId  | score |
+------+------+-------+
| 07   | 02   |  89.0 |
+------+------+-------+
```

### 5、查询平均成绩大于等于60分的同学的学生编号和学生姓名以及平均成绩

```sql
SELECT sc.SId,Sname,ROUND(AVG(score),2) as avg_score
FROM sc,student
WHERE sc.SId = student.SId
GROUP BY sc.SId,Sname
HAVING avg_score >= 60;
结果：
+------+-------+-----------+
| SId  | Sname | avg_score |
+------+-------+-----------+
| 01   | 李雷  |     89.67 |
| 02   | 钱电  |     70.00 |
| 03   | 孙风  |     80.00 |
| 05   | 周梅  |     81.50 |
| 07   | 郑竹  |     93.50 |
+------+-------+-----------+
```

### 6、查询在sc表存在成绩的学生信息

```sql
SELECT DISTINCT stu.*
FROM student as stu
JOIN sc
on sc.SId = stu.SId;
结果：
+------+-------+---------------------+------+
| SId  | Sname | Sage                | Ssex |
+------+-------+---------------------+------+
| 01   | 李雷  | 1990-01-01 00:00:00 | 男   |
| 02   | 钱电  | 1990-12-21 00:00:00 | 男   |
| 03   | 孙风  | 1990-12-20 00:00:00 | 男   |
| 04   | 李云  | 1990-12-06 00:00:00 | 男   |
| 05   | 周梅  | 1991-12-01 00:00:00 | 女   |
| 06   | 吴兰  | 1992-01-01 00:00:00 | 女   |
| 07   | 郑竹  | 1989-01-01 00:00:00 | 女   |
+------+-------+---------------------+------+
```

### 7、查询所有同学的学生编号、学生姓名、选课总数、所有课程的总成绩（没成绩的显示为null）

```sql
SELECT stu.SId,stu.Sname,COUNT(sc.CId) as num,SUM(sc.score) as total_score
FROM student as stu
LEFT JOIN sc 
on stu.SId = sc.SId
GROUP BY stu.SId,stu.Sname;
结果：
+------+-------+-----+-------------+
| SId  | Sname | num | total_score |
+------+-------+-----+-------------+
| 01   | 李雷  |   3 |       269.0 |
| 02   | 钱电  |   3 |       210.0 |
| 03   | 孙风  |   3 |       240.0 |
| 04   | 李云  |   3 |       100.0 |
| 05   | 周梅  |   2 |       163.0 |
| 06   | 吴兰  |   2 |        65.0 |
| 07   | 郑竹  |   2 |       187.0 |
| 09   | 张三  |   0 |        NULL |
| 10   | 李四  |   0 |        NULL |
| 11   | 李四  |   0 |        NULL |
| 12   | 赵六  |   0 |        NULL |
| 13   | 孙七  |   0 |        NULL |
+------+-------+-----+-------------+
```

### 8、查询【李】姓老师的数量

```sql
SELECT Tname,COUNT(teacher.TId)
FROM teacher
WHERE Tname LIKE '李%'
GROUP BY Tname;
结果：
+-------+--------------------+
| Tname | COUNT(teacher.TId) |
+-------+--------------------+
| 李四  |                  1 |
+-------+--------------------+
```

### 9、查询学过【张三】老师授课的学生信息

```sql
SELECT s2.*,teacher.Tname
FROM teacher 
JOIN (
	SELECT student.*,s1.TId
	FROM student
	JOIN(SELECT sc.SId,sc.CId,course.TId
			FROM sc
			JOIN course
			on sc.CId=course.CId
			) as s1
	on s1.SId=student.SId
) as s2
on s2.TId=teacher.TId
WHERE teacher.Tname='张三';
结果：（张三所教学科为数学02，验证正确）
+------+-------+---------------------+------+------+-------+
| SId  | Sname | Sage                | Ssex | TId  | Tname |
+------+-------+---------------------+------+------+-------+
| 01   | 李雷  | 1990-01-01 00:00:00 | 男   | 01   | 张三  |
| 02   | 钱电  | 1990-12-21 00:00:00 | 男   | 01   | 张三  |
| 03   | 孙风  | 1990-12-20 00:00:00 | 男   | 01   | 张三  |
| 04   | 李云  | 1990-12-06 00:00:00 | 男   | 01   | 张三  |
| 05   | 周梅  | 1991-12-01 00:00:00 | 女   | 01   | 张三  |
| 07   | 郑竹  | 1989-01-01 00:00:00 | 女   | 01   | 张三  |
+------+-------+---------------------+------+------+-------+
```

### 10、查询没有学全所有课程的同学的信息

```sql
SELECT student.*
FROM student
WHERE SId not in (
	SELECT s3.sid
	FROM (SELECT sid,score,cid FROM sc WHERE cid ='03') as s3
	JOIN
			(SELECT s1.sid
				FROM
					(SELECT sid,score,cid FROM sc WHERE cid ='01') as s1
					JOIN
					(SELECT sid,score,cid FROM sc WHERE cid ='02') as s2
					on s1.sid=s2.sid
			)as s4
	on s3.SId=s4.SId	
);
结果：（先找出学完全部课程的学生，再检索没有在此集合内的学生，只有1、2、3、4号学生学完了全部课程）
+------+-------+---------------------+------+
| SId  | Sname | Sage                | Ssex |
+------+-------+---------------------+------+
| 05   | 周梅  | 1991-12-01 00:00:00 | 女   |
| 06   | 吴兰  | 1992-01-01 00:00:00 | 女   |
| 07   | 郑竹  | 1989-01-01 00:00:00 | 女   |
| 09   | 张三  | 2017-12-20 00:00:00 | 女   |
| 10   | 李四  | 2017-12-25 00:00:00 | 女   |
| 11   | 李四  | 2012-06-06 00:00:00 | 女   |
| 12   | 赵六  | 2013-06-13 00:00:00 | 女   |
| 13   | 孙七  | 2014-06-01 00:00:00 | 女   |
+------+-------+---------------------+------+
```

### 11、查询至少有一门课与学号为“01”的同学所学相同的同学的信息

```sql
SELECT DISTINCT stu.*
FROM student as stu
JOIN sc on sc.sid = stu.sid
WHERE sc.CId in (SELECT CId FROM sc WHERE sid='01');
结果：
+------+-------+---------------------+------+
| SId  | Sname | Sage                | Ssex |
+------+-------+---------------------+------+
| 01   | 李雷  | 1990-01-01 00:00:00 | 男   |
| 02   | 钱电  | 1990-12-21 00:00:00 | 男   |
| 03   | 孙风  | 1990-12-20 00:00:00 | 男   |
| 04   | 李云  | 1990-12-06 00:00:00 | 男   |
| 05   | 周梅  | 1991-12-01 00:00:00 | 女   |
| 06   | 吴兰  | 1992-01-01 00:00:00 | 女   |
| 07   | 郑竹  | 1989-01-01 00:00:00 | 女   |
+------+-------+---------------------+------+
```

### 12、查询和“01”的同学学习的课程完全相同的其他同学的信息

```sql
SELECT s2.SId,student.Sname
FROM sc as s1
JOIN sc as s2 
on s1.CId=s2.CId AND s1.SId = '01' AND s2.SId !='01'
JOIN student on s2.SId=student.SId
GROUP BY s2.SId,student.Sname
HAVING COUNT(s1.SId) = (SELECT COUNT(*) FROM sc WHERE SId='01');
结果：
+------+-------+
| SId  | Sname |
+------+-------+
| 02   | 钱电  |
| 03   | 孙风  |
| 04   | 李云  |
+------+-------+
```

### 13、查询没学过“张三”老师讲授的任意一门课程的学生姓名

```sql
SELECT student.*
FROM student
WHERE student.SId not in(
	SELECT s2.SId
	FROM teacher 
	JOIN (
		SELECT student.*,s1.TId
		FROM student
		JOIN(SELECT sc.SId,sc.CId,course.TId
				FROM sc
				JOIN course
				on sc.CId=course.CId
				) as s1
		on s1.SId=student.SId
	) as s2
	on s2.TId=teacher.TId
	WHERE teacher.Tname='张三'
);
结果：
+------+-------+---------------------+------+
| SId  | Sname | Sage                | Ssex |
+------+-------+---------------------+------+
| 06   | 吴兰  | 1992-01-01 00:00:00 | 女   |
| 09   | 张三  | 2017-12-20 00:00:00 | 女   |
| 10   | 李四  | 2017-12-25 00:00:00 | 女   |
| 11   | 李四  | 2012-06-06 00:00:00 | 女   |
| 12   | 赵六  | 2013-06-13 00:00:00 | 女   |
| 13   | 孙七  | 2014-06-01 00:00:00 | 女   |
+------+-------+---------------------+------+
```

### 14、查询两门及以上不及格课程的同学的学号、姓名及平均成绩

```sql
SELECT stu.SId,stu.Sname,ROUND(AVG(sc.score),2) as avg_score
FROM student as stu
JOIN sc on stu.SId = sc.SId
WHERE sc.score<60
GROUP BY stu.SId,stu.Sname
HAVING COUNT(sc.cid)>=2;
结果：
+------+-------+-----------+
| SId  | Sname | avg_score |
+------+-------+-----------+
| 04   | 李云  |     33.33 |
| 06   | 吴兰  |     32.50 |
+------+-------+-----------+
```

### 15、检索“01”课程分数小于60，按分数降序排列的学生信息

```sql
SELECT stu.*,sc.CId,sc.score
FROM student as stu
JOIN sc on sc.SId=stu.SId
WHERE sc.CId='01' AND sc.score<60
ORDER BY sc.score DESC;
结果：
+------+-------+---------------------+------+------+-------+
| SId  | Sname | Sage                | Ssex | CId  | score |
+------+-------+---------------------+------+------+-------+
| 04   | 李云  | 1990-12-06 00:00:00 | 男   | 01   |  50.0 |
| 06   | 吴兰  | 1992-01-01 00:00:00 | 女   | 01   |  31.0 |
+------+-------+---------------------+------+------+-------+
```

### 16、按平均成绩从高到低显示所有学生的所有课程的成绩及平均成绩

```sql

select sc.*,s2.avg_score
from sc
join (select sid,avg(score) as avg_score from sc group by sid) as s2
on sc.sid = s2.sid
order by s2.avg_score desc,sc.sid;
结果：
+------+------+-------+-----------+
| SId  | CId  | score | avg_score |
+------+------+-------+-----------+
| 07   | 03   |  98.0 |  93.50000 |
| 07   | 02   |  89.0 |  93.50000 |
| 01   | 01   |  80.0 |  89.66667 |
| 01   | 02   |  90.0 |  89.66667 |
| 01   | 03   |  99.0 |  89.66667 |
| 05   | 01   |  76.0 |  81.50000 |
| 05   | 02   |  87.0 |  81.50000 |
| 03   | 03   |  80.0 |  80.00000 |
| 03   | 01   |  80.0 |  80.00000 |
| 03   | 02   |  80.0 |  80.00000 |
| 02   | 03   |  80.0 |  70.00000 |
| 02   | 01   |  70.0 |  70.00000 |
| 02   | 02   |  60.0 |  70.00000 |
| 04   | 02   |  30.0 |  33.33333 |
| 04   | 03   |  20.0 |  33.33333 |
| 04   | 01   |  50.0 |  33.33333 |
| 06   | 03   |  34.0 |  32.50000 |
| 06   | 01   |  31.0 |  32.50000 |
+------+------+-------+-----------+

select
stu.sname,
a.score as '语文',
b.score as '数学',
c.score as '英语',
avg(d.score) as '平均成绩'
from student as stu
left join sc as a on stu.sid = a.sid and a.cid = '01'
left join sc as b on stu.sid = b.sid and b.cid = '02'
left join sc as c on stu.sid = c.sid and c.cid = '03'
left join sc as d on stu.sid = d.sid
group by stu.sname,语文,数学,英语
order by 平均成绩 desc;
结果：
+-------+------+------+------+----------+
| sname | 语文 | 数学 | 英语 | 平均成绩 |
+-------+------+------+------+----------+
| 郑竹  | NULL | 89.0 | 98.0 | 93.50000 |
| 李雷  | 80.0 | 90.0 | 99.0 | 89.66667 |
| 周梅  | 76.0 | 87.0 | NULL | 81.50000 |
| 孙风  | 80.0 | 80.0 | 80.0 | 80.00000 |
| 钱电  | 70.0 | 60.0 | 80.0 | 70.00000 |
| 李云  | 50.0 | 30.0 | 20.0 | 33.33333 |
| 吴兰  | 31.0 | NULL | 34.0 | 32.50000 |
| 李四  | NULL | NULL | NULL |     NULL |
| 赵六  | NULL | NULL | NULL |     NULL |
| 张三  | NULL | NULL | NULL |     NULL |
| 孙七  | NULL | NULL | NULL |     NULL |
+-------+------+------+------+----------+
```

### 17、查询各科成绩最高分、最低分和平均分：以如下形式显示：课程ID，课程name，最高分，最低分，平均分，及格率，中等率，优良率，优秀率（）及格为>=60,中等为：70-80，优良为：80-90，优秀为>=90），要求输出课程号和选修人数，查询结果按人数降序排列，若人数相同，按课程号升序排列

```sql
select
sc.cid,
c.cname,
max(sc.score) as '最高分',
min(sc.score) as '最低分',
round(avg(sc.score),2) as '平均分',
count(sc.cid) as '选修人数',
sum(CASE WHEN sc.score >= 60 THEN 1 ELSE 0 END) / count(sc.cid) as '及格率',
sum(CASE WHEN sc.score >= 70 and sc.score < 80 THEN 1 ELSE 0 END) / count(sc.cid) as '中等率',
sum(CASE WHEN sc.score >= 80 and sc.score < 90 THEN 1 ELSE 0 END) / count(sc.cid) as '优良率',
sum(CASE WHEN sc.score >= 90 THEN 1 ELSE 0 END) / count(sc.cid) as '优秀率'
from sc join course as c on sc.cid = c.cid
group by sc.cid,c.cname
order by '选修人数' desc,sc.cid;
结果：
+------+-------+--------+--------+--------+----------+--------+--------+--------+--------+
| cid  | cname | 最高分 | 最低分 | 平均分 | 选修人数 | 及格率 | 中等率 | 优良率 | 优秀率 |
+------+-------+--------+--------+--------+----------+--------+--------+--------+--------+
| 01   | 语文  |   80.0 |   31.0 |  64.50 |        6 | 0.6667 | 0.3333 | 0.3333 | 0.0000 |
| 02   | 数学  |   90.0 |   30.0 |  72.67 |        6 | 0.8333 | 0.0000 | 0.5000 | 0.1667 |
| 03   | 英语  |   99.0 |   20.0 |  68.50 |        6 | 0.6667 | 0.0000 | 0.3333 | 0.3333 |
+------+-------+--------+--------+--------+----------+--------+--------+--------+--------+
```

### 18、按各科平均成绩进行排序，并显示排名，Score重复时保留名次空缺

```sql
select s2.cid,s2.avg_sc,count(distinct s1.avg_sc) as rank
from
(select cid,round(avg(score),2) as avg_sc from sc group by cid) as s1
join
(select cid,round(avg(score),2) as avg_sc from sc group by cid) as s2
on s1.avg_sc >= s2.avg_sc
group by s2.cid,s2.avg_sc
order by rank;
结果：
+------+--------+------+
| cid  | avg_sc | rank |
+------+--------+------+
| 02   |  72.67 |    1 |
| 03   |  68.50 |    2 |
| 01   |  64.50 |    3 |
+------+--------+------+
```

### 19、按各科平均成绩进行排序，并显示排名，Score重复时不保留名次空缺

```sql
select b.cid,b.avg_sc,@i := @i+1 as rank
from (select @i := 0) as a,
(select cid,round(avg(score),2) as avg_sc from sc group by cid order by avg_sc desc) as b;
结果：
+------+--------+------+
| cid  | avg_sc | rank |
+------+--------+------+
| 02   |  72.67 |    1 |
| 03   |  68.50 |    2 |
| 01   |  64.50 |    3 |
+------+--------+------+
（出现同样数据时不并列排序）
```

### 20、查询学生的总成绩，并进行排名，总分重复时保留名次空缺

```sql
select s2.SId,s2.sum,count(distinct s1.sum) as rank
from
(select SId,SUM(sc.score) as sum from sc group by SId) as s1
join
(select SId,SUM(sc.score) as sum from sc group by SId) as s2
on s1.sum >= s2.sum
group by s2.SId,s2.sum
order by rank;
结果：
+------+-------+------+
| SId  | sum   | rank |
+------+-------+------+
| 01   | 269.0 |    1 |
| 03   | 240.0 |    2 |
| 02   | 210.0 |    3 |
| 07   | 187.0 |    4 |
| 05   | 163.0 |    5 |
| 04   | 100.0 |    6 |
| 06   |  65.0 |    7 |
+------+-------+------+
```

### 21、查询学生的总成绩，并进行排名，总分重复时不保留名次空缺

```sql
select b.SId,b.sum,@i := @i+1 as rank
from (select @i := 0) as a,
(select sc.SId,SUM(sc.score) as sum from sc group by SId order by sum desc) as b;
结果：
+------+-------+------+
| SId  | sum   | rank |
+------+-------+------+
| 01   | 269.0 |    1 |
| 03   | 240.0 |    2 |
| 02   | 210.0 |    3 |
| 07   | 187.0 |    4 |
| 05   | 163.0 |    5 |
| 04   | 100.0 |    6 |
| 06   |  65.0 |    7 |
+------+-------+------+
```

### 22、统计各科成绩各分数段人数：课程编号，课程名称，【100-85】【85-70】【70-60】【60-0】及所占百分比

```sql
SELECT 
sc.CId,
course.Cname,
sum(CASE WHEN sc.score >= 85 THEN 1 ELSE 0 END) / count(sc.cid) as '[100-85]',
sum(CASE WHEN sc.score >= 70 and sc.score < 85 THEN 1 ELSE 0 END) / count(sc.cid) as '[85-70]',
sum(CASE WHEN sc.score >= 60 and sc.score < 70 THEN 1 ELSE 0 END) / count(sc.cid) as '[70-60]',
sum(CASE WHEN sc.score <60 THEN 1 ELSE 0 END) / count(sc.cid) as '[60-0]'
FROM sc JOIN course on course.CId=sc.CId
GROUP BY sc.CId,course.Cname;
结果：
+------+-------+----------+---------+---------+--------+
| CId  | Cname | [100-85] | [85-70] | [70-60] | [60-0] |
+------+-------+----------+---------+---------+--------+
| 01   | 语文  |   0.0000 |  0.6667 |  0.0000 | 0.3333 |
| 02   | 数学  |   0.5000 |  0.1667 |  0.1667 | 0.1667 |
| 03   | 英语  |   0.3333 |  0.3333 |  0.0000 | 0.3333 |
+------+-------+----------+---------+---------+--------+
```

### 23、查询各科前三名的记录

```sql
SELECT *
FROM sc as a
WHERE (
	SELECT count(*)
	FROM sc as b
	WHERE a.CId=b.CId
	AND b.score>=a.score
)<=3
ORDER BY a.CId;
结果：
+------+------+-------+
| SId  | CId  | score |
+------+------+-------+
| 01   | 01   |  80.0 |
| 03   | 01   |  80.0 |
| 05   | 01   |  76.0 |
| 01   | 02   |  90.0 |
| 05   | 02   |  87.0 |
| 07   | 02   |  89.0 |
| 01   | 03   |  99.0 |
| 07   | 03   |  98.0 |
+------+------+-------+
```

### 24、查询每门课程被选修的学生数

```sql
SELECT sc.CId,course.Cname,COUNT(stu.SId) as count
FROM student as stu
JOIN sc
on stu.SId=sc.SId
JOIN course
on course.CId=sc.CId
GROUP BY sc.CId,course.Cname;
结果：
+------+-------+-------+
| CId  | Cname | count |
+------+-------+-------+
| 01   | 语文  |     6 |
| 02   | 数学  |     6 |
| 03   | 英语  |     6 |
+------+-------+-------+
```

### 25、查询出只选修两门课程的学生学号和姓名

```sql
SELECT stu.SId,stu.Sname
FROM student as stu
WHERE stu.SId in(
		SELECT t.SId
		FROM
			( SELECT sc.SId, COUNT( sc.CId ) AS count FROM sc GROUP BY sc.SId ) AS t 
		WHERE count = 2
);
结果：
+------+-------+
| SId  | Sname |
+------+-------+
| 05   | 周梅  |
| 06   | 吴兰  |
| 07   | 郑竹  |
+------+-------+
```

### 26、查询男生、女生人数

```sql
SELECT Ssex as '性别',COUNT(Ssex) as '人数' FROM student GROUP BY Ssex;
结果：
+------+------+
| 性别 | 人数 |
+------+------+
| 女   |    8 |
| 男   |    4 |
+------+------+
```

### 27、查询名字中含有’风‘的学生信息

```sql
SELECT * FROM student WHERE Sname LIKE '%风%';
结果：
+------+-------+---------------------+------+
| SId  | Sname | Sage                | Ssex |
+------+-------+---------------------+------+
| 03   | 孙风  | 1990-12-20 00:00:00 | 男   |
+------+-------+---------------------+------+
```

### 28、查询同名同姓学生名单，并统计同名人数

```sql
SELECT stu.Sname,COUNT(stu.sid)
FROM student as stu
GROUP BY stu.Sname
HAVING COUNT(stu.SId)>1;
结果：
+-------+----------------+
| Sname | COUNT(stu.sid) |
+-------+----------------+
| 李四  |              2 |
+-------+----------------+
```

### 29、查询1990年出生的学生名单

```sql
SELECT * FROM student WHERE Sage like '%1990%';
结果：
+------+-------+---------------------+------+
| SId  | Sname | Sage                | Ssex |
+------+-------+---------------------+------+
| 01   | 李雷  | 1990-01-01 00:00:00 | 男   |
| 02   | 钱电  | 1990-12-21 00:00:00 | 男   |
| 03   | 孙风  | 1990-12-20 00:00:00 | 男   |
| 04   | 李云  | 1990-12-06 00:00:00 | 男   |
+------+-------+---------------------+------+
```

### 30、查询每门课程的平均成绩，结果按平均成绩降序排列，平均成绩相同时，按照课程编号升序排列

```sql
SELECT sc.CId,AVG(sc.score)
FROM sc
GROUP BY sc.CId
ORDER BY AVG(sc.score) DESC,sc.CId;
结果：
+------+---------------+
| CId  | AVG(sc.score) |
+------+---------------+
| 02   |      72.66667 |
| 03   |      68.50000 |
| 01   |      64.50000 |
+------+---------------+
```

### 31、查询平均成绩大于等于85的所有学生的学号、姓名、平均成绩

```sql
SELECT s1.*
FROM (
		SELECT sc.SId,stu.Sname,AVG(sc.score) as avg
		FROM sc
		JOIN student as stu
		on sc.SId=stu.SId
		GROUP BY sc.SId,stu.Sname
)as s1
WHERE avg>=85;
结果：
+------+-------+----------+
| SId  | Sname | avg      |
+------+-------+----------+
| 01   | 李雷  | 89.66667 |
| 07   | 郑竹  | 93.50000 |
+------+-------+----------+
```

### 32、查询课程名称为【数学】，且分数低于60的学生姓名和分数

```sql
SELECT stu.Sname,sc.CId,sc.score
FROM student as stu
JOIN sc on sc.SId=stu.SId
JOIN course on course.CId=sc.CId
WHERE course.Cname='数学' AND sc.score<60;
结果：
+-------+------+-------+
| Sname | CId  | score |
+-------+------+-------+
| 李云  | 02   |  30.0 |
+-------+------+-------+
```

### 33、查询所有学生的课程及分数情况（存在学生没成绩，没选课的情况）

```sql
SELECT DISTINCT stu.SId,stu.Sname,a.score as '语文',b.score as'数学',c.score as '英语'
FROM student as stu
LEFT JOIN sc as a on stu.SId=a.SId AND a.CId='01'
LEFT JOIN sc as b on stu.SId=b.SId AND b.CId='02'
LEFT JOIN sc as c on stu.SId=c.SId AND c.CId='03'
GROUP BY stu.SId,stu.Sname,语文,数学,英语;
结果：
+------+-------+------+------+------+
| SId  | Sname | 语文 | 数学 | 英语 |
+------+-------+------+------+------+
| 01   | 李雷  | 80.0 | 90.0 | 99.0 |
| 02   | 钱电  | 70.0 | 60.0 | 80.0 |
| 03   | 孙风  | 80.0 | 80.0 | 80.0 |
| 04   | 李云  | 50.0 | 30.0 | 20.0 |
| 05   | 周梅  | 76.0 | 87.0 | NULL |
| 06   | 吴兰  | 31.0 | NULL | 34.0 |
| 07   | 郑竹  | NULL | 89.0 | 98.0 |
| 09   | 张三  | NULL | NULL | NULL |
| 10   | 李四  | NULL | NULL | NULL |
| 11   | 李四  | NULL | NULL | NULL |
| 12   | 赵六  | NULL | NULL | NULL |
| 13   | 孙七  | NULL | NULL | NULL |
+------+-------+------+------+------+
```

### 34、查询任何一门课程在70分以上的姓名、课程名称和分数

```sql
SELECT DISTINCT stu.SId,stu.Sname,a.score as '语文',b.score as'数学',c.score as '英语'
FROM student as stu
LEFT JOIN sc as a on stu.SId=a.SId AND a.CId='01' AND a.score>70
LEFT JOIN sc as b on stu.SId=b.SId AND b.CId='02' AND b.score>70
LEFT JOIN sc as c on stu.SId=c.SId AND c.CId='03' AND c.score>70
GROUP BY stu.SId,stu.Sname,语文,数学,英语;
结果：
+------+-------+------+------+------+
| SId  | Sname | 语文 | 数学 | 英语 |
+------+-------+------+------+------+
| 01   | 李雷  | 80.0 | 90.0 | 99.0 |
| 02   | 钱电  | NULL | NULL | 80.0 |
| 03   | 孙风  | 80.0 | 80.0 | 80.0 |
| 04   | 李云  | NULL | NULL | NULL |
| 05   | 周梅  | 76.0 | 87.0 | NULL |
| 06   | 吴兰  | NULL | NULL | NULL |
| 07   | 郑竹  | NULL | 89.0 | 98.0 |
| 09   | 张三  | NULL | NULL | NULL |
| 10   | 李四  | NULL | NULL | NULL |
| 11   | 李四  | NULL | NULL | NULL |
| 12   | 赵六  | NULL | NULL | NULL |
| 13   | 孙七  | NULL | NULL | NULL |
+------+-------+------+------+------+
```

### 35、查询不及格的课程

```sql
SELECT DISTINCT stu.SId,stu.Sname,a.score as '语文',b.score as'数学',c.score as '英语'
FROM student as stu
LEFT JOIN sc as a on stu.SId=a.SId AND a.CId='01' AND a.score<60
LEFT JOIN sc as b on stu.SId=b.SId AND b.CId='02' AND b.score<60
LEFT JOIN sc as c on stu.SId=c.SId AND c.CId='03' AND c.score<60
GROUP BY stu.SId,stu.Sname,语文,数学,英语;
结果：
+------+-------+------+------+------+
| SId  | Sname | 语文 | 数学 | 英语 |
+------+-------+------+------+------+
| 01   | 李雷  | NULL | NULL | NULL |
| 02   | 钱电  | NULL | NULL | NULL |
| 03   | 孙风  | NULL | NULL | NULL |
| 04   | 李云  | 50.0 | 30.0 | 20.0 |
| 05   | 周梅  | NULL | NULL | NULL |
| 06   | 吴兰  | 31.0 | NULL | 34.0 |
| 07   | 郑竹  | NULL | NULL | NULL |
| 09   | 张三  | NULL | NULL | NULL |
| 10   | 李四  | NULL | NULL | NULL |
| 11   | 李四  | NULL | NULL | NULL |
| 12   | 赵六  | NULL | NULL | NULL |
| 13   | 孙七  | NULL | NULL | NULL |
+------+-------+------+------+------+
```

### 36、查询课程编号为01且课程成绩在80分以上的学生的学号和姓名

```sql
SELECT stu.SId,stu.Sname
FROM student as stu
JOIN sc on stu.SId=sc.SId
WHERE sc.CId='01' AND sc.score>=80;
结果：
+------+-------+
| SId  | Sname |
+------+-------+
| 01   | 李雷  |
| 03   | 孙风  |
+------+-------+
```

### 37、查询每门课程的学生数

```sql
SELECT sc.CId,course.Cname,COUNT(stu.SId) as count
FROM student as stu
JOIN sc
on stu.SId=sc.SId
JOIN course
on course.CId=sc.CId
GROUP BY sc.CId,course.Cname;
结果：
+------+-------+-------+
| CId  | Cname | count |
+------+-------+-------+
| 01   | 语文  |     6 |
| 02   | 数学  |     6 |
| 03   | 英语  |     6 |
+------+-------+-------+
```

### 38、成绩不重复，查询【张三】老师所授课程学生中，成绩最高的学生信息及其成绩

```sql
SELECT stu.*,sc.cid,sc.score
FROM student as stu
JOIN sc on sc.SId=stu.SId
JOIN course on course.CId=sc.CId
JOIN teacher on teacher.TId=course.TId
WHERE teacher.Tname='张三'
ORDER BY score DESC LIMIT 1;
结果：
+------+-------+---------------------+------+------+-------+
| SId  | Sname | Sage                | Ssex | cid  | score |
+------+-------+---------------------+------+------+-------+
| 01   | 李雷  | 1990-01-01 00:00:00 | 男   | 02   |  90.0 |
+------+-------+---------------------+------+------+-------+
```

### 39、成绩有重复的情况下，查询【张三】老师所授课程学生中，成绩最高的学生信息及其成绩

```sql
SELECT stu.Sname,sc.CId,sc.score
FROM student as stu
JOIN sc on sc.SId=stu.SId
WHERE sc.score IN (
		SELECT MAX(sc.score)
		FROM student as stu
		JOIN sc on sc.SId=stu.SId
		JOIN course on course.CId=sc.CId
		JOIN teacher on teacher.TId=course.TId
		WHERE teacher.Tname='张三'
);
结果：
+-------+------+-------+
| Sname | CId  | score |
+-------+------+-------+
| 李雷  | 02   |  90.0 |
+-------+------+-------+
```

### 40、查询不同课程成绩相同的学生的学生编号，课程编号，学生成绩

```sql
SELECT *
FROM sc as a
WHERE (
	SELECT count(*)
	FROM sc as b
	WHERE a.CId=b.CId
	AND b.score=a.score
)>=2
ORDER BY a.score;
结果：
+------+------+-------+
| SId  | CId  | score |
+------+------+-------+
| 01   | 01   |  80.0 |
| 02   | 03   |  80.0 |
| 03   | 01   |  80.0 |
| 03   | 03   |  80.0 |
+------+------+-------+
```

### 41、查询每门课程成绩最好的前两名

```sql
SELECT *
FROM sc as a
WHERE (
	SELECT count(*)
	FROM sc as b
	WHERE a.CId=b.CId
	AND b.score>=a.score
)<=2
ORDER BY a.CId;
结果：
+------+------+-------+
| SId  | CId  | score |
+------+------+-------+
| 01   | 01   |  80.0 |
| 03   | 01   |  80.0 |
| 01   | 02   |  90.0 |
| 07   | 02   |  89.0 |
| 01   | 03   |  99.0 |
| 07   | 03   |  98.0 |
+------+------+-------+
```

### 42、统计每门课程的学生选修人数（超过5人统计）

```sql
SELECT * 
FROM
	(SELECT sc.CId,COUNT(sc.SId)as count
	FROM sc
	GROUP BY sc.CId)as t
WHERE t.count>5;
结果：
+------+-------+
| CId  | count |
+------+-------+
| 01   |     6 |
| 02   |     6 |
| 03   |     6 |
+------+-------+
```

### 43、检索至少选修两门课程的学生学号

```sql
SELECT * 
FROM
	(SELECT sc.SId,COUNT(sc.CId)as count
	FROM sc
	GROUP BY sc.SId)as t
WHERE t.count>=2;
结果：
+------+-------+
| SId  | count |
+------+-------+
| 01   |     3 |
| 02   |     3 |
| 03   |     3 |
| 04   |     3 |
| 05   |     2 |
| 06   |     2 |
| 07   |     2 |
+------+-------+
```

### 44、查询选修了全部课程的学生信息

```sql
SELECT * 
FROM
	(SELECT sc.SId,COUNT(sc.CId)as count
	FROM sc
	GROUP BY sc.SId)as t
WHERE t.count=3;
结果：
+------+-------+
| SId  | count |
+------+-------+
| 02   |     3 |
| 03   |     3 |
| 04   |     3 |
| 01   |     3 |
+------+-------+
```

### 45、查询各学生的年龄，只按照年份来算

```sql
SELECT stu.Sname,(YEAR(NOW())-YEAR(stu.Sage)) as age
FROM student as stu;
结果：
+-------+------+
| Sname | age  |
+-------+------+
| 李雷  |   30 |
| 钱电  |   30 |
| 孙风  |   30 |
| 李云  |   30 |
| 周梅  |   29 |
| 吴兰  |   28 |
| 郑竹  |   31 |
| 张三  |    3 |
| 李四  |    3 |
| 李四  |    8 |
| 赵六  |    7 |
| 孙七  |    6 |
+-------+------+
```

### 46、按照出生日期来算，当前月日<出生年月的月日则，年龄减一

```sql
SELECT stu.Sname,stu.Sage,
(CASE 
	WHEN DAYOFYEAR(NOW())<DAYOFYEAR(stu.Sage) THEN
		YEAR(NOW())-YEAR(stu.Sage)-1
	ELSE
		YEAR(NOW())-YEAR(stu.Sage)
END)as age
FROM student as stu; 当前日期：2020-08-01
结果：
+-------+---------------------+------+
| Sname | Sage                | age  |
+-------+---------------------+------+
| 李雷  | 1990-01-01 00:00:00 |   30 |
| 钱电  | 1990-12-21 00:00:00 |   29 |
| 孙风  | 1990-12-20 00:00:00 |   29 |
| 李云  | 1990-12-06 00:00:00 |   29 |
| 周梅  | 1991-12-01 00:00:00 |   28 |
| 吴兰  | 1992-01-01 00:00:00 |   28 |
| 郑竹  | 1989-01-01 00:00:00 |   31 |
| 张三  | 2017-12-20 00:00:00 |    2 |
| 李四  | 2017-12-25 00:00:00 |    2 |
| 李四  | 2012-06-06 00:00:00 |    8 |
| 赵六  | 2013-06-13 00:00:00 |    7 |
| 孙七  | 2014-06-01 00:00:00 |    6 |
+-------+---------------------+------+
```

### 47、查询本周过生日的学生

返回日期从范围内的数字日历星期一到53

```sql
SELECT stu.Sname,stu.Sage
FROM student as stu
WHERE WEEKOFYEAR('2020-12-20')=WEEKOFYEAR(stu.Sage);
结果：
+-------+---------------------+
| Sname | Sage                |
+-------+---------------------+
| 钱电  | 1990-12-21 00:00:00 |
| 孙风  | 1990-12-20 00:00:00 |
| 张三  | 2017-12-20 00:00:00 |
+-------+---------------------+
```

### 48、查询下周过生日的学生

```sql
SELECT stu.Sname,stu.Sage
FROM student as stu
WHERE (WEEKOFYEAR('2020-12-20')+1)%53=WEEKOFYEAR(stu.Sage);
结果：
+-------+---------------------+
| Sname | Sage                |
+-------+---------------------+
| 郑竹  | 1989-01-01 00:00:00 |
| 李四  | 2017-12-25 00:00:00 |
+-------+---------------------+
```

### 49、查询本月过生日的学生

```sql
SELECT stu.Sname,stu.Sage
FROM student as stu
WHERE MONTH('2020-12-20')=MONTH(stu.Sage);
结果：
+-------+---------------------+
| Sname | Sage                |
+-------+---------------------+
| 钱电  | 1990-12-21 00:00:00 |
| 孙风  | 1990-12-20 00:00:00 |
| 李云  | 1990-12-06 00:00:00 |
| 周梅  | 1991-12-01 00:00:00 |
| 张三  | 2017-12-20 00:00:00 |
| 李四  | 2017-12-25 00:00:00 |
+-------+---------------------+
```

### 50、查询下月过生日的学生

```sql
SELECT stu.Sname,stu.Sage
FROM student as stu
WHERE (MONTH('2020-12-20')+1)%12=MONTH(stu.Sage);
结果：
+-------+---------------------+
| Sname | Sage                |
+-------+---------------------+
| 李雷  | 1990-01-01 00:00:00 |
| 吴兰  | 1992-01-01 00:00:00 |
| 郑竹  | 1989-01-01 00:00:00 |
+-------+---------------------+
```


# Java集合

- 为什么要学习Java集合？

  Java是一门面向对象的语言，为了方便操作多个对象，需要把对象存储器起来，而想要存储多个对象，需要一个**容器**（集合）来装载。

  简单来说：==Java提供了集合这个工具来方便我们去操纵多个Java对象==

## 一、集合（Collection）

### 数组和集合的区别：

- 1、**长度**
  - 数组的长度固定
  - 集合的长度可变
- 2、**元素的数据类型**
  - 数组可以存储**基本数据类型**，也可以存储**引用数据类型**（类、接口、数组）
  - 集合只能存储**引用数据类型**

![Collection结构图](C:\Users\jasmi\Desktop\collection图.png)

### Collection功能：

1、添加功能：

- boolean add(E e)：添加一个元素
- boolean addAll(Collection<? extends E> c)：添加一个集合的元素

2、删除功能：

- boolean remove(Object o)：删除一个元素
- boolean removeAll(Collection<?> c)：删除一个集合的所有元素
- void clear()：从此集合中删除所有元素

3、判断功能：

- boolean contains(Object o)：判断集合是否包含此元素
- boolean containsAll(Collection<?> c)：判断集合是否包含指定的集合中的所有元素
- boolean isEmpty()：判断集合是否为空

4、获取功能：

- Iterator<E> iterator()：迭代器

5、长度功能：

- int size()：返回集合中的元素个数

6、交集功能：

- boolean retainAll(Collection<?> c)：从此集合中删除未包含在指定集合中的所有元素，也就是集合A与集合B做交集，最终的结果保存在集合A，返回值表示A集合是否发生变化

### List集合

特点：有序（存储顺序与取出顺序一致），可重复

常用子类：

- ArrayList：底层数据结构是数组，线程不安全
- LinkedList：底层数据结构是链表，线程不安全
- Vector：底层数据结构是数组，线程安全

### Set集合

特点：元素不可重复

常用子类：

- HashSet：底层数据结构是哈希表（是一个元素为链表的数组）
- TreeSet：底层数据结构是红黑树（是一个自平衡的二叉树），保证元素的排序方式
- LinkedHashSet：底层数据结构有哈希表和链表组成

## 二、List集合

### 1、ArrayList

底层的数据结构是数组，线程不安全，对于添加删除慢，查找快

#### 构造方法：

- ArrayList()：构造一个初始容量为==10==的空列表【在最开始时是一个长度为0的数组，在第一次添加数据时扩容为10】

- ArrayList(int initialCapacity)：构造指定容量的空列表

- ArrayList(Collection<? extends E> c)：按照集合的迭代器返回的顺序构造一个包含指定集合元素的列表

#### add方法

##### 1、public boolean add(E e)：添加元素到此列表的末尾，返回的一定是==true==

实现方式：

- 首先检查数组的容量是否足够
  - 足够：直接添加数据
  - 不够：扩容
    - 扩容到原来的1.5倍
    - 第一次扩容后，如果容量还是小于minCapacity（最小需要的长度），就将容量扩充为minCapacity

##### 2、public void add(int index, E element)：将指定元素插入此列表中的指定位置

实现方式：

- 首先查看角标是否越界
- 空间检查，如果有需要，进行扩容
- 利用arraycopy（）来插入元素

#### get方法

public E get(int index)：返回此列表中指定未知的元素

实现方式：

- 检查角标
- 返回相应元素

```java
public E get(int index){
    rangeCheck(index);
    return elementData(index);
}
// 检查角标
private void rangeCheck(int index) {
if (index >= size)
throw new IndexOutOfBoundsException(outOfBoundsMsg(index));
}
//返回相应元素
E elementData(int index) {
return (E) elementData[index];
}
```

#### set方法

public E set(int index, E element)：用指定的元素替代此列表中指定未知的元素

实现方法：

- 检查角标
- 替代元素
- 放回旧元素的值

```java
public E set(int index, E element){
    rangeCheck(index);
    E oldValue = elementData(index);
    elementData[index] = element;
    return oldValue;
}
```



#### remove方法

public E remove(int index)：删除指定位置的元素，返回被删除的元素，删除元素时不会减少容量

实现方式：

- 检查角标
- 删除元素
- 计算出需要移动的个数，并进行移动
- 将数组最后一个多余的数据设置为null，让GC回收

```java
public E remove(int index) {
    rangeCheck(index);
    modCount++;
    E oldValue = elementData(index);
    int numMoved = size - index - 1;
    if (numMoved > 0)
        System.arraycopy(elementData, index+1, elementData, index,numMoved);
    elementData[--size] = null; // clear to let GC do its work
    return oldValue;
}
```

### 2、Vector

Vector底层也是**数组**，与ArrayList最大的区别是：

1、==线程安全==

2、ArrayList在底层数组不够用时，扩容为原来的1.5倍，Vector扩容为原来的2倍（如果指定增量，则按照增量扩容）

### 3、LinkedList

LinkedList底层是**双向链表**，增删更快，查找慢【和数组相反】

#### 构造方法：

- LinkedList()：构造一个空列表
- LinkedList(Collection<? extends E> c)：按照集合的迭代器返回的顺序构造一个包含指定集合元素的列表

### List集合总结

ArrayList：

- 底层实现结构是数组
- 默认初始化是10，每次扩容为原来的1.5倍
- 在增删时需要数组的拷贝复制【arraycopy()】

Vector：

- 底层实现结构是数组
- 默认初始化是10，每次扩容为原来的2倍

LinkedList：

- 底层实现结构是双向链表

==使用场景：增删多使用LinkedList，查询多使用ArrayList==

## 三、Set集合




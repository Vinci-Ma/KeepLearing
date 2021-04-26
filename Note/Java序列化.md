一般在定义实体类时会继承Serializable接口

# 1、Serializable接口

一个对象序列化的接口，一个类只有实现了Serializable接口，它的对象才能被序列化。

# 2、序列化

序列化时将对象状态转换为可保持或传输的格式的过程。与序列化相对的时反序列化，它将流转换为对象。这两个过程结合起来，可以轻松地存储和传输数据。

实现了序列化的对象==》

1、可以被ObjectOutputStream转为字节流

2、可以通过ObjectInputStream将其解析为对象

# 3、序列化对象的原因

Java中为了实现对象的 IO 操作，必须实现 Serializable 接口，进行序列化操作。

- 1、把对象转换为字节序列的过程称为对象的序列化
- 2、把字节序列恢复为对象的过程称为对象的反序列化

1、存储对象在存储介质中，以便在下次使用的时候，可以很快捷的重建一个副本，也就是 “When the resulting series of bits is reread according to the serialization format,it can be used to create a semantically identical clone of the original object”。

2、便于数据传输，尤其是在远程调用的时候。

# 4、什么情况下需要序列化

当我们需要把对象的状态信息通过网络进行传输，或者需要将对象的状态信息持久化，以便将来使用时都需要把对象进行序列化

- 1、需要把内存中的对象状态数据保存到一个文件或数据库中。eg：利用Mybatis框架编写持久层insert对象数据到数据库中。
- 2、网络通信时需要用套接字在网络中传送对象时。eg：使用RPC协议进行网络通信时。

Serializable接口中什么都没有，可以将它理解成一个标识接口，这个接口是给jvm看的，通知jvm帮忙将此类进行序列化。

Serializable接口就是Java提供用来进行高效率的异地共享实例对象的机制，实现这个接口即可。

# 5、为什么要定义 serialversionUID 变量

如果没有自定义一个serialversionUID 变量，接口会默认生成一个serialversionUID。但是建议用户自定义一个serialversionUID，因为默认的serialversionUID对于class的细节非常敏感，反序列化时可能会导致 InvalidClassException 这个异常。

serialversionUID 是用来辅助对象的序列化与反序列化的，原则上序列化后的数据中的serialversionUID 与当前类中的serialversionUID 一致，那么对象才能被反序列化成功。

**详细工作机制**：在序列化的时候系统将serialVersionUID写入到序列化的文件中去，当反序列化的时候系统会先去检测文件中的serialVersionUID是否跟当前的文件的serialVersionUID是否一致，如果一致则反序列化成功，否则就说明当前类跟序列化后的类发生了变化，比如是成员变量的数量或者是类型发生了变化，那么在反序列化时就会发生crash，并且会报出错误。

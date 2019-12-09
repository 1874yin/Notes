# JAVA 的 IO 流

## 概念

流是一组有顺序的，有起点和终点的字节集合，是对数据传输的总称或抽象。即数据在两设备间的传输称为流，流的本质是数据传输

## 分类

根据处理数据类型的不同，IO 流可以分为：*字节流*、*字符流*

根据数据流向的不同，又可以分为：*输入流*、*输出流*

## 结构图

<img src="../pics/IOstream.png" alt="IOstream" style="zoom:50%;" align="left"/>



## 用法

### 字节流操作

**读取文件**：创建文件对象 --> 创建字节流 （输入）--> 使用 byte 数组接收文件 --> read 方法读取 --> new String() 方法将 byte 转为 String 类型

```java
File file = new File("D:/coding/test/in/test.txt");
FileInputStream in = new FileInputStream(file);
byte[] data = new byte[(int) file.length()];
in.read(data);
String str = new String(data);
```

**写入文件**：创建文件对象 --> 创建字节流 --> 使用 getBytes() 获得 byte 数组 --> write 方法写入

```java
String string = "Text to be written.";
File file = new file("D:/coding/test/out/test.txt");
FileOutputStream out = new FileOutputStream(file);
byte[] data = string.getBytes();
out.write(data);
```

*P.S. 字节流是直接在文件上操作，所以即使不关闭流也会改变文件内容*

**关闭方法**：

```java
1. try {out.close(); } catch...
  
2. try {...} catch... finally {
  if (null != out) {
    try {out.close(); } catch...
  }
}

3. try (FileOutputStream out = new FileOutputStream(file)) {...}
```

### **字符流操作**

**读取文件**：创建文件对象 --> 创建字符流 --> 使用 char 数组接收文件 --> read 方法读取 --> 使用 String.valueOf() 方法转为 String 类型

```java
File file = new File("D:/coding/test/in/test.txt");
FileReader reader = new FileReader(file);
char[] data = new char[(int)file.length()];
reader.read(data);
String str = String.valueOf(data);
```

**写入文件**：创建文件对象 --> 创建字符流 --> 使用 String 类的 toCharArray() 方法获得 char 数组 --> write 方法写入

```java
String str = "Text to be written.";
File file = new File("D:/coding/test/out/test.txt");
FileWriter writer = new FileWriter(file);
char[] data = str.toCharArray();
writer.write(data);
```

### 缓存流操作

1. 创建在一个存在的流的基础上

2. 比较高效，相对于一个字符/字节地读取、转换、返回来说，它有一个缓冲区，读满缓冲区才返回

**BufferedReader**：

- 可以使用 readLine() 方法，每次读回来一行，省去手动拼接的繁琐

```java
BufferedReader reader = new BufferedReader(new FileReader(new File("D:/coding/test/in/test.txt")));
String newLine = null;
while(null != (newLine = reader.readLine())) {
  System.out.println(newLine);
}
```

**BufferedWriter**：

- 使用 newLine() 方法换行
- 写入后需要使用 flush() 方法刷新缓存，将数据写入到文件中

```java
BufferedWriter writer = new BufferedWriter(new FileWriter(new File("D:/coding/test/out/test.txt")));
writer.write("Hello world!");
writer.newLine();
writer.write("This is a new line.");
writer.write("This is another new sentence.");
writer.flush();
```

### 转换流

字符流和字节流转换

**特点**

- 字符流和字节流之间的桥梁
- 可对读取到的字节流数据经过指定编码转换成字符
- 可对读取到的字符数据经过指定编码转换成字节

**何时使用转换流？**

- 当字节和字符之间有转换动作时
- 流操作的数据需要编码或解码时

**具体的对象体现**

- *InputStreamReader*：字节到字符的桥梁

- *OutputStreamWriter*：字符到字节的桥梁

这两个流对象是字符体系的成员，有转换作用，本身又是字符流，所以在构造的时候需要**传入字节流对象**进来

【**实例**】将字节输入流转换为字符输入流

```java
File file = new File("D:/coding/test/in/test.txt");
Reader reader = new InputStreamReader(new FileInputStream(file));
char[] b = new char[100]; // 根据需要调整大小
int len = reader.read(b);
System.out.println(new String(b, 0, len));
reader.close();
```

【**实例**】将字节输出流转换为字符输出流

```java
File file = new File("D:/coding/test/out/test.txt");
Writer writer = new OutputStreamWriter(new FileOutputStream(file));
writer.write("hello");
writer.close();
```


# JVM

### 一、jvm,jdk,jre区别

- jvm,即java virtual machine. Java虚拟机。这也是java可以一处编译，多处运行的原因。
- jdk,即java develepment kit. java开发工具。开发者使用。
- jre,java runtime environment java运行时环境。jdk内置。

### 二、jdk类加载过程

- 加载
  1. 将类的字节码载入方法区，并创建类.class文件
  2. 如果该类的父类没有加载，先加载父类
  3. 类加载是懒惰的

- 链接
  1. 验证类是否合法，检查合法性和安全性
  2. 为static静态变量分配空间，设置默认值
  3. 将常量池的符号引用解析为直接引用
- 初始化
  1. 为静态代码块和非final修饰的变量赋值
  2. 初始化是懒惰执行


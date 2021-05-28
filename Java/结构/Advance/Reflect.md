反射

## 反射是什么？

<p>反射的概念是由 Smith 在 1982 年首次提出的，主要是指程序可以访问、检测和修改它本身状态或行为的一种能力。通俗地讲，一提到反射，我们就可以想到镜子。镜子可以明明白白地照出我是谁，还可以照出别人是谁。反映到程序中，反射就是用来让开发者知道这个类中有什么成员，以及别的类中有什么成员。</p>

![反射](https://pic4.zhimg.com/80/v2-312b1b34ca2ebd1317bedf70ddfad38b_720w.jpg)

## 反射API

Java 类的成员包括以下三类：属性字段、构造函数、方法。反射的 API 也是与这几个成员相关：

- Field 类：提供有关类的属性信息，以及对它的动态访问权限。它是一个封装反射类的属性的类。
- Constructor 类：提供有关类的构造方法的信息，以及对它的动态访问权限。它是一个封装反射类的构造方法的类。
- Method 类：提供关于类的方法的信息，包括抽象方法。它是用来封装反射类方法的一个类。
- Class 类：表示正在运行的 Java 应用程序中的类的实例。
- Object 类：Object 是所有 Java 类的父类。所有对象都默认实现了 Object 类的方法。

### **3.1 获取 Class 对象的三种方式**

获取 Class 对象有三种方式：

```java
// 1.通过字符串获取Class对象，这个字符串必须带上完整路径名
Class studentClass = Class.forName("com.test.reflection.Student");
// 2.通过类的class属性
Class studentClass2 = Student.class;
// 3.通过对象的getClass()函数
Student studentObject = new Student();
Class studentClass3 = studentObject.getClass();
```

- 第一种方法是通过类的全路径字符串获取 Class 对象，这也是我们平时最常用的反射获取 Class 对象的方法；
- 第二种方法有限制条件：需要导入类的包；
- 第三种方法已经有了 Student 对象，不再需要反射。

通过这三种方式获取到的 Class 对象是同一个，也就是说 Java 运行时，每一个类只会生成一个 Class 对象。

我们将其打印出来测试一下：

```java
System.out.println("class1 = " + studentClass + "\n" +
        "class2 = " + studentClass2 + "\n" +
        "class3 = " + studentClass3 + "\n" +
        "class1 == class2 ? " + (studentClass == studentClass2) + "\n" +
        "class2 == class3 ? " + (studentClass2 == studentClass3));
```

运行程序，输出如下：

```text
class1 = class com.test.reflection.Student
class2 = class com.test.reflection.Student
class3 = class com.test.reflection.Student
class1 == class2 ? true
class2 == class3 ? true
```

OK，拿到 Class 对象之后，我们就可以为所欲为啦！

### **3.2 获取成员变量**

获取字段有两个 API：`getDeclaredFields`和`getFields`。他们的区别是:`getDeclaredFields`用于获取所有声明的字段，包括公有字段和私有字段，`getFields`仅用来获取公有字段：

```java
// 1.获取所有声明的字段
Field[] declaredFieldList = studentClass.getDeclaredFields();
for (Field declaredField : declaredFieldList) {
    System.out.println("declared Field: " + declaredField);
}
// 2.获取所有公有的字段
Field[] fieldList = studentClass.getFields();
for (Field field : fieldList) {
    System.out.println("field: " + field);
}
```

运行程序，输出如下：

```text
declared Field: private java.lang.String com.test.reflection.Student.studentName
declared Field: public int com.test.reflection.Student.studentAge
field: public int com.test.reflection.Student.studentAge
```



### **3.3 获取构造方法**

获取构造方法同样包含了两个 API：用于获取所有构造方法的 `getDeclaredConstructors`和用于获取公有构造方法的`getConstructors`:

```java
// 1.获取所有声明的构造方法
Constructor[] declaredConstructorList = studentClass.getDeclaredConstructors();
for (Constructor declaredConstructor : declaredConstructorList) {
    System.out.println("declared Constructor: " + declaredConstructor);
}
// 2.获取所有公有的构造方法
Constructor[] constructorList = studentClass.getConstructors();
for (Constructor constructor : constructorList) {
    System.out.println("constructor: " + constructor);
}
```

运行程序，输出如下：

```text
declared Constructor: public com.test.reflection.Student()
declared Constructor: private com.test.reflection.Student(java.lang.String)
constructor: public com.test.reflection.Student()
```



### **3.4.获取非构造方法**

同样地，获取非构造方法的两个 API 是：获取所有声明的非构造函数的 `getDeclaredMethods` 和仅获取公有非构造函数的 `getMethods`：

```java
// 1.获取所有声明的函数
Method[] declaredMethodList = studentClass.getDeclaredMethods();
for (Method declaredMethod : declaredMethodList) {
    System.out.println("declared Method: " + declaredMethod);
}
// 2.获取所有公有的函数
Method[] methodList = studentClass.getMethods();
for (Method method : methodList) {
    System.out.println("method: " + method);
}
```

运行程序，输出如下：

```text
declared Method: public void com.test.reflection.Student.setStudentAge(int)
declared Method: private java.lang.String com.test.reflection.Student.show(java.lang.String)
method: public void com.test.reflection.Student.setStudentAge(int)
method: public final void java.lang.Object.wait(long,int) throws java.lang.InterruptedException
method: public final native void java.lang.Object.wait(long) throws java.lang.InterruptedException
method: public final void java.lang.Object.wait() throws java.lang.InterruptedException
method: public boolean java.lang.Object.equals(java.lang.Object)
method: public java.lang.String java.lang.Object.toString()
method: public native int java.lang.Object.hashCode()
method: public final native java.lang.Class java.lang.Object.getClass()
method: public final native void java.lang.Object.notify()
method: public final native void java.lang.Object.notifyAll()
```

从输出中我们看到，`getMethods` 方法不仅获取到了我们声明的公有方法`setStudentAge`，还获取到了很多 Object 类中的公有方法。这是因为我们前文已说到：Object 是所有 Java 类的父类。所有对象都默认实现了 Object 类的方法。 而`getDeclaredMethods`是无法获取到父类中的方法的。



![img](https://pic3.zhimg.com/80/v2-93fc28cf9f5b87fb9747ba63d94a5c82_720w.jpg)

## **实践**

学以致用，让我们来一个实际的应用感受一下。还是以 Student 类为例，如果此类在其他的包中，并且我们的需求是要在程序中通过反射获取他的构造方法，构造出 Student 对象，并且通过反射访问他的私有字段和私有方法。那么我们可以这样做：

```java
// 1.通过字符串获取Class对象，这个字符串必须带上完整路径名
Class studentClass = Class.forName("com.test.reflection.Student");
// 2.获取声明的构造方法，传入所需参数的类名，如果有多个参数，用','连接即可
Constructor studentConstructor = studentClass.getDeclaredConstructor(String.class);
// 如果是私有的构造方法，需要调用下面这一行代码使其可使用，公有的构造方法则不需要下面这一行代码
studentConstructor.setAccessible(true);
// 使用构造方法的newInstance方法创建对象，传入构造方法所需参数，如果有多个参数，用','连接即可
Object student = studentConstructor.newInstance("NameA");
// 3.获取声明的字段，传入字段名
Field studentAgeField = studentClass.getDeclaredField("studentAge");
// 如果是私有的字段，需要调用下面这一行代码使其可使用，公有的字段则不需要下面这一行代码
// studentAgeField.setAccessible(true);
// 使用字段的set方法设置字段值，传入此对象以及参数值
studentAgeField.set(student,10);
// 4.获取声明的函数，传入所需参数的类名，如果有多个参数，用','连接即可
Method studentShowMethod = studentClass.getDeclaredMethod("show",String.class);
// 如果是私有的函数，需要调用下面这一行代码使其可使用，公有的函数则不需要下面这一行代码
studentShowMethod.setAccessible(true);
// 使用函数的invoke方法调用此函数，传入此对象以及函数所需参数，如果有多个参数，用','连接即可。函数会返回一个Object对象，使用强制类型转换转成实际类型即可
Object result = studentShowMethod.invoke(student,"message");
System.out.println("result: " + result);
```

程序的逻辑注释已经写得很清晰了，我们再梳理一下：

1. 先用第一种全路径获取 Class 的方法获取到了 Student 的 Class 对象
2. 然后反射调用它的私有构造方法 `private Student(String studentName)`，构建出 newInstance
3. 再将其公有字段 studentAge 设置为 10
4. 最后反射调用其私有方法 `show`，传入参数 “message”，并打印出这个方法的返回值。

其中，`setAccessible` 函数用于动态获取访问权限，Constructor、Field、Method 都提供了此方法，让我们得以访问类中的私有成员。

运行程序，输出如下：

```text
show: NameA,10,message
result: testReturnValue
```

## 参考

- [详解面试中常考的 Java 反射机制](https://zhuanlan.zhihu.com/p/86293659)
- [大白话说Java反射：入门、使用、原理](https://www.cnblogs.com/chanshuyi/p/head_first_of_reflection.html)


# Java

## 参考文献

### MacOS Java环境配置

[如何在苹果M1芯片 (Apple Silicon) 上安装 JDK 环境](https://www.winsonlo.com/it/howto/zulu-jdk8-on-m1/)

[Mac M1 Java 开发环境配置](https://www.jianshu.com/p/c85349d6310e)

[Java8在线API文档](https://www.matools.com/api/java8)

[IDEA文档](https://github.com/judasn/IntelliJ-IDEA-Tutorial)

[鱼皮java学习路线](https://luxian.yupi.icu/#/roadmap/Java%E5%AD%A6%E4%B9%A0%E8%B7%AF%E7%BA%BF)

[鱼皮github学习路线](https://github.com/liyupi/code-roadmap)

## java基础

### java编译

* 一个java文件只能有一个public类，并且以这个public类命名
* 一个java文件可以有多个其他的类，`javac`编译后，每一个类都会生成对应的`类名.class`文件
  <img src="https://gitee.com/dexport/blog-image/raw/master/img/202202121532046.png" alt="类编译" style="zoom:50%;" />  
* 非public类也可由main方法，直接运行生成的`.class`文件就可以运行指定的`类.class`的main方法
  <img src="https://gitee.com/dexport/blog-image/raw/master/img/202202121539167.png" style="zoom:50%;" />

### 基本数据类型

```java
public class PrimitiveTypeTest {  
    public static void main(String[] args) {  
        // byte  
        System.out.println("基本类型：byte 二进制位数：" + Byte.SIZE);  
        System.out.println("包装类：java.lang.Byte");  
        System.out.println("最小值：Byte.MIN_VALUE=" + Byte.MIN_VALUE);  
        System.out.println("最大值：Byte.MAX_VALUE=" + Byte.MAX_VALUE);  
        System.out.println();  
  
        // short  
        System.out.println("基本类型：short 二进制位数：" + Short.SIZE);  
        System.out.println("包装类：java.lang.Short");  
        System.out.println("最小值：Short.MIN_VALUE=" + Short.MIN_VALUE);  
        System.out.println("最大值：Short.MAX_VALUE=" + Short.MAX_VALUE);  
        System.out.println();  
  
        // int  
        System.out.println("基本类型：int 二进制位数：" + Integer.SIZE);  
        System.out.println("包装类：java.lang.Integer");  
        System.out.println("最小值：Integer.MIN_VALUE=" + Integer.MIN_VALUE);  
        System.out.println("最大值：Integer.MAX_VALUE=" + Integer.MAX_VALUE);  
        System.out.println();  
  
        // long  
        System.out.println("基本类型：long 二进制位数：" + Long.SIZE);  
        System.out.println("包装类：java.lang.Long");  
        System.out.println("最小值：Long.MIN_VALUE=" + Long.MIN_VALUE);  
        System.out.println("最大值：Long.MAX_VALUE=" + Long.MAX_VALUE);  
        System.out.println();  
  
        // float  
        System.out.println("基本类型：float 二进制位数：" + Float.SIZE);  
        System.out.println("包装类：java.lang.Float");  
        System.out.println("最小值：Float.MIN_VALUE=" + Float.MIN_VALUE);  
        System.out.println("最大值：Float.MAX_VALUE=" + Float.MAX_VALUE);  
        System.out.println();  
  
        // double  
        System.out.println("基本类型：double 二进制位数：" + Double.SIZE);  
        System.out.println("包装类：java.lang.Double");  
        System.out.println("最小值：Double.MIN_VALUE=" + Double.MIN_VALUE);  
        System.out.println("最大值：Double.MAX_VALUE=" + Double.MAX_VALUE);  
        System.out.println();  
  
        // char  
        System.out.println("基本类型：char 二进制位数：" + Character.SIZE);  
        System.out.println("包装类：java.lang.Character");  
        // 以数值形式而不是字符形式将Character.MIN_VALUE输出到控制台  
        System.out.println("最小值：Character.MIN_VALUE="  
                + (int) Character.MIN_VALUE);  
        // 以数值形式而不是字符形式将Character.MAX_VALUE输出到控制台  
        System.out.println("最大值：Character.MAX_VALUE="  
                + (int) Character.MAX_VALUE);  
    }  
}
/**
基本类型：byte 二进制位数：8
包装类：java.lang.Byte
最小值：Byte.MIN_VALUE=-128
最大值：Byte.MAX_VALUE=127

基本类型：short 二进制位数：16
包装类：java.lang.Short
最小值：Short.MIN_VALUE=-32768
最大值：Short.MAX_VALUE=32767

基本类型：int 二进制位数：32
包装类：java.lang.Integer
最小值：Integer.MIN_VALUE=-2147483648
最大值：Integer.MAX_VALUE=2147483647

基本类型：long 二进制位数：64
包装类：java.lang.Long
最小值：Long.MIN_VALUE=-9223372036854775808
最大值：Long.MAX_VALUE=9223372036854775807

基本类型：float 二进制位数：32
包装类：java.lang.Float
最小值：Float.MIN_VALUE=1.4E-45
最大值：Float.MAX_VALUE=3.4028235E38

基本类型：double 二进制位数：64
包装类：java.lang.Double
最小值：Double.MIN_VALUE=4.9E-324
最大值：Double.MAX_VALUE=1.7976931348623157E308

基本类型：char 二进制位数：16
包装类：java.lang.Character
最小值：Character.MIN_VALUE=0
最大值：Character.MAX_VALUE=65535
*
```

### 基本数据类型转换

#### 自动类型转换

<img src="https://gitee.com/dexport/blog-image/raw/master/img/202202122329221.png" style="zoom:50%;" />



<img src="https://gitee.com/dexport/blog-image/raw/master/img/202202122335778.png" style="zoom:50%;" />

#### 强制类型转换

### 程序控制结构

#### switch

<img src="https://gitee.com/dexport/blog-image/raw/master/img/202202142351797.jpg" alt="switch" style="zoom:50%;" />

#### 增加for循环

> for(声明语句 : 表达式) {   //代码句子 }

```java
public class Hello {  
    public static void main(String[] args) {  
        int[] nums = {1,2,3,4,5};
        for(int num : nums) {
            System.out.println(num);
        }
    }  
}
```

### 包装类

> 将基础数据类型封装成一个对象，所有的包装类**（Integer、Long、Byte、Double、Float、Short）**都是抽象类 Number 的子类。
>
> 布尔类型的包装类是**Boolean**，char的包装类是**Character**
>
> 这种由编译器特别支持的包装称为装箱，所以当内置数据类型被当作对象使用的时候，编译器会把内置类型装箱为包装类。相似的，编译器也可以把一个对象拆箱为内置类型。

![Java Number类](https://www.runoob.com/wp-content/uploads/2013/12/OOP_WrapperClass.png)

```java
public class Hello {  
    public static void main(String[] args) {  
        Integer x = 5;
        x =  x + 10;
        System.out.println(x); 
    }  
}
```

### Math类

> Java 的 Math 包含了用于执行基本数学运算的属性和方法，如初等指数、对数、平方根和三角函数。
>
> Math 的方法都被定义为 static 形式，通过 Math 类可以在主函数中直接调用。

```java
public class Hello {  
    public static void main(String[] args) {  
        System.out.println("90 度的正弦值：" + Math.sin(Math.PI/2));  
        System.out.println("0度的余弦值：" + Math.cos(0));  
        System.out.println("60度的正切值：" + Math.tan(Math.PI/3));  
        System.out.println("1的反正切值： " + Math.atan(1));  
        System.out.println("π/2的角度值：" + Math.toDegrees(Math.PI/2));  
        System.out.println(Math.PI);  
    }  
}
```

### String

> String 类是不可改变的，所以你一旦创建了 String 对象，那它的值就无法改变了
>
> 如果需要对字符串做很多修改，那么应该选择使用 **StringBuffer** & **StringBuilder** 类

### StringBuffer 和 StringBuilder

> 和 String 类不同的是，StringBuffer 和 StringBuilder 类的对象能够被多次的修改，并且不产生新的未使用对象。
>
> StringBuilder 类在 Java 5 中被提出，它和 StringBuffer 之间的最大不同在于 StringBuilder 的方法不是线程安全的（不能同步访问）。
>
> 由于 StringBuilder 相较于 StringBuffer 有速度优势，所以多数情况下建议使用 StringBuilder 类。
>
> 然而在应用程序要求线程安全的情况下，则必须使用 StringBuffer 类

<img src="https://gitee.com/dexport/blog-image/raw/master/img/202202240008990.png" style="zoom:67%;" />

```java
public class RunoobTest{
    public static void main(String args[]){
        StringBuilder sb = new StringBuilder(10);
        sb.append("Runoob..");
        System.out.println(sb);  
        sb.append("!");
        System.out.println(sb);
      
      	StringBuffer sBuffer = new StringBuffer("菜鸟教程官网：");
    		sBuffer.append("www");
    		sBuffer.append(".runoob");
    		sBuffer.append(".com");
    		System.out.println(sBuffer);  
    }
}
```

### 数组

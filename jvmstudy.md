#JVM学习
##引言
###什么是JVM
####定义
Java virtual Machine-java程序运行环境(java二进制字节码的运行环境)
####好处
+ 一次编写,到处运行
+ 自动内存管理,立即回收功能
+ 数组下表越界的检查(java会抛异常,其他没有这种机制的语言,会覆盖其他的内存)
+ 多态(多态是面向对象的基石,jvm实现了多态)
####比较
jvm jre jdk  
他们的关系是:jdk>>jre>>jvm
![Image text](https://raw.githubusercontent.com/laozhang6783445/github/master/jvmguanxi.jpg)
###学习JVM有什么用
+ 面试
+ 理解底层的实现原理(想往长远发展,就得学这个)
+ 中高级程序员的必备技能
###常见的JVM
我们是以hotspot来说得
###学习路线
![Image text](https://raw.githubusercontent.com/laozhang6783445/github/master/jvmnvt.jpg)
##内存结构
###1. 程序程序计数器
####1.1 定义
####1.2 作用
记住下一条指令的执行地址
二进制字节码(jvm指令)>>解释器>>机器码>>CPU  
```java
public class Test{
    public static void main(String[] args){
        System.out.println("Hello World!");
    }
}
```
在物理上是由寄存器实现的,寄存器是cpu中读取最快的.  
+ 程序计数器是线程私有的
+ JVM规范找那个唯一规定程序计数器部分是不会存在内存溢出的
###2.虚拟机栈
####2.1 栈的数据结构
+ 特点:先进后出
####2.2 定义
Java Virtual Machine Stacks(Java 虚拟机栈)
+ 线程运行时需要的内存空间
+ 一个栈是有多个栈帧组成的
+ 一个栈帧代表着一次方法的调用
+ 栈帧就是方法运行时需要的内存
+ 方法有参数,局部变量,返回的地址
+ 一个栈内可能存在多个栈帧(递归就会出现这样的东西)
+ 每个线程只能有一个活动栈帧,对应着当前正在执行的那个方法(栈顶部正在执行的方法)
```java
public class Demo1_1 {
    public static void main(String[] args){
        method1();
    }

    public static void method1(){
        method2(1,2);
    }

    public static int method2(int a,int b){
        int c=a+b;
        return c;
    }
}
```
####2.2 问题解析
+ 垃圾回收是否涉及栈内存?  
答:不会 
+ 栈内存是不是分配的越大越好?  
答:不是,栈内存越大,能够创建的线程数就越小,它们成反比关系.
+ 方法内的局部变量是否线程安全?  
答:
```java
public class Demo2 {
    public static void main(String[] args){
        for(int i=0;i<100;i++){
            Thread t=new Thread("第"+i+"个"){
                @Override
                public void run(){
                    Demo2.m1();
                }
                
            };
            t.start();
        }
    }
    
    
    public static void m1(){
        int i=0;
        for(int j=0;j<5000;j++){
            System.out.println(i++);
        }
    }
}
```
```java
public class Demo3 {
    public static void main(String[] args){
    }
    /**
     * 局部变量是安全的
     */
    public static void m1(){
        StringBuilder sb=new StringBuilder();
        sb.append(1);
        sb.append(2);
        sb.append(3);
        System.out.println(sb.toString());
    }

    /**
     * 局部变量不是安全的
     * @param sb
     */
    public static void m2(StringBuilder sb){
        sb.append(1);
        sb.append(2);
        sb.append(3);
        System.out.println(sb.toString());
    }

    /**
     * 线程不是安全的
     * @return
     */
    public static StringBuilder m3(){
        StringBuilder sb=new StringBuilder("");
        sb.append(1);
        sb.append(2);
        sb.append(3);
        return sb;
    }
}
``` 









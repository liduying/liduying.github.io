---
layout: post
title: java static 静态变量
no-post-nav: true
category: java
tags: [java]
keywords: static
---

一、static代表着什么

&ensp;&ensp;&ensp;在Java中并不存在全局变量的概念，但是我们可以通过static来实现一个“伪全局”的概念，在Java中static表示“全局”或者“静态”的意思，用来修饰成员变量和成员方法，当然也可以修饰代码块。

&ensp;&ensp;&ensp;Java把内存分为栈内存和堆内存，其中栈内存用来存放一些基本类型的变量、数组和对象的引用，堆内存主要存放一些对象。在JVM加载一个类的时候，若该类存在static修饰的成员变量和成员方法，则会为这些成员变量和成员方法在固定的位置开辟一个固定大小的内存区域（只要这个类被加载，Java虚拟机就能根据类名在运行时数据区的方法区内定找到他们），有了这些“固定”的特性，那么JVM就可以非常方便地访问他们。同时如果静态的成员变量和成员方法不出作用域的话，它们的句柄都会保持不变。同时static所蕴含“静态”的概念表示着它是不可恢复的，即在那个地方，你修改了，他是不会变回原样的，你清理了，他就不会回来了。

&ensp;&ensp;&ensp;同时被static修饰的成员变量和成员方法是独立于该类的，它不依赖于某个特定的实例变量，也就是说它被该类的所有实例共享。所有实例的引用都指向同一个地方，任何一个实例对其的修改都会导致其他实例的变化。


```java
        public class User {
            private static int userNumber  = 0 ;
            
            public User(){
                userNumber ++;
            }
            
            public static void main(String[] args) {
                User user1 = new User();
                User user2 = new User();
                
                System.out.println("user1 userNumber：" + User.userNumber);
                System.out.println("user2 userNumber：" + User.userNumber);
            }
        }    
```
Output:
user1 userNumber：2  
user2 userNumber：2  

二、怎么使用static

&ensp;&ensp;&ensp;static可以用于修饰成员变量和成员方法，我们将其称之为静态变量和静态方法，直接通过类名来进行访问。

      ClassName.propertyName

      ClassName.methodName(……)

&ensp;&ensp;&ensp;static修饰的代码块表示静态代码块，当JVM装载类的时候，就会执行这块代码，其用处非常大。

1、static变量

&ensp;&ensp;&ensp;static修饰的变量我们称之为静态变量，没有用static修饰的变量称之为实例变量，他们两者的区别是：

&ensp;&ensp;&ensp;静态变量是随着类加载时被完成初始化的，它在内存中仅有一个，且JVM也只会为它分配一次内存，同时类所有的实例都共享静态变量，可以直接通过类名来访问它。但是实例变量则不同，它是伴随着实例的，每创建一个实例就会产生一个实例变量，它与该实例同生共死。

&ensp;&ensp;&ensp;所以我们一般在这两种情况下使用静态变量：对象之间共享数据、访问方便。

```java
    public class TestStatic {
        
        public static int count = 0;
        
        public static void main(String[] args){
            TestStatic test1=new TestStatic();
            System.out.println(test1.count);
            TestStatic test2=new TestStatic();
            test2.count++;
            System.out.println(test1.count+" "+test2.count+" "+TestStatic.count);
        }
    }
```
输出结果：

0  
1   
1  
1
 
 &ensp;&ensp;&ensp;可见，static变量并不是所在类的某个具体对象所有，而是该类的所有对象所共有的，静态变量既能被对象调用，也能直接拿类来调用。

 2、static方法

&ensp;&ensp;&ensp;static方法一般称作静态方法，由于静态方法不依赖于任何对象就可以进行访问，因此对于静态方法来说，是没有this的，因为它不依附于任何对象，既然都没有对象，就谈不上this了。并且由于这个特性，在静态方法中不能访问类的非静态成员变量和非静态成员方法，因为非静态成员方法/变量都是必须依赖具体的对象才能够被调用。但是要注意的是，虽然在静态方法中不能访问非静态成员方法和非静态成员变量，但是在非静态成员方法中是可以访问静态成员方法/变量的。
因为static方法独立于任何实例，因此static方法必须被实现，而不能是抽象的abstract。
 
总结一下，对于静态方法需要注意以下几点：  
（1）它们仅能调用其他的static 方法。  
（2）它们只能访问static数据。  
（3）它们不能以任何方式引用this 或super。  

举个简单的例子：

&ensp;&ensp;&ensp;在上面的代码中，由于print2方法是独立于对象存在的，可以直接用过类名调用。假如说可以在静态方法中访问非静态方法/变量的话，那么如果在main方法中有下面一条语句：

    MyObject.print2();

&ensp;&ensp;&ensp;此时对象都没有，str2根本就不存在，所以就会产生矛盾了。同样对于方法也是一样，由于你无法预知在print1方法中是否访问了非静态成员变量，所以也禁止在静态成员方法中访问非静态成员方法。

&ensp;&ensp;&ensp;而对于非静态成员方法，它访问静态成员方法/变量显然是毫无限制的。

&ensp;&ensp;&ensp;因此，如果说想在不创建对象的情况下调用某个方法，就可以将这个方法设置为static。我们最常见的static方法就是main方法，至于为什么main方法必须是static的，现在就很清楚了。因为程序在执行main方法的时候没有创建任何对象，因此只有通过类名来访问。

&ensp;&ensp;&ensp;另外记住，即使没有显示地声明为static，类的构造器实际上也是静态方法。

3、static代码块

&ensp;&ensp;&ensp;static关键字还有一个比较关键的作用就是 用来形成静态代码块以优化程序性能。static块可以置于类中的任何地方，类中可以有多个static块。在类初次被加载的时候，会按照static块的顺序来执行每个static块，并且只会执行一次。

&ensp;&ensp;&ensp;为什么说static块可以用来优化程序性能，是因为它的特性:只会在类加载的时候执行一次。下面看个例子:


```java           
            class Person{ 
                private Date birthDate; 
                
                public Person(Date birthDate) { 
                    this.birthDate = birthDate; 
                } 
                
                boolean isBornBoomer() { 
                    Date startDate = Date.valueOf("1946"); 
                    Date endDate = Date.valueOf("1964"); 
                    return birthDate.compareTo(startDate)>=0 && birthDate.compareTo(endDate) < 0; 
                } 
            } 
```
&ensp;&ensp;&ensp;isBornBoomer是用来判断这个人是否是1946-1964年出生的，而每次isBornBoomer被调用的时候，都会生成startDate和birthDate两个对象，造成了空间浪费，如果改成这样效率会更好：

```java
    class Person{ 
        private Date birthDate; 
        private static Date startDate,endDate; 
        static{ 
            startDate = Date.valueOf("1946"); 
            endDate = Date.valueOf("1964"); 
        } 
        
        public Person(Date birthDate) { 
            this.birthDate = birthDate; 
        } 
        
        boolean isBornBoomer() { 
            return birthDate.compareTo(startDate)>=0 && birthDate.compareTo(endDate) < 0; 
        } 
    }
```
&ensp;&ensp;&ensp;因此，很多时候会将一些只需要进行一次的初始化操作都放在static代码块中进行。

三、static关键字的误区

1、static关键字会改变类中成员的访问权限吗？

&ensp;&ensp;&ensp;有些初学的朋友会将java中的static与C/C++中的static关键字的功能混淆了。在这里只需要记住一点：与C/C++中的static不同，Java中的static关键字不会影响到变量或者方法的作用域。在Java中能够影响到访问权限的只有private、public、protected（包括包访问权限）这几个关键字。看下面的例子就明白了：



&ensp;&ensp;&ensp;提示错误"Person.age 不可视"，这说明static关键字并不会改变变量和方法的访问权限。

2、能通过this访问静态成员变量吗？

&ensp;&ensp;&ensp;虽然对于静态方法来说没有this，那么在非静态方法中能够通过this访问静态成员变量吗？先看下面的一个例子，这段代码输出的结果是什么？

```java
    public class Main {　　 
        static int value = 33;  
    
        public static void main(String[] args) throws Exception{ 
            new Main().printValue(); 
        } 
    
        private void printValue(){ 
            int value = 3; 
            System.out.println(this.value); 
        } 
    } 
```
输出结果：33

&ensp;&ensp;&ensp;这里面主要考查对this和static的理解。this代表什么？this代表当前对象，那么通过new Main()来调用printValue的话，当前对象就是通过new Main()生成的对象。而static变量是被对象所享有的，因此在printValue中的this.value的值毫无疑问是33。在printValue方法内部的value是局部变量，根本不可能与this关联，所以输出结果是33。在这里永远要记住一点：静态成员变量虽然独立于对象，但是不代表不可以通过对象去访问，所有的静态方法和静态变量都可以通过对象访问（只要访问权限足够）。

3、static能作用于局部变量么？

&ensp;&ensp;&ensp;在C/C++中static是可以作用域局部变量的，但是在Java中切记：static是不允许用来修饰局部变量。不要问为什么，这是Java语法的规定。

4、static和final一块用表示什么？  
&ensp;&ensp;&ensp;static final用来修饰成员变量和成员方法，可简单理解为“全局常量”！ 
      对于变量，表示一旦给值就不可修改，并且通过类名可以访问。 
      对于方法，表示不可覆盖，并且可以通过类名直接访问。

四、常见的笔试面试题

下面列举一些面试笔试中经常遇到的关于static关键字的题目

1、下面这段代码的输出结果是什么？

```java
    public class Test extends Base{ 
    
        static{ 
            System.out.println("test static"); 
        } 
        
        public Test(){ 
            System.out.println("test constructor"); 
        } 
        
        public static void main(String[] args) { 
            new Test(); 
        } 
    } 
    
    class Base{ 
        
        static{ 
            System.out.println("base static"); 
        } 
        
        public Base(){ 
            System.out.println("base constructor"); 
        } 
    } 
```
输出结果：

base static  
test static  
base constructor  
test constructor  

&ensp;&ensp;&ensp;至于为什么是这个结果，我们先不讨论，先来想一下这段代码具体的执行过程，在执行开始，先要寻找到main方法，因为main方法是程序的入口，但是在执行main方法之前，必须先加载Test类，而在加载Test类的时候发现Test类继承自Base类，因此会转去先加载Base类，在加载Base类的时候，发现有static块，便执行了static块。在Base类加载完成之后，便继续加载Test类，然后发现Test类中也有static块，便执行static块。在加载完所需的类之后，便开始执行main方法。在main方法中执行new Test()的时候会先调用父类的构造器，然后再调用自身的构造器。因此，便出现了上面的输出结果。

2、这段代码的输出结果是什么？

```java
    public class Test { 
        Person person = new Person("Test"); 
        static{ 
            System.out.println("test static"); 
        } 
        
        public Test() { 
            System.out.println("test constructor"); 
        } 
        
        public static void main(String[] args) { 
            new MyClass(); 
        } 
    } 
  
    class Person{ 
        static{ 
            System.out.println("person static"); 
        } 
        public Person(String str) { 
            System.out.println("person "+str); 
        } 
    } 
    
    
    class MyClass extends Test { 
        Person person = new Person("MyClass"); 
        static{ 
            System.out.println("myclass static"); 
        } 
        
        public MyClass() { 
            System.out.println("myclass constructor"); 
        } 
    } 
```
输出结果：


test static  
myclass static  
person static  
person Test  
test constructor  
person MyClass  
myclass constructor  

&ensp;&ensp;&ensp;类似地，我们还是来想一下这段代码的具体执行过程。首先加载Test类，因此会执行Test类中的static块。接着执行new MyClass()，而MyClass类还没有被加载，因此需要加载MyClass类。在加载MyClass类的时候，发现MyClass类继承自Test类，但是由于Test类已经被加载了，所以只需要加载MyClass类，那么就会执行MyClass类的中的static块。在加载完之后，就通过构造器来生成对象。而在生成对象的时候，必须先初始化父类的成员变量，因此会执行Test中的Person person = new Person()，而Person类还没有被加载过，因此会先加载Person类并执行Person类中的static块，接着执行父类的构造器，完成了父类的初始化，然后就来初始化自身了，因此会接着执行MyClass中的Person person = new Person()，最后执行MyClass的构造器。

3、这段代码的输出结果是什么？

```java
    public class Test {
        
        static{
            System.out.println("test static 1");
        }
        public static void main(String[] args) {
            
        }
        
        static{
            System.out.println("test static 2");
        }
    }
```
运行结果：

test static 1  
test static 2  

&ensp;&ensp;&ensp;虽然在main方法中没有任何语句，但是还是会输出，原因上面已经讲述过了。另外，static块可以出现类中的任何地方（只要不是方法内部，记住，任何方法内部都不行），并且执行是按照static块的顺序执行的。

4、静态代码块的初始化顺序

```java
    class Parent {
        static String name = "hello";
        {
            System.out.println("parent block");
        }
        static {
            System.out.println("parent static block");
        }

        public Parent() {
            System.out.println("parent constructor");
        }
    }

    class Child extends Parent {
        static String childName = "hello";
        {
            System.out.println("child block");
        }
        static {
            System.out.println("child static block");
        }

        public Child() {
            System.out.println("child constructor");
        }
    }

    public class TestStatic {

        public static void main(String[] args) {
            new Child();// 语句(*)
        }
    }
```
问题：当执行完语句(*)时，打印结果是什么顺序？为什么？

输出结果：

parent static block  
child static block  
parent block  
parent constructor  
child block  
child constructor  

&ensp;&ensp;&ensp;分析:当执行new Child()时，它首先去看父类里面有没有静态代码块，如果有，它先去执行父类里面静态代码块里面的内容，当父类的静态代码块里面的内容执行完毕之后，接着去执行子类(自己这个类)里面的静态代码块，当子类的静态代码块执行完毕之后，它接着又去看父类有没有非静态代码块，如果有就执行父类的非静态代码块，父类的非静态代码块执行完毕，接着执行父类的构造方法；父类的构造方法执行完毕之后，它接着去看子类有没有非静态代码块，如果有就执行子类的非静态代码块。子类的非静态代码块执行完毕再去执行子类的构造方法，这个就是一个对象的初始化顺序。

总结:  
&ensp;&ensp;&ensp;对象的初始化顺序:首先执行父类静态的内容，父类静态的内容执行完毕后，接着去执行子类的静态的内容，当子类的静态内容执行完毕之后，再去看父类有没有非静态代码块，如果有就执行父类的非静态代码块，父类的非静态代码块执行完毕，接着执行父类的构造方法；父类的构造方法执行完毕之后，它接着去看子类有没有非静态代码块，如果有就执行子类的非静态代码块。子类的非静态代码块执行完毕再去执行子类的构造方法。总之一句话，静态代码块内容先执行，接着执行父类非静态代码块和构造方法，然后执行子类非静态代码块和构造方法。

注意:子类的构造方法，不管这个构造方法带不带参数，默认的它都会先去寻找父类的不带参数的构造方法。如果父类没有不带参数的构造方法，那么子类必须用supper关键子来调用父类带参数的构造方法，否则编译不能通过。

 
---
title: UML类图
date: 2017-01-15 23:49:06
category:
     - UML
tags:
    - UML
---
## 什么是UML类图
UML类图应该是UML13种图形中最常用的，它属于静态图大类，是一种面向对象式的图形，用来展示一组对象、接口它们之间的关系。

## UML类图的画法
### 对象、接口的表示
- 类 （类中可以有内部类）
  ![class](https://github.com/tks6754/Photos01/raw/master/uml-class.png)
  ![class](https://github.com/tks6754/Photos01/raw/master/uml-inner%20class.png)

- 抽象类 抽象类一般加　“abstract” 进行说明。
  ![abstract](https://github.com/tks6754/Photos01/raw/master/uml-abstract%20class.png)

- 接口 接口有两种方式，文字说明，也有使用“棒棒糖”符号标识，“小棒棒糖”表示很常见。
  ![interface](https://github.com/tks6754/Photos01/raw/master/uml-interface.png)
  ![interface](https://github.com/tks6754/Photos01/raw/master/uml-interface2.png)

<!-- more -->

画法总结：
1. 类、接口的名称放在第一行，抽象类和接口需要用文字或符号说明
2. 第二行是属性，具体格式如下
    > 可见性　属性名:类型 [= 默认值]
3. 第三行是方法声明，格式如下
    > 可见性　方法名（入参）[：返回值类型]
4. 可能还有内部类

可见性表示有三种
```
+ public
- private
# protected
```

## 关系表示
### 泛化
泛化（Generalization）关系也就是继承关系，描述父类与子类之间的关系。
> 表示方法：带空心三角形的实线

如图，父类是动物类，具有age属性和walk方法；子类Bird和Dog继承动物类，并各自新增了属性和自己独有的方法。
![Generalization](https://github.com/tks6754/Photos01/raw/master/uml-fanhua.png)

代码体现
```
//父类
public class Animal{
  public int age;
  public void walk(){
    ...
  }
}
//子类
public class Bird extends Animal{
  public int wins;
  public void fly(){
    ...
  }
}
//子类
public class Dog extends Animal{
  public int leg;
  public void shout(){
    ...
  }
}
```

### 实现
实现(Realization)是指实现接口，接口通常只有方法声明，实现接口需要在类中实现方法的具体操作。
> 表示方法：带空心三角形的虚线

如图，接口Person中有eat方法，类Asian和American实现了接口
![Realization](https://github.com/tks6754/Photos01/raw/master/uml-shixian.png)

代码体现
```
//接口
public interface Person{
  void est();
}
//亚洲人
public class Asian implements Person{
  public void eat(){
    ...
  }
}
//美洲人
public class American implements Person{
  public void eat(){
    ...
  }
}
```

### 依赖
依赖(Dependency)表示一个事物使用另一个事物，被使用事物的改变会影响到使用它的事物。
代码中常表现为在方法中将被使用的类作为参数传入；或作为类方法中的局部变量；或在类中使用另一个类中的静态方法。

> 表示方法：带箭头的虚线

如图，Oil类被Car类的speed方法使用，作为方法参数传入。
![Dependency](https://github.com/tks6754/Photos01/raw/master/uml-yilai.png)

代码体现
```
public class Oil{
  private String number;
}
public class Car{
  private String type;
  public double speed(Oil oil){
    ...
  }
}
```

### 关联
关联就名字的意思，表示两个类之间相互联系、有关联。
代码中通常表现为一个类的对象作为另一个类的成员变量。
> 表示方法：带箭头或不带箭头的实线

关联关系表现为多种形式：单向关联、双向关联、自关联、多重性关联，还有单独分出来的聚合关系和组合关系。

**单向关联**
单向关联表示联系是单方向的。类的对象作为另一个类的成员变量存在。
> 表示方法：带箭头的实线

如图，电脑类拥有成员变量cpu类。
![](https://github.com/tks6754/Photos01/raw/master/uml-guanliandan.png)

代码体现
```
public class CPU{}
public class Computer{
  private CPU cpu;
  ...
}
```

**双向关联**
双向关联意味着两者相互联系，这是最普遍的联系。两个类的对象相互作为类的成员变量。
> 表示方法：不带箭头的实线，可以在实线附近标明关系

如图，父亲类拥有成员变量儿子类；儿子类拥有成员变量父亲类。
![](https://github.com/tks6754/Photos01/raw/master/uml-guanlianshuang.png)

代码体现
```
public class Father{
  private Son son;
  ...
}
public class Son{
  private Father father;
  ...
}
```

**自关联**
自关联表示自身的关联，常见于类的对象重复出现。一个类的对象作为自身的成员变量。
> 表示方法：带箭头的实线，指向自身。

如图，Node类对象作为自身的成员变量。
![](https://github.com/tks6754/Photos01/raw/master/uml-guanlianzishen.png)

代码体现
```
public class Node{
  private Node node;
  ...
}
```

**多重性关联**
多重性关联就是在关联加上数量信息，表示两个对象数量上的关系。类中成员变量是数组或其他形式，以表明数量信息。
> 表示方法：在实线上写明数量关系

数量关系表
| 表示方式 | 含义     |
| :------------- | :------------- |
|  1..1      | 两个类对象是１对１的关系      |
| 1..*       | １对１～ｎ      |
| 0..1       | ０对１      |
|  0..*      |  ０对０～ｎ     |
| m..1n       |  >=m对<=n （m<=n）     |

如图，一个员工只能与一家公司关联，而一家公司可以与多个员工关联。
![](https://github.com/tks6754/Photos01/raw/master/uml-guanlianduochong.png)

代码体现
```
public class Staff{
  ...
}
public calss Company{
  private Staff[] staff;
  ...
}
```

## 聚合关系
聚合关系是一种特殊的关联关系，除了表示部分与整体是有关系的，还表示部分组成整体，是整体的一部分，但部分又是可以单独存在的对象。
代码中体现为在整体类的构造方法中，将部分类的对象作为参数传入；或通过setter()方法注入。
> 表示方法：空心菱形带箭头的实线

如图，车轮作为汽车的一部分，而且其自身也可以独立于汽车存在
![](https://github.com/tks6754/Photos01/raw/master/uml-juhe.png)

代码体现
```
public class Wheel{
  ...
}
public class Car{
  private Wheel wheel;
  public Car(Wheel wheel){
    this.wheel = wheel;
  }
  public void setWheel(Wheel wheel){
    this.wheel = wheel;
  }
}
```

## 组合关系
组合关系也是一种特殊的关联关系，表示部分与整体是有关系的，并且部分组成整体，但部分不能作为独立的存在（整体不存在时，部分就不能得以存在）。
代码中表现为整体类的构造方法中直接实例化部分类。
> 表示方法：实心菱形带箭头的实线

如图，腿作为人体的一部分，但腿不能独立存在。
![](https://github.com/tks6754/Photos01/raw/master/uml-zuhe.png)

代码体现
```
public class Leg{
  ...
}
public class Body{
  private Leg leg;
  public Body(){
      leg = new Leg();
  }
}
```

## 聚合与组合比较
网络上看到一个很形象的例子
大雁群－大雁－翅膀，大雁群由大雁组成，大雁可以独立存在，大雁有两只翅膀，但翅膀不能独立存在。
![](https://github.com/tks6754/Photos01/raw/master/uml-jubijaozu.png)

# Immutable & Mutable

[https://blog.csdn.net/leixingbang1989/article/details/48463755](https://blog.csdn.net/leixingbang1989/article/details/48463755)

## 不可变对象（immutable objects）

immutable Objects就是那些一旦被创建，它们的状态就不能被改变的Objects，每次对他们的改变都是产生了新的immutable的对象，而mutable Objects就是那些创建后，状态可以被改变的Objects.

举个例子：String和StringBuilder，String是immutable的，每次对于String对象的修改都将产生一个新的String对象，而原来的对象保持不变，而StringBuilder是mutable，因为每次对于它的对象的修改都作用于该对象本身，并没有产生新的对象。

但有的时候String的immutable特性也会引起安全问题，这就是[密码应该存放在字符数组中而不是String中](http://my.oschina.net/jasonultimate/blog/166968)的原因！

immutable objects 比传统的mutable对象在多线程应用中更具有优势，它不仅能够保证对象的状态不被改变，而且还可以不使用锁机制就能被其他线程共享。

## 如何在Java中写出Immutable的类？

要写出这样的类，需要遵循以下几个原则：

1）immutable对象的状态在创建之后就不能发生改变，任何对它的改变都应该产生一个新的对象。

2）Immutable类的所有的属性都应该是final的。

3）对象必须被正确的创建，比如：对象引用在对象创建过程中不能泄露\(leak\)。

4）对象应该是final的，以此来限制子类继承父类，以避免子类改变了父类的immutable特性。

5）如果类中包含mutable类对象，那么返回给客户端的时候，返回该对象的一个拷贝，而不是该对象本身（该条可以归为第一条中的一个特例）

当然不完全遵守上面的原则也能够创建immutable的类，比如String的hashcode就不是final的，但它能保证每次调用它的值都是一致的，无论你多少次计算这个值，它都是一致的，因为这些值的是通过计算final的属性得来的！

另外，如果你的Java类中存在很多可选的和强制性的字段，你也可以使用建造者模式来创建一个immutable的类。

```java
public final class Contacts {
 
    private final String name;
    private final String mobile;
 
    public Contacts(String name, String mobile) {
        this.name = name;
        this.mobile = mobile;
    }
   
    public String getName(){
        return name;
    }
   
    public String getMobile(){
        return mobile;
    }
}

```

**我们为类添加了final修饰，从而避免因为继承和多态引起的immutable风险。**

上面是最简单的一种实现immutable类的方式，可以看到它的所有属性都是final的。

有时候你要实现的immutable类中可能包含mutable的类，比如java.util.Date,尽管你将其设置成了final的，但是它的值还是可以被修改的，为了避免这个问题，我们建议返回给用户该对象的一个拷贝，这也是Java的最佳实践之一。下面是一个创建包含mutable类对象的immutable类的例子：

```java
public final class ImmutableReminder{
    private final Date remindingDate;
   
    public ImmutableReminder (Date remindingDate) {
        if(remindingDate.getTime() < System.currentTimeMillis()){
            throw new IllegalArgumentException("Can not set reminder” +
                        “ for past time: " + remindingDate);
        }
        this.remindingDate = new Date(remindingDate.getTime());
    }
   
    public Date getRemindingDate() {
        return (Date) remindingDate.clone();
    }
}

```

上面的getRemindingDate\(\)方法可以看到，返回给用户的是类中的remindingDate属性的一个拷贝，这样的话如果别人通过getRemindingDate\(\)方法获得了一个Date对象，然后修改了这个Date对象的值，那么这个值的修改将不会导致ImmutableReminder类对象中remindingDate值的修改。

使用Immutable类的好处：  
1）Immutable对象是线程安全的，可以不用被synchronize就在并发环境中共享

2）Immutable对象简化了程序开发，因为它无需使用额外的锁机制就可以在线程间共享

3）Immutable对象提高了程序的性能，因为它减少了synchroinzed的使用

4）Immutable对象是可以被重复使用的，你可以将它们缓存起来重复使用，就像字符串字面量和整型数字一样。你可以使用静态工厂方法来提供类似于valueOf（）这样的方法，它可以从缓存中返回一个已经存在的Immutable对象，而不是重新创建一个。

immutable也有一个缺点就是会制造大量垃圾，由于他们不能被重用而且对于它们的使用就是”用“然后”扔“，字符串就是一个典型的例子，它会创造很多的垃圾，给垃圾收集带来很大的麻烦。当然这只是个极端的例子，合理的使用immutable对象会创造很大的价值。


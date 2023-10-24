---
title: Java集合框架
date: 2023-10-24 18:54:37
categories: Java
---

 java中的集合包括三大类，它们是Set、List和Map。它们都处于java.util包中，Set、List和Map都是接口。 本文参考源码为 jdk1.7u79。开始之前，先简单描述下类之间的关系，方便后续看类图。

| 集合架构   | 特性                                                         |
| ---------- | ------------------------------------------------------------ |
| Collection | 接口存储一组不唯一，无序的对象                               |
| List       | 接口存储一组不唯一，有序（索引顺序）的对象                   |
| Set        | 接口存储一组唯一，无序的对象                                 |
| Map        | 接口存储一组键值对象，提供key到value的映射Key 唯一 无序；value  不唯一 无序 |

```
继承关系    —▷    实线 + 空心三角形    鸟 —▷ 动物；鸟继承动物
实现接口    •••▷    虚线 + 空心三角形    大雁 •••▷ 飞翔；大雁实现了飞翔接口
实现接口    —○    棒棒糖表示法    唐老鸭 —○ 讲人话；唐老鸭实现讲人话接口
关联关系    —>    实线剪头    企鹅 —> 气候；企鹅需要‘知道’气候的变化
依赖关系    •••>    虚线剪头    动物 •••> 氧气；动物依赖于氧气
聚合关系    ◇—>    空心菱形 + 实线剪头    大雁 ◇—> 翅膀；部分和整体的关系
合成关系    ◆—>    实心菱形 + 实线剪头    大雁 ◆—> 雁群；A包含B，但B不是A的一部分
```

## 接口继承树

// Todo: 待补充

## **List集合**

List：有序  不唯一（可重复）

ArrayList：在内存中分配连续的空间，实现了长度可变的数组

* 优点：遍历元素和随机访问元素的效率比较高

* 缺点：添加和删除需大量移动元素效率低，按照内容查询效率低

LinkedList：采用双向链表存储方式。

* 缺点：遍历和随机访问元素效率低下

* 优点：插入、删除元素效率比较高（但是前提也是必须先低效率查询才可。如果插入删除发生在头尾可以减少查询次数）

### ArrayList

#### 源码理解

底层实现：一个长度可以动态增长的Object数组 

扩容： 容量不足时进行扩容，默认扩容50%。如果扩容50%还不足容纳新增元素，就扩容为能容纳新增元素的最小数量。

遍历： ArrayList中提供了一个内部类Itr，实现了Iterator接口，实现对集合元素的遍历

```java
public class TestArrayList {
    public static void main(String[] args) {
        //创建一个集合对象
        //ArrayList list = new ArrayList();
        //实例1
        List list = new ArrayList();
        //泛型
        //List<Integer> list = new ArrayList<Integer>();
        //添加元素
        list.add(80);//末尾添加
        list.add(90);//自动填装 int ---- Integer
        list.add(80);

        list.add(1, 100);//指定索引添加，底层发生大量元素后移，并且可能扩容

        //元素的数量--list.size()
        System.out.println(list.size());
        //获取指定索引的元素--list.get(1)
        System.out.println(list.get(1));
        //遍历元素
        System.out.println("==================");
        System.out.println(list.toString());
        //方法1：for循环
        for(int i = 0; i < list.size(); ++i){
            int elem = (int) list.get(i);
            System.out.println(i + "--->"+ elem);
        }
        System.out.println("==================");

        //方法2：for-each循环
        for(Object elem:list){
            System.out.println(elem);
        }
        System.out.println("==================");

        //方法3：Iterator
        Iterator it = list.iterator();
        while(it.hasNext()){
            int elem = (int)it.next();
            System.out.println(elem);
        }
        System.out.println("==================");

        //方法4：lambda表达式—+流式编程（JDK1。8）
        //list.forEach((i)->System.out.println(i));
        list.forEach(System.out::println);
    }
}
```

 在代码实例1，可知添加元素时可以加入任何类型--->不安全；获取元素时需要强制类型转换--->繁琐；为了实现安全和简单，通过使用泛型gerneic。

### LinkedList

#### 对比ArrayList

问题1：将ArrayList替换成LinkedList之后，不变的是什么？

（1）运算结果没有变

（2）执行的功能代码没有变

问题2：将ArrayList替换成LinkedList之后，变化的是什么？

（1）底层的结构变了

​		ArrayList：数组 	LinkedList：双向链表

具体的执行过程变化了 list.add(2,99)

​		ArrayList：大量的后移元素 

LinkedList：不需要大量的移动元素，修改节点的指向即可

问题3：到底是使用ArrayList还是LinkedList

（1）根据使用场合而定

（2）大量的根据索引查询的操作，大量的遍历操作（按照索引0--n-1逐个查询一般），建议使用ArrayList

（3）如果存在较多的添加、删除操作，建议使用LinkedList

问题4：LinkedList增加了哪些方法

（1）增加了对添加、删除、获取首尾元素的方法

（2）addFirst()、addLast()、removeFirst()、removeLast()、getFirst()、getLast()、

 #### 源码理解

1. 底层实现：双向链表
2. LinkedList实现了Deque接口，所以除了可以作为线性表来使用外，还可以当做队列和栈来使用
3. 链表节点

```java
    private static class Node<E> {
        E item;
        Node<E> next;
        Node<E> prev;

        Node(Node<E> prev, E element, Node<E> next) {
            this.item = element;
            this.next = next;
            this.prev = prev;
        }
    }
```



```java
public class TestLinkedList1 {
    public static void main(String[] args) {
        //LinkedList<Integer> list = new LinkedList();
        //ArrayList list = new ArrayList(100);
        List<Integer> list = new LinkedList();
        list.add(80);
        list.add(70);
        list.add(90);
        list.remove(2);
        //list.addAll()
        list.add(0,100);
        System.out.println(list.size());
        System.out.println(list.isEmpty());
        System.out.println(list.indexOf(780));
        System.out.println(list.contains(80));
        int elem = list.get(2);
        System.out.println(elem);
        System.out.println(list);
        Iterator it = list.iterator();

        //list.addFirst(60);
        list.add(0,60);
        //list.addLast(50);
        list.add(50);

    }
}
```

## Map

Map：存储的键值对映射关系，根据key可以找到value

HashMap: 采用Hashtable哈希表存储结构（神奇的结构）

* 优点：添加速度快  查询速度快 删除速度快

* 缺点：key无序

LinkedHashMap: 采用哈希表存储结构，同时使用链表维护次序

* key有序（添加顺序）

TreeMap: 采用二叉树（红黑树）的存储结构

* 优点：key有序  查询速度比List快（按照内容查询）

* 缺点：查询速度没有HashMap快

### 哈希表的原理

**引入哈希表**

1. 在无序数组中按照内容查找，效率低下，时间复杂度是O（n） 

2. 在有序数组中按照内容查找，可以使用折半查找，时间复杂度O（log2n）

3. 在二叉平衡树中按照内容查找，时间复杂度O（log2n）

在数组中按照索引查找，不进行比较和计数，直接计算得到，效率最高，时间复杂度O（1）

4. 哈希表：按照内容查找，能否也不进行比较，而是通过计算得到地址，实现类似数组按照索引查询的高效率呢O（1）

**哈希表的结构和特点**

hashtable 也叫散列表；特点：快  很快  神奇的快

结构：结构有多种。最流行、最容易理解：顺序表+链表

主结构：顺序表，每个顺序表的节点在单独引出一个链表

**哈希表是如何添加数据的**

1.  计算哈希码(调用hashCode(),结果是一个int值，整数的哈希码取自身即可)

2. 计算在哈希表中的存储位置  y=k(x)=x%11

 x:哈希码  k(x) 函数y：在哈希表中的存储位置

3.  存入哈希表

（1）情况1：一次添加成功

（2）情况2：多次添加成功（出现了冲突，调用equals()和对应链表的元素进行比较，比较到最后，结果都是false，创建新节点，存储数据，并加入链表末尾）

（3） 情况3：不添加（出现了冲突，调用equals()和对应链表的元素进行比较， 经过一次或者多次比较后，结果是true，表明重复，不添加）

结论1：哈希表添加数据快（3步即可，不考虑冲突）

结论2：唯一、无序

**哈希表是如何查询数据的**

  和添加数据的过程是相同的

（1）情况1：一次找到  23  86  76

（2）情况2：多次找到  67  56  78

（3）情况3：找不到  100 200

 结论1：哈希表查询数据快       

**hashCode和equals到底有什么神奇的作用**

hashCode():计算哈希码，是一个整数，根据哈希码可以计算出数据在哈希表中的存储位置

equals()：添加时出现了冲突，需要通过equals进行比较，判断是否相同；查询时也需要使用equals进行比较，判断是否相同  

**各种类型数据的哈希码应该如何获取 hashCode()**

int  取自身 看Integer的源码

1. double  3.14 3.15  3.145  6.567  9.87  取整不可以  看Double的源码

2. String java  oracle  java  将各个字符的编码值相加不可以

​     abc cba  bac  a:97  b:98  c:99

​     abc 1*97+2*98+3*99   

 cba 1*99+2*98+3*97

Student 先各个属性的哈希码，进行某些相加相乘的运算

​    int id	    String name      int age      double score;

### 如何减少冲突

1. 哈希表的长度和表中的记录数的比例--装填因子：

   如果Hash表的空间远远大于最后实际存储的记录个数，则造成了很大的空间浪费， 如果选取小了的话，则容易造成冲突。 在实际情况中，一般需要根据最终记录存储个数和关键字的分布特点来确定Hash表的大小。还有一种情况时可能事先不知道最终需要存储的记录个数，则需要动态维护Hash表的容量，此时可能需要重新计算Hash地址。

​    **装填因子=表中的记录数/哈希表的长度， 4/ 16  =0.25  8/ 16=0.5**

​    如果装填因子越小，表明表中还有很多的空单元，则添加发生冲突的可能性越小；而装填因子越大，则发生冲突的可能性就越大，在查找时所耗费的时间就越多。 有相关文献证明当装填因子在0.5左右时候，Hash性能能够达到最优。 

**因此，一般情况下，装填因子取经验值0.5**。

2. 哈希函数的选择

​     直接定址法   平方取中法  折叠法  **除留取余法（y = x%11）**

3. 处理冲突的方法

​      链地址法  开放地址法  再散列法  建立一个公共溢出区

## Set集合

Set：无序  唯一（不重复）

HashSet: 采用Hashtable哈希表存储结构（神奇的结构）

* 优点：添加速度快  查询速度快 删除速度快

* 缺点：无序

LinkedHashSet: 采用哈希表存储结构，同时使用链表维护次序

* 有序（添加顺序）

 TreeSet: 采用二叉树（红黑树）的存储结构

* 优点：有序  查询速度比List快（按照内容查询）

* 缺点：查询速度没有HashSet快

### 源码理解

#### **HashSet**

HashSet的底层使用的是HashMap，所以底层结构也是哈希表

HashSet的元素到HashMap中做key，value统一是同一个Object()

#### **TreeSet**

TreeSet的底层使用的是TreeMap，所以底层结构也是红黑树

TreeSet的元素e是作为TreeMap的key存在的，value统一为同一个 Object()

#### Comparator

内部比较器只能定义一个，一般将使用频率最高的比较规则定义为内部比较器的规则；外部比较器可以定义多个； 

注意1：对于外部比较器，如果使用次数较少，可以通过匿名内部类来实现。

注意2：需要比较的场合才需要实现内部比较器或者外部比较器，比如排序、比如TreeSet中数据的存储和查询，在HashSet、LinkedHashSet、ArrayList中存储元素，不需要实现内部比较器或者外部比较器。

## 总结

HashSet  哈希表  唯一  无序

LinkedHashSet  哈希表+链表  唯一 有序（添加顺序）

TreeSet  红黑树 一种二叉平衡树 唯一  有序（自然顺序）

List针对Collection增加了一些关于索引位置操作的方法 get(i) add(i,elem),remove(i),set(i,elem)

Set是无序的，不可能提供关于索引位置操作的方法，set针对Collection没有增加任何方法

List的遍历方式：for循环、for-each循环、Iterator迭代器、流式编程forEach

Set的遍历方式： for-each循环、Iterator迭代器、流式编程forEach

 ## 多线程下注意事项

### List

Vector 或者 CopyOnWriteArrayList 是两个线程安全的List实现。

ArrayList 不是线程安全的。若必须使用，则需要使用Collections.synchronizedList(List list)进行包装。

#### ArrayList

```java
private static void listNotSafe() {
    List<String> list=new ArrayList<>();
    for (int i = 1; i <= 30; i++) {
        new Thread(() -> {
            list.add(UUID.randomUUID().toString().substring(0, 8));
            System.out.println(Thread.currentThread().getName() + "\t" + list);
        }, String.valueOf(i)).start();
    }
}
/*
ArrayList线程不安全主要是由它的add()方法引起的.

/**
     * Appends the specified element to the end of this list.
     *
     * @param e element to be appended to this list
     * @return <tt>true</tt> (as specified by {@link Collection#add})
     */

		//ArrayList的add()源码
		//ArrayList的add()方法主要有两步，ensureCapacityInternal(size + 1);和elementData[size++] = e;

    public boolean add(E e) {
    	// 确保容量足够，不够则进行扩容
        ensureCapacityInternal(size + 1);  // Increments modCount!!
        // 将元素添加进列表的元素数组里面
        elementData[size++] = e;
        return true;
    }

/*
场景一：数组越界异常 ArrayIndexOutOfBoundsException
单线程下是完全没有问题的，但在多线程中，有可能出现一种情况：假设当前ArrayList的长度是9（源码中ArrayList的初始容量是10），线程A、B同时执行add()方法。当线程A执行ensureCapacityInternal(size + 1);时，因为size等于9，容量为10，线程A判断不需要扩容，此时CPU调度线程B执行，同样执行ensureCapacityInternal(size + 1);，此时size还是等于9（线程A没有改变size的值），线程B也判断不需要扩容，然后线程B继续执行elementData[size++] = e;将元素e放到elementData[9]中，再执行size++并返回，此时size的值等于10。线程B执行完毕后，线程A继续往下执行elementData[size++] = e;，此时size的值是10，相当于线程A将元素e放到elementData[10]中，但ArrayList的容量也只是10（下标只有0-9）,这样就会抛出越界异常。
*/

/*
场景二：元素值覆盖和为空问题
这个场景主要是与elementData[size++] = e;这句代码有关，因为它不是一个原子操作，有可能被其他线程中断，这句代码至少分为两步：
elementData[size] = e；
size = size + 1;

当多线程执行这段代码时，有可能出现这种场景：假设当前size=1，线程A、B执行这段代码，线程A执行elementData[size] = e；后，e被放在elementData[1]中，此时线程A中断，CPU调度线程B，线程B执行elementData[size] = e；，此时size的值还是1（线程A没来得及执行size+1操作就被中断），所以线程B还是把元素放在elementData[1]中，覆盖了线程A已经存放的值，然后对size进行+1操作并返回，此时size=2。线程B执行完后，CPU调度线程A继续执行，线程A对size+1，此时size就变成3。最终，两个线程执行完后，期待的结果是elementData[1]、elementData[2]各有一个元素，现在却是两个线程都把元素放在elementData[1]上，且线程B覆盖了线程A的元素，而elementData[2]为null。
*/
```

### **解决方法**

1.使用Vector（ArrayList所有方法加synchronized，太重）。

2.使用Collections.synchronizedList()转换成线程安全类。

```java
List list = Collections.synchronizedList(new ArrayList());
    ...
synchronized (list) {
    Iterator i = list.iterator(); // 必须在同步块中
    while (i.hasNext())
        foo(i.next());
}
```

3.使用java.concurrent.CopyOnWriteArrayList（推荐）。

 **CopyOnWriteArrayList**：JUC的类，通过**写时复制**来实现**读写分离**。如add()方法，就是先**复制**一个新数组，长度为原数组长度+1，然后将新数组最后一个元素设为添加的元素。

源码分析：

```java
public boolean add(E e) {
    final ReentrantLock lock = this.lock;
    lock.lock();
    try {
        //得到旧数组
        Object[] elements = getArray();
        int len = elements.length;
        //复制新数组
        Object[] newElements = Arrays.copyOf(elements, len + 1);
        //设置新元素
        newElements[len] = e;
        //设置新数组
        setArray(newElements);
        return true;
    } finally {
        lock.unlock();
    }
}
```

 优点：CopyOnWriteArrayList属于线程安全的，并发的读是没有异常的，读写操作被分离。缺点：在写入时不止加锁，使用了Arrays.copyOf()进行了数组复制，性能开销较大，遇到复杂对象也会导致内存占用较大。

### Set

HashSet和TreeSet都不是线程安全的，对应的有线程安全类CopyOnWriteSet这个线程安全类，类底层维护了一个CopyOnWriteArrayList数组。

```java
private final CopyOnWriteArrayList<E> al;
public CopyOnWriteArraySet() {
    al = new CopyOnWriteArrayList<E>();
}
```

### Map

HashMap不是线程安全的，Hashtable是线程安全的，但是跟Vector类似，太重量级。所以也有类似CopyOnWriteMap，叫ConcurrentHashMap。

```java
import java.util.*;
import java.util.concurrent.ConcurrentHashMap;
import java.util.concurrent.CopyOnWriteArrayList;
import java.util.concurrent.CopyOnWriteArraySet;

public class ContainerNotSafeDemo {
    public static void main(String[] args) {
        listNotSafe();
        setNoSafe();
        mapNotSafe();
    }

    private static void mapNotSafe() {
        //Map<String,String> map=new HashMap<>();
        Map<String, String> map = new ConcurrentHashMap<>();
        for (int i = 1; i <= 30; i++) {
            new Thread(() -> {
                map.put(Thread.currentThread().getName(), UUID.randomUUID().toString().substring(0, 8));
                System.out.println(Thread.currentThread().getName() + "\t" + map);
            }, String.valueOf(i)).start();
        }
    }

    private static void setNoSafe() {
        //Set<String> set=new HashSet<>();
        Set<String> set = new CopyOnWriteArraySet<>();
        for (int i = 1; i <= 30; i++) {
            new Thread(() -> {
                set.add(UUID.randomUUID().toString().substring(0, 8));
                System.out.println(Thread.currentThread().getName() + "\t" + set);
            }, String.valueOf(i)).start();
        }
    }

    private static void listNotSafe() {
        //List<String> list=new ArrayList<>();
        List<String> list = new CopyOnWriteArrayList<>();
        for (int i = 1; i <= 30; i++) {
            new Thread(() -> {
                list.add(UUID.randomUUID().toString().substring(0, 8));
                System.out.println(Thread.currentThread().getName() + "\t" + list);
            }, String.valueOf(i)).start();
        }
    }
}
```



## 参考链接

[Java中的Set、Map、List等数据结构的存取以及基本使用](https://blog.csdn.net/weixin_39240837/article/details/108006486)

[黑马程序员Java数据结构与算法，全网资料最全，154张数据结构图](https://www.bilibili.com/video/BV1iJ411E7xW?p=49)

[Java Collections Framework Internals](https://github.com/CarpenterLee/JCFInternals)

[JAVA list、set、map等集合类线程不安全的问题及解决方法](https://blog.csdn.net/luweibin19/article/details/106215036/)

[java多线程环境下数据结构的安全问题](https://blog.csdn.net/weixin_43932465/article/details/103006622)

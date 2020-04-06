# 容器
## 18.java 容器都有哪些？
---
<img src="/_markdownimg/javaCollectionMap1.jpg" alt="图片名称" align=center />

Collection：主要是单个元素的集合，由List、Queue、Set三个接口区分不同的集合特征，然后由下面的具体的类来实现对应的功能。

Map：有一组键值对的存储形式来保存，可以用键对象来查找值

## 19.Collection 和 Collections 有什么区别？
---
+ Collection:是单列集合的顶层接口，有子接口List和Set。
+ Collections:是针对集合操作的工具类，有对集合进行排序和二分查找的方法

## 20.List、Set、Map 之间的区别是什么？
---
+ List：有序集合，元素可重复
+ Set：不重复集合，LinkedHashSet按照插入排序，SortedSet可排序，HashSet无序
+ Map：键值对集合，存储键、值和之间的映射；Key无序，唯一；value 不要求有序，允许重复

## 21.HashMap 和 Hashtable 有什么区别？
---
Hashtable是线程安全，而HashMap则非线程安全。

## 22.如何决定使用 HashMap 还是 TreeMap？
---
HashMap基于散列桶（数组和链表）实现；TreeMap基于红黑树实现。

HashMap不支持排序；TreeMap默认是按照Key值升序排序的，可指定排序的比较器，主要用于存入元素时对元素进行自动排序。

HashMap大多数情况下有更好的性能，尤其是读数据。在没有排序要求的情况下，使用HashMap。

都是非线程安全。
## 23.说一下 HashMap 的实现原理？
---
+ hashMap基于hashing原理，我们通过put()和get()方法存储和获取对象。当我们将键值对传给put()方法时；它调用键对象的hashCode()方法来计算hashCode，然后找到bucket位置来存值对象。当获取对象时，通过键值对的equals()方法来找到正确的键值对。然后返回值对象。HashMap使用链表来解决碰撞问题，当发生碰撞时，对象会存储在链表的下一个节点。hashMap在每个链表的阶段存储键值对对象。
+ 当两个不同的键对象hashCode相同时会发生什么？他们会存储在同一个bucket位置的链表中。建对象的equals()方法用来找到键值对。

## 24.说一下 HashSet 的实现原理？
---
HashSet是基于HashMap实现的，HashSet 底层使用HashMap来保存所有元素，
因此HashSet 的实现比较简单，相关HashSet 的操作，基本上都是直接调用底层HashMap的相关方法来完成，HashSet不允许有重复的值，并且元素是无序的。
## 25.ArrayList 和 LinkedList 的区别是什么？
---
1. ArrayList是实现了基于动态数组的数据结构,LinkedList基于链表的数据结构。
2. 对于随机访问get和set,ArrayList觉得优于LinkedList,因为LinkedList要移动指针。
3. 对于新增和删除操作add和remove,LinedList比较占优势,因为ArrayList要移动数据。

## 26.如何实现数组和 List 之间的转换？
---
数组转List
```
public static void main(String[] args) {
    
    //数组转list
    String[] str=new String[] {"hello","world"};
    //方式一：使用for循环把数组元素加进list
    List<String> list=new ArrayList<String>();
    for (String string : str) {
        list.add(string);
    }
    System.out.println(list);
    
    //方式二：
    List<String> list2=new ArrayList<String>(Arrays.asList(str));
    System.out.println(list2);
    
    //方式三：
    //同方法二一样使用了asList()方法。这不是最好的，
    //因为asList()返回的列表的大小是固定的。
    //事实上，返回的列表不是java.util.ArrayList类，而是定义在java.util.Arrays中一个私有静态类java.util.Arrays.ArrayList
    //我们知道ArrayList的实现本质上是一个数组，而asList()返回的列表是由原始数组支持的固定大小的列表。
    //这种情况下，如果添加或删除列表中的元素，程序会抛出异常UnsupportedOperationException。
    //java.util.Arrays.ArrayList类具有 set()，get()，contains()等方法，但是不具有添加add()或删除remove()方法,所以调用add()方法会报错。
    List<String> list3 = Arrays.asList(str);
    //list3.remove(1);
    //boolean contains = list3.contains("s");
    //System.out.println(contains);
    System.out.println(list3);
    
    //方式四：使用Collections.addAll()
    List<String> list4=new ArrayList<String>(str.length);
    Collections.addAll(list4, str);
    System.out.println(list4);
    
    //方式五：使用Stream中的Collector收集器
    //转换后的List 属于 java.util.ArrayList 能进行正常的增删查操作
    List<String> list5=Stream.of(str).collect(Collectors.toList());
    System.out.println(list5);
}
```
List转数组
```
public static void main(String[] args) {
    //list转数组
    List<String> list=new ArrayList<String>();
    list.add("hello");
    list.add("world");
    
    //方式一：使用for循环
    String[] str1=new String[list.size()];
    for(int i=0;i<list.size();i++) {
        str1[i]=list.get(i);
    }
    for (String string : str1) {
        System.out.println(string);
    }
    
    //方式二：使用toArray()方法
    //list.toArray(T[]  a); 将list转化为你所需要类型的数组
    String[] str2=list.toArray(new String[list.size()]);
    for (String string : str2) {
        System.out.println(string);
    }
    
    //错误方式：易错   list.toArray()返回的是Object[]数组，怎么可以转型为String
    //ArrayList<String> list3=new ArrayList<String>();
    //String strings[]=(String [])list.toArray();
}
```
## 27.ArrayList 和 Vector 的区别是什么？
---
1. 同步性
Vector是线程安全的，也就是说它的方法之间是线程同步的，而ArrayList是线程不安全的，它的方法之前是线程不同步的。如果只有一个线程会访问集合，那最好是使用ArrayList，因为不考虑线程安全，效率会高一些；如果多个线程访问到集合，那么最好是使用Vector，因为我们不需要自己再去考虑和编写线程安全的代码。
2. 数据增长
ArrayList和Vector都有一个初始的容量大小。当存储他们里面的元素的个数超过了容量时，就小增加ArrayList与Vector的存储空间，每次要增加存储空间时，不是只增加一个存储单元，而是增加多个存储单元，每次增加的存储单元的个数在内存空间利用与恒旭效率之前取的一定的平衡。Vector 默认增长为原来的两倍，而ArrayList的增长策略在文档中没有明确规定（从源代码看到的是增长为原来的1.5倍）。ArrayList和Vector都可以设置初始空间的大小，Vector还可以设置增长的空间大小，而ArrayList没有提供设置增长空间的方法。

## 28.Array 和 ArrayList 有何区别？
---
1. 存储内容比较：
Array 数组可以包含基本类型和对象类型，
ArrayList 却只能包含对象类型。
Array 数组在存放的时候一定是同种类型的元素。ArrayList 就不一定了 。
2. 空间大小比较：
Array 数组的空间大小是固定的,所以需要事前确定合适的空间大小。
ArrayList 的空间是动态增长的,而且，每次添加新的元素的时候都会检查内部数组的空间是否足够。
3. 方法上的比较：
ArrayList 方法上比 Array 更多样化，比如添加全部 addAll()、删除全部 removeAll()、返回迭代器 iterator() 等。

## 29.在 Queue 中 poll()和 remove()有什么区别？
---
Queue 中 remove() 和 poll()都是用来从队列头部删除一个元素。
在队列元素为空的情况下，remove() 方法会抛出NoSuchElementException异常，poll() 方法只会返回 null 。

## 30.哪些集合类是线程安全的？
---
+ Vector：就比Arraylist多了个同步化机制（线程安全）。
+ Hashtable：就比Hashmap多了个线程安全。
+ ConcurrentHashMap:是一种高效但是线程安全的集合。
+ Stack：栈，也是线程安全的，继承于Vector。

## 31.迭代器 Iterator 是什么？
---
迭代器，提供一种访问一个集合对象各个元素的途径，同时又不需要暴露该对象的内部细节。java通过提供Iterator和Iterable俩个接口来实现集合类的可迭代性，迭代器主要的用法是：首先用hasNext（）作为循环条件，再用next（）方法得到每一个元素，最后在进行相关的操作。

## 32.Iterator 怎么使用？有什么特点？
---
```
//是否有下一个元素
boolean hasNext();
 
//下一个元素
E next();
 
//从迭代器指向的集合中删除迭代器返回的最后一个元素
default void remove() {
    throw new UnsupportedOperationException("remove");
}
 
//遍历所有元素
default void forEachRemaining(Consumer<? super E> action) {
    Objects.requireNonNull(action);
    while (hasNext())
        action.accept(next());
}
```
Iterator 的使用示例
```
public class TestIterator {
    
    static List<String> list = new ArrayList<String>();
    
    static {
        list.add("111");
        list.add("222");
        list.add("333");
    }
    
 
    public static void main(String[] args) {
        testIteratorNext();
        System.out.println();
        
        testForEachRemaining();
        System.out.println();
        
        testIteratorRemove();
    }
    
    //使用 hasNext 和 next遍历 
    public static void testIteratorNext() {
        Iterator<String> iterator = list.iterator();
        while (iterator.hasNext()) {
            String str = iterator.next();
            System.out.println(str);
        }
    }
    
    //使用 Iterator 删除元素 
    public static void testIteratorRemove() {
        Iterator<String> iterator = list.iterator();
        while (iterator.hasNext()) {
            String str = iterator.next();
            if ("222".equals(str)) {
                iterator.remove();
            }
        }
        System.out.println(list);
    }
    
    //使用 forEachRemaining 遍历
    public static void testForEachRemaining() {
        final Iterator<String> iterator = list.iterator();
        iterator.forEachRemaining(new Consumer<String>() {
 
            public void accept(String t) {
                System.out.println(t);
            }
            
        });
    }
}
```
注意事项
+ 在迭代过程中调用集合的 remove(Object o) 可能会报 java.util.ConcurrentModificationException 异常
+ forEachRemaining 方法中 调用Iterator 的 remove 方法会报 java.lang.IllegalStateException 异常

```
//使用迭代器遍历元素过程中，调用集合的 remove(Object obj) 方法可能会报 java.util.ConcurrentModificationException 异常
    public static void testListRevome() {
         ArrayList<String> aList = new ArrayList<String>();
         aList.add("111");
         aList.add("333");
         aList.add("222");
         System.out.println("移除前："+aList);
         
         Iterator<String> iterator = aList.iterator();
         while(iterator.hasNext())
         {
             if("222".equals(iterator.next()))
             {
                aList.remove("222");          
             }
         }
         System.out.println("移除后："+aList);
    }
    
    //JDK 1.8 Iterator forEachRemaining 方法中 调用Iterator 的 remove 方法会报 java.lang.IllegalStateException 异常
    public static void testForEachRemainingIteRemove () {
        final Iterator<String> iterator = list.iterator();
        iterator.forEachRemaining(new Consumer<String>() {
 
            public void accept(String t) {
                if ("222".equals(t)) {
                    iterator.remove();
                }
            }
        });
    }
```    
## 33.Iterator 和 ListIterator 有什么区别？
---
+ Iterator可用来遍历Set和List集合，但是ListIterator只能用来遍历List。
+ Iterator对集合只能是前向遍历，ListIterator既可以前向也可以后向。
+ ListIterator实现了Iterator接口，并包含其他的功能，比如：增加元素，替换元素，获取前一个和后一个元素的索引，等等。

## 34.怎么确保一个集合不能被修改？
---
我们很容易想到用final关键字进行修饰，我们都知道

final关键字可以修饰类，方法，成员变量，final修饰的类不能被继承，final修饰的方法不能被重写，final修饰的成员变量必须初始化值，如果这个成员变量是基本数据类型，表示这个变量的值是不可改变的，如果说这个成员变量是引用类型，则表示这个引用的地址值是不能改变的，但是这个引用所指向的对象里面的内容还是可以改变的
。

那么，我们怎么确保一个集合不能被修改？首先我们要清楚，集合（map,set,list…）都是引用类型，所以我们如果用final修饰的话，集合里面的内容还是可以修改的。

我们可以做一个实验：

可以看到：我们用final关键字定义了一个map集合，这时候我们往集合里面传值，第一个键值对1,1；我们再修改后，可以把键为1的值改为100，说明我们是可以修改map集合的值的。

那我们应该怎么做才能确保集合不被修改呢？
我们可以采用Collections包下的unmodifiableMap方法，通过这个方法返回的map,是不可以修改的。他会报 java.lang.UnsupportedOperationException错。

同理：Collections包也提供了对list和set集合的方法。
Collections.unmodifiableList(List)
Collections.unmodifiableSet(Set)
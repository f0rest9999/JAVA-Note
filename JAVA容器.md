## JAVA容器

### 概览

```

```

###Collections

####1. Set

- TreeSet：基于红黑树实现，支持有序性操作，例如根据一个范围查找元素的操作。但是查找效率不如 HashSet，HashSet 查找的时间复杂度为 O(1)，TreeSet 则为 O(logN)。
- HashSet：基于哈希表实现，支持快速查找，但不支持有序性操作。并且失去了元素的插入顺序信息，也就是说使用 Iterator 遍历 HashSet 得到的结果是不确定的。
- LinkedHashSet：具有 HashSet 的查找效率，并且内部使用双向链表维护元素的插入顺序。

#### 2. List

- ArrayList：基于动态数组实现，支持随机访问。
- Vector：和 ArrayList 类似，但它是线程安全的。
- LinkedList：基于双向链表实现，只能顺序访问，但是可以快速地在链表中间插入和删除元素。不仅如此，LinkedList 还可以用作==栈、队列和双向队列==。

#### 3. Queue

- LinkedList：可以用它来实现双向队列。
- PriorityQueue：==基于堆结构实现==，可以用它来实现==优先队列==。

###Map

- TreeMap：基于红黑树实现。

  `红黑树：是一棵二叉搜索树，且满足以下条件：`

  `1.每个结点或是RED，或是BLACK  2.根节点是黑色  3.每一个叶结点黑色的  4.若一个结点是RED，那么它的两个子结点都是BLACK`

  `5.对每一个结点，从该结点到达其所有后代叶节点的简单路径上，均包含相同数目的BLACK结点（黑高相同）`

  （红黑树是一种支持任何一种基本动态集合操作的一种好的搜索树）

- HashMap：基于==哈希表==实现。

- HashTable：和 HashMap 类似，但它是线程安全的，这意味着同一时刻多个线程同时写入 HashTable 不会导致数据不一致。==它是遗留类，不应该去使用它，而是使用 ConcurrentHashMap 来支持线程安全，==ConcurrentHashMap 的效率会更高，因为 ConcurrentHashMap 引入了分段锁。

- LinkedHashMap：使用双向链表来维护元素的顺序，顺序为插入顺序或者最近最少使用（LRU）顺序。

### 容器中的设计模式

#### 迭代器模式

- 因为Collection继承了Iterable接口，所以可以通过调用其 iterator() 方法来产生一个Iterator 对象，通过对这个对象就可以迭代遍历Collection中的元素，从JDK1.5之后，可以使用==foreach==方法来遍历实现了 Iterable 接口的聚合对象。

####适配器模式

- java.util.Arrays下的asList()方法可以将==数组==转化成==List==类型。但是注意不能直接使用基本类型的数组作为参数，只能使用相应的包装类型的数组

- ```java
  Integer[] arr = {1, 2, 3};
  List list = Arrays.asList(arr);
  ```

- ```java
  List list = Arrays.asList(1, 2, 3);
  ```

### 源码分析

在 IDEA 中 double shift 调出 Search EveryWhere，查找源码文件，找到之后就可以阅读源码。

#### ArrayList

- 因为 ArrayList 是基于数组实现的，所以支持快速随机访问。![image-20201229192139072](https://gitee.com/f0rest9999/images/raw/master/20201229192139.png)接口标识着该类支持快速随机访问。

```java
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable
```

- 数组的默认大小为 10。

![image-20201229192232195](https://gitee.com/f0rest9999/images/raw/master/20201229192232.png)

- 初始化一个大小为`initialCapacity`的ArrayList，==鼓励创建对象时就指定大概的容量大小==，因为这样可以减少扩容的次数

  ![image-20201229192729006](https://gitee.com/f0rest9999/images/raw/master/20201229192729.png)

- 在`add`一个元素时，通过调用`ensureCapacity()`确保容量足够

  ![image-20201229193054847](https://gitee.com/f0rest9999/images/raw/master/20201229193054.png)

![image-20201229193021038](https://gitee.com/f0rest9999/images/raw/master/20201229193021.png)

- 扩容操作`grow()`，在`add`时如果发现容量不够，则需要扩容，新容量的大小为 `oldCapacity + (oldCapacity >> 1)`，大约为原容量大小的1.5倍，扩容操作需要调用 `Arrays.copyOf()` 把原数组整个复制到新数组中，这个操作代价很高。

  ![image-20201229193237708](https://gitee.com/f0rest9999/images/raw/master/20201229193237.png)

- 删除操作`remove()`，所有的后续元素都要向前移动，该操作的时间复杂是O(n)，因此对于ArrayList来说删除的代价是十分高的

![image-20201229200555582](https://gitee.com/f0rest9999/images/raw/master/20201229200555.png)

- `modCount`用来标识ArrayList 结构发生变化的次数。结构发生变化是指添加或者删除至少一个元素的所有操作，或者是调整内部数组的大小，仅仅只是设置元素的值不算结构发生变化。在进行某些操作时，需要比较操作前后 modCount 是否改变，如果改变了需要抛出 ConcurrentModificationException。

#### Vector

- 它的实现与 ArrayList 类似，但是其中一部分方法的实现使用了 synchronized 进行同步。

  ![image-20201229201529503](https://gitee.com/f0rest9999/images/raw/master/20201229201529.png)

![image-20201229201559124](https://gitee.com/f0rest9999/images/raw/master/20201229201559.png)

- Vector 的构造函数可以传入 capacityIncrement 参数，它的作用是在扩容时使容量 `capacity` 增长 `capacityIncrement`。如果这个参数的值小于等于 0，扩容时每次都令 capacity 为原来的两倍。==（这个和ArrayList也是有区别的）==

![](https://gitee.com/f0rest9999/images/raw/master/20201229201800.png)

- `grow()`

  ![image-20201229202019774](https://gitee.com/f0rest9999/images/raw/master/20201229202019.png)

- 与ArrayList的区别和比较
  - Vector 是同步的，因此开销就比 ArrayList 要大，访问速度更慢。最好使用 ArrayList 而不是 Vector，因为同步操作完全可以由程序员自己来控制；
  - Vector 每次扩容请求其大小的 2 倍（也可以通过构造函数设置增长的容量），而 ArrayList 是 1.5 倍。

#### LinkedList

- 基于==双向链表==实现，使用Node存储链表结点的信息

![image-20201229202425322](https://gitee.com/f0rest9999/images/raw/master/20201229202425.png)

- 链表保存了一个first和一个last，分别指向起始和结束的位置

  ![image-20201229202606180](https://gitee.com/f0rest9999/images/raw/master/20201229202606.png)

- 与ArrayList的区别和比较

  - ArrayList 基于动态==数组==实现，LinkedList 基于双向==链表==实现。其实两者的区别可以类比于数组和链表的区别。

  - 数组支持随机访问，但插入删除的代价很高，需要移动大量元素；

  - 链表不支持随机访问，但插入删除只需要改变指针。

#### HashMap(重点)

- 内部包含了一个 Node类型的数组 table![image-20201229203749148](https://gitee.com/f0rest9999/images/raw/master/20201229203749.png)

  ==JDK1.8之前HashMap采用数组 + 链表实现==，这种实现在hash值相等的元素较多时，通过key值依次查找的效率较低。==JDK1.8之后HashMap==

  底层进行了优化采用了==数组 + 链表 + 红黑树==进行实现，当链表长度超过阈值`8`时，将链表转换为红黑树，这样大大减少了查找时间。

![image-20201229211504802](https://gitee.com/f0rest9999/images/raw/master/20201229211504.png)

![image-20201229211525479](https://gitee.com/f0rest9999/images/raw/master/20201229211525.png)

![image-20201229211721103](https://gitee.com/f0rest9999/images/raw/master/20201229211721.png)

- 初始化大小是==16==![image-20201229205605272](https://gitee.com/f0rest9999/images/raw/master/20201229205605.png)

- `hash()`是用来计算hash值的

  ​								![image-20201229220248258](https://gitee.com/f0rest9999/images/raw/master/20201229220248.png) 

- 每一个Node是一个键值对，包含四个字段，从 next 字段我们可以看出 Entry 是一个链表。即数组中的每个位置被当成一个==桶（bucket）==，一个桶存放一个链表。==HashMap 使用拉链法来解决冲突，同一个链表中存放哈希值和散列桶取模运算结果相同的 Node。==

  ```java
   /**
       * Basic hash bin node, used for most entries.  (See below for
       * TreeNode subclass, and in LinkedHashMap for its Entry subclass.)
       */
      static class Node<K,V> implements Map.Entry<K,V> {
          final int hash;
          final K key;
          V value;
          Node<K,V> next;
  
          Node(int hash, K key, V value, Node<K,V> next) {
              this.hash = hash;
              this.key = key;
              this.value = value;
              this.next = next;
          }
  
          public final K getKey()        { return key; }
          public final V getValue()      { return value; }
          public final String toString() { return key + "=" + value; }
  
          public final int hashCode() {
              return Objects.hashCode(key) ^ Objects.hashCode(value);
          }
  
          public final V setValue(V newValue) {
              V oldValue = value;
              value = newValue;
              return oldValue;
          }
  
          public final boolean equals(Object o) {
              if (o == this)
                  return true;
              if (o instanceof Map.Entry) {
                  Map.Entry<?,?> e = (Map.Entry<?,?>)o;
                  if (Objects.equals(key, e.getKey()) &&
                      Objects.equals(value, e.getValue()))
                      return true;
              }
              return false;
          }
      }
  ```

- 拉链法解决冲突的工作原理（==JDK8以前是头插法，JDK8后是尾插法==）

  - 新建一个 HashMap，默认大小为 16；
  - 插入 <K1,V1> 键值对，先计算 K1 的 hashCode 为 115，使用除留余数法得到所在的桶下标 115%16=3。
  - 插入 <K2,V2> 键值对，先计算 K2 的 hashCode 为 118，使用除留余数法得到所在的桶下标 118%16=6。
  - 插入 <K3,V3> 键值对，先计算 K3 的 hashCode 为 118，使用除留余数法得到所在的桶下标 118%16=6，插入。

  - 查找需要分成两步进行：（1）计算键值所在的桶（2）在链表上顺序查找并插入

- `put()`方法

  ![image-20201229212334214](https://gitee.com/f0rest9999/images/raw/master/20201229212334.png)

  可以看到`put()`方法调用了`putVal`方法，分析`putVal()`方法如下：

  - 使用`key`的hash值和数组长度进行与运算得到下标i，判断这个下面的是否存在值，如果不存在，就在该下面i下初始化一个链表 

    ​													![image-20201229213406931](https://gitee.com/f0rest9999/images/raw/master/20201229213406.png)    

  - 如果这个桶已经被使用了，需要分三种情况讨论

  - - 桶的首个元素的key与待插入节点的key相同，替换之
    - 桶的首个元素key与待插入节点的key不同且首个节点是红黑树节点，调用putTreeVal方法
    - 桶的首个元素key与待插入节点的key不同且首个节点是链表节点，继续执行本方法。在此过程中，如果发现链表长度大于8就调用treeifyBin方法将链表转为红黑树

  ```java
   /**
       * Implements Map.put and related methods.
       *
       * @param hash hash for key
       * @param key the key
       * @param value the value to put
       * @param onlyIfAbsent if true, don't change existing value
       * @param evict if false, the table is in creation mode.
       * @return previous value, or null if none
       */
      final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                     boolean evict) {
          Node<K,V>[] tab; Node<K,V> p; int n, i;
          if ((tab = table) == null || (n = tab.length) == 0)
              n = (tab = resize()).length;
          if ((p = tab[i = (n - 1) & hash]) == null)
              tab[i] = newNode(hash, key, value, null);
          else {
              Node<K,V> e; K k;
              if (p.hash == hash &&
                  ((k = p.key) == key || (key != null && key.equals(k))))
                  e = p;
              else if (p instanceof TreeNode)
                  e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
              else {
                  for (int binCount = 0; ; ++binCount) {
                      if ((e = p.next) == null) {
                          p.next = newNode(hash, key, value, null);
                          if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                              treeifyBin(tab, hash);
                          break;
                      }
                      if (e.hash == hash &&
                          ((k = e.key) == key || (key != null && key.equals(k))))
                          break;
                      p = e;
                  }
              }
              if (e != null) { // existing mapping for key
                  V oldValue = e.value;
                  if (!onlyIfAbsent || oldValue == null)
                      e.value = value;
                  afterNodeAccess(e);
                  return oldValue;
              }
          }
          ++modCount;
          if (++size > threshold)
              resize();
          afterNodeInsertion(evict);
          return null;
      }
  ```

- `get()`方法

  ![image-20201229215248180](https://gitee.com/f0rest9999/images/raw/master/20201229215248.png)

  可以看到调用了getNode的方法

  ```java
  /**
       * Implements Map.get and related methods.
       *
       * @param hash hash for key
       * @param key the key
       * @return the node, or null if none
       */
      final Node<K,V> getNode(int hash, Object key) {
          Node<K,V>[] tab; Node<K,V> first, e; int n; K k;
          if ((tab = table) != null && (n = tab.length) > 0 &&
              (first = tab[(n - 1) & hash]) != null) {
              if (first.hash == hash && // always check first node
                  ((k = first.key) == key || (key != null && key.equals(k))))
                  return first;
              if ((e = first.next) != null) {
                  if (first instanceof TreeNode)
                      return ((TreeNode<K,V>)first).getTreeNode(hash, key);
                  do {
                      if (e.hash == hash &&
                          ((k = e.key) == key || (key != null && key.equals(k))))
                          return e;
                  } while ((e = e.next) != null);
              }
          }
          return null;
      }
  ```

  - 首先判断table是否为空，并且hash值算出当前索引下node是否为空，为空直接返回null
  - 不为空：
    - 如果第一个节点hash值相等，key也相当，那就是第一个值，直接返回
    - 如果第一个节点不相等
      - 判断是否为红黑树类型，如果是从树种获取数据
      - 不是红黑树类型，进行while遍历下一个节点，已知结点的hash值，key值相等时，返回。

- `resize()`扩容方法

  在以下三种情况下会调用`resize()`方法

  - 当`hashmap`中存储数据的数量到达总桶数*加载因子这个阈值时会触发扩容
  - 当某个桶中的链表长度达到8进行链表扭转为**红黑树**的时候，会检查总桶数是否小于64，如果总桶数小于64也会进行扩容；
  - new完HashMap之后，第一次往HashMap进行put操作的时候，首先会进行扩容。
  - **oldTab：为数组类型，代表扩容之前HashMap中的数组，也就是所有的桶；
    oldCap：为int类型代表扩容之前总桶数量；
    oldThr：为int类型代表扩容之前下次扩容的阈值；
    newCap：为int类型代表这次扩容之后总桶数量；
    newThr：为int类型代表这次扩容之后下次扩容的阈值；
    newTab：为数组类型，代表扩容之后HashMap中的数组。**
  - ==详见注释==

  ```java
    final Node<K,V>[] resize() {
          Node<K,V>[] oldTab = table;
          int oldCap = (oldTab == null) ? 0 : oldTab.length;
          int oldThr = threshold;
          int newCap, newThr = 0;
          if (oldCap > 0) {
              //如果oldCap大于最大容量的话，把下次扩容的阈值设置为MAX_VALUE，并且返回	
              if (oldCap >= MAXIMUM_CAPACITY) {
                  threshold = Integer.MAX_VALUE;
                  return oldTab;
              }//如果newCap = oldCap * 2小于最大容量的话，新的
              else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                       oldCap >= DEFAULT_INITIAL_CAPACITY)
                  newThr = oldThr << 1; // double threshold
          }//当oldCap为0，并且oldThr大于0的时候，以为着这是第一次进行扩容，在16、17行把newCap赋值为扩容之前的阈值，该阈值当时代表着是你要申请的容量进行处理为接近2次幂的那个数
          else if (oldThr > 0) // initial capacity was placed in threshold
              newCap = oldThr;
          else {               // zero initial threshold signifies using defaults
              newCap = DEFAULT_INITIAL_CAPACITY;
              newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
          }
          if (newThr == 0) {
              float ft = (float)newCap * loadFactor;
              newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                        (int)ft : Integer.MAX_VALUE);
          }
          threshold = newThr;
        	//在这里真正对HashMap里面的数组进行初始化的
          @SuppressWarnings({"rawtypes","unchecked"})
          Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
          table = newTab;
          if (oldTab != null) {
              for (int j = 0; j < oldCap; ++j) {
                  Node<K,V> e;
                  if ((e = oldTab[j]) != null) {
                      oldTab[j] = null;
                      //情况1
                      if (e.next == null)
                          newTab[e.hash & (newCap - 1)] = e;
                      //情况2-是红黑树
                      else if (e instanceof TreeNode)
                          ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                      //情况3-是链表
                      else { // preserve order
                          Node<K,V> loHead = null, loTail = null;
                          Node<K,V> hiHead = null, hiTail = null;
                          Node<K,V> next;
                          do {
                              next = e.next;
                              if ((e.hash & oldCap) == 0) {
                                  if (loTail == null)
                                      loHead = e;
                                  else
                                      loTail.next = e;
                                  loTail = e;
                              }
                              else {
                                  if (hiTail == null)
                                      hiHead = e;
                                  else
                                      hiTail.next = e;
                                  hiTail = e;
                              }
                          } while ((e = next) != null);
                          if (loTail != null) {
                              loTail.next = null;
                              newTab[j] = loHead;
                          }
                          if (hiTail != null) {
                              hiTail.next = null;
                              newTab[j + oldCap] = hiHead;
                          }
                      }
                  }
              }
          }
          return newTab;
      }
  ```

  - **HashMap扩容完了，下面就是之前的K-V在新的HashMap中的分配问题，两种情况**

    - **第一种当之前桶中的结点个数只有一个的时候，他是重新计算了一下映射到扩容之后对应的桶的位置**

    - **结点个数大于1时，又有两种情况（红黑树和链表情况）**

      - **链表情况下：**对该链表一分为二，注意扩容前桶中的结点分为两种，一种是依旧在之前那个桶对应的下标的桶中，另一种是之前所在的桶的下标+oldCap ，分开的条件是该结点的K的hash值与扩容之前的总桶数n做了一个&运算（以前做映射的时候的公式为hash&（n-1）n为总桶数注意两个公式区别），为什么要使用这个条件分为两个链表，主要是判断出扩容之后哪些结点依旧在之前那个桶对应的下标的桶中，哪些结点在之前所在的桶的下标+oldCap的桶中原理如下图

      ![image-20201230124600512](https://gitee.com/f0rest9999/images/raw/master/20201230124607.png)

      - 红黑树情况下，类似于链表的操作


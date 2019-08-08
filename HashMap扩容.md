```JAVA
final Node<K,V>[] resize() {
        Node<K,V>[] oldTab = table; ////首次初始化后table为Null
        int oldCap = (oldTab == null) ? 0 : oldTab.length;
        int oldThr = threshold;
        int newCap, newThr = 0;
        if (oldCap > 0) {//table扩容过
             //当前table容量大于最大值得时候返回当前table
            if (oldCap >= MAXIMUM_CAPACITY) {
                threshold = Integer.MAX_VALUE;
                return oldTab;
            }
            else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                     oldCap >= DEFAULT_INITIAL_CAPACITY)
                //table的容量乘以2，threshold的值也乘以2 
                newThr = oldThr << 1; // double threshold
        }
        else if (oldThr > 0) // initial capacity was placed in threshold
             //使用带有初始容量的构造器时，table容量为初始化得到的threshold
            newCap = oldThr;
        else {               // zero initial threshold signifies using defaults
            				//默认构造器下进行扩容
            newCap = DEFAULT_INITIAL_CAPACITY;
            newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
        }
        if (newThr == 0) {
            //使用带有初始容量的构造器在此处进行扩容
            float ft = (float)newCap * loadFactor;
            newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                      (int)ft : Integer.MAX_VALUE);
        }
        threshold = newThr;
        @SuppressWarnings({"rawtypes","unchecked"})
        Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];//定义一个扩容后的数组
        table = newTab;
        if (oldTab != null) {
            for (int j = 0; j < oldCap; ++j) { 
                Node<K,V> e;
                if ((e = oldTab[j]) != null) { //引用了旧的数组
                    oldTab[j] = null;	//释放旧Entry数组的对象引用
                    if (e.next == null)
                        newTab[e.hash & (newCap - 1)] = e;
                    else if (e instanceof TreeNode)
                        ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                    else { // preserve order
                        // 在JDK1.8中发生hashmap扩容时，遍历hashmap每个bucket里的链表，
                        // 每个链表可能会被拆分成两个链表，不需要移动的元素置入loHead为首的链表，
                        // 需要移动的元素置入hiHead为首的链表，然后分别分配给老的buket和新的buket
                        Node<K,V> loHead = null, loTail = null;
                        Node<K,V> hiHead = null, hiTail = null;
                        Node<K,V> next;
                        //看原来的hash值新增的那个bit是1还是0就好了，
                        //是0的话索引没变，是1的话索引变成“原索引+oldCap”
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

```java
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
               boolean evict) {
    Node<K,V>[] tab; Node<K,V> p; int n, i;
    if ((tab = table) == null || (n = tab.length) == 0) //首次初始化的时候table为null
        n = (tab = resize()).length; //对HashMap进行扩容
    if ((p = tab[i = (n - 1) & hash]) == null) //根据hash值来确认存放的位置。如果当前位置是空直接添加到table中
        tab[i] = newNode(hash, key, value, null);
    else {
        //如果存放的位置已经有值
        Node<K,V> e; K k;
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))
            e = p; //确认当前table中存放键值对的Key是否跟要传入的键值对key一致
        else if (p instanceof TreeNode) //确认是否为红黑树
            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
        else {//如果hashCode一样的两个不同Key就会以链表的形式保存
            for (int binCount = 0; ; ++binCount) {
                if ((e = p.next) == null) {
                    p.next = newNode(hash, key, value, null);
                    if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st 判断链表长度是否大于8
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
                e.value = value; //替换新的value并返回旧的value
            afterNodeAccess(e);
            return oldValue;
        }
    }
    ++modCount;
    if (++size > threshold)
        resize();//如果当前HashMap的容量超过threshold则进行扩容
    afterNodeInsertion(evict);
    return null;
} 
```

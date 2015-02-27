---

layout: default
title: HashMap 和 HashTable的区别
abstract: 
comments: false

---

## 1. HashMap

- 非线程安全的

- key 和 value 允许为 null 值

- HashMap 内部是一个数组 + 链表的结构，put 一个元素的时候 HashMap 会根据 key 的 hash 值算出要存放的数组的位置，如果两个元素算出的数组值相同，那么他们会被放到同一个位置。获取该元素的时候，会根据 key 的 hash 找到数组的位置，然后再找到链表中的元素。

- 数组大小，默认为 16 HashMap 的最大值也是有设定的

```java

    /**
     * The default initial capacity - MUST be a power of two.
     */
    static final int DEFAULT_INITIAL_CAPACITY = 16;

    /**
     * The maximum capacity, used if a higher value is implicitly specified
     * by either of the constructors with arguments.
     * MUST be a power of two <= 1<<30.
     */
    static final int MAXIMUM_CAPACITY = 1 << 30;

```

- HashMap的大小一定是2的幂数，初始化时指定的大小不是2的幂数，则取大于该数的2的幂数

- 如果指定的值大于HashMap的最大值则抛弃你所指的的值，使用默认最大值

```java

   /**
     * Constructs an empty <tt>HashMap</tt> with the specified initial
     * capacity and load factor.
     *
     * @param  initialCapacity the initial capacity
     * @param  loadFactor      the load factor
     * @throws IllegalArgumentException if the initial capacity is negative
     *         or the load factor is nonpositive
     */
    public HashMap(int initialCapacity, float loadFactor) {
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal initial capacity: " +
                                               initialCapacity);
        if (initialCapacity > MAXIMUM_CAPACITY)
            initialCapacity = MAXIMUM_CAPACITY;
        if (loadFactor <= 0 || Float.isNaN(loadFactor))
            throw new IllegalArgumentException("Illegal load factor: " +
                                               loadFactor);

        // Find a power of 2 >= initialCapacity
        int capacity = 1;
        while (capacity < initialCapacity)
            capacity <<= 1;

        this.loadFactor = loadFactor;
        threshold = (int)(capacity * loadFactor);
        table = new Entry[capacity];
        init();
    }

```





## 2. Hashtable

- 线程安全的
- 如果 value 为 null 则抛出 NullPointerException 异常  







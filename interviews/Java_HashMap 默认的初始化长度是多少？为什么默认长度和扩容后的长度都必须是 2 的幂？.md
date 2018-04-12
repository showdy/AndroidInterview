### `HashMap` 默认的初始化长度是多少？为什么默认长度和扩容后的长度都必须是 2 的幂？

在 `JDK` 中默认长度是 16（在 Android `SDK` 中的 `HashMap` 默认长度为 4），并且默认长度和扩容后的长度都必须是 2 的幂。因为我们可以先看下 `HashMap` 的 put 方法核心，如下：

```java
public V put(K key, V value) {  
  ......    
  //计算出 key 的 hash 值    
  int hash = hash(key);    
  //通过 key 的 hash 值和当前动态数组的长度求当前 key 的 Entry 在数组中的下标    
  int i = indexFor(hash, table.length);    
  ......
}

//最核心的求数组下标方法
static int indexFor(int h, int length) {    
  // assert Integer.bitCount(length) == 1 : "length must be a non-zero power of 2";    
  return h & (length-1);
}
```

可以看到获取数组索引的计算方式为 key 的 hash 值按位与运算数组长度减一，为了说明长度尽量是 2 的幂的作用我们假设执行了 put("android", 123); 语句且 "android" 的 hash 值为 234567，二进制为 111001010001000111，然后由于 HashMap 默认长度为 16，所以减一后的二进制为 1111，接着两数做按位与操作二进制结果为 111，即十进制的 7，所以 index 为 7，可以看出这种按位操作比直接取模效率要高。 

如果假设 HashMap 默认长度不是 2 的幂，譬如数组长度为 6，减一的二进制为 101，与 111001010001000111 按位与操作二进制 101，此时假设我们通过 put 再放一个 key-value 进来，其 hash 为 111001010001000101，其与 101 按位与操作后的二进制也为 101，很容易发生哈希碰撞，这就不符合 index 的均匀分布了。

通过上面两个假设例子可以看出 **HashMap 的长度为 2 的幂时减一的值的二进制位数一定全为 1，这样数组下标 index 的值完全取决于 key 的 hash 值的后几位，因此只要存入 HashMap 的 Entry 的 key 的 hashCode 值分布均匀，HashMap 中数组 Entry 元素的分部也就尽可能是均匀的（也就避免了 hash 碰撞带来的性能问题），所以当长度为 2 的幂时不同的 hash 值发生碰撞的概率比较小，这样就会使得数据在 table 数组中分布较均匀，查询速度也较快。不过即使负载因子和 hash 算法设计的再合理也免不了哈希冲突碰撞的情况，一旦出现过多就会影响 HashMap 的性能，所以在 JDK 1.8 中官方对数据结构引入了红黑树，当链表长度太长（默认超过 8）时链表就转为了红黑树，而红黑树的增删改查都比较高效，从而解决了哈希碰撞带来的性能问题。**


### 谈谈`ArrayList`,`LinkedList`,`List`的区别

`List`是集合列表的接口, `ArrayList`与`LinkedList`都是接口`List`的实现类. `ArrayList`是动态数组顺序表, 顺序表的存储地址是连续的, 所以查询比较是比较快(random access), 但是插入和删除时由于需要把其他元素顺序移动, 所以比较耗时. `LinkedList`是双向链表实现的, 同时实现双端队列`Deque`接口, 链表节点的存储地址是不连续的,每个存储地址都是通过指针关联, 在查找是需要进行指针遍历节点(sequence access), 查找比较耗时, 而插入和删除则由于链表的特征比较快. 
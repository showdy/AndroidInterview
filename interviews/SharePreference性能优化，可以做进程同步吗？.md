在Android中, SharePreferences是一个轻量级的存储类，特别适合用于保存软件配置参数。使用SharedPreferences保存数据，其背后是用xml文件存放数据，文件
存放在/data/data/ < package name > /shared_prefs目录下.
之所以说SharedPreference是一种轻量级的存储方式，是因为它在创建的时候会把整个文件全部加载进内存，如果SharedPreference文件比较大，会带来以下问题：

* 第一次从sp中获取值的时候，有可能阻塞主线程，使界面卡顿、掉帧。

* 解析sp的时候会产生大量的临时对象，导致频繁GC，引起界面卡顿。

* 这些key和value会永远存在于内存之中，占用大量内存。

优化建议

* 不要存放大的key和value，会引起界面卡、频繁GC、占用内存等等。

* 毫不相关的配置项就不要放在在一起，文件越大读取越慢。

* 读取频繁的key和不易变动的key尽量不要放在一起，影响速度，如果整个文件很小，那么忽略吧，为了这点性能添加维护成本得不偿失。

* 不要乱edit和apply，尽量批量修改一次提交，多次apply会阻塞主线程。

* 尽量不要存放JSON和HTML，这种场景请直接使用JSON。

SharedPreference无法进行跨进程通信，MODE_MULTI_PROCESS只是保证了在API 11以前的系统上，如果sp已经被读取进内存，再次获取这个SharedPreference的时候，如果有这个flag，会重新读一遍文件，仅此而已。


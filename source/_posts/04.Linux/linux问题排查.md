
cat /proc/meminfo


默认为0
echo 1 > /proc/sys/vm/drop_caches:表示清除pagecache.
echo 2 > /proc/sys/vm/drop_caches:表示清除回收slab分配器中的对象（包括目录项缓存和inode缓存）.slab分配器是内核中管理内存的一种机制，其中很多缓存数据实现都是用的pagecache.
echo 3 > /proc/sys/vm/drop_caches:表示清除pagecache和slab分配器中的缓存对象.


[![](https://static.segmentfault.com/v-5b1df2a7/global/img/creativecommons-cc.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)

# 常用命令

## 1．存储字符串string

字符串类型是Redis中最为基础的数据存储类型，它在Redis中是二进制安全的，这    便意味着该类型可以接受任何格式的数据，如JPEG图像数据或Json对象描述信息等。        在Redis中字符串类型的Value最多可以容纳的数据长度是512M

1）**set key value**：设定key持有指定的字符串value，如果该key存在则进行覆盖    操作。总是返回”OK”

2）**get key**：获取key的value。如果与该key关联的value不是String类型，redis 将返回错误信息，因为get命令只能用于获取String value；如果该key不存在，返   回null。

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image002.png)

3）**getset key value**：先获取该key的值，然后在设置该key的值。

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image003.png)

4）**incr key**：将指定的key的value原子性的递增1.如果该key不存在，其初始值     为0，在incr之后其值为1。如果value的值不能转成整型，如hello，该操作将执      行失败并返回相应的错误信息。

5）**decr key**：将指定的key的value原子性的递减1.如果该key不存在，其初始值    为0，在incr之后其值为-1。如果value的值不能转成整型，如hello，该操作将执      行失败并返回相应的错误信息。



## 2．存储lists类型

在Redis中，List类型是按照插入顺序排序的字符串链表。和数据结构中的普通链表    一样，我们可以在其头部\(left\)和尾部\(right\)添加新的元素。在插入时，如果该键并不   存在，Redis将为该键创建一个新的链表。与此相反，如果链表中所有的元素均被移 除，那么该键也将会被从数据库中删除。List中可以包含的最大元素数量是       4294967295。

                  从元素插入和删除的效率视角来看，如果我们是在链表的两头插入或删除元素，这将                  会是非常高效的操作，即使链表中已经存储了百万条记录，该操作也可以在常量时间                  内完成。然而需要说明的是，如果元素插入或删除操作是作用于链表中间，那将会是                  非常低效的。相信对于有良好数据结构基础的开发者而言，这一点并不难理解。

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image001.png)

1）**lpush key value1 value2...**：在指定的key所关联的list的头部插入所有的 values，如果该key不存在，该命令在插入的之前创建一个与该key关联的空链 表，之后再向该链表的头部插入数据。插入成功，返回元素的个数。

2）**rpush key value1、value2…**：在该list的尾部添加元素

3）**lrange key start end**：获取链表中从start到end的元素的值，start、end可    为负数，若为-1则表示链表尾部的元素，-2则表示倒数第二个，依次类推…

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image002.png)

4）**lpushx key value**：仅当参数中指定的key存在时（如果与key管理的list中没    有值时，则该key是不存在的）在指定的key所关联的list的头部插入value。

5）**rpushx key value**：在该list的尾部添加元素

## 3．存储sets类型

在Redis中，我们可以将Set类型看作为没有排序的字符集合，和List类型一样，我   们也可以在该类型的数据值上执行添加、删除或判断某一元素是否存在等操作。需要 说明的是，这些操作的时间是常量时间。Set可包含的最大元素数是4294967295。

和List类型不同的是，Set集合中不允许出现重复的元素。和List类型相比，Set类     型在功能上还存在着一个非常重要的特性，即在服务器端完成多个Sets之间的聚合计  算操作，如unions、intersections和differences。由于这些操作均在服务端完成，  因此效率极高，而且也节省了大量的网络IO开销

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image001.png)   
 1）sadd key value1、value2…：向set中添加数据，如果该key的值已有则不会      重复添加

2）smembers key：获取set中所有的成员

3）scard key：获取set中成员的数量

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image002.png)

4）sismember key member：判断参数中指定的成员是否在该set中，1表示存       在，0表示不存在或者该key本身就不存在

5）srem key member1、member2…：删除set中指定的成员

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image003.png)

                    6）srandmember key：随机返回set中的一个成员

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image004.png)

7）sdiff sdiff key1 key2：返回key1与key2中相差的成员，而且与key的顺序有     关。即返回差集。


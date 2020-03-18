# Why
1. 数据库为什么使用B-树的存储模式？
2. 为什么不用二叉查找树（时间复杂度O(logN)）？

答:逻辑上二叉查找树的查找速度和比较速度都是最小的，但是要靠路磁盘IO，数据库索引是存储在磁盘上的，当数据较大的时候缩印的大小可能几个G甚至更多，所以当利用索引查询的时候，不能把整个索引都加载到内存，要逐一加载每一个磁盘，这里的磁盘对应的就是索引树的节点。

二叉查找树查找元素的次数是由树的深度决定的，所以要提高查找效率就要减少树的深度，把“瘦高”变成“矮胖”

# what
B树是一种多路平衡查找树，它的每个节点最多包含K个孩子，k被称为B树的阶。k的大小取决于磁盘页的大小
## B树特征
1. 根节点至少有两个子女。
2. 每个中间节点都包含k-1个元素和k个孩子，其中m/2<=k<=m
3. 每一个叶子节点都包含k-1个元素，其中m/2<=k<=m
4. 所有的叶子节点都位于同一层。
5. 每个节点中的元素从小到大排序，节点当中k-1个元素正好是k个孩子包含的元素的值域分划。


例如:
3阶B—树
 ![image](http://mmbiz.qpic.cn/mmbiz_jpg/NtO5sialJZGoBqEgxvibyZ3S2O8GG2Ecibs5LFXxLRhEVH55mqXMhGf9C2icZ4iaFcBhUhABThvG0hiaoD1z4icgQJbbA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1)

#  How
## 查找
## 插入
自顶向下查找4的节点位置，发现4应当插入到节点元素3，5之间
![image](http://mmbiz.qpic.cn/mmbiz_jpg/NtO5sialJZGrbI83f8meYn1UlzibHGBcqRYk5euDrbMgVubTR4gvIL3U4LdicK3Fu1f7ATq9tGChL9YLXibJeuCTCA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1)
节点3，5已经是两元素节点，无法再增加。父亲节点 2， 6 也是两元素节点，也无法再增加。根节点9是单元素节点，可以升级为两元素节点。于是拆分节点3，5与节点2，6，让根节点9升级为两元素节点4，9。节点6独立为根节点的第二个孩子。![image](http://mmbiz.qpic.cn/mmbiz_jpg/NtO5sialJZGrbI83f8meYn1UlzibHGBcqRSBqOKQc3NYe8HA7mX4QjSjUpfkWLqGBfWm8wHmic7qajFaickGSTX8NA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1)
## 删除
自顶向下查找元素11的节点位置。
![image](http://mmbiz.qpic.cn/mmbiz_jpg/NtO5sialJZGrbI83f8meYn1UlzibHGBcqRfnGwPGrX4NhnkJfBuKqNdXAyHWJiaBd5b0ruibuhHJvUnmCYpt1eJxdg/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1)
删除11后，节点12只有一个孩子，不符合B树规范。因此找出12,13,15三个节点的中位数13，取代节点12，而节点12自身下移成为第一个孩子。（这个过程称为左旋）![image](http://mmbiz.qpic.cn/mmbiz_jpg/NtO5sialJZGrbI83f8meYn1UlzibHGBcqRta9bOwANA1gjzduWKKAhl5N9IkFgZPNYNNzzdByY7LYiautLXVNnJ4w/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1)![image](http://mmbiz.qpic.cn/mmbiz_jpg/NtO5sialJZGrbI83f8meYn1UlzibHGBcqRbDOA2SYhksUO3BlibgZciauxcTZFfXobCnFwPegUjLHYqwq4CW7gTYGg/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1)
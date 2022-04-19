# 常用 STL

## vector 线性容器

vector 的特点

- 随即访问元素效率很高

- push_back 的效率也很高

- push_front 的效率非常低，不建议使用

- insert 需要把插入位置后的元素全部后移，效率比较低下

- erase 需要把删除位置后面的元素全部前移，效率比较低下

- 当内存不够时，需要重新申请内存，再把以前的元素复制过来，效率也比较低

## map

map 的本质是一类关联式容器，属于模版类关联的本质在于元素的值与某个特定的键相关联，而并非通过
元素在数组中的位置类获取。
它的特定是增加和删除节点对迭代器的影响很小，除了操作节点，对其他的节点都没什么影响，对迭代器来说，
不可以修改键值，只能修改其对应的实值。
map 内部数据的组织，map 内部自建一颗红黑树(一种非严格意义上的平衡二叉树)，这棵树具有对数据自动
排序的功能，所以在 map 内部所有的数据都是有序的

实例，每个学生的学号跟他的姓名就存在着一一映射的关系，这个模型可以用 map 轻易描述

```
Map<int, string>mapStudent;
```

根据 key 值快速查找记录，查找的复杂度基本是`log(n)`，如果有 1000 条记录，最多查询 10 次；
100 万条记录，最多查询 20 次。
除此之外，还能快速插入`key-value`记录、快速删除记录和根据`key`快速修改`value`

序列容器，如`vector`, `list`, `deque`

map 是集合容器

## map 的原理

红黑树 R-B Tree，一种二叉查找树，但在每个节点上增加一个存储位表示节点的颜色`Red`和`Black`。
通过对任何一条从根到叶子的路径上各个节点着色方式的限制，红黑树确保没有一条路径会比其他路径长出两倍，
因而是接近平衡的

红黑树作为一颗二叉查找数，满足二叉查找树的一般性质

二叉查找树，也称有序二叉树(ordered binary tree)，或已排序二叉树，是指一颗空树或者具有下列
性质的二叉树

- 若任意节点的左子树不空，则左子树上所有节点的值均小于它的根节点的值

- 若任意节点的右子树不空，则右子树上所有节点的值均大于它的根节点

- 任意节点的左、右子树叶分别为二叉查找树

- 没有键值相等的节点(no duplicate node)

因为一颗由 n 个节点随机构造的二叉查找树的高度为`lgn`，所以二叉查找树的一般操作的执行时间
为`O(lgn)`。
但二叉查找树若退化成了一棵具有 n 个节点的线性链后，则这些操作最坏运行时间为`O(n)`

红黑树虽然本质上是一棵二叉查找树，但它在二叉查找树的基础上增加了着色和相关的性质使得红黑树
相对平衡，从而保证了红黑树的查找、插入、删除的时间复杂度最坏为`O(lgn)`

**它是如何保证一棵 n 个节点的红黑树的高度始终保持在`lgn`的呢**

- 每个节点要么是红的要么是黑的

- 根节点是黑的

- 每个叶节点(叶节点即指树尾端`NIL`指针或`NULL`节点)都是黑的

- 如果一个节点是红的，那么它的两个儿子都是黑的

- 对于任何节点而言，其到叶节点树尾端`NIL`指针的每条路径都包含相同数目的黑节点

当在对红黑树进行插入和删除等操作时，对树做了修改可能会破坏红黑树的性质，为了继续保持红黑树的性质，
可以通过对节点进行重新着色，以及对树进行相关的旋转操作，即通过修改树中某些节点的颜色及指针结构，
来达到对红黑树进行插入或删除节点等操作以继续保持它的性质或平衡的目的

树在经过左旋右旋之后，树的搜索性质保持不变，但树的红黑性质被破坏了，所以红黑树插入和删除数据后，需要利用
旋转与颜色重涂来重新恢复树的红黑性质

## set

C++ STL 之所以得到广泛的赞誉，不只是提供了像`vector`, `string`, `list`等方便的容器，更重要的是封装了
许多复杂的数据结构算法和大量常用数据结构操作

vector 封装数组，list 封装了链表，map 和 set 封装了二叉树等。在封装这些数据结构的时候，提供了常用的插入、
排序、删除、查找等

set 作为一个容器也是用来存储同一数据类型的，并且能从一个数据集合中取出数据，在 set 中每个元素的值都是唯一的，
而且系统能根据元素的值自动进行排序。

C++ STL 中标准关联容器 set，multiset，map，multimap 内部采用的都是红黑树，红黑树的统计性能要好于一般
平衡二叉树

为何 map 和 set 的插入删除效率比用其他序列容器高

表面上看，因为对于关联容器来说，不需要做内存拷贝和内存移动，set 容器所有元素都是以节点的方式来存储，
其节点结构和链表差不多。
因此插入的时候只需要稍做变换，把节点的指针指向新的节点就可以了。删除的时候类似，稍做变换后把指向
删除节点的指针指向其他节点即可。
这里的一切操作就是指针换来换取，和内存移动没有关系

**为何每次 insert 后，以前保存的 iterator 不会失效**

iterator 这里就相当于指向节点的指针，内存没有变，指向内存的指针怎么会失效呢，当然被
删除的那个元素本身已经失效了。

对于 vector 来说，每一次删除和插入，指针都有可能失效。即使是调用`push_back`在尾部插入也是如此，
容器内部空间可能不够，需要一块新的更大内存，只有把以前的内存释放，并申请新的更大的内存，并复制已有的数据
元素到新的内存，最后把需要插入的元素插入来解决

特别是在和`find`等算法在一起使用的时候，要牢记`不要使用过期的iterator`

**当数据元素增多时，set 的插入和搜索速度如何变化**

在 set 中查找是使用二分查找，也就是说有 16 个元素需要比较 4 次，有 32 个元素，最多比较 5 次，那么有 10000 个
元素时最多为 14 次，如果是 20000 个元素，最多不过 15 次
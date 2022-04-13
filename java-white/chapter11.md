# 对象容器--集合

集合本质是基于某种数据结构数据容器。常见的数据结构，数组(Array)、集合(Set)、队列(Queue)、链表(Linkedlist)、
树(Tree)、堆(Heap)、栈(Stack)、映射(Map)等

Java 中提供了丰富的集合接口和类，它们来自于 java.util 包，Java 集合类型分为 Collection 和 Map，
Collection 子接口有 Set、Queue 和 List 接口

## List 集合

List 集合的元素是有序的，可以重复出现。

List 接口的实现类有，ArrayList 和 LinkedList。ArrayList 是基于动态数组数据结构的实现，LinkedList 是
基于链表数据结构的实现。ArrayList 访问元素速度优于 LinkedList，LinkedList 占用的内存空间比较大，但 LinkedList
在批量插入或删除时优于 ArrayList

不同的结构对应不同的算法

List 接口继承自 Collection 接口，List 接口中的很多方法都是继承自 Collection 接口

### 常用方法

List 接口继承自 Collection 接口，List 接口中的很多方法都继承自 Collection 接口

**操作元素**

- get(int index)，返回 List 集合中指定位置的元素

- set(int index, Object element)，用指定的元素替换 List 集合中指定的位置的元素

- add(Object element)，在 List 集合的尾部添加指定的元素，该方法从 Collection 集合继承过来的

- add(int index, Object element) 在 List 集合的指定位置插入指定元素

- remove(int index)，移除 List 集合指定位置的元素

- remove(Object element)，如果 List 集合中存在指定元素，则从 List 集合中移除第一次出现的指定元素，该方法从 Collection
  继承过来的

- clear()，从 List 集合中删除所有元素，该方法从 Collection 继承过来的

**判断元素**

- isEmpty()，判断 List 集合中是否有元素，没有返回 true，该方法从 Collection 集合继承过来的

- contains(Object element)，判读 List 集合是否包含指定元素，包含返回 true，该方法从 Collection 集合继承过来的

**查询元素**

- indexof(Object o)，从前往后查找 List 集合元素，返回第一次出现指定元素的索引，如果不包含该元素，则返回-1

- lastIndexof(Object o) 从后往前查找 List 集合元素，返回第一次出现指定元素索引，如果此列表不包含该元素，则返回-1

**其他**

- iterator()，返回迭代器对象，迭代器对象用于遍历集合，该方法从 Collection 集合继承下来的

- size() 返回 List 集合中的元素数，返回值时 int 类型

- subList(int fromIndex, int toIndex)，返回 List 集合中指定 fromIndex(包含)和 toIndex(不包含)之间
  的元素，返回值 List 集合

> Java 中任何集合中存放的都是对象，即引用数据类型，基本数据类型不能放到集合中。整数可以放到集合，这个过程中
> 发生了自动装箱

### 遍历集合

集合最常用的操作之一是遍历，遍历就是将集合中的每一个元素取出来，进行操作或计算

List 集合遍历有三种方法

- for 循环遍历，for 循环中有循环变量，通过循环变量可以访问 List 集合中的元素

- for-each 循环遍历，for-each 循环是针对遍历各种类型集合而推出的

- 使用迭代器遍历，Java 提供了多种迭代器，List 集合可以使用 Iterator 和 ListIterator 迭代器

```java
List list = new ArrayList();
list.add("A");
list.add("B");
list.add("C");
Iterator it = list.iterator();

while(it.hasNext()) {
  Object item  = it.next();
  String s = (String)item;
  System.out.println("读取集合元素：" + s);
}
```

## Set 集合

Set 集合是由一串无序的，不能重复的相同类型元素构成的集合。不能通过类似于 List 集合的序号访问，而且不能重复

Set 接口直接实现类主要是 HashSet，HashSet 是基于散列表数据结构实现。Set 接口也继承自 Collection 接口

**操作元素**

- add(Object element) 在 Set 集合尾部添加指定的元素

- remove(Object element) 删除 Set 集合中指定元素

- clear() 从 Set 集合中删除所有元素

**判断元素**

- isEmpty() 判断 Set 集合中是否有元素，没有返回 true

- contains(Object element) 判断 Set 集合是否包含指定元素，包含返回 true

**其他**

- iterator 返回迭代器(Iterator)对象

- size() 返回 Set 集合中的元素数，值是 int 类型

Set 集合只能通过 for-each 循环和迭代器进行遍历

## Map 集合

Map 集合是一种非常复杂的集合，允许按照键来访问元素。Map 集合由两个集合构成，一个是键(key)集合，一个是值(value)集合，
键集合是 Set 集合类型，不能有重复，值集合是 Collection 类型，可以有重复的元素

Map 接口直接实现是 HashMap，HashMap 是基于散列表数据结构实现

### 常用方法

**操作元素**

- get(Object key) 返回指定键所对应的值，如果 Map 集合中不包含该键值对，则返回 null

- put(Object key, Object value) 指定键值对添加到集合中

- remove(Object key) 移除键值对

- clear() 移除 Map 集合中所有键值对

**判断元素**

- isEmpty() 判断 Map 集合中是否有键值对，没有返回 true

- containsKey(Object key) 判断键集合中是否包含指定元素，包含返回 true

- containsValue(Object value) 判断值集合中是否包含指定元素

**查看集合**

- keySet() 返回 Map 中的所有键集合，返回值是 Set 类型

- values() 返回 Map 中的所有值集合，返回值是 Collection 类型

- size() 返回 Map 集合中键值对数

Map 集合添加键值对需要注意，如果键已经存在，则会替换原有值
如果键不同，值相同，则会添加，值可以重复

Map 有两个集合，遍历过层可以只遍历值的集合，也可以只遍历键的集合，也可以同时遍历

```java
Map map = new HashMap();
Set keys = map.keySet();
Collection values = map.values();
```

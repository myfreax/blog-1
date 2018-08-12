# 数据结构

一些约定：

- 下面讨论的都是一般情况下的标准数据结构描述，比如链表一般情况下不支持索引操作

一些通用技巧：

- 数据结构中存在大量的正反推理，比如从树的遍历结果重新建立树、从序列判断是栈还是队列等等
- 时 / 空交换
- 用自己的话来描述算法的结构，对代码实现很有帮助

## 结构

- 物理结构
  - 连续固定内存分配
  - 链式动态内存分配

- 逻辑结构：逻辑结构不同的数据结构，其物理结构可以是一样。像栈和队列，其实物理结构是一样的，可以同为连续固定或链式动态。只是指针操作不相同造成的逻辑结构不同，比如栈的头指针总是指着最后的 item，从而形成先进后出的逻辑结构，队列的头指针总是指向第一个 item，所以形成先进先出的逻辑结构

- 线性结构
- 非线性结构

## 代码实现

- 线性结构：数组、链表、栈/队列等，代码实现相对容易，普遍以指针在线性结构上如何摆放、如何移动为主
- 非线性结构：二叉树、二叉搜索树、散列表、图等，代码实现相对困难，夹杂着额外需要处理的事项，如二叉搜索树的删除操作、散列表的冲突解决

##  Array

- 特点
  - 索引查的时间效率高，可用来作为 hashing 的物理结构
  - 空间效率低，一般固定大小，创建前需要知道大小；增删若超过固定大小，默认出错，可进行扩容，但动态扩容比较影响性能
- 时间复杂度
  - 增：O(1)
  - 删：O(1)
  - 查：O(n)
  - 索引查：O(1)

## Linked List

*特指基本的后向链表。其他的像双向链表、循环链表、双向循环链表、指针随意指的复杂链表等不在这讨论。*

- 特点
  - 空间效率高，天生动态扩容，创建前无需知道大小 (其实这个是链式物理结构的特点，也不是只有链表才是这样，像链式的栈也不需要提前知道大小)
  - 时间效率偏低，因为不支持索引查
- 时间复杂度
  - 增：O(n)
  - 删：O(n)
  - 查：O(n)

> 对于一些复杂的链表，可以分解成 “基本链表 + 其他”

## Stack/Queue

栈和队列之间看似相反，但是也存在着物理结构可以相同的特质。所以会有一些像 “用两个队列实现标栈功能” 或 “用两个栈实现队列功能” 的问题。

### Stack

- 特点
  - 先进后出，适用于反转、颠倒输入输出等场景
- 时间复杂度
  - 增：O(1)
  - 删：O(1)
  - 查：O(n)

> 栈搭配其他数据结构来解决问题是比较常用的技巧

### Queue

*特指基本的先进先出队列。其他的像循环队列、双端队列等不在这讨论。*

- 特点
  - 先进先出
- 时间复杂度
  - 增：O(1)
  - 删：O(1)
  - 查：O(n)

> 对于一些复杂的队列，可以分解成 “基本的 + 其他”；队列搭配其他的数据结构是比较常用的技巧

## Tree

- 伴随着递归
- 指针操作复杂
- 树的遍历分两类：纵向遍历和横向遍历 (类似图的深度优先和广度优先)
- 树的路径问题优先使用纵向遍历，特别是前序遍历
- 树的层级问题优先使用层序遍历
- “画出最小树 + 标明指针 + 树的递归遍历模型填空” 是使用树的递归模型来解决问题的一般性方法

### Binary Tree

- 特点
  - 最常见
  - 每个节点的子节点数量 <= 2
  - 关键结构
    - 父亲节点
    - 左子树
    - 右子树
  - 遍历 (查)
    - 前序遍历：root 总在遍历结果序列的第一个
    - 中序遍历：root 的左边总是左子树，右边是右子树
    - 后序遍历：root 总在遍历结果序列的最后一个
- [代码实现](https://github.com/hsxhr-10/blog/blob/master/LeetCode/binary_tree_traverse.py)
- 时间复杂度 (这里偏向完全二叉树，不讨论退化成链表的树)
  - 查：O(logn)

### Binary Search Tree

- 特点
  - 左二字节点 < 父节点
  - 右儿子节点 > 父节点
- 代码实现
  - 增
  - 查
- 时间复杂度 (要看数据的分布情况，可能会退化成链表，可以通过引入随机算法尽量避免，具体做法是在创建子节点时随机选取)
  - 增：[O(log(n)), O(n)]
  - 删：[O(log(n)), O(n)]
  - 查：[O(log(n)), O(n)]

## Hashing

精华在于通过散列函数将值转换成散列表的下标，这样就可以 O(1) 的时间复杂度完成查询。效率/高效/高性能的代名词，空间换时间的杰出代表，你值得拥有。

- 特点
  - 散列函数
    - 整数
    - 浮点数
    - 字符串 (参考：http://www.cse.yorku.ca/~oz/hash.html)
  - 处理冲突
    - 拉链法：表大小一般跟元素个数差不多。当发生冲突时，以链表的形式存放冲突的 item
    - 线性探测法：表大小一般为元素个数的 2 倍。当发生冲突时，即 table[hash(key)] 非空时，new_hash = (old_hash + 1) % M
  - 载荷因子
    - 载荷因子 = 表中元素个数 / 表长度。因子越大代表冲突的可能性越大，反之越小。一般限制在 0.7 ~ 0.8 以下，若超出则需要 resize 表操作
    - 对于拉链法的散列表：负荷因子代表每条链的平均长度，一般大于 1
    - 对于线性探测法的散列表：负荷因子代表已被占用的表空间比例，不会大于 1
- [代码实现](https://github.com/hsxhr-10/blog/blob/master/LeetCode/hash_function.py)
- 时间复杂度
  - 增：O(1)
  - 删：O(1)
  - 查：
    - 平均：O(1)
    - 最好：O(1)
    - 最差：O(n)

## Graph

- TODO

# 算法

## Enumeration

枚举法是最直接和淳朴的方法，当其他的方法都尝试无果时，枚举将是解决问题的最后保障。很多时候也可以从这个保底算法出发进行优化，从而更自然地得到效率更高的算法。

- 常用的优化方法
  - O(n) => O(n - k)：先过滤掉一部分不符合要求的数据，减少待处理的数据量，即减少循环的次数
  - O(n^2) => O(n(n - k))：同上先过滤；或者记录下不符合要求且后面会重复出现的数据，以减少内层循环的次数

## Divide And Conquer

- 判断是否能使用分治法
  - 是否能划分出小规模的同性质子问题
  - 子问题是否能简单到一眼就看出答案

- 分治法类型
  - 一维分治
  - 树形分治
  - 二维分治

- 经典的两种分治方式
  - 归并排序/快速排序
  - 二分查找/快速查找

- 下图就是经典的一维分治

![](https://raw.githubusercontent.com/hsxhr-10/picture/master/一维分治算法图解.png)

## Search

### Binary Search

- 代码实现描述
  - 核心思想：分治
  - 实现标签：选中间数比较
  - 二分查找 = 算出中间点 + 递归划分
- [代码实现](https://github.com/hsxhr-10/blog/blob/master/LeetCode/binary_search.py)
- 时间复杂度
  - 平均：O(logn)
  - 最优：O(1)
  - 最差：O(logn)
- 空间复杂度
  - 循环：O(1)
  - 递归：O(logn)

### Hashing Search

- [参考](https://github.com/hsxhr-10/blog/blob/master/LeetCode/README.md#hashing)
- 在第一次遍历的时候顺便做 hashing 是个实用的技巧

### Binary Search Tree

- [参考](https://github.com/hsxhr-10/blog/blob/master/LeetCode/README.md#binary-search-tree)

### Quick Select

- 就是快排的挖坑填数，[参考](https://github.com/hsxhr-10/blog/blob/master/LeetCode/README.md#quick-sort)
- [代码实现](https://github.com/hsxhr-10/blog/blob/master/LeetCode/quick_select.py)
- 时间复杂度
  - 平均：O(n)
  - 最优：O(n)
  - 最差：O(n^2)。和快速排序同理
- 空间复杂度：O(1)

## Sorting

### Quick Sort

- 代码实现描述
  - 核心思想：分治
  - 难点：划分的实现
  - 实现标签：挖坑填数
  - 划分 = 基数选择 (可引入随机选择) + 计数器初始化 + 大循环条件 while (i != j) + j 从后往前移动，找比基准数小的数，找到就交换 arr[i] 和 arr[j]，i 后移一位 + i 从前往后移动，找比基准数大的数，找到就交换 arr[i] 和 arr[j]，j 前移一位 + 最后返回基准数位置 i 或 j
- [代码实现](https://github.com/hsxhr-10/blog/blob/master/LeetCode/quick_sort.py)
- 时间复杂度
  - 平均：O(nlogn)
  - 最优：O(nlogn)
  - 最差：O(n^2)。以下两种情况会容易造成最差情况：1. 对于一个已经排好序的数组，如果基数选择是以第一个或者最后一个为准的；2. 基准数固定的选择不一定是第一个或者最后一个，但是有一个固定形式，很巧每次选的基准数都是最大或者最小值。这两种情况都会导致快速排序退化成类似冒泡算法，解决方法是通过引入随机算法可以尽量避免最差情况，具体是在选基准数时随机选择
- 空间复杂度：O(logn)。对于一般的递归版原地快速排序而言，空间消耗体现在递归调用栈，所以空间复杂度对应递归深度

### Merge Sort

- 代码实现描述
  - 核心思想：分治
  - 难点：原地二路归并的实现
  - 实现标签：辅助数组排序原数组
  - 原地二路归并 = 计数器初始化 + 辅助数组初始化 + 大循环条件 for (k=start, k<= end, k++) + 四个条件判断：1. 左边取完了，a[k] = a[j], j++；2. 右边取完了，a[k] = a[i], i++；3. 左边当前小于右边当前，a[k] = a[i], i++；4. 右边当前小于左边，a[k] = a[j], j++
- [代码实现](https://github.com/hsxhr-10/blog/blob/master/LeetCode/merge_sort.py)
- 时间复杂度
  - 平均：O(nlogn)
  - 最优：O(nlogn)
  - 最差：O(nlogn)
- 空间复杂度：O(n)。对于原地归并而言

### Count Sort

计数排序是一种非比较排序方式。

- 代码实现描述
  - 核心思想：下标原生有序
  - 优化提升：将 arr_counter 换成 byte 类型或者 bit 类型, 可以很好地降低计数排序的空间复杂度
- [代码实现](https://github.com/hsxhr-10/blog/blob/master/LeetCode/count_sort.py)
- 时间复杂度
  - 平均：O(n + k)
  - 最优：O(n + k)
  - 最差：O(n + k)
- 空间复杂度：O(n + k)

## String

### BM

- 代码实现描述
  - 核心思想：从后往前比较 + 匹配串的移动位置
  - 核心步骤
    - 被匹配串和匹配串头部对齐
    - 从后往前匹配
    - 当字符匹配时出现好后缀情况，此时匹配串的移动位置 = 好后缀最后一个字符的位置 - min(好后缀中各个字符在匹配串中第一次出现的位置)
    - 当遇到不匹配的字符时则出现坏字符情况，分两种情况
      - 坏字符不在匹配串中，此时匹配串的移动位置 = 坏字符位置 - (-1)
      - 坏字符在匹配串中，此时匹配串的移动位置 = 坏字符位置 - 坏字符在匹配串中第一次出现的位置
    - 最终匹配串的移动位置 = min(最好后缀移动位置, 最坏字符移动位置)
    - 循坏上述步骤，知道匹配串移动到匹配的位置，或者匹配失败
- 时间复杂度
  - n 代表被匹配串长度；m 代表匹配串长度  
  - 平均：O(n/m)
  - 最优：O(n/m)
  - 最差：O(nm)
- 空间复杂度：O(1)

## Graph

- TODO

## Greedy

- 判断是否能使用贪心法
  - 是否存在最优子结构：结果的最优是否包含每一步的最优
  - 是否存在最优选择：到达结果最优之前的每一步，是否都存在最优选择

# 时间复杂度

- 时间复杂度重点描述的是一种变化趋势，大致上地说明了一个算法随着处理数据量的增加，所需的运算次数的增加情况。

- 直观图
  - ![](https://raw.githubusercontent.com/hsxhr-10/picture/master/时间复杂度.png)

- 常见的时间复杂度代表
  - 查找一个碰撞少的散列表会得到 O(1) 的时间复杂度
  - 查找一颗平衡树会得到 O(log(n)) 的时间复杂度
  - 查找一个一维阵列会得到 O(n) 的时间复杂度
  - 好的排序算法会得到  O(n*log(n)) 的时间复杂度
  - 差的排序算法会得到  O(n^2) 的时间复杂度
  








































































































































































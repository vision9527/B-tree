# B+树理论与实现
#### 1、B+树出现的背景（大量数据->磁盘搜索）
* 应用数据极速变多，在内存中已经不能经济实惠的保存大量搜索的数据，所以考虑通过磁盘来帮助搜索，需要将全部或部分数据保存在磁盘。

#### 2、B+树特点

1. 平衡多叉树，从根节点到所有叶子节点的高度相同
1. 叶子节点之间通过指针互连，方便遍历叶子节点
#### 3、B+树结构
1. 节点：主要是**索引key**和**值value**(统称)的集合，一般一个节点都保存在一个磁盘的block上，一次IO读取一个block
1. 内部节点组成
    * 内部节点的元素为**索引key**和关联的**子节点指针**
    * 根节点是特殊的内部节点
1. 叶子节点组成
    * 叶子节点的元素为**索引key**和**实际数据或指向实际数据的地址**
    * 兄弟叶子节点之间通过指针互连
1. 左子树总是比右子树小，每个节点内部的索引key都是有序排列


![B+树数据结构图示](./btree.png)

#### 4、节点大小
1. fanout：节点可包含的子节点数量
1. 如何计算fanout？
    * 内部节点：假设，n个索引，n+1指针，block大小4096B，一个指针4B，一个索引4B
        * 4\*n+4\*(n+1) = 4096
        * n = 512
    * 叶子节点：假设，n个索引，n+1指针，block大小4096B，一个数据64B，一个索引4B
        * 64\*n+4\*(n+1) = 4096
        * n = 60
1. 最大、最小节点的数量，n代表key的数量
    * 根节点含子节点的个数：2<= node <= n+1
    * 内部节点包含子节点的个数：(n+1)/2 < node <= n+1
    * 叶子节点包含数据的个数：n/2 < node <= n

1. 一般3层高度的B+树即可保存千万级别的数据，如何计算一颗B+树能保存的数据量？
    * 根据以上的计算数据可推出，512 * 512 * 60 = 15728640
    * 约保存1千万左右数据

#### 5、查询
#### 6、更新
#### 7、删除
#### 8、增删改的时间复杂度、空间复杂度、填充因子
#### 9、其它：
1. 聚簇索引、非聚簇索引
1. 前缀压缩
1. 范围查询
1. 重复key
1. 批量加载
1. 并发读写
1. 合并、分裂策略

#### 10. mysql选择B+树而不是B树原因
1. B+树内部节点不保存具体数据，只保存在叶子节点
1. B+树查询在范围查询效率更高，B+树在范围查询可以根据叶子节点的链接直接顺序遍历，B树需要遍历完子树才能完成范围查找，访问的磁盘IO次数更多
1. B+树查询效率更稳定，B+树每次访问数据都会到达叶子节点，查询时间稳定，而B树访问的数据不确定在第几层所以查询效率不太稳定
1. B+树访问磁盘数更少，由于结构原因，B+树更矮胖访问的磁盘树更少，B树更高访问的磁盘树更多
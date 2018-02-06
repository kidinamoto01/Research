IAVL Tree

Note: Requires Go 1.8+

一种带版本的，不可变的AVL树，用于存储持久化数据。


这种数据类型是为了存储提供处久化的键值对存储，例如<账户-余额>。它可以用于计算确定的Merkle根值。树的结构是平衡的，所以所有操作的复杂度为O(log(n))。

树上的节点是不可变的且通过哈希值建立索引。因此，任何节点都可以作为一个不可变的快照，让我们从mempool方便地处理未提交的事务，并且我们可以立即回滚到最后提交的状态来处理新提交的块的事务（可能不是那些与那些事务相同的事务 从mempool）。


在AVL树中，任何节点的两个子树的高度最多相差一。 每当更新违反此条件时，通过创建指向旧树的未修改节点的O（log（n））个新节点来重新平衡树。 在原始的AVL算法中，内部节点也可以保存键值对。 AVL+算法（注意+号）修改AVL算法以保留叶节点上的所有值，而只使用分支节点存储key。 这简化了算法，同时保持merkle散列轨迹简短。

在以太坊中与之对应的是Patricia tries。 这里有折衷。 Key在插入IAVL+树之前不需要被哈希，因此这提供了在密钥空间中更快的迭代，这可能有益于一些应用。 逻辑实现起来比较简单，只需要两种类型的节点 - 内部节点和叶节点。 另一方面，虽然IAVL +树提供了一个确定性的merkle根哈希，它取决于交易的顺序。 实际上，这不应该成为问题，因为在序列化树内容时可以高效地编码树结构。
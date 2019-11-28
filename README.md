# 时间复杂度

## 从小到大排序

`O(1) -> O(n) -> O(logn) -> O(nlogn) -> O(n^2) -> O(n^3) -> O(2^n)`

> 时间复杂度越大, 即随着数据规模的增大, 算法所需时间的增长率就越大

## 所有对数均可看作logn

如`log2(n)`和`log5(n)` : `log2(n) = log5(2) * log2(n)`, 可以省去常数项系数`log5(2)`, 也即`log2(n) <=> log5(n)`; 从函数图像角度来看, 随n的变大, 两函数增长率变大的幅度处于同一个级别。

# 小技巧

1.  如果递归的过程中有很多重复计算, 考虑将依赖的计算先算出来并做缓存
2.  避免使用`HashMap/HashTree/HashSet`做缓存, 而使用数组等简单数据结构
3.  尽量使用贴近题目规则的数据结构辅助解题, 如约瑟夫环问题可使用循环链表
4.  对于解题过程中感觉无从下手的, 子数据集计算过程如出一辙的, 可考虑采用递归进行分而治之, 如题目`括号的分数`
5.  避免使用`*, /, %, 乘方, 开方`, 而多使用`^, &, >>`
    
    1.  `n % m` => `n - (m > n ? 0 : m) `(`n >= 0, m > 0, n < 2m` `)

# 树

## 二叉树的性质

1.  非空二叉树的第i层的节点数最多为`2^(i-1)`
2.  高为h的非空二叉树节点数最多为`2^(h-1)`
3.  节点的子节点个数称为该节点的度, 树的度为树中节点的度的最大值, 二叉树是度为2的树
4.  节点的高度是该节点到叶子节点的最长路径经过的节点数, 包括他自己和叶子节点; 节点的深度是该节点到根节点路径中经过的节点数, 包括他自己和根节点; 树的高度就是树的深度
5.  二叉树的边数之和`T`等于
    1.  从节点的度的角度来看, 就是`T = 度为1的节点数*1 + 度为2的节点*2`, 即`T = n1 + 2 * n2`
    2.  从每条边都会指向一个节点来看, 只有根节点没有被指向, 因此为`T = 节点数 - 1`, 即`T = n0 + n1 + n2 - 1`
    3.  综上有`n0 = n2 + 1`, 使用满二叉树举例可快速验证
6.  真二叉树: 只有度为0和2的节点; 满二叉树: 在真二叉树的条件下, 度为0的节点都是叶子节点; 完全二叉树: 只有最后两层包含叶子节点, 且最后一层中的叶子节点从左至右依次排列

## 完全二叉树的性质

1.  完全二叉树: 度为1的节点树要么为1个要么为0个; 节点数相同的不同二叉树中完全二叉树的高度最小
2.  若完全二叉树的高度为`h`, 则有
    1.  前`h-1`层为满二叉树, 最后一层至少有1个节点, 因此节点数`n >= [2^(h-1) - 1] + 1`, 即`n >= 2^(h-1)`
    2.  高为`h`的二叉树节点数最多为`2^h - 1`, 因此总结点数`n < 2^h - 1`
    3.  综上有: `2^(h-1) <= n < 2^h`, 即`h-1 <= log2(n) < h`, 由于`h`只能取整, 因此`h = floor(log2(n)) + 1`, `floor`为向下取整, `ceiling`为向上取整; 向上向下取整与四舍五入无关, 仅看小数位数是否为零, `ceiling(4.0) = 4`, `ceiling(4.1
    ) = ceiling
    (4.8
    ) = 5`.
3.  若按从上到下, 从左到右的顺序对节点从1开始进行编号, 那么对于某个节点`i`有:
    1.  `i = 1`, 根节点
    2.  `i > 1`, 他的父节点编号为`floor(i/2)`
    3.  `2i <= n`, 他的左子节点编号为`2i`; `2i > n`, 则无左子节点; `2i + 1 <= n `, 他的左子节点编号为`2i + 1`; `2i + 1 > n`, 则无左子节点; 若从`0`开始编号, 对应将`n`换成`n - 1`即可
4.  已知完全二叉树节点个数`n`, 求叶子节点个数`n0`
    1.  `n = n0 + n1 + n2`
    2.  `n0 = n2 + 1`
    3.  综上, 有`n = 2 * n0 + n1 - 1`, 由于完全二叉树度为1的节点数只可能为0或1, 由此题解
5.  上述`7.iv`中为了避免使用`%2`以判断`n`的奇偶性, 可做如下优化
    1.  当`n`为偶数时, `n1 = 1`, `n0 = n/2`
    2.  当`n`为奇数时, `n1 = 0`, `n0 = (n + 1) / 2 = n/2 + 1/2`
    3.  假设有一个函数`fn`可以综上以上两种情况, `n0 = fn(n)`, 则`fn`应该为`floor`; 因此书写代码时, 以`java`为例, `if-else + %2`可优化为`n0 = (n + 1) >> 1`, 因为`java`中的运算浮点转整型时默认向下取整. 
    4. 总结: `n0 = floor( (n+1)/2 )`, `n1+n2 = floor( n/2 )`

## 二叉树的遍历

1.  先序遍历：拿到根节点就直接访问，然后将其右、左子节点依次入栈
2.  中序遍历：要遍历完左子树才能遍历根节点，借助一个遍历指针初始指向根节点，循环入栈遍历节点并将指针指向左孩子，直到左孩子为空则弹出栈顶节点进行访问，然后将指针指向该节点的右孩子（如果为空，则继续弹出栈顶节点，直到右孩子不为空或栈为空），进入下一次循环
3.  后序遍历：根节点要最后访问，将根节点入栈，栈非空循环，瞥一眼栈顶节点，如果栈顶节点是叶子节点或上次访问的节点是该节点的子节点，则弹出栈顶节点并进行访问，否则依序将其非空的右、左子节点入栈
4.  层序遍历：可借助队列，将根节点入队，队不空循环，出队一个节点进行访问，依序将其不空的左、右子节点入队；如果要求分别每一层有哪些节点，则可借助两个队列或插入null值标识
5.  翻转二叉树：将二叉树中每个节点的左右子节点交换位置，这相当于对每个节点进行访问操作（交换左右指针的值）
6.  前驱节点
    1.  如果存在左子树，前驱节点为左子树的最右节点
    2.  如果没有左子树，则到祖先节点中找：遍历指针沿着父节点依次向上，直到遍历节点是父节点的右子树，那么该父节点就是前驱节点；直到遍历节点没有父节点了，则前驱节点不存在
    3.  同理可推后继节点的查找

## 二叉搜索树（Binary Search Tree, BST）

删除节点，注意不能破环BST的性质

1.  删除度为0的节点（叶子节点），直接解除其父节点指向其的指针（可通过比较两节点的大小来判定是左指针还是右指针）；如果该节点是根节点，则直接将根节点指针置空即可
2.  删除度为1的节点
    1.  节点为根节点，根节点指针指向根节点的孩子节点，并将孩子节点的父指针指向null
    2.  节点不为根节点，将其父节点指向其的指针指向其孩子节点，并将其孩子节点的父指针指向其父节点
3.  删除度为2的节点，将其前驱或后继节点的元素覆盖该节点的，然后再删除其前驱或后继节点，注意其前驱或后继节点的度只有可能是0或1（因为是左子树中最右的节点或右子树总最左的节点）
4.  判断bst节点是否失衡，根据其左右子树的高度计算平衡因子即可

## 多路平衡搜索树——B树

1. 每个节点，所有子树高度相同

2. m阶B树表示节点的子节点个数最多为m，2-3树就是3阶B树，2-3-4树就是4阶B树，以此类推

3. 节点的元素个数限制：
   -   根节点：1 <= x <= m - 1
   -   非根节点：（⌈m/2⌉ - 1） <= x <= （m - 1）；

4. 节点的子节点个数限制：
   -   2 <= x <= m
   -   ⌈m/2⌉ <= x <= m

5. 添加元素：新添加的元素必定会添加到叶子节点中

6. 上溢：当向某个节点添加元素导致该节点元素个数超出限制时，需将该节点中间元素（从左到右第 m>>1 个）沿父指针向上移动，并将左右两边的元素作为该元素的左右子节点

   ![Snipaste_2019-11-26_16-11-06.png](http://ww1.sinaimg.cn/large/006zweohgy1g9bic6xp2ej306s0cmjrs.jpg)

   > 这是唯一一种能让B树长高的情况，即添加元素时，上溢传播到了根节点

7. 删除元素

   1.  当删除叶子节点中的元素时，直接移除即可
   2.  当删除非叶子节点中的元素时，先将其前驱/后继元素覆盖该元素，然后再删除其前驱/后继元素
   3.  某元素的前驱或后继元素必定在叶子节点中
   4.  真正的删除必定发生在叶子节点中

8. 下溢：

   1. 下溢发生在某元素后，该元素所在节点元素个数变为（ ⌊m/2⌋ -2）

   2. 如果下溢节点有元素个数为（⌊m/2⌋）或以上的同层相邻节点，可从该节点“借”一个元素

      1. 将下溢节点A和满足条件的相邻节点B中间的父节点元素复制一份添加到A中

      2. 将B中的最大元素（如果B在A的左边）或最小元素（如果B在A的右边）覆盖父节点元素

      3. 删除B中的最大/最小元素

         <img src="http://ww1.sinaimg.cn/large/006zweohgy1g9bh9qf452j30eo0btwfn.jpg" alt="Snipaste_2019-11-26_15-34-02.png" style="zoom: 67%;" />

   3.  如果下溢节点的同层相邻节点元素小于或等于（⌊m/2⌋-1），这时没办法借了，则合并下溢节点和任意一个相邻节点，并将两者之间的父节点元素也扯下来

       ![Snipaste_2019-11-26_15-38-51.png](http://ww1.sinaimg.cn/large/006zweohgy1g9bhejrcv3j30u5052dgt.jpg)

       1.  合并之后的节点元素个数为（ ⌊m/2⌋ + ⌊m/2⌋ -2），不超过（m-1）
       2.  此操作可能会导致父节点下溢，下溢现象可能沿父节点向上传播

   >  如果下溢调整时，扯下一个父节点元素之后导致父节点没有元素了，此时树的高度减一
   
9. 依序向4阶B树（2-3-4树）中添加1~22这22个元素步骤图：

   ![10-52-49.png](http://ww1.sinaimg.cn/large/006zweohgy1g9dkhr9bfrj314n0lu40s.jpg)

   ![10-58-15.png](http://ww1.sinaimg.cn/large/006zweohgy1g9dkj7hsqcj31ac0n8mzm.jpg)

   ![10-58-43.png](http://ww1.sinaimg.cn/large/006zweohgy1g9dkjmtewlj30j00gdq3t.jpg)

10. 依序删除22~1

    ![11-41-02.png](http://ww1.sinaimg.cn/large/006zweohgy1g9dlrymgn0j30zh0k6dhe.jpg)

    ![11-41-10.png](http://ww1.sinaimg.cn/large/006zweohgy1g9dlryiq22j31300kata2.jpg)

    ![11-41-17.png](http://ww1.sinaimg.cn/large/006zweohgy1g9dlt270dlj31490mh408.jpg)

## 红黑树

### 红黑树的性质

![11-52-26.png](http://ww1.sinaimg.cn/large/006zweohgy1g9dm3jsogzj30qf06rdik.jpg)

上图中的树并不是一棵红黑色，因为绿色路径上只有两个黑色节点，而其他红色路径有3个黑色节点，不符合性质5

### 红黑树 VS 2-3-4树

![12-39-46.png](http://ww1.sinaimg.cn/large/006zweohgy1g9dngs8m86j30qf0btwi5.jpg)

当B树叶子节点元素分别为：红黑红、黑红、红黑、黑。（将红黑树当做2-3-4树来看，只有黑色节点才能独立成为一个B树节点，且其左右红色子节点可以融入该B树节点中）

![14-30-03.png](http://ww1.sinaimg.cn/large/006zweohgy1g9dr5aq4nvj30px096myz.jpg)

### 添加元素

#### 染成红色添加到叶子节点中

![14-30-24.png](http://ww1.sinaimg.cn/large/006zweohgy1g9dr5as961j30p7091dhw.jpg)

新元素的定位逻辑与BST一致，需要注意的是添加之后的调整逻辑。

#### 分情况讨论

![14-30-37.png](http://ww1.sinaimg.cn/large/006zweohgy1g9dr5b7htfj30pp0c6q6b.jpg)

#### 1、添加第一个元素

将节点染成黑色

#### 2、添加的节点作为黑色节点的子节点

染成红色添加即可，无需调整

![15-05-50.png](http://ww1.sinaimg.cn/large/006zweohgy1g9drox9t90j30hk054755.jpg)

#### 3、添加的节点作为红色节点的子节点，但无需上溢

##### LL/RR，单旋

![14-30-57.png](http://ww1.sinaimg.cn/large/006zweohgy1g9dr5bh6rlj30q80bgdig.jpg)

##### RL/LR，双旋

![14-31-07.png](http://ww1.sinaimg.cn/large/006zweohgy1g9dr5bg9epj30qg0b8goa.jpg)

#### 4、添加的节点作为红色的子节点，且需上溢

##### LL上溢

![14-45-03.png](http://ww1.sinaimg.cn/large/006zweohgy1g9dr5arrikj30qb0cagoo.jpg)

##### RR上溢

![14-45-18.png](http://ww1.sinaimg.cn/large/006zweohgy1g9dr5aqgk8j30qf0c776y.jpg)

##### LR上溢

![14-45-28.png](http://ww1.sinaimg.cn/large/006zweohgy1g9dr5aybd6j30qa0buwh5.jpg)

##### RL上溢

![14-45-54.png](http://ww1.sinaimg.cn/large/006zweohgy1g9dr5bik71j30qd0bv417.jpg)

#### 代码实现

##### 二叉搜索树

```java
package top.zhenganwen.learn.algorithm.datastructure.tree;

import top.zhenganwen.learn.algorithm.commons.printer.BinaryTreeInfo;
import top.zhenganwen.learn.algorithm.commons.printer.BinaryTrees;

import java.util.*;

import static java.util.Objects.isNull;

/**
 * @author zhenganwen
 * @date 2019/11/6 17:48
 */
public class BinarySearchTree<E> implements BinaryTreeInfo {

    protected Node<E>       root;

    private int           size;

    protected Comparator<E> comparator;

    public BinarySearchTree() {

    }

    public BinarySearchTree(Comparator<E> comparator) {
        this.comparator = comparator;
    }

    public void add(E element) {
        nonNullCheck(element);

        if (root == null) {
            root = createNode(element, null);
            size++;
            afterAdd(root);
            return;
        }

        Node<E> parent = root, cur = root;
        int compare = 0;
        while (cur != null) {
            parent = cur;
            compare = compare(element, cur.element);
            cur = compare > 0 ? cur.right : compare < 0 ? cur.left : cur;
            if (cur == parent) {
                cur.element = element;
                return;
            }
        }
        Node<E> node = createNode(element, parent);
        if (compare > 0) {
            parent.right = node;
        } else {
            parent.left = node;
        }
        size++;
        afterAdd(node);
    }

    protected void afterAdd(Node<E> node) {

    }

    protected Node<E> createNode(E element, Node<E> parent) {
        return new Node<>(element, parent);
    }

    public void remove(E element) {
        remove(node(element));
    }

    private void remove(Node<E> node) {
        if (node == null)
            return;

        size--;
        if (hasTwoChild(node)) {
            // the node's degree is 2, use node's predecessor/successor's element
            // cover the node, and then delete the predecessor/successor
            Node<E> successor = Objects.requireNonNull(successor(node));
            node.element = successor.element;
            node = successor;
        }

        // reach here, the degree of the node is possible only 0 or 1
        // that is to say, the node only has one child
        Node replacement = node.left == null ? node.right : node.left;
        if (replacement != null) {
            // the node's degree is 1
            replacement.parent = node.parent;
            if (isRoot(node)) {
                root = replacement;
            } else if (compare(node.element, node.parent.element) >= 0) {
                node.parent.right = replacement;
            } else {
                node.parent.left = replacement;
            }
        } else {
            // the node is leaf node
            if (isRoot(node)) {
                root = null;
            } else if (compare(node.element, node.parent.element) >= 0) {
                node.parent.right = null;
            } else {
                node.parent.left = null;
            }
        }
        afterRemove(node);
    }

    protected void afterRemove(Node<E> node) {
        // let auto-balance bst overwrite the method to balance the tree
    }

    private boolean isRoot(Node<E> node) {
        return node.parent == null;
    }

    public boolean contains(E element) {
        return node(element) != null;
    }

    public void clear() {
        root = null;
        size = 0;
    }

    public Node node(E element) {
        Node<E> node = root;
        while (node != null) {
            int compare = compare(element, node.element);
            if (compare == 0)
                return node;
            else if (compare > 0) {
                node = node.right;
            } else {
                node = node.left;
            }
        }
        return null;
    }

    private Node predecessor(Node<E> node) {
        if (node.left != null) {
            node = node.left;
            while (node.right != null) {
                node = node.right;
            }
            return node;
        } else {
            Node parent = node.parent;
            while (parent != null) {
                if (node == parent.right) {
                    return parent;
                } else {
                    node = parent;
                    parent = node.parent;
                }
            }
            return null;
        }
    }

    private Node successor(Node<E> node) {
        if (node.right != null) {
            node = node.right;
            while (node.left != null) {
                node = node.left;
            }
            return node;
        } else {
            Node parent = node.parent;
            while (parent != null) {
                if (node == parent.left) {
                    return parent;
                } else {
                    node = parent;
                    parent = node.parent;
                }
            }
            return null;
        }
    }

    private int compare(E insert, E current) {
        if (comparator != null) {
            return Objects.compare(insert, current, comparator);
        }
        return ((Comparable<E>) insert).compareTo(current);
    }

    private void nonNullCheck(E element) {
        if (isNull(element)) {
            throw new IllegalArgumentException("element must not be null");
        }
    }

    @Override
    public Object root() {
        return root;
    }

    @Override
    public Object left(Object node) {
        return ((Node<E>) node).left;
    }

    @Override
    public Object right(Object node) {
        return ((Node<E>) node).right;
    }

    @Override
    public Object string(Object node) {
        return node;
    }

    protected static class Node<E> {
        E       element;
        Node<E> left;
        Node<E> right;
        Node<E> parent;

        Node(E element, Node<E> parent) {
            this(element);
            this.parent = parent;
        }

        Node(E element) {
            this.element = element;
        }

        boolean isLeftChildOf(Node node) {
            return this == node.left;
        }

        @Override
        public String toString() {
            return "Node{" +
                    "element=" + element +
                    '}';
        }
    }

    private static boolean oneIsChildOfAnother(Node one, Node another) {
        return one != null && (one == another.left || one == another.right);
    }

    private static boolean isLeaf(Node node) {
        return node.left == null && node.right == null;
    }

}
```



### 删除元素

![14-46-19.png](http://ww1.sinaimg.cn/large/006zweohgy1g9dr5aqc2sj30i706rmy5.jpg)







# k近邻法
#### 基本思想
在一个训练数据集中，其中的实例类别一定。分类时，对新的实例，根据其k各最近邻的训练实例的类别，通过多数表决等方式进行预测。实际上，KNN利用训练数据集对特征向量空间进行划分，并作为其分类的模型。
#### KNN的三个基本要素
1. k值的选择
2. 距离度量
3. 分类决策规则
### KNN算法
* 根据给定的距离度量，在训练集T中找出与x最邻近的k个点，涵盖这k个点的领域记做$N_k(x)$
* 在$N_k(x)$根据分类决策规则（如多数表决）决定x的类别y
  $y = argmax_{c_j}\sum_{x_i\in N_k(x)}I(y_i=c_j)$
  其中I为指示函数，即当$y_i=c_j$时，I为1，否则I为0

特殊情况：当k=1时，称为最近邻算法，对于输入的实例点，最近邻法将训练数据集中与x最邻近的类作为x的类

---

### 三要素
#### 距离度量
使用不同的距离度量所确定的最近邻点是不同的
距离度量中，一般使用的距离是欧式距离，但也可以是其他距离，比如更一般的$L_p$距离
距离的定义：特征空间中两个实例点的距离是两个实例点相似程度的反映
$L_p$距离的定义：
$L_p(x_i, x_j) = \displaystyle \left(\sum_{l=1}^n|x_i^{(l)} - x_j^{(l)}|^p\right)^{\frac{1}{p}}$
$L_1$距离-曼哈顿距离
$L_1(x_i, x_j) = \displaystyle \left(\sum_{l=1}^n|x_i^{(l)} - x_j^{(l)}|\right)$
$L_2$距离-欧式距离
$L_2(x_i, x_j) = \displaystyle \left(\sum_{l=1}^n|x_i^{(l)} - x_j^{(l)}|^2\right)^{\frac{1}{2}}$
当p = $\infty$, $L_p$距离是各个坐标距离的最大值
#### k值的选择
k值的选择会对kNN的结果产生重大影响。
>近似误差和估计误差
>1.近似误差和估计误差主要在underfitting和overfitting中
>当F很小（假设空间很小），就会欠拟合，估计误差比较小，但是近似误差比较大
>当F很大（假设空间很大），就会overfitting，估计误差比较大，近似误差比较小
>可以认为估计误差是一种局部最优解（经验风险最小化），近似误差是一种全局最优解（期望风险最小化）
> | | 估计误差 | 近似误差 |
>| ----| ---- | ---- |
>| 假设空间增大| 增大 | 减小 |
>| 训练集大小增大| 减小| 无关 |
* 选择较小的k值时，整体模型变得复杂，容易overfitting
* 选择较大的k值时，整体模型变得简单，过大时可能回underfitting
* 如果k = N（训练数据集大小），则无论输入实例是什么，都会预测它属于训练数据中最多的类，模型过于简单，完全忽略了训练数据中的大量有效信息
* 在应用中，k值一般取一个比较小的值。通常采用交叉验证法选择最优的k值
#### 分类决策规则
KNN中分类决策规则一般使用多数表决。可证多数表决规则等价于经验风险最小化。
#### Kd树
KNN存在的问题：当训练集较大（这很常见）时，计算每两个样本点之间的距离耗时太多，模型性能会变得很差，因此需要使用特殊的结构存储训练数据，较少计算距离的次数，kd树就因此诞生。
**K**：Kd树里的k并不是KNN算法中选取k个点，而是指存储k维空间的数据。
kd树：kd树是一个二叉树，表示对k维空间的一个划分，构造kd树相当于不断用垂直于坐标轴的超平面将将k维空间划分。
>超平面的本质：超平面的自由度比空间维度少一
>超平面的性质：超平面可以把线性空间分成不相交的两部分

##### 算法1: 构造kd树
这个算法构造出了平衡kd树（每次选择训练实例点在选定坐标轴上的中位数作为切分点）
平衡kd树的搜索效率未必是最优的。kd树是针对k维空间的一个划分，与k近邻还是最近邻没有关系。
` input: `
k维空间数据集T = {$x_1, x_2, x_3,...,x_n$}, 其中$x_i$ = $(x_i^{(1)},x_i^{(2)},x_i^{(3)}...x_i^{(k)})^{T}$
` output: `
kd树

* 构造根结点，根结点对应于包含T的k维空间的超矩形区域。
    * 选择$x^{(1)}$为坐标轴，以T中所有实例的$x^{(1)}$坐标的中位数为切分店，将根结点对应的超矩形区域划分为两个子区域。
    * 由根结点生成深度为1的左、右两个子节点，左子节点对应坐标$x^{(1)}$小于切分店的子区域，右子节点对应坐标$x^{(1)}$大于切分店的子区域。
* 重复：对深度为j的节点，选择$x^{(1)}$为坐标轴，$l=j(mod k) + 1$,以该节点的区域中所有实例的$x^{(1)}$坐标的中位数为切分点，将该节点对应的超矩形区域划分为两个子区域。切分由通过切分点并与坐标轴$x^{(1)}$垂直的超平面实现。
* 知道两个子区域没有实例存在时停止。
> 例子：构造一个平衡kd树
$T = \displaystyle\left\{(2, 3)^T,(5, 4)^T,(9, 6)^T,(4, 7)^T,(8, 1)^T,(7, 2)^T\right\}$

1. 选择$x^{(1)}$轴，2，4，5，7，8，9这六个数的中位数是6，但是没有6这个实例，所以选择7切分。以平面$x^{(1)} = 7$将空间分为左右两个子矩形。
2. 左子矩形：23，47，54，以$x^{(2)} = 4$切分，右子矩形：81，96，以$x^{(2)} = 6$切分。
3. 左左子矩形：23，以$x^{(1)} = 2$切分，左右子矩形：47，以$x^{(1)} = 4$切分……

##### 算法2: 使用kd树搜索最近邻（回溯算法）
**最近邻**
1. 在kd树中找出包含目标点x的叶节点
   从根结点出发，递归的向下访问kd树，若目标点x当前维的坐标小于切分点的坐标，则移动到左子节点，否则到右子节点。直到子节点为叶节点。
2. 以此叶节点为当前最近点
3. 递归的向上回退，对每个节点进行如下操作
   1. 如果该节点保存的实例点比当前最近点距离目标更近，则以该实例点为**当前最近点**
   2. **当前最近点**一定存在于该节点（指每个回退上去经历的节点）一个子节点对应的区域，检查该子节点的父节点的另一个子节点对应的区域是否有更近的点。
      1. 具体的方法：检查另一子节点对应的区域是否以目标点为球心，以目标点与**当前最近点**之间的距离为半径的超球体相交。
       * 若相交，则在另一个子节点对应的区域内可能存在距目标点更近的点，移动到另一个子节点，接着递归的搜索。
       * 不相交，向上回退
4.回退到根结点时，最后的**当前最近点**即为x的最近邻点
>例题：在上一个例题构造的kd树中，找到$(3, 4)^Y$的最近邻（在书上）

##### 算法3: 对kd树进行k近邻的搜索（找到k个近邻）
在寻找k近邻时，维护一个“当前k近邻点集”
1. 在kd树中找出包含目标点x的叶节点（从根结点开始向下，按照规则看是往左还是往右子节点）
2. 递归回退，在每个节点进行一下操作
   1. 每经过一个节点，若当前k近邻点集没满（元素个数小于k），或者有距离更近的点（距离小于点集中的最大距离），就将当前节点插入当前k近邻点集
   2. 检查另一子节点对应的区域是否与以目标点为圆心，以目标点到“当前k近邻点集”中最远点为半径的超球体相交。
      1. 若相交，在另一个子节点对应的区域可能有距目标点更近的点，则移动到另一个子节点对应的区域
      2. 否则，向上回退
>例题：在二维空间中给出实例点，画出k为1和2时的k近邻法构成的空间划分，并对其进行比较，体会k值选择和模型复杂度及预测准确度的关系
k值变小时，说明模型更复杂，容易overfitting，在训练集上预测准确度较好，但是在训练集之外预测准确度可能一般。
k值较大时，说明模型更简单，容易underfitting，在训练集和训练集之外预测准确度可能差不多。
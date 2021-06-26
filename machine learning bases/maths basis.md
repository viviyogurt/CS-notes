## 概率学中的数学基础汇总
####贝叶斯
基本思想：贝叶斯在概率的计算中引入了先验概率，即在知道了一些事情的情况下，概率会发生变化
#### 极大似然估计
基本思想：待估计参数$\theta$是客观存在的，只是未知而已，当$\theta_{mle}$满足$\theta=\theta_{mle}$，若该组观测样本更容易被观测到，就可以说$\theta_{mle}$是$\theta$的极大似然估计值。也可以理解为：估计值$\theta_{mle}$使事件出现的可能性最大
$L(\theta|x) = f(x|\theta) = \prod_{i=1}^nf(x_i|\theta)$
$\theta_{mle} = argmax_{theta}L(\theta|x)$

#### 贝叶斯估计
基本思想：待估计参数$\theta$也是随机的，和一般随机变量没有本质区别，因此只能根据观测样本估计参数$\theta$的分布
贝叶斯估计的数学描述：贝叶斯估计利用了贝叶斯公式
$P(\theta|x) = \displaystyle\frac{P(\theta)P(x|\theta)}{\sum_{j=1}^NP(\theta_j)P(x|\theta_j)}$
其中$P(\theta)$是参数$\theta$的先验分布，表示对参数$\theta$的主观认识，是非样本信息。$P(\theta|x)$是参数$\theta$的后验分布，因此贝叶斯估计可以看做，在假定$\theta$服从$P(\theta)$的先验分布前提下，根据样本信息去校正先验分布，得到后验分布$P(\theta|x)$.
因为后验分布是一个条件分布，通常取后验分布的期望作为参数的估计值
$\theta_{be} = EP(\theta|x)$

#### 最大后验估计MAP
如果在贝叶斯估计中，考虑极大似然估计的思想，将后验分布极大化而去求解$\theta$，就变成了最大后验估计MAP（在贝叶斯公式中求后验概率最大值的那个参数）
$$
\theta_{map} = argmax_{\theta}P(\theta)P(x|\theta) = argmax_{\theta}(\log{P(x|\theta)} + \log{P(\theta)})
$$
作为贝叶斯估计的一种近似解，MAP有其存在的价值，因为贝叶斯估计中后验分布的计算往往是非常棘手的；而且，MAP并非简单地回到极大似然估计，它依然利用了来自先验的信息，这些信息无法从观测样本获得。
如果将机器学习结构风险中的正则化项对应为上式的$\log{\theta}$，那么带有正则化项的最大似然学习就可以被解释为MAP。当然，这并不是总是正确的，例如，有些正则化项可能不是一个概率分布的对数，还有些正则化项依赖于数据，当然也不会是一个先验概率分布。不过，MAP提供了一个直观的方法来设计复杂但可解释的正则化项，例如，更复杂的惩罚项可以通过混合高斯分布作为先验得到，而不是一个单独的高斯分布。
#### 似然
>Likelihood is the hypothetical probability that an event that has already occurred would yield a specific outcome. The concept differs from that of a probability in that a probability refers to the occurrence of future events, **while a likelihood refers to past events with known outcomes**.

#### 信息增益与互信息
**互信息**在信息论和机器学习中非常重要，其可以评价两个分布之间的距离，这主要归因于其对称性，假设互信息不具备对称性，那么就不能作为距离度量，例如相对熵，由于不满足对称性，故通常说相对熵是评价分布的相似程度，而不会说距离。互信息的定义为：一个随机变量由于已知另一个随机变量而减少的不确定性，或者说从贝叶斯角度考虑，由于新的观测数据y到来而导致x分布的不确定性下降程度。
**需要注意的是**：在数值上，信息增益和互信息完全相同，但意义不一样，需要区分，当我们说互信息时候，两个随机变量的地位是相同的，可以认为是纯数学工具，不考虑物理意义，当我们说信息增益时候，是把一个变量看成是减少另一个变量不确定度的手段

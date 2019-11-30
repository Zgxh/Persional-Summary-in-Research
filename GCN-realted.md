### GCN
![avatar](./src/pic/GCN-schematic.png "GCN原理图")
<center>GCN原理图</center>

给定一个图，$G = (V, A)$，V是顶点集，A是邻接矩阵（对称阵）。用D表示图G的度矩阵，$D=diag(d_1,\cdots,d_n)$。用$H^{(i)}$表示图卷积中的结点特征。
输入的结点特征矩阵：
$$X = \{x_1^T,x_2^T,\cdots,x_n^T\}^T \in \mathbb{R}^{n \times d}$$
初始特征:$$H^{(0)} = X$$
图卷积包括三个步骤：特征传播、线性变换、非线性激活。
##### 特征传播：
给初始的邻接矩阵 A 添加自环(self-loops):
$$\tilde{A} = A + I$$ 用 $\tilde{D}$ 表示 $\tilde{A}$ 的度矩阵。
定义归一化的邻接矩阵：
$$S = \tilde{D}^{-\frac{1}{2}} \tilde{A} \tilde{D}^{-\frac{1}{2}}$$ 这个S在所有层都是一样的，直接通过$\tilde{A}$即可求得。
第k层的特征传播：
$$\tilde{H}^{(k)} \leftarrow SH^{(k-1)}$$
分开写：
$$\tilde{h}^{k}_{i} \leftarrow \frac{1}{d_i+1}h_i^{(k-1)} + \sum\limits_{j=1}^{n} \frac{a_{ij}}{\sqrt{(d_i+1)(d_j+1)}} h_j^{(k-1)}$$
##### 特征变换 + 非线性激活：
每一层有个权值矩阵$\theta^{(k)}$
$$H^{(k)} \leftarrow ReLU(\tilde{H^{(k)}} \theta^{(k)})$$
##### 分类器：
$$Y_{GCN} = softmax(SH^{(k-1)} \theta^{(k)})$$


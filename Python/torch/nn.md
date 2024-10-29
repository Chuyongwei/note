<table>
    <tr>
        <th> </th>
        <th> W的维度 </th>
    <th> b的维度 </th>
    <th> 激活值的计算</th>
    <th> 激活值的维度</th>
    </tr>
    <tr>
        <td> 第 1 层 </td>
        <td> $(n^{[1]},12288)$ </td>
        <td> $(n^{[1]},1)$ </td>
        <td> $Z^{[1]} = W^{[1]} X + b^{[1]} $ </td>
        <td> $(n^{[1]},209)$ </td>
    </tr>
    <tr>
        <td> 第 2 层 </td>
        <td> $(n^{[2]}, n^{[1]})$ </td>
        <td> $(n^{[2]},1)$ </td>
        <td>$Z^{[2]} = W^{[2]} A^{[1]} + b^{[2]}$ </td>
        <td> $(n^{[2]}, 209)$ </td>
    </tr>
    <tr>
        <td> $\vdots$ </td>
        <td> $\vdots$ </td>
        <td> $\vdots$ </td>
        <td> $\vdots$</td>
        <td> $\vdots$ </td>
    </tr>
</table>



| W的维度              | b的维度              | 激活值的计算                          | 激活值的维度                          |
| -------------------- | -------------------- | ------------------------------------- | ------------------------------------- |
| $(n^{[1]},12288)$    | $(n^{[1]},1)$        | $(n^{[1]},1)$                         | $(n^{[1]},1)$                         |
| $(n^{[2]}, n^{[1]})$ | $(n^{[2]}, n^{[1]})$ | $Z^{[2]} = W^{[2]} A^{[1]} + b^{[2]}$ | $Z^{[2]} = W^{[2]} A^{[1]} + b^{[2]}$ |
| $\vdots$             | $\vdots$             | $\vdots$                              | $\vdots$                              |

第 L-1 层 

 $(n^{[L-1]}, n^{[L-2]})$ 

 $Z^{[L-1]} = W^{[L-1]} A^{[L-2]} + b^{[L-1]}$

 $(n^{[L-1]}, 209)$ 

第 L 层 

$(n^{[L]}, n^{[L-1]})$ 

$(n^{[L]}, 1)$

 $Z^{[L]} = W^{[L]} A^{[L-1]} + b^{[L]}$ 

$(n^{[L]}, 209)$



W=⎣⎡jmpknqlor⎦⎤X=⎣⎡adgbehcfi⎦⎤b=⎣⎡stu⎦⎤



 $W^{[1]},W^{[2]}和W^{[3]}将这三个项相加并乘以 \frac{1}{m} \frac{\lambda}{2} 在后向传播中，使用\frac{d}{dW} ( \frac{1}{2}\frac{\lambda}{m} W^2) = \frac{\lambda}{m} W$



$D^{[1]} = [d^{[1]} d^{[2]} … d^{[m]} ]$





下面我们将关闭第一层和第三层的一些节点，我们需要做以下四步：

在视频中，吴恩达老师讲解了使用np.random.rand() 来初始化和a [ 1 ] a^{[1]}a 
 具有相同维度的 d [ 1 ] d^{[1]}d 

  ，在这里，我们将使用向量化实现，我们先来实现一个和A [ 1 ] A^{[1]}A 
 相同的随机矩阵$D^{[1]} = [d^{1} d^{1} … d^{1}] $。
如果D [ 1 ] D^{[1]}D 
  低于 (keep_prob)的值我们就把它设置为0，如果高于(keep_prob)的值我们就设置为1。
把A [ 1 ] A^{[1]}A 

  更新为 A [ 1 ] ∗ D [ 1 ] A^{[1]} * D^{[1]}A 

 ∗D 
 。 (我们已经关闭了一些节点)。我们可以使用 D [ 1 ] D^{[1]}D 
[1]
  作为掩码。我们做矩阵相乘的时候，关闭的那些节点（值为0）就会不参与计算，因为0乘以任何值都为0。
使用 A [ 1 ] A^{[1]}A 
[1]
  除以 keep_prob。这样做的话我们通过缩放就在计算成本的时候仍然具有相同的期望值，这叫做反向dropout。





> 使用SGD和小批量更新方法

```py
#仅做比较，不运行。

#批量梯度下降，又叫梯度下降
X = data_input
Y = labels

parameters = initialize_parameters(layers_dims)
for i in range(0,num_iterations):
    #前向传播
    A,cache = forward_propagation(X,parameters)
    #计算损失
    cost = compute_cost(A,Y)
    #反向传播
    grads = backward_propagation(X,Y,cache)
    #更新参数
    parameters = update_parameters(parameters,grads)

#随机梯度下降算法：
X = data_input
Y = labels
parameters = initialize_parameters(layers_dims)
for i in (0,num_iterations):
    for j in m:
        #前向传播
        A,cache = forward_propagation(X,parameters)
        #计算成本
        cost = compute_cost(A,Y)
        #后向传播
        grads = backward_propagation(X,Y,cache)
        #更新参数
        parameters = update_parameters(parameters,grads)
        
```

$$
\left \{
\begin{array}{c} 
v_{dw^{[l]}}=\beta v_{dw^{[l]}}+(1-\beta)dW^{[l]} \\
W^{[l]} = W^{[l]}-\alpha v_{dW^{[l]}}
\end{array}
\right.
$$

$$
\left \{
\begin{array}{c}
v_{db^{[l]}} = \beta v_{db^{[l]}}+(1-\beta)db^{[l]} \\
b^{[l]} = b{[l]}-\alpha v_{[db^{[l]}]}
\end{array}
\right.
$$

其中：

+ l是当前神经网络的层数
+ \betaβ是动量，是一个实数
+ \alphaα是学习率


$$
\left \{
\begin{array}{c}
v_{dW^{[l]}}=\beta_1v_{dW^{[l]}}+(1-\beta_1)\frac{
		\partial\tau
	}{
		\partial W^{[l]}
	}
\\
v_{dW^{[l]}}^{corrected} = \frac{v_{dW^{[l]}}}{1-(\beta)^t}
\\
S_{dW^{[l]}} = \beta_2s_{dW^{[l]}}+(1-\beta_2)(\frac{\partial\tau}{\partial W^{[l]}})^2
\\
S^{corrected}_{dW^{[l]}} = \frac{S_{[dW^{[l]}]}}{1-(\beta_l)^t}
\\
W^{[l]} = W^{[l]}-\alpha \frac{v^{corrected}_{dW^{[l]}}}{\sqrt{s^{corrected}_{dW^{[l]}}}+\epsilon}
\end{array}
\right.
$$


### 代价函数

$$
J = -\frac{1}{m}
\sum^{m}_{i=1}{
	(y^{(i)
	}
	\log{a^{[L](i)}}
+(i-y^{9i})\log{(1-a^{[l](i)})}})
\tag{1}
$$


$$
J_{正则化} = \underbrace{
	-\frac{1}{m}\sum^{m}_{i=1}{
		y^{(i)} \log{(a^{[L](i)})
	}
	+
	(1-y^{(i)})\log{
		(1-a^{[L](i)})})
	}
}_{交叉熵成本}
+\underbrace{
	\frac{1}{m}\frac{\lambda}{2}\sum_l\sum_k\sum_j{W^{[l]2}_{kj}}
}_{L2正则化成本}
\tag{2}
$$

### 更新函数


$$
dW^{[l]} = \frac{\partial\zeta}{\partial W^{[l]}} = \frac{1}{m}dZ^{[l]}A^{[l-1]T}\tag{5}
$$

$$
db^{[l]} = \frac{\partial\zeta}{\partial b^{[l]}}=\frac{1}{m}\sum^{m}_{i=1}dZ^{[l](i)}\tag7
$$

$$
dA^{[l-1]} = \frac{\partial\zeta}{\partial A^{[l-1]}} = W^{[l]T}dZ^{[l]}\tag7
$$

### sofrmax函数

$$
softMax = \frac{\exp(x_{i})}{\sum^{n}_{i}\exp(x_{i})}
$$

> Softmax函数，或称归一化指数函数，是`逻辑斯谛函数`的一种推广。它能将一个含任意实数的K维向量 ${\mathbf {z} }$ “压缩”到另一个K维实向量${\displaystyle \sigma (\mathbf {z} )} $中，使得每一个元素的范围都在(0,1)之间，并且所有元素的和为1(也可视为一个 (k-1)维的hyperplane或subspace)。



# CLNet

$$
(\nu^{l}_{i},\epsilon^{l}_{i})
$$


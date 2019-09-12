# Lab02 Lagrange插值

**古宜民 PB17000002**

**2019.3.10**

### 1. 计算结果

选取插值节点为均匀节点和Chebyshev点对函数 $ f(x)=\frac {1}{4+x+x^2} $ 进行插值，去插值函数和原函数的最大偏差作为近似误差，取不同插值点数N=4，8，16，计算结果如下：

第一组节点：
N = 4  Err = 6.475332068311e-02
N = 8  Err = 5.156056628632e-02
N = 16  Err = 1.071904436126e-01

第二组节点：
N = 4  Err = 5.437602650073e-02
N = 8  Err = 1.426092606546e-02
N = 16  Err = 8.367272096925e-04

#### 函数图像：

其中蓝色为原函数f，橙色为插值函数。
第一组节点：

![N4type1](C:\Users\petergu\MyHome\USTCcomputer\2019Spring\CompMethod\2\N4type1.png)

![N8type1](C:\Users\petergu\MyHome\USTCcomputer\2019Spring\CompMethod\2\N8type1.png)

![N16type1](C:\Users\petergu\MyHome\USTCcomputer\2019Spring\CompMethod\2\N16type1.png)

第二组节点：

![N4type2](C:\Users\petergu\MyHome\USTCcomputer\2019Spring\CompMethod\2\N4type2.png)

![N8type2](C:\Users\petergu\MyHome\USTCcomputer\2019Spring\CompMethod\2\N8type2.png)

![N16type2](C:\Users\petergu\MyHome\USTCcomputer\2019Spring\CompMethod\2\N16type2.png)

### 2. 程序算法

使用MATLAB。对于给定的N和节点类型，使用两层循环，内层计算插值函数$l_i(x)$，外层计算插值多项式$p(x)=\sum_{i=0}^{n}f(x_i)l_i(x)$。之后在一系列值上计算误差$max\{p(x)-f(x)\}$。

关键代码如下：

```matlab
        p = 0;
        for i = 0:N(k)
            l = 1;
            for j = 0:i-1
                l = conv(l, [1, -xi(j+1)] / (xi(i+1) - xi(j+1)));
            end
            for j = (i+1):N(k)
                l = conv(l, [1, -xi(j+1)] / (xi(i+1) - xi(j+1)));
            end
            p = polyadd(p, f(xi(i+1)) * l);
        end
        % p is the result Lagrange polynomial
        % calculate the max err
        err = max(abs(arrayfun(f, y) - polyval(p, y)));
```

### 3. 结果分析

对于不同次数的插值函数，可见均匀取插值节点时，在N=8时误差最小，N=4、16时误差较大。N=4时误差来自于次数太低，插值函数不能很好地反应原函数性质，偏差较大；而N=16时误差来自于在定义域边缘处出现了Runge现象，误差很大，而中间区域于原函数符合得很好。取Chebyshev节点时，N=4，8，16误差依次减小，N=16时几乎于原函数完全相同。而若取更大的N值，如N=32，则也出现了误差增大的现象。当N更大时，两种插值函数在定义域边缘处都出现了几个数量级的误差。

![N32type2](C:\Users\petergu\MyHome\USTCcomputer\2019Spring\CompMethod\2\N32type2.png)

相比之下，取Chebyshev节点的插值性质明显好于取均匀节点。N=16时其与原函数几乎完全符合。并且当N过大时产生的偏差也小于均匀节点。

### 4. 小结

由实验结果可见，插值函数的好坏不仅由插值函数的的次数决定，次数过低拟合不好，过高又会在区间端点附近出现较大误差，必须根据作图结果合理选择。另外插值节点的选择也影响结果，Chebyshev节点在端点附近取点较密，中间取点较为稀疏，能得到比均匀取点更稳定的结果、更小的误差。
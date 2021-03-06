### 470.Implement-Rand10--Using-Rand7

#### 解法１：
所谓实现Rand10()，本质就是保证：(1) 输出是1-10， (2) 每个输出都是等概率的。

感觉可以实现的方法有很多。我想到的是，Rand7()输出的是1-7,七种等概率的输出对Rand10()有帮助吗？好像差点什么。如果给我们的函数有五种等概率的输出，好歹还可将其映射到1-2,3-4,5-6,7-8,9-10这五类，每一类（两个数字）出现的概率还能够平均一点。怎么办呢？思路立马就有了，如果Rand7()给出七种可能，我们就取前五种呗，如果抽到了6或者7，那就不算，重新再来不就行了。这样，我们就能通过Rand7()得到五种等概率的输出。

有了这个思路，接下来也好办了。刚才通过一次Rand7()，得到等概率的五种输出，但是每种输出可以包含两个数字。如何将这两个数字再等概率地分开呢？那就再摇一次Rand7()呗。如果是奇数就选这两个数字中的奇数，如果是偶数就选这两个数字中的偶数。当然Rand7()会得到四个奇数和三个偶数并不公平，那我们就采用相同的思路，抽到７就不算，重来就行了。这样，对于奇偶性的抽取总是均等概率的。

综上，我们的Rand10()是分两步走，第一步利用Rand7()得到等概率的五种可能，然后再利用Rand7()得到等概率的两种可能。

对于这道题，面试官肯定会问，抽出一个数，需要调用Rand7()的次数的期望是多少。之所以我们抽一个Rand１０()可能不止需要两次Rand7()操作，是因为可能会遇到“重来”的情况。我们可以大致算一算：

第一步：
```1*5/7+2*(2/7)*5/7+3*(2/7)^2*5/7+...+k*(2/7)^(k-1)*5/7```

第二步：
```1*6/7+2*(1/7)*6/7+3*(1/7)^2*6/7+...+k*(1/7)^(k-1)*6/7```

可以用等差等比数列的方法算一下，大概是2.6次左右。

当然，从信息论的角度来看，rand7()给出的信息量是log(7)，rand10()给出的信息量是log(10)，因此理论上的极限应该是log(10)/log(7)=1.18。所以上述的方法其实还是挺粗糙的，但是非常简单。

#### 解法２：
用```(rand7-1)*7+(rand7-1)```可以得到在[0,48]区间内均匀分布的随机变量。剩下的步骤和解法一相同，当结果大于40的时候reject，也就是重来一次。否则的话直接对10取余就得到[0,9]内均匀分布的随机数了。

记住，对于两个独立同均匀分布的随机变量X,Y，它们的加和不是均匀分布，他们的乘积也不是均匀分布，只有类似```a*X+b*Y```的线性变换才保持均匀分布。

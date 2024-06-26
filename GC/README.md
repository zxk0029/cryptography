[![English](https://img.shields.io/badge/English-README-blue)](EnglishReadme.md)


## 混淆电路

### 一. 混淆电路的发展现状

1982 年：姚期智提出混淆电路，此时的混淆电路识两方计算

2009 年：Lindell 等给出了安全性证明和详细介绍

2013 年：Goldwasser 给出了简化形式


### 二. 混淆电路

早期的MPC协议都是基于电路模型，其中最为典型的就是混淆电路协议。混淆电路是一种两方安全计算方法，最早由姚期智先生提出，为此，他还提出了经典的百万富翁问题：在双方不透露任何资产的情况下比较谁更富有。参与的双方分别是电路生成者和电路执行者，首先双方达成一致地将要执行的联合函数转换成布尔电路。通常地，生成者使用对称加密算法（AES等）对每个门电路进行加密。对电路上的每一条线路的可能输入（输出）值，生成者选取对等个数的随机数分别进行一一替换，这些随机数被称为混淆密钥。生成者再利用这些混淆密钥，对电路里的每个门的输出的混淆密钥进行加密，得到混淆电路的真值表。

|  X  |  Y |  Z  |
|:----|:---|:----|
|   0 |  0 |  0  |
|   0 |  1 |  0  |
|   1 |  0 |  0  |
|   1 |  1 |  1  |

以简单的与门电路为例，假设有输入x，y，输出为z，则有真值表： 

首先，x和y不能公开（也不能全部发送给执行者），可以用随机数表示，执行者拿到的是z的相关信息。而z的信息也不能完全透露，如果采用简单的编码替换z的结果，很容易便可以推出z的输入信息，因此，需要真值表中z的四个结果都使用不一样的表示，也可采用随机数，同时，z的随机数可以由某一组的xy解得，因此可以使用对应的xy来对z进行加密。整个过程就是混淆真值表生成的过程，最终的混淆真值表： 

.： 
    ![.： 
](https://github.com/guoshijiang/cryptography/blob/master/img/121.png)


生成者将混淆真值表的z顺序打乱后连同自己的输入随机数发送给执行者（注意：前提是生成者与执行者共同约定执行电路），执行者接受到消息后开始执行电路。一般地，生成者提供自己的x输入后，执行者在和之间选择自己的输入便可解得z，和同样由生成者提供，但是生成者不能都发送给执行者，同时，执行者也不想让生成者知道自己选择了哪个，这时，OT协议就起作用了，混淆电路正是使用了OT协议解决这个问题。最终，执行者使用生成者提供的x输入和自己的输入依次解密z，解密成功的z即为最终结果（注意：结果有且只有一个，并且只是解密得到z的混淆密钥）。执行者再将结果也即混淆密钥发送给生成者，生成者对照真值表把密钥对应的真实z的输出结果发送给执行者，至此，双方便完成了一次混淆电路计算。

混淆电路的前提是需要双方彼此事先约定好执行电路，并且双方都能诚实地把结果发送给对方，这是一半诚实模型。最初的混淆电路协议可以对抗半诚实攻击，经过后续的许多改进和优化，也可以对抗完全恶意攻击。在实际应用过程中，还存在复杂运算逻辑的表示和电路生成计算的性能问题。

### 三.GMW 协议

GMW 协议由 Goldreich 等人提出，基于混淆电路（Garbled Circuit），支持多方的半诚实的安全计算协议。和之前所述的姚氏混淆电路估值方案的不同之处在于，GMW 混淆电路估值方案不需要使用混淆真值表，因此没用混淆真值表带来的查表和加解密操作，节省了非常大的计算量和通信量。混淆电路在上次的科普已经介绍过了，在将安全多方计算的目标函数转换为布尔电路后，通过对电路进行混淆（加密）操作来得到混淆电路。混淆电路通过对电路进行混淆（加密）操作来掩盖电路的输入和电路的结构，以此来实现对各个参与者的隐私信息的保密，再通过电路计算来实现安全多方计算的目标函数的计算。

GMW 协议的目标函数由异或门和与门、非门组成。非门的输出值是输入值的反，输入为 1 则输出为 0，反之输入为 0 则输出为 1

与门的真值表如下图所示：

.： 
    ![.： 
](https://github.com/guoshijiang/cryptography/blob/master/img/new_gmw1.jpg)


异或门及其真值表如下图所示：

.： 
    ![.： 
](https://github.com/guoshijiang/cryptography/blob/master/img/new_gmw2.png)


算法流程如下：



### 四.BGW 协议

BGW 协议也是支持多方的安全计算协议，BGW 协议基于 Shamir(t, n)门限秘密共享机制，利用了 Shamir 秘密共享机制的加法同态和乘法同态的性质。

假设 BGW 协议的参与者一共有 n 人，假设参与者 Pi 需要输入秘密 ，则参与者 Pi 首先利用Shamir(t,n)门限秘密共享机制将秘密 共享给其他所有参与者，阈值 t 的选择根据具体使用情景下的安全性要求决定。

当所有参与者的输入都通过Shamir(t, n)门限秘密共享机制分享后，每个参与者都掌握了协议输入的子秘密。假设一个门的输入分别为 a 和 b ，秘密 ai 和 bi 已经分别由秘密分配函数

.： 
    ![.： 
](https://github.com/guoshijiang/cryptography/blob/master/img/bgw1.png)

分配完成，fa(0) = a, fb(0) = b， 参与者 Pi 掌握 a 和 b 的子秘密 ai , bi。在布尔电路上，可将异或门和与门分别看成在有限域 F2 上的加法和乘法。将异或用模为 2 的加法进行计算，与用模为 2 的乘法进行计算。

.： 
    ![.： 
](https://github.com/guoshijiang/cryptography/blob/master/img/bgw2.jpg)


对于异或门：由于 Shamir 具有加法同态性，因此

.： 
    ![.： 
](https://github.com/guoshijiang/cryptography/blob/master/img/bgw3.jpg)

假设 ci = ai xor bi，则 c(i) = a(i) xor b(i) ，而 a(i) = ai和 bi =b(i) 都由 Pi 掌握，因此 Pi 可以本地计算出 ci = ai xor bi 。当所有计算完成后，每个参与者Pi公布自己计算出的 c(i) ，即可恢复出 c(x) 和 c(0)。 

.： 
    ![.： 
](https://github.com/guoshijiang/cryptography/blob/master/img/bgw4.jpg)


对于与门，和之前叙述过的基于 Shamir(t, n)门限共享机制的多方乘法计算相同，只是 BGW 是在有限域 F2 上。每个参与者 Pi 计算 ai * bi ，接着每个Pi独自选取次数为 t 次的随机多项式 hi(x) ，且满足 hi(x) = di ,1 ≤ t ≤ n ， t < n / 2 。向各个参与者分配 di ，且 cij = hi(x) , 所有参与者分配结束后，Pi 掌握了信息 c1i, c2i ... cni ，同时再利用公开的重组向量1, ... n， 

Pi 计算:

.： 
    ![.： 
](https://github.com/guoshijiang/cryptography/blob/master/img/bgw5.jpeg)

此时 Pi 掌握的子秘密 ci 即为 c 的子秘密。当所有计算完成后，每个参与者公开自己的子秘密，再根据之前叙述的 Shamir(t, n)门限秘密重构算法即可获得 。

### 五.CCD 协议


### 六.总结

经过近几年的发展，MPC协议系统已经不再使用严格的电路表示，而是使用混合模型，在混合模型中，函数被编译成一组优化的子协议函数，用于公共操作。这组原语可以包括传统的基于电路的协议模型，可以在单个协议中被无缝地描述。混合模型系统通常将中间值表示为一个大的有限域上的密钥分享。它们可以使用信息论和密码协议的混合，因此，计算参与方的数量和威胁模型都不同。

与严格的基于电路的模型相比，混合模型允许存在非常不同的性能特性。例如，在有限域中，比较、位移和相等性测试等操作作为算术电路表示代价高昂。然而，操作密钥分享的专用子协议在共享计算结果方面却非常高效。

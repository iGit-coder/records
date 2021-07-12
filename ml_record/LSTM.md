### 一、RNN/LSTM理论

------



1. 首先对于LSTM, 下面是一张很经典的图：

<img src="E:\md\iRecode\resources\LSTM.png" style="zoom:100%;" />

2. 在学习LSTM之前，需要对循环神经网络(RNN)进行了解。

   > BP、CNN之后为什么还会有RNN?
   >
   > BP、CNN算法的输出都只考虑前一个输入，并没有考虑其他时刻输入的影响，比如简单的猫，狗，手写数字等单个物体的识别具有好的效果。但是，对于一些与时间先后有关的，比如视频的下一时刻的预测，文档前后文内容的预测等，这些算法的表现都不尽如人意。因此RNN就应运而生了。
   >
   > 什么是RNN?
   >
   > RNN是一种特殊的神经网络结构，它是根据 **人的认知是基于过往的经验和记忆** 这一观点提出的。它与深度神经网络(DNN)和CNN不同的是：它不仅考虑前一时刻的输入，而且赋予了网络对前面的内容的一种 `记忆` 功能。具体表现形式为：网络会对前面的信息进行记忆并应用与当前输出的计算中，即隐藏层之间不再无连接而是有连接的，并且隐藏层的输入不仅包括输入层的输出还包括上一时刻隐藏层的输出。
   >
   > RNN的主要应用领域？
   >
   > - 自然语言处理(NLP)：主要有视频处理，文本生成，语言模型，图像处理
   > - 机器翻译: 机器写小说
   > - 语音识别
   > - 图像描述生成
   > - 文本相似度计算
   > - 音乐推荐，网易考拉商品推荐，Youtube视频推荐等新的应用领域

3. RNN

   - RNN模型结构

     ![](E:\md\iRecode\resources\RNN.png)

     RNN是如何实现记忆的？如上图所示，RNN主要有输入层(Input Layer)、隐藏层(Hidden Layer)、输出层(Output Layer)组成，并且发现在Hidden Layer 有一个箭头表示数据的循环更新，这就是实现时间记忆功能的方法。

     下面是RNN的Hidden Layer的的层级展开图：

     ![](E:\md\iRecode\resources\RNN_HiddenLayer.png)

     如上图所示：`t-1` ,`t+1` 表示时间序列。X表示输入的样本。St表示样本在时间t处的记忆，`St=f(W*St-1 + U*Xt)`.  W表示输入的权重，U表示此刻输入样本的权重，V表示输出样本的权重。

     > 在 `t-1` 时刻，一般初始化输入S0=0, 随机初始化权重W,U,V, 进行下面的公式计算：
     > $$
     > \begin{align}
     > h_1 &= Ux_1 + Ws_0 \\
     > s_1 &= f(h_1)\\
     > o_1 &= g(Vs_1)
     > \end{align}
     > $$
     > 其中，`f 和` `g` 均为激活函数，其中 `f` 可以是 `tanh` 、`relu` 、`sigmod` 等激活函数，`g`通常是`softmax` 也可以是其他。时间就是向前推进，此时的状态`s1` 作为时刻1的记忆状态参与下一时刻的预测活动，也就是：
     > $$
     > \begin{align}
     > h_2 &= Ux_2 + Ws_1 \\
     > s_2 &= f(h_2)\\
     > o_2 &= g(Vs_2)
     > \end{align}
     > $$
     > 一次类推，可以得到最终的输出值为：
     > $$
     > \begin{align}
     > h_t &= Ux_t + Ws_1\\
     > s_t &= f(h_t)\\
     > o_t &= g(Vs_t)
     > \end{align}
     > $$
     > *注意* ：
     >
     > 1.  这里的`WUV`在每一时刻都是相等的(权重共享)
     > 2. 隐藏状态可以理解为：`S=f(现有输入+过去记忆总结)` 
     >
     > 

   - RNN的反向传播

     > 由于每一步的输出不仅仅依赖当前步的网络，并且还需要前若干步网络的状态，那么这种BP改版的算法叫做Backpropagation Through Time(BPTT),也就是将输出端的误差值反向传递，运用梯度下降法进行更新。
     > $$
     > \begin{align}
     > E &= \sum_{i}^n e_i\\
     > \nabla U &= \frac{\partial E}{\partial U} = \sum_{i} \frac{\partial e_i}{\partial U}\\
     > \nabla V &= \frac{\partial E}{\partial V} = \sum_{i} \frac{\partial e_i}{\partial V}\\
     > \nabla W &= \frac{\partial E}{\partial W} = \sum_{i} \frac{\partial e_i}{\partial W}
     > \end{align}
     > $$
     >   

4. LSTM的大体结构

   > 相比于原始的RNN的隐藏层，LSTM增加了一个细胞状态(cell state)，下面是LSTM中间一个时刻t的输入输出：
   >
   > ![](E:\md\iRecode\resources\LSTM_Input_Output_exmple.png)
   >
   > 1. 细胞状态C<sub>t-1</sub> 的信息，一直在上面那条线传递，t时刻的隐藏层状态h<sub>t</sub> 与输入x<sub>t</sub> 会对C<sub>t</sub> 进行适当修正，然后传到下一时刻去。
   >
   > 2. C<sub>t-1</sub> 会参与t时刻输出h<sub>t</sub> 的计算
   >
   > 3. 隐藏层状态h<sub>t-1</sub> 的信息，通过LSTM的“门”结构，对细胞状态进行修改，并且参与输出计算
   >
   >    总的来说，细胞状态信息一直在上面那条线上传递，隐藏层状态一直在下面那条线上传递，不过它们会有一些交互，在LSTM中，通常被叫做“门”结构
   >
   >    

5. LSTM的输入输出

   > LSTM也是RNN的一种，输入基本没什么差别。通常需要一个时序的结构数据输入LSTM，数据会被分为t个部分，也就是上面图里的Xt, Xt可以看作一个向量，在实际训练的时候，会用batch来训练，所以通常它的shape : **(batch_size,input_dim)**。另外C0和h0的值，也就是两个隐藏层的初始值，一般是全0初始化。两个隐藏层同样是向量的形式，在定义LSTM的时候，会定义隐藏层大小(hidden size)， 即`Shape(Ct) =Shape(ht)=HiddenSize`  ,输出的维度与对应输入是一致的。

6. LSTM的门结构

   > **LSTM的输入单元**
   >
   > ![](E:\md\iRecode\resources\输入单元.png)
   >
   > LSTM的输入，是将一个字符经过一系列处理之后输入的，并不是直接将字符作为输入的。这一系列操作包括：首相将整个文本内容分为多个长度相等的batch,每个batch包含多个长度为seq_length的文本块(对于预测下一个字符的任务来说，`seq_length+1`)，最后一个batch可能会含有小于seq_length+1的文本块.然后每一个batch中的文本进行数字编码，就是将字符与数字一一对应(这个可以通过构建一个对应表来实现，需要注意的是要统计好文本中一共有多少种字符)。数字编码完成后，需要将每一个数字编码映射到维度相等的向量空间里，即向量空间中每一点对应一个字符。最终X<sub>t</sub>  表示第t步(时刻t)的输入向量,(t的取值应该是在batch间累计的，即学习下一个batch里面的内容时，也有之前batch的 ’记忆‘),每一次输入为一个字符对应的向量。
   >
   > **LSTM的输出单元**
   >
   > ![](E:\md\iRecode\resources\输出.png)
   >
   > *输出为预测字符所应的向量*
   >
   > LSTM的门结构，简单来说，就是被设计出来的一些计算步骤，通过这些计算，来调整输入与两个隐层的值。
   >
   > ![](E:\md\iRecode\resources\LSTM门元素.png)
   >
   > 这里首先讲解一下图里的组件，上面几个黄色的图案，代表“神经元”，也就是对W<sup>T</sup> x + b 的操作。区别在于使用的激活函数不同，\sigma 表示sigmoid函数，它的输出是0到1之间，tanh是双曲线正切函数，它的输出在-1到1之间。
   >
   > <img src="E:\md\iRecode\resources\LSTM门元素1.png" style="zoom:50%;" />
   >
   > 这个粉色的操作，根据图中画的内容，操作略有不同，不过整体的意思就是向量的按元素操作。在当前场景下，可以认为是两个相同维度的向量，对应的元素进行圆圈内部的操作，比如 `x` 就是两个相同维度对应元素的乘积组成新的向量。
   >
   > **遗忘门：**
   >
   > ![](E:\md\iRecode\resources\遗忘门.png)
   > $$
   > f_t = \sigma(W_f \cdot[h_{t-1} + x_t] + b_f)
   > $$
   > 首先说一下[h<sub>t-1</sub>,x<sub>t</sub>] 这个东西，就是代表向量连接起来(numpy.concatenate), 然后f<sub>t</sub> 就是一个网络的输出。它为什么叫遗忘门，下面是一种解释，\sigma 的输出在0到1之间，这个f<sub>t</sub> 逐位与C<sub>t-1</sub> 的元素相乘，我们可以发现，当f<sub>t</sub> 的某一位的值为0的时候，这C<sub>t-1</sub> 对应的那一位的信息就被干掉了，而值为(0,1)，对应位的信息就保留了一部分，只有值为1的时候，对应的信息才会完整的保留。因此，这个操作被称之为遗忘门，也算是“实至名归”
   >
   > **更新门：**
   >
   > ![](E:\md\iRecode\resources\更新门.png)
   > $$
   > \begin{align}
   > i_t &= \sigma (W_i \cdot [h_{t-1},x_t] + b_i)\\
   > \overline{C_t} &= tanh(W_C \cdot [h_{t-1},x_t] + b_C)
   > \end{align}
   > $$
   > 这个门有两个部分，一个是C<sub>t</sub> ，这个可以看作是新的输入带来的信息，tanh这个激活函数将内容归一化到-1到1.另一个值i<sub>t</sub> ,这个东西看起来和遗忘门的结构是一样的，这里可以看作是新的信息保留那些部分。
   >
   > ![](E:\md\iRecode\resources\更新门1.png)
   > $$
   > C_t = f_t * C_{t-1} + i_t * \overline{C_t}
   > $$
   > 上面操作就是对C<sub>t</sub> 进行更新，左边是前面遗忘门给出的f<sub>t</sub> 乘C<sub>t-1</sub> ,表示过去的信息有选择的遗忘(保留)。右边也同理，新的信息\overline C<sub>t</sub> 乘 i<sub>t</sub> 表示新的信息有选择的遗忘(保留)，最后再把这部分信息加起来，就是新的状态C<sub>t</sub> 了。
   >
   > 
   >
   > **输出门：** 
   >
   > ![](E:\md\iRecode\resources\输出门.png)
   > $$
   > \begin{align}
   > o_t &= \sigma (W_0[h_{t-1},x_t] + b_0)\\
   > h_t &= o_t * tanh(C_t)
   > \end{align}
   > $$
   > 最后就是LSTM的输出了，此时细胞状态c<sub>t</sub> 已经被更新了，这里的o<sub>t</sub> 还是用一个sigmoid函数，表述输出那些内容，而C<sub>t</sub> 通过tanh缩放后与o<sub>t</sub> 相乘，这就是这一个time step的输出了。

### 二、RNN训练过程

------

1. 输入输出

   ![image-20201104221554941](E:\md\iRecode\resources\RNN训练.png)






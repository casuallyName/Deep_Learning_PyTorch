# RNN条件生成

## 机器翻译

### V1: Encoder-Decoder

![image-20200229180824157](D:\Deep_Learning_PyTorch\docs\image\image-20200229180824157.png)

两个RNN，先编码，再解码预测

缺点：

1. s是个向量，长度有限，可能会造成信息瓶颈
2. 取得是Encoder的最后一个输出，里该输出进的此会被保留下来的概率会大，越靠前的信息越有可能会被丢弃

### V2: Attention-based Encoder-Decoder

![image-20200229181954495](D:\Deep_Learning_PyTorch\docs\image\image-20200229181954495.png)
>Encoder的输出加权平均，输入到Decoder中，Decoder的输出也用来更新下一组系数
>
>右侧Decoder继续向右移动

* 对所有输出进行线性加权
* 所以信息都可以看到，解决信息瓶颈问题

### V3: Bi-Directional Encode layer
![image-20200229184009198](D:\Deep_Learning_PyTorch\docs\image\image-20200229184009198.png)

* 加入双向RNN，可以提取上下文信息

### V4: Residual Encode layer

![image-20200229184116391](D:\Deep_Learning_PyTorch\docs\image\image-20200229184116391.png)

* 残差连接

###  GoogleRNN

![image-20200229184201623](D:\Deep_Learning_PyTorch\docs\image\image-20200229184201623.png)

 

## Encoder-Decoder

![image-20200301185822815](D:\Deep_Learning_PyTorch\docs\image\image-20200301185822815.png)

## Multi-model RNN

![image-20200301183731872](D:\Deep_Learning_PyTorch\docs\image\image-20200301183731872.png)

![image-20200301184842987](D:\Deep_Learning_PyTorch\docs\image\image-20200301184842987.png)

* 一个词投影成128的向量，在经过全连接形成256的向量
* 再经过一个RNN

* Multi-model  将多个输入（Embedding2，Recurrent，AlexNet的全连接）拼接，全连接到512层上
* 做softmax，取最大
* 继续下一轮

## Show And Tell

![image-20200301190255866](D:\Deep_Learning_PyTorch\docs\image\image-20200301190255866.png)

* 图像特征使用更强大的CNN提取 GoogNet
* 图像特征只提取一次
* LSTM生成文本

## Show Attend and Tell

![image-20200301190947647](D:\Deep_Learning_PyTorch\docs\image\image-20200301190947647.png)

* 输入图片取得kernel(n)个feature map（m,m）
* 某个位置上不同feature map上的值共同组成一个向量(则向量长度为n)，代表这个位置的特征表达，共有mXm个这样的向量（位置信息）
* 将位置信息和特征输入到LSTM

## Top Down Attention

![image-20200301193401381](D:\Deep_Learning_PyTorch\docs\image\image-20200301193401381.png)

* 与Show Attend and Tell相比，Top Down Attention使用了两个LSTM

  * 第一层LSTM负责学习attention，其输入为
    * 前一时刻Language Model的hidden state
    * 图像的全局信息（所有feature map的平均）
    * 输入词语的embedding
  * 第一步的输出再输入到Attend里计算 $\alpha$ ,再用 $\alpha$ 和 ${v_1,...,v_k}$ 做加权平均得到 $\hat{v_t}$ ，输入第二个LSTM

  * 第二个LSTM用来学习生成的词语


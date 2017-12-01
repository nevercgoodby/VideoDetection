https://www.zhihu.com/question/40382198

http://blog.csdn.net/sparkexpert/article/details/77587849

http://blog.csdn.net/sm9sun/article/details/53322325

http://blog.csdn.net/c571121319/article/details/78135901

http://blog.csdn.net/sinat_26917383/article/details/62231841

http://blog.csdn.net/roach_zfq/article/details/54426133

https://www.cnblogs.com/itor/p/7687013.html

1.Yahoo 最近开放了一个nsfw (not safe for work) 的model，测试了一下，效果还行。后续可以根据业务需求做一些微调（当然，这个时候你就得自己准备一些数据了）。地址： GitHub - yahoo/open_nsfw: code for running Model and code for Not Suitable for Work (NSFW) classification using deep neural network Caffe models这里还有一个网友基于上述模型搭建的一个online demo地址：nsfw   方便试玩

作者：Yubin Wang
链接：https://www.zhihu.com/question/40382198/answer/125502340
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

2.介绍一个开源实现：  https://github.com/boozyguo/mosaic

3.

作者：网易云
链接：https://www.zhihu.com/question/40382198/answer/237692470
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

一句话回答： 用mxnet的图像分类样例训练一个resnet-v2即可。传统的鉴黄算法准确率完全无法和深度学习模型相比。深度学习模型的泛化能力要强很多：对一般光照、图像尺度、清晰度和图片类型的变化都能有较强的鲁棒性。假如lz想要通过开源算法处理90%的情况，那么借助任何一个主流深度学习框架的通用物体识别模型，你都可以十分方便地训练出一个表现相当过得去的模型。所谓的主流深度学习框架，个人推荐mxnet、tensorflow、pytorch和caffe2。lz不必拘泥于具体选择哪一个，它们设计哲学和侧重略有不同，各有优劣。这几个框架背后都有顶级公司极强的研究团队长期支持，各自都有十分活跃的开源生态圈，会像不同编程语言一样共存下去。由于学习曲线都不陡，文档十分丰富，不妨都去试一试。所谓通用物体识别，这里是指图像的分类。卷积神经网络在图像分类任务上的表现已经和人接近。其最早的应用即是识别MNIST手写字符。第二次爆发则是因为近几年在ImageNet的ILSVRC比赛中大放异彩：<img src="https://pic2.zhimg.com/50/v2-762e55ee47ac6c90f499896d393680b5_hd.jpg" data-rawwidth="1037" data-rawheight="461" class="origin_image zh-lightbox-thumb" width="1037" data-original="https://pic2.zhimg.com/v2-762e55ee47ac6c90f499896d393680b5_r.jpg">自2012年AlexNet以来，卷积神经网络大幅度地降低了这个千类物体分类任务的错误率。所以可以效仿Yahoo的open_nsfw做个二分类。我不建议直接用open_nsfw，因为这个模型在国内线上内容审核的场景下表现很差。原因很简单，Yahoo的训练图像和产品使用的实际情况不匹配。所以还是建议lz自行训练一个模型。收集数据自然是个不小的任务。但是谁还没有上百G的种子呢，大家都懂的。数据量是模型效果的直接决定因素。我这里引用一个简化版的经典学习理论中的泛化误差公式：这里ϵ是误差，m是训练样本数量。可以看到在其他条件不变的情况下，增加样本数量可以有效地降低误差的上界。至于需要多少样本，我可以负责任地告诉lz，正常图像和色情图像各10万张即可训练出一个效果还可以的模型，自己玩玩是够了。但若想做成产品级的应用，这样的效果是不够的。那么如何才能更完善地解决这个问题呢？分类模型准确率有限，而鉴黄严格定义应该属于细粒度分类问题，其有效特征又很容易和人体正常部位混淆。我看lz一心问道，天资聪颖，是个不世出的鉴黄奇才，就在这里抛砖引玉提出一些研究上的方向：收集合适的数据集：数据是根本，挖掘有效、有价值、有难度的数据需要对业务的熟稔和大量的时间积累。当然，也有一些技术上的方法可以平衡，例如OHEM和最近的Focal Loss，但也要建立在数据量足够大的基础上。检测/多任务模型：将卷积神经网络的输出略作修改，我们即可获得目标的位置信息。这不但可以提升模型输出的稳定性，也能在训练过程中增加有效的监督，让网络更充分地学习局部特征。但这会带来一定的上下文信息丢失。同一个网络进行分类和检测的多任务训练是一个解决此问题的方向。细粒度分类：这是个自成体系研究领域，比较关键的问题是如何高效地关注到局部特征。近期围绕Soft Attention有一些比较出彩的成果。毕竟例如CUB-200-2011这样的数据集近几年准确率可是从17%上升到了85%。通过条件生成模型进行调试和判别器增强。利益相关：网易云易盾 反垃圾智能反垃圾_文本检测过滤_图片检测过滤_视频检测-网易云

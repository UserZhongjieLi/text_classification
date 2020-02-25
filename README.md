# text_classification

使用rnn,lstm,gru,fasttext,textcnn做文本分类，以及对比

直接在train.py修改配置文件--config的路径即可，也可以通过命令行的方式。

### 结果对比
#### RNN :93.2%             
参数："input_dim":20002,"embedding_dim": 300,"hidden_dim": 256,"output_dim": 1,"n_layers":1,"bidirectional": true,"dropout": 0.5, "batch_first": false
对于文本二分类（weibo数据）
如果不使用双向：bidirectional=false，准确率在50%左右，等于瞎猜（即使n_layer大于1 也没用）
如果使用双向：bidirectional=true,准确率能保证上90%，看来单向传播的时候，是会丢失很多前面的信息
对于参数 n_layer,层数的增加对于rnn模型来说，在这个数据集上看不到能带来多大的性能提升，反而会产生过拟合现象（bidirectional=true）
总结：是否是双向bidirectional对结果影响很大
#### LSTM :96.8%
参数："input_dim":20002,"embedding_dim": 300,"hidden_dim": 256,"output_dim": 1,"n_layers":1,"bidirectional": true,"dropout": 0.5, "batch_first": false
对于文本二分类任务（weibo数据）
不使用双向：bidirectional=false,准确率也能上96%.但是当n_layers设置大于1的时候，准确率50%，多一层的传播，导致信息丢失。
使用双向：bidirectional=true，准确率能上97%，能记住上下文信息很重要。但是当n_layers设置大于1的时候，准确率50%，不会带来性能的提升，反而会导致文本信息的丢失。
总结：n_layer层数的增加导致性能下降
分析：
1. 层数多，使模型丢失了语义信息。
2. 对模型输出的结果设计不当，导致模型没有获取到上下文的语义信息。当n_layers 大于1的时候导致性能下降的原因还有一种可能是我model只取了最后输出的hidden层，如果将所有的output层取出来，求均值得到的可能结果会更好一些。最后输出的tensor选取也是至关重要。

#### GRU:96.9%
对于文本二分类任务（weibo数据）
其表现和LSTM基本一致97%，想比于LSTM 会减轻过拟合现象。

#### FastText、TextCNN、TextCNN1D
对于文本二分类（weibo数据）
效果可以直接上93%
但是与RNN系列模型相比，词汇表的大小不宜过大，过大的会导致模型性能的急剧下降。

fast_text:93%左右,但是其计算速度确实很快。
text_cnn: 92.9% 左右.
text_cnn_1d : 93%


可以调的参数还有很多，模型也可按照自己的想法去优化


## todo
加载word_embedding 进行训练。
文本多分类---->再另外开一个仓库。




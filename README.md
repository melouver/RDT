# RDT
Implement RDT3.0 and GBN (pipelined version) for Computer Network textbook's assignment.

Lab Writeup:
[writeup](http://media.pearsoncmg.com/aw/aw_kurose_network_3/labs/lab5/lab5.html)

[sine-x](https://github.com/ZhangZhenghao) had finished this lab which implemented bi-directional data transfer.But I'd rather 
implement only unidirectional data transfer so I studied his code and remove some to complete this lab.
**Special thanks** to his implementation of multitimer and buffer on sender-side , that's really neat and sophiscated.

Ref:
https://sine-x.com/kurose-ross-a-reliable-transport-protocol/

这是计算机网络自顶向下方法教科书的配套实验之一，也是我比较关注的运输层实验。因此我抽了3天时间完成了这个实验，收获不小。
在作者提供的模拟环境下，会出现自定义的包丢包、损毁事件，我们需要做的是填好相应的事件响应函数，以达到按序、完整的传输效果。
这些事件包括发送端、接受端的来自上下层请求，以及发送端的超时事件。发送端有buffer，接收端没有设置buffer。

发送端上层请求：需要将应用层数据封包，传输给网络层，其中需要填好如校验码、序列号等信息。
发送端下层请求：来自接收端的响应ACK，发送端需要适当地进行窗口的移动或者重传。
接收端下层请求：来自发送端的包，接收端会验证包的完整性并作出反应。
超时事件： 如果丢包事件发生，则发送方会在超时后进行重传，我们这里使用GBN的重传策略，会重传窗口内所有的包([base,nextseq))。

定时器：
发送方使用一个单一定时器模拟了多个定时器，多次使用循环队列(用数组实现的循环队列)来维护计数器队列的相关信息如seqnum、expire time。

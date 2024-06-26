# lab1

欢迎来到CS144：计算机网络入门。在这个热身活动中，你将在你的电脑上安装Linux，学习如何手动在互联网上执行一些任务，编写一个在互联网上获取网页的C++小程序，并在内存中实现网络的关键抽象之一：写入者和读取者之间的可靠字节流。我们预计这个热身活动将花费你2到6小时的时间（未来的实验将占用你更多的时间）。关于实验作业有三点快速说明。


在这个热身实验的下一部分，你将编写一个简短的程序，该程序通过互联网获取一个网页。你将利用Linux内核以及大多数其他操作系统提供的一个特性：在两个程序之间创建一个可靠的双向字节流，一个程序在你的计算机上运行，另一个程序在互联网上的另一台计算机上运行（例如，Apache或nginx这样的Web服务器，或者netcat程序）。

这个特性被称为流套接字。对于你的程序和Web服务器来说，套接字看起来就像一个普通的文件描述符（类似于磁盘上的文件，或者stdin或stdout I/O流）。当两个流套接字连接时，任何写入一个套接字的字节最终都会按照相同的顺序从另一台计算机上的另一个套接字中输出。

然而，实际上，互联网并不提供可靠字节流的服务。相反，互联网真正做的只是尽其“最大努力”将称为互联网数据报的短片段数据送达目的地。每个数据报包含一些元数据（头部），指定了诸如源地址和目标地址——它来自哪台计算机，以及它要去哪台计算机——以及一些要送达目标计算机的有效载荷数据（最多约1500字节）。

虽然网络试图传递每一个数据报，但实际上数据报可能会（1）丢失，（2）顺序错乱地被传递，（3）内容被更改后传递，甚至（4）被复制并传递多次。通常，连接两端的操作系统的任务是将“尽力而为的数据报”（互联网提供的抽象）转换为“可靠的字节流”（应用程序通常想要的抽象）。

两台计算机必须合作，确保流中的每个字节最终按照其在队列中的正确位置被传递到另一侧的流套接字。他们还必须告诉对方他们准备接受对方的多少数据，并确保不发送超过对方愿意接受的数据。所有这些都是使用一个在1981年确定下来的共同约定的方案来完成的，这个方案被称为传输控制协议，或者TCP。

在这个实验中，你将简单地使用操作系统对传输控制协议的预先支持。你将编写一个名为“webget”的程序，该程序创建一个TCP流套接字，连接到一个Web服务器，并获取一个页面——就像你在这个实验的早些时候所做的那样。在未来的实验中，你将实现这个抽象的另一侧，通过自己实现传输控制协议，将不那么可靠的数据报转换为可靠的字节流.
+ 用户数据报协议UDP只在IP的数据报服务上增加了很少的一点功能
  + 复用，分用功能
  + 差错检测功能

+ UDP的主要特点
  + UDP是无连接的
    + 发送数据之前不需要建立连接
    + 发送数据结束后也没有连接可释放
  + UDP不保证可靠交付，使用尽最大努力交付
  + UDP是面向报文的
    + 发送方UDP对应用程序交下来的报文，在添加首部后就向下交付IP层
    + 接收方UDP对IP层交上来的报文，在去除首部后就向上交付给应用程序
    + UDP对应用层交下来的报文，或IP层交上来的报文，既不合并，也不拆分，而是保留这些报文的边界
    + UDP一次交付一个完整的报文，因此应用程序必须选择合适的报文大小
      + 若报文太长，UDP交付给IP层后，IP层可能需要分片
      + 若报文太短，会使得IP数据报首部的相对长度太大，降低网络效率
  + UDP没有拥塞控制
    + 网络出现拥塞不会使得发送速率降低，源主机会以恒定的速率发送数据，这对一些实时应用很重要
    + UDP允许在网络拥塞时丢失一些数据，但是不允许数据有太大的时延
    + 但是当很多源主机都向网络发送高速率的实时数据的时候，网络可能会发生严重拥塞，导致大家都无法正常接收
  + UDP支持一对一，一对多，多对一，多对多的交互通信
  + UDP的首部开销小
    + UDP首部只有8个字节，相比于TCP首部的20字节，要短得多
  
+ UDP的首部格式，8字节
  + 源端口，2字节
    + 需要对方回信时选用，不需要时可用全0
  + 目的端口，2字节
    + 如果接收方UDP发现收到的报文中的目的端口号不正确（不存在对应于该端口号的应用进程），就丢弃该报文，并使用网际控制报文协议ICMP发送”端口不可达“的差错报文给发送方
    + 注意：虽然UDP之间的通信需要用到端口号，但由于UDP的通信是无连接的，因此不需要使用套接字（TCP之间的通信必须在两个套接字之间建立连接）
  + 长度，2字节
    + UDP用户数据报的长度，其最小值是8（仅有首部）
  + 校验和，2字节
    + 检测UDP数据报在传输中是否有错，有错就丢弃
    + 在计算校验和的时候，要在UDP用户数据报之前增加12字节的伪首部
    + 伪首部既不向下传送，也不向上递交，仅仅是为了计算校验和
    + UDP的校验和是把伪首部，首部和数据部分一起都检验
    + 校验和既检查了UDP数据报的源端口号和目的端口号以及UDP用户数据报的数据部分，还检查了IP数据报的源IP地址和目的IP地址

+ UDP的伪首部格式，12字节
  + 源IP地址，4字节
  + 目的IP地址，4字节
  + 全0，1字节
  + IP首部中协议字段的值，对于UDP，此协议字段值为17，1字节
  + UDP用户数据报的长度（包括首部和数据部分），2字节

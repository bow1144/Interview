 ## Netty 网络传输

### Netty传输位于网络结构的哪一层？
* 传输层：支持TCP和UDP；可以开启，关闭连接和传输数据，`EventLoop`和`EventGroup`等组件基于NIO多路复用
* 应用层：支持WebServer、HTTP等组件；`ChannelPipeline`和`ChannelHandler`等组件了一个时间处理链

### 项目中Netty的作用和执行流程？
* 作用：
  - 高效的网络传输
  - 抽象NIO的复杂性简化网络编程
  - 提供组件方便网络数据处理
* 执行流程：
  - 客户端发起请求：
    - 客户端通过NettyAPI创建一个`Channel`，连接到服务器的指定端口
    - 客户端将RPC调用信息封装成请求消息，通过Netty编码器（Encoder）将请求消息序列化成字节流
    - 将字节流数据发送给服务器
  - 服务端接收请求并处理：
    - 服务端通过NettyAPI监听端口，等待客户端的连接请求
    - 接收到请求后服务端通过（Decoding）解码器将收到的字节流反序列化
    - 服务端通过请求消息中的方法和参数，反射调用本地服务实例，并将执行结果封装成响应消息
    - 服务端通过Netty字编码器将响应消息封装成字节流，并通过网络发送回客户端
  - 客户端接收响应
    - 客户端接收响应字节流后，反序列化为响应消息
    - 业务处理

### 为什么会出现粘包问题，如何解决？
* netty 默认使用TCP存储，TCP是面向流的协议，不知道数据的界限
* 粘包问题是接收消息时不能把两条信息拆开导致少读多读问题
* 解决：发送信息时同时发送信息的大小

### 有哪些序列化方式，绝得哪种最好？
* Java对象序列化：
  - 优点：兼容性高，Java内部传输方便
  - 缺点：时间较长，不支持跨语言
* JSON：
  - 优点：可读性强、跨语言支持
  - 缺点：效率较低
* Protobuf、Hession
  - 高效、简单易用
  - 可读性差、安全性不足
 
### 自定义编码/解码器有什么好处？
* 编码解码过程简单易读
* 消息头加工，解决粘包问题
* 消息头加入了`messageType`消息类型，扩展读消息机制

## ZooKeeper注册中心

### Zookeeper在项目中的角色？为什么使用ZK？
* 作为项目的注册中心，实现服务注册，服务发现和维护服务状态的功能
* 高可用、一致性、Hadoop等框架都有ZK

### 注册中心的意义？
* 服务注册与发现：各个微服务可以将自己的地址信息注册到中心
* 动态性：服务器的数量和位置可能会不断变化，ZK注册中心能动态处理变化，使得用户获取的信息是最新的
* 增强微服务的去中心化：微服务之间的依赖关系不再是直接的函数引用，而是注册中心间接调用；这样可以提高项目的灵活性
* 提升容错性，部分节点故障仍能稳定运行

### 如何更新动态缓存？
* 不用经典的`cache side`算法原因是*如果服务在ZK注册了一个新地址，客户端永远感知不到*
* 要在ZK注册Watcher，监听注册中心的变化，实现本地缓存的动态更新

## 算法
### 三种负载均衡算法比较？
* 轮询法：
  - 原理：将所有请求按顺序轮流分配
  - 优点：简单成本低
  - 缺点：死板、导致某些服务器负载过大或过小
* 随机法：
  - 随机分发
  - 均匀分配，简单
  - 服务器负载过大或过小
* 一致性哈希法
  - 将输入、服务器通过Hash函数映射成一个环，客户端请求在环上顺时针查找
  - 服务器失效时减少缓存失效的数量，提高系统可用率
  - 可能存在哈希环倾斜，导致缓存分配不均
 
### 限流算法有哪些？
- 针对IP、业务ID等限流
- 计数器法
  - 以固定时间窗口为单位，时间窗口结束后清零
  - 简单但用户体验极差
- 滑动窗口算法
  - 改成随时间向前的滑动窗口
  - 可以应对激增算法，略显复杂
- 漏桶算法
  - 将流量比作水，容量比作桶
  - 无法应对突发流量，桶满后新用户体验极差
- 令牌桶算法
  - 请求被处理前获取令牌，桶里装的是令牌，按速率分发令牌
  - 动态调整、但对令牌产生速率有依赖性

## 场景题
### 本地缓存怎么做？能保证缓存和服务的一致性吗？
* 客户端设计一个缓存层，每次调用时先访问缓存，避免调用注册中心
* 一致性使用了ZK的监听机制，在服务端注册Watcher，注册中心的服务地址变动时，Watcher异步通知服务端缓存，实现二者一致性

### 某个服务多个节点承压能力不一样，怎么办？
* 将服务器的负载能力写入注册中心，控制流量分发

### 网络抖动导致某一节点下线，过一会网络好了，考虑这个问题吗？
* 调用失败后，RPC重试并发送请求
* 项目使用了`Google Guava`框架实现失败重试的功能

### 每个服务都重试吗？
* 将服务分为两类：幂等和不幂等；将幂等服务放在白名单（ZK中）中
* 对于幂等的项目，可以重试；否则不能重试

### 如果下游有一个服务的服务器全部宕机了，怎么办？
* 项目在客户端调用的链路头部设置了熔断器，失败超过阈值后熔断器关闭，阻止后续请求；
* 一段时间后熔断器半开，根据请求决定开关

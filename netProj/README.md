## 一、TCP连接管理

### 1. 三次握手·建立连接·测试

测试客户端所需文件：

- `receiver`
- `test_client.c -> client`

测试服务端所需文件：

- `sender`
- `test_server.c -> server`



测试建立连接（测试必须在客户端进行）

```sh
vagrant@client:/vagrant/tju_tcp/test$ ./test establish
```



### 2. 四次挥手·断开连接·测试

- 关闭服务端`test_close_server.c -> close_server`
- 关闭客户端`test_close_client.c -> close_client`
- [服务端]先后断开连接&双方同时断开连接`test_close_server_for_server.c -> close_server_for_server`
- [客户端]先后断开连接&双方同时断开连接`test_close_client_for_server.c -> close_client_for_server`

​	

测试断开连接（测试必须在客户端进行）

```sh
vagrant@client:/vagrant/tju_tcp/test$ ./test close
```



## 二、可靠数据传输RDT

- 测试客户端`test_rdt_client.c -> rdt_client`

- 测试服务端`test_rdt_server.c -> rdt_server`



测试可靠数据传输（测试必须在客户端进行）

```sh
vagrant@client:/vagrant/tju_tcp/test$ ./test rdt
```



## 三、流量控制

#### 测试思路

从可靠数据传输记录的`server.event.log`日志中提取出拥塞窗口、接收方接收窗口和发送窗口大小，将三个窗口随时间变化的曲线绘制在一张图中，检查曲线趋势以及三者之间的大小关系是否正确

#### 测试方法

在客户端终端输入如下指令，即可在`/vagrant/tju_tcp/test/`文件下生成`AllWindowSize_VS_Time.png`，根据png图片展示的曲线趋势以及三者之间的大小关系进行人工测试

```
/vagrant/tju_tcp/test/gen_graph_win /vagrant/tju_tcp/test/server.event.log
```



## 四、拥塞控制

测试拥塞控制的3个文件：

- `test_congestion.py`
- `gen_graph_seq.py` 绘制数据包的**SeqNum随时间变化**的曲线
- `gen_graph_win.py` 绘制**拥塞窗口随时间变化**的曲线

#### 步骤一：测试->生成日志文件->并画SeqNum随时间变化的曲线图（生成1个日志文件和1个图）

```
python3 /vagrant/tju_tcp/test/test_congestion.py
```

生成的客户端日志`client.event.trace`

#### 步骤二：画拥塞窗口随时间变化的曲线图（生成1个日志文件和4个图）

```
python3 /vagrant/tju_tcp/test/gen_graph_win.py
/vagrant/tju_tcp/test/client.event.trace
```

### 最终产生（5个图）

步骤一：

- `SeqNum_VS_Time.png`：SeqNum随时间变化的曲线

步骤二：

- `CongestionWindowSize_VS_Time.png`：拥塞窗口随时间变化的曲线
- `ReceiveWindowSize_VS_Time.png`：接收方接收窗口随时间变化的曲线
- `SendWindowSize_VS_Time.png`：发送窗口随时间变化的曲线
- `AllWindowSize_VS_Time.png`：拥塞窗口、接收方接收窗口、发送窗口随时间变化的曲线（该曲线可 用于观察拥塞窗口、接收方接收窗口、发送窗口三者随时间之间的变化趋势以及三者之间的关系，辅助流量控制的观测）


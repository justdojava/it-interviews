# SOCKET通信实现
##Socket
Socket 是通信的句柄

一般而言，Socket编程包括：
1. 创建Socket

2. 打开连接到Socket的输入/输出流

3. 对Socket进行读/写

4. 关闭Socket

##server端的Socket编程实现
**step1**： server端口建立一个ServerSocket，从其accept()方法中得到一个Socket。

其中，ServerSocket只负责在端口上**监听**，没有读写功能
```java
ServerSocket serverSocket = new ServerSocket(6228);
Socket socket = serverSocket.accept();
```

**step2**：从Socket上得到InputStream/OutputStream(如果读写字符则用BufferedReader/PrintWriter)
```java
BufferedReader reader = new BufferReader(new InputStreamReader(socket.getInputStream()));
PrintWriter writer = new PrintWriter(socket.getOutputStream());
```

**step3**：用step2得到的IO进行读写操作
**step4**：关闭socket，顺序是：reader->socket->serverSocket

##client端的Socket编程实现
**step1**： client建立一个Scoket来连接server端口与ip


```java
Socket socket = new Socket("127.0.0.1", 6228);
```

剩下步骤与server端口一致
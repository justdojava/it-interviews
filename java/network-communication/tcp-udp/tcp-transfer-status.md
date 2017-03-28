# TCP状态迁移图
![状态转移图](http://7d9o4k.com1.z0.glb.clouddn.com/tcp-status.JPG)


|序号|状态|备注|
|--|---|---|
|1|CLOSED| 关闭状态|
|2|LISTEN|server在等待连接的状态（server在调用socket、bind、listen进入此状态），等待客户端的连接|
|3|SYN_SENT|client发起连接，发SYN给server（如果server不能连接，直接进入CLOSED）|
|4|SYN_RCVD|与3相对，server收到SYN后，server进入此状态；同时server回应一个ACK+SYN给client；当client收到server回复的ACK+SYN后，也会进入此状态|
|5|ESTABLISHED|完成3次握手，server与client进入此状态|
|6|FIN_WAIT_1|主动关闭的一方，从状态5进入此状态，并发送FIN给对方|
|7|FIN_WAIT_2|主动关闭的一方，收到对方的FIN ACK进入此状态；此后不能再接收对方数据，但是可以发送数据|
|8|CLOSE_WAIT|接收到FIN后，被动关闭方进入此状态，同时回复ACK|
|9|LAST_ACK|被动关闭方发送FIN，由状态8进入此状态|
|10|CLOSING|两边同时发起关闭请求时，会由FIN_WAIT_1进入此状态。具体动作是，接收到FIN请求，同时响应一个ACK|
|11|TIME_WAIT|3个状态可进入此状态，具体分析见下|


##TIME_WAIT
1. 由**FIN_WAIT_2**进入此状态：在双方不同时发起FIN的情况下，主动关闭的一方在完成自身发起的关闭请求后，接收到被动关闭一方的FIN后进入的状态。

2. 由**CLOSING**状态进入:双方同时发起关闭，都做了发起FIN的请求，同时接收到了FIN并做了ACK的情况下，由CLOSING状态进入。

3. 由**FIN_WAIT_1**状态进入：同时接受到FIN（对方发起），ACK（本身发起的FIN回应），与b的区别在于本身发起的FIN回应的ACK先于对方的FIN请求到达，而b是FIN先到达。这种情况概率最小。




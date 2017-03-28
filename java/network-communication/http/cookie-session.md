# Cookie与Session
Cookie与Session都是为了用来保存状态信息，保存客户端状态的机制，它们是我了解决HTTP无状态问题而努力

- Session可以用Cookie来实现，也可以用URL回写机制实现

![Cookie与Session](http://7d9o4k.com1.z0.glb.clouddn.com/cookie-session.png)

##Session
Session是一种**服务器端**的机制，服务器使用一种类似于散列表的结构来保存信息(Session id来保存用户信息)

###Cookie实现Session机制
1. 服务器给每个 Session分配一个唯一的 JSESSIONID， 并通过 Cookie发送给客户端

2. 当客户端发起新的请求的时候，将在 Cookie头中携带这个 JSESSIONID。这样服务器能够找到这个客户端对应的 Session

![Cookie实现Session机制](http://7d9o4k.com1.z0.glb.clouddn.com/cookie-session-2.png)

###URL回写实现Session机制
服务器在发送给浏览器页面的所有链接中都携带 JSESSIONID的参数，这样客户端点击任何一个链接都会把 JSESSIONID带会服务器

##Cookie
Cookies是服务器在本地机器上存储的**小段文本**，并随每一个请求发送至同一个服务器，用来保存一种状态

##两者比较
* Cookie将状态保存在客户端， Session将状态保存在服务器端

* Session是针对每一个用户的，变量的值保存在服务器上，用一个sessionID来区分是哪个用户session变量,这个值是通过用户的浏览器在访问的时候返回给服务器，当客户禁用cookie时，这个值也可能设置为由get来返回给服务器

##具体应用-基于表单的认证
1. 客户端发送：已登录信息（username && passport）=>服务器端

2. 服务器端回：包含session_id的Cookie =>客户端






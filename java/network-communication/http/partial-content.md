# 续传与多线程下载原理

##断点续传：HTTP协议的 GET方法，支持只请求某个资源的某一部分
* 响应码：206 Partial Content 部分内容响应

* Range 请求的资源范围

* Content-Range 响应的资源范围

##多线程下载
* 下载工具开启多个发出 HTTP请求的线程

* 每个 http请求只请求资源文件的一部分： Content-Range: bytes 20000-40000/47000

* 合并每个线程下载的文件



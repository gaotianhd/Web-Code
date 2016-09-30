---
title: python网络编程
---
`osi模型：从上到下分别是应用层，表示层，会话层，传输层，网络层，链路层，物理层。`
--------------------------------------------------------------------------

物理层：提供为建立、维护和拆除物理链路所需要的机械的、电气的、功能的和规程的特性；有关的物理链路上传输非结构的位流以及故障检测指示。

数据链路层：在网络层实体间提供数据发送和接收的功能和过程；提供数据链路的流控。

网络层：控制分组传送系统的操作、路由选择、拥护控制、网络互连等功能，它的作用是将具体的物理传送对高层透明。

传输层：提供建立、维护和拆除传送连接的功能；选择网络层提供最合适的服务；在系统之间提供可靠的透明的数据传送，提供端到端的错误恢复和流量控制。

会话层：提供两进程之间建立、维护和结束会话连接的功能；提供交互会话的管理功能，如三种数据流方向的控制，即一路交互、两路交替和两路同时会话模式 。

表示层：代表应用进程协商数据表示；完成数据转换、格式化和文本压缩。

应用层：提供OSI用户服务，例如事务处理程序、文件传送协议和网络管理等。

--------------------------------------------------------------------------

协议：协议(networking protocol)，网络协议的简称，网络协议是通信计算机双方必须共同遵从的一组约定。如怎么样建立连接、怎么样互相识别等。只有遵守这个约定，计算机之间才能相互通信交流。它的三要素是：语法、语义、时序。

--------------------------------------------------------------------------

TCP/IP协议：TCP/IP协议，Transmission Control Protocol/Internet Protocol的简写，中译名为传输控制协议/因特网互联协议，又名网络通讯协议，是Internet最基本的协议、Internet国际互联网络的基 础，由网络层的IP协议和传输层的TCP协议组成。TCP/IP 定义了电子设备如何连入因特网，以及数据如何在它们之间传输的标准。协议采用了4层的层级结构，每一层都呼叫它的下一层所提供的协议来完成自己的需求。`而实际上我们现在所说的TCP/IP 协议有时想表达的是tcp/ip协议簇`，这是一组常用的协议。
# socket套接字：

Socket套接字分为以下几种：

流式套接字(SOCK_STREAM)
`提供了一个面向连接、可靠的数据传输服务，数据无差错、无重复的发送且按发送顺序接收。`内设置流量控制，避免数据流淹没慢的接收方。数据被看作是字节流，无长度限制。

数据报套接字(SOCK_DGRAM)

`提供无连接服务。数据包以独立数据包的形式被发送，不提供无差错保证，数据可能丢失或重复，顺序发送，可能乱序接收。`

原始套接字(SOCK_RAW)

可以对较低层次协议如IP、ICMP直接访问。

使用协议，IP，端口号来发消息

字节序：小端序：低字节存储在低地址
       大端序：高序字节存储在低地址

inet_aton('ip')将IP地址转化为网络字节序地址。

inet_ntoa('网络地址')还原

htons(端口号)将端口号转化为网络端口号

ntohs()还原

--------------------------------------------------------------------------
## 使用socket()模块创建套接字
```python
socket(socket_family, socket_type, protocol=0)
```

socket_family 可以是 AF_UNIX 或 AF_INET。socket_type 可以是 SOCK_STREAM 或 SOCK_DGRAM。这几个常量的意义可以参考之前的解释。protocol 一般不填，默认值为 0。 创建一个 TCP/IP 的套接字，你要这样调用 socket.socket()：

tcpSock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

服务器端套接字函数：
`s.bind()        绑定地址（主机，端口号对）到套接字`
`s.listen()      开始 TCP 监听`
`s.accept()      被动接受 TCP 客户的连接， （阻塞式）等待连接的到来`

客户端套接字函数：
`s.connect()     主动初始化 TCP 服务器连接`

公共用途的套接字函数：
`s.recv()        接收 TCP 数据`
`s.send()        发送 TCP 数据`
s.sendall()     完整发送 TCP 数据
`s.recvfrom()    接收 UDP 数据`
`s.sendto()      发送 UDP 数据`
`s.getpeername() 连接到当前套接字的远端的地址`
s.getsockname() 当前套接字的地址
s.getsockopt()  返回指定套接字的参数
s.setsockopt()  设置指定套接字的参数
`s.close()       关闭套接字`
# TCP（即传输控制协议）：
`是一种面向连接的传输层协议，它能提供高可靠性通信(即数据无误、数据无丢失、数据无失序、数据无重复到达的通信)`
适用情况：`适合于对传输质量要求较高，以及传输大量数据的通信。在需要可靠数据传输的场合，`通常使用TCP协议MSN/QQ等即时通讯软件的用户登录账户管理相关的功能通常采用TCP协议。

--------------------------------------------------------------------------

TCP编写服务器端和客户端：

服务器端：
```python
#! /usr/bin/python
#coding=utf-8
from socket import *
from time import ctime
HOST = '192.168.1.247'
PORT = 5858
BUFSIZE = 1024
ADDR = (HOST,PORT)
sockfd = socket(AF_INET,SOCK_STREAM)
sockfd.bind(ADDR)
sockfd.listen(5)#同时可以有多少个监听在等待，就像打电话似的，通话过程中再有电话会等待。超过5个后会拒绝
print "waiting for connection......"
while True:
    connfd,addr = sockfd.accept()#连接一个就在等待队列里面减少一个，返回值是两个,第一个是一个新的套接字
    #sockfd是用在本程序的一些操作，比如bind等等。connd是用来和用户进行联系，addr同时接受到了ip地址
    print "connected from :",addr
    while True:
        data = connfd.recv(BUFSIZE)#一次接收的最大字节数
        if not data:
            break
        connfd.send("[%s] : %s"%(ctime(),data))
sockfd.close()
connfd.close()
```
分析：定义访问的ip地址，端口号。地址是由ip地址和端口号组成的元组。定义流式套接字。绑定地址，设置监听。accept()是用来连接客户端的，会返回一个用于和客户端交换信息的新的套接字，和用户端的地址信息。一个recv对应客户端一个send，一个send对应客户端的recv。
客户端：
```python
#! /usr/bin/python
from socket import *
import sys
HOST = sys.argv[1]
PORT = int(sys.argv[2])
BUFSIZE = 1024
ADDR = (HOST,PORT)
sockfd = socket(AF_INET,SOCK_STREAM)
sockfd.connect(ADDR)
while True:
    data = raw_input('>')
    if not data:
        break
    sockfd.send(data)
    data = sockfd.recv(BUFSIZE)
    print data
sockfd.close()
```
分析：动态的从终端输入ip和端口号，向服务器端传值send，接收服务器端值recv。

--------------------------------------------------------------------------
# UDP（User Datagram Protocol）用户数据报协议:
`是不可靠的无连接的协议。在数据发送前，因为不需要进行连接，所以可以进行高效率的数据传输。`
适用情况：发送小尺寸数据（如对DNS服务器进行IP地址查询时）在接收到数据，给出应答较困难的网络中使用UDP。（如：无线网络）适合于广播/ 组播式通信中。MSN/QQ/Skype等即时通讯软件的点对点文本通讯以及音视频通讯通常采用UDP协议。流媒体、VOD、VoIP、IPTV等网络多 媒体服务中通常采用UDP方式进行实时数据传输。

UDP编写服务器端和客户端：

服务器端：
```python
#! /usr/bin/python
from socket import *
from time import ctime
HOST = '192.168.1.247'
PORT = 5858
BUFSIZE = 1024
ADDR = (HOST,PORT)
sockfd = socket(AF_INET,SOCK_DGRAM)
sockfd.bind(ADDR)
while True:
    print "waiting for message"
    data,addr = sockfd.recvfrom(BUFSIZE)
    print "client addr : ",addr
    print "client data : ",data
    sockfd.sendto('[%s : %s]'%(ctime(),data),addr)

sockfd.close()
```
分析：到绑定地址同理。UDP不需要accept直接就可以使用recvfrom和sendto进行信息传递。recvfrom返回值是数据和地址信息，sendto参数也是数据和地址信息。

客户端：
```python
#! /usr/bin/python
from socket import *
import sys
HOST = sys.argv[1]
PORT = int(sys.argv[2])
BUFSIZE = 1024
ADDR = (HOST,PORT)
sockfd = socket(AF_INET,SOCK_DGRAM)
while True:
    data = raw_input('>')
    if not data:
        break
    sockfd.sendto(data,ADDR)
    data,ADDR = sockfd.recvfrom(BUFSIZE)
    print data
sockfd.close()
```
分析：动态从终端接收ip和端口号。用sendto向服务器端传值，用recvfrom接收服务器端的值（注：从终端输入的的都是字符串，所以端口号要强转）

--------------------------------------------------------------------------

# 使用ftp实现文件服务器：
服务器端代码：
```python
#! /usr/bin/python
#coding=utf-8
from socket import *
import os
from time import sleep
import sys
class server:
    BUFSIZE = 1024
    def __init__(self,sockfd):
        self.sockfd = sockfd
    def do_list(self):
        filelist = os.listdir('.')
        for i in filelist:
            self.sockfd.send(i)
            sleep(0.1)
        print "list ok!"
    def download(self,filename):
        f = open(filename,'r')
        while True:
            data = f.readline()
            if not data:
                break
            self.sockfd.send(data)
            sleep(0.1)
        f.close()
        print "download ok!"
        return
    def upload(self,filename):
        f = open(filename,'w')
        while True:
            data = self.sockfd.recv(self.BUFSIZE)
            if not data:
                break
            f.write(data)
        f.close()
        print "upload ok!"
        return
def main():
    if len(sys.argv) < 3:
        print "input valid arg"
        sys.exit(0)
    HOST = sys.argv[1]
    PORT = int(sys.argv[2])
    BUFSIZE = 1024
    ADDR = (HOST,PORT)
    sockfd = socket(AF_INET,SOCK_STREAM)
    sockfd.bind(ADDR)
    sockfd.listen(5)
    while True:
        connfd,addr = sockfd.accept()
        print "connected from :",addr
        data = connfd.recv(BUFSIZE)
        ftp = server(connfd)
        print "----",data
        if data[0] == 'L':
            ftp.do_list()
        if data[0] == 'D':
            ftp.download(data[2:])
        if data[0] == 'U':
            ftp.upload(data[2:])
        connfd.close()
    sys.exit(0)
if __name__ == '__main__':
    main()
```
客户端代码：
```python
#! /usr/bin/python
#coding=utf-8
from socket import *
import sys
import os
from time import *
class client:
    BUFSIZE = 1024
    def __init__(self,serveraddr):
        self.serveraddr = serveraddr
    def do_list(self):
        data = 'L'
        sockfd = socket(AF_INET,SOCK_STREAM)
        sockfd.connect(self.serveraddr)
        sockfd.send(data)
        while True:
            data = sockfd.recv(self.BUFSIZE)
            if not data:
                break
            print "%s"%data
        print "list ok!"
    def download(self,filename):
        f = open(filename,'w')
        data = 'D '+filename
        sockfd = socket(AF_INET,SOCK_STREAM)
        sockfd.connect(self.serveraddr)
        sockfd.send(data)
        while True:
            data = sockfd.recv(self.BUFSIZE)
            if not data:
                break
            f.write(data)
        print "download ok!"
        sockfd.close()
        f.close()
        return
    def upload(self,filename):
        f = open(filename,'r')
        print filename
        data1 = 'U '+filename
        sockfd = socket(AF_INET,SOCK_STREAM,0)
        sockfd.connect(self.serveraddr)
        sockfd.send(data1)
	       sleep(0.1)
        while True:
            data = f.readline()
            if not data:
                break
            sockfd.send(data)
        print "uplaod ok!",filename
        sockfd.close()
        f.close()
        return
def main():
    if len(sys.argv) < 3:
        print "error"
        sys.exit(0)
    HOST = sys.argv[1]
    PORT = int(sys.argv[2])
    ADDR = (HOST,PORT)
    ftp = client(ADDR)
    while True:
        print "---list---"
        print "---download---"
        print "---upload---"
        print "---quit---"
        data = raw_input('>')
        if data[0] == "l":
            ftp.do_list()
        elif data[0] == "d":
            ftp.download(data[9:])
        elif data[0] == "u":
            ftp.upload(data[7:])
        elif data[0] == "q":
            sys.exit(0)
        else:
            continue
    sys.exit(0)
if __name__ == "__main__":
    main()
```
分析：主要完成文件显示，文件上传，文件下载，具体实现看代码。
# IO 模型

在UNIX/Linux下主要的I/O 模型有：阻塞I/O,非阻塞I/O,异步I/O等。下面一一介绍下每种IO模型的特点。

# 阻塞IO

在linux中，默认情况下所有的socket都是blocking。当用户进程调用了recv，kernel就开始了IO的第一个阶段：准备数据。对于网络IO来说，很多时候数据在一开始还没有到达（比如，还没有收到一个完整的数据包），这个时候kernel就要等待足够的数据到来。而在用户进程这边，整个进程会被阻塞。当kernel一直等到数据准备好了，它就会将数据从kernel中拷贝到用户内存，然后kernel返回结果，用户进程才解除block的状态，重新运行起来。所以，blocking IO的特点就是在IO执行的两个阶段（等待数据和拷贝数据两个阶段）都被阻塞了。几乎所有的程序员第一次接触到的网络编程都是从listen()、 send()、recv()等接口开始的，这些接口都是阻塞型的。使用这些接口可以很方便的构建服务器/客户机的模型。因此阻塞IO是最常用、最简单、效率最低的。

# 非阻塞IO

Linux下，可以通过设置socket使其变为non-blocking。当对一个non-blocking socket执行读操作时。当用户进程发出read操作时，如果kernel中的数据还没有准备好，那么它并不会block用户进程，而是立刻返回一个 error。从用户进程角度讲 ，它发起一个read操作后，并不需要等待，而是马上就得到了一个结果。用户进程判断结果是一个error时，它就知道数据还没有准备好，于是它可以再次 发送read操作。一旦kernel中的数据准备好了，并且又再次收到了用户进程的system call，那么它马上就将数据拷贝到了用户内存，然后返回。所以，在非阻塞式IO中，用户进程其实是需要不断的主动询问kernel数据准备好了没有。非 阻塞的接口相比于阻塞型接口的显著差异在于，在被调用之后立即返回。使用一定的函数可以将某句柄fd设为非阻塞状态。非阻塞IO可防止进程阻塞在I/O操 作上，但是需要轮询耗费资源较多
服务器端：
```python
#! /usr/bin/python
#coding=utf-8
from socket import *
def start():
    try:
        sockfd = socket(AF_INET,SOCK_STREAM)
        sockfd.bind(('192.168.1.247',5858))
        sockfd.listen(5)
    except Exception as e:
        print "error"
        exit(0)
    while True:
        sockfd.setblocking(1)
        connfd,addr = sockfd.accept()
        buf = connfd.recv(1024)
        connfd.close()
        print "-----"
if __name__ =="__main__":
    start()
```
客户端：
```python
#! /usr/bin/python
#coding=utf-8
from socket import *
def client():
    while True:
        sockfd = socket(AF_INET,SOCK_STREAM)
        sockfd.connect(('192.168.1.247',5858))
        print "输入字符"
        buf = raw_input()
        if buf == '':
            break
        sockfd.send(buf)
        print sockfd.recv(1024)
        sockfd.close()
if __name__ == "__main__":
    client()
```
# 异步IO

实际上同步与异步是针对应用程序与内核的交互而言的。同步过程中进程触发IO操作并等待或者轮询的去查看IO操作是否完成。异步过程中进程触发IO 操作以后，直接返回，做自己的事情，IO交给内核来处理，完成后内核通知进程IO完成。Python的异步I/O支持是相当的好。有一堆库可以做这个工 作。在这其中select/poll是较为常见的异步IO实现模式。虽然有些说法称这种IO多路复用实际也是轮询而不是真正的异步，但是这并不影响我们现 阶段的理解和学习。

## select实现异步IO

Python中的select模块以列表形式接受四个参数，原型为：
```python
select(rlist,wlist,xlist[,timeout])
```
其中rlist是等待读取的对象，wlist是等待写入的对象，xlist是等待异常的对象，最后一个是可选对象，指定等待的时间。
```python
#! /usr/bin/python
from socket import *
from select import *
from time import ctime
HOST = '192.168.1.247'
PORT = 5858
BUFSIZE = 1024
ADDR = (HOST,PORT)
sockfd = socket(AF_INET,SOCK_STREAM,0)
sockfd.bind(ADDR)
sockfd.listen(5)
inputs = [sockfd]
while True:
    rs,ws,es = select(inputs,[],[])
    for r in rs:
        if r is sockfd:
            connfd,addr = sockfd.accept()
            print "connect from :",addr
            inputs.append(connfd)
        else:
            try:
                data = r.recv(BUFSIZE)
                disconnect = not data
            except sockt.error:
                disconnect = True
            if disconnect:
                print r.getpeername(),'disconnected'
                inputs.remove(r)
                r.close()
            else:
                r.send('[%s] :%s'%(ctime(),data))
sockfd.close()
```
分析：select函数返回一个读列表，里面放的都是准备好的套接字。循环遍历rs。

--------------------------------------------------------------------------

## poll实现异步IO

poll实现服务器时，需要用到register()和unregister()方法，作用是加入和移除对象，poll()的返回值包括了文件描述 符和事件，polling的事件常量有POLLIN，POLLPRI，POLLPOUT，POLLERR，POLLHUP，POLLVAL，分别表示读取 数据，读取紧急数据，文件描述符已经准备好，文件描述符出错，连接丢失，无效请求。
```python
#! /usr/bin/python
from socket import *
from select import *
s = socket()
host = '192.168.1.247'
port = 5858
s.bind((host,port))
fdmap = {s.fileno():s}
s.listen(5)
p = poll()
p.register(s)
while True:
    events = p.poll()
    for fd,event in events:
        if fd == s.fileno():
            c,addr = s.accept()
            print "got connection fron ",addr
            p.register(c)
            fdmap[c.fileno()] = c
        elif event & POLLIN:
            data = fdmap[fd].recv(1024)
            if not data:
                print fdmap[fd].getpeername(),'disconnected'
                p.unregister(fd)
                del fdmap[fd]
            else:
                print data
                fdmap[fd].send('receive message')
s.close()
```
分析：先将套接字s注册，生成一个poll函数对象，调用poll函数返回一个文件描述符和事件。fileno函数是将套接字转换为文件描述符。event&POLLIN是按位相与，匹配上为True。

--------------------------------------------------------------------------

# 循环服务器
循环服务器采用UDP模式较多，TCP很少有采用循环服务器的。大体流程是：

服务器端运行后等待客户端的连接请求。

服务器接受一个客户端的连接后开始处理，完成了客户的所有请求后断开连接。

循环服务器一次只能处理一个客户端的请求。

只有在当前客户的所有请求都完成后，服务器才能处理下一个客户的连接/服务请求。

如果某个客户端一直占用服务器资源，那么其它的客户端都不能被处理。

# 并发服务器
为了弥补循环服务器的缺陷，人们又设计了并发服务器的模型。并发服务器的设计思想是服务器接受客户端的连接请求后采用多进程或者多线程的方法来为客 户端服务 TCP并发服务器可以避免TCP循环服务器中客户端独占服务器的情况。在并发服务器中大多采用TCP的模式。另外还有一种方式就是采取异步IO的方式实现 并发服务器。这就是我们上面讲到的select/poll的方法。

# 多进程实现并发服务器：
```python
#! /usr/bin/python
import socket,traceback,os,sys
def reap():
    while True:
        try:
            result = os.waitpid(-1,os.WHOHANG)
            if not result[0]:break
        except:
            break
        print 'reaped child process %d'%result[0]
host = ''
port = 5858
s = socket.socket()
s.bind((host,port))
s.listen(5)
print 'parent at %d listen for connections'%os.getpid()
while True:
    try:
        c,addr = s.accept()
    except KeyboardInterrupt:
        raise
    except:
        traceback.print_exc()
        continue
    reap()
    pid = os.fork()
    if pid:
        c.close()
        continue
    else:
        s.close()
        try:
            print "child from %s being hanled PID %d"%(c.getpeername(),os.getpid())
            while True:
                data = c.recv(1024)
                if not data:
                    break
                print data
                c.send('recv your message')
        except (KeyboardInterrupt,SystemExit):
            raise
        except:
            traceback.print_exc()
        c.close()
        sys.exit(0)
```
分析：用fork函数创建进程，父进程关闭客户端套接字。子进程关闭父进程套接字。每有一个客户端接入就创建一个子进程与之通信，父进程就是用来创建子进程的。

--------------------------------------------------------------------------

# 多线程实现并发服务器：
```python
#! /usr/bin/python
import socket,os,sys,traceback
from threading import *
host = '192.168.1.247'
port = 5858
def handler(clientsock):
    print 'new child'
    print 'go connection from',clientsock.getpeername()
    while True:
        data = clientsock.recv(1024)
        if not data:
            break
        clientsock.send('receive your message')
    clientsock.close()
s = socket.socket()
s.bind((host,port))
s.listen(5)
while True:
    try:
        clientsock,clientaddr = s.accept()
    except KeyboardInterrupt:
        raise
    except:
        traceback.print_exc()
        continue
    t = Thread(target = handler,args = (clientsock,))
    t.setDaemon(1)
    t.start()
s.close()
```
分析：每有一个客户端接入创建一个线程，并将客户端套接字传入子线程中，用线程和客户端进行通信。

--------------------------------------------------------------------------

# 集成模块：
```python
#! /usr/bin/python
from SocketServer import *
from time import ctime
class Server(ForkingMixIn,TCPServer):
    pass
class Handler(StreamRequestHandler):
    def handle(self):
        addr = self.request.getpeername()
        print 'got connection from',addr
        while True:
            data = self.request.recv(1024)
            if not data:
                break
            self.request.send('[%s] :%s'%(ctime(),data))
server = Server(('192.168.1.247',5858),Handler)
server.serve_forever()
```

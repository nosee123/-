**SS服务器搭建-详细教程**



更新：2018-11-12

### 一、简介

shadowsocks是一种基于Socks5代理方式的网络数据加密传输包，并采用Apache许可证、GPL、MIT许可证等多种自由软件许可协议开放源代码。shadowsocks分为服务器端和客户端，在使用之前，需要先将服务器端部署到服务器上面，然后通过客户端连接并创建本地代理。目前包使用Python、C、C++、C#、Go语言等编程语言开发。



Shadowsocks的运行原理与其他代理工具基本相同，使用特定的中转服务器完成数据传输。在服务器端部署完成后，用户需要按照指定的密码、加密方式和端口使用客户端软件与其连接。在成功连接到服务器后，客户端会在用户的电脑上构建一个本地Socks5代理。浏览网络时，网络流量会被分到本地socks5代理，客户端将其加密之后发送到服务器，服务器以同样的加密方式将流量回传给客户端，以此实现代理上网。

### 二、安装

- Debian / Ubuntu:

```
apt-get update   //记得先执行apt-get update
apt-get install python-pip
pip install shadowsocks
```

- CentOS:

```
yum install python-setuptools && easy_install pip
pip install shadowsocks
```

- Windows

See [Install Server on Windows](https://github.com/shadowsocks/shadowsocks/wiki/Install-Shadowsocks-Server-on-Windows) 



### 三、创建配置文件

```
vim /etc/shadowsocks.json 
```



配置文件示例（单服务器单账号配置）: 

```
{
    "server":"0.0.0.0",              //你的服务器IP
    "server_port":8099,              //自定义端口
    "local_address": "127.0.0.1",       //可以不设置
    "local_port":1080,                  //可以不设置
    "password":"mypassword",        //自定义密码
    "timeout":300,                  
    "method":"rc4-md5",           //加密方式
    "fast_open": false
}
```

配置文件示例（单服务器多账号配置）:

```
{
    "server":"0.0.0.0",
    "local_address": "127.0.0.1",
    "local_port：":1080,
    "port_password":{
      "8051":"xxxx",
      "8052":"xxxx",
      "8053":"xxxx",
      "8054":"xxxx",
      "8055":"xxxx",
      "8056":"xxxx"
    },
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
```

参数说明：

| Name                   | Explanation                                                  |
| ---------------------- | ------------------------------------------------------------ |
| server                 | the address your server listens                              |
| server_port            | server port                                                  |
| local_address          | the address your local listens                               |
| local_port             | local port                                                   |
| port_password (多用户) | password used for encryption                                 |
| password               | password used for encryption                                 |
| timeout                | in seconds                                                   |
| method                 | default: "aes-256-cfb", see [Encryption](https://github.com/shadowsocks/shadowsocks/wiki/Encryption) |
| fast_open              | use [TCP_FASTOPEN](https://github.com/shadowsocks/shadowsocks/wiki/TCP-Fast-Open), true / false |
| workers                | number of workers, available on Unix/Linux                   |



### 四、操作

- 前台启动 

  ```
  ssserver -p 443 -k password -m aes-256-cfb
  ```

  带配置文件运行：

  ```
  ssserver -c /etc/shadowsocks.json
  ```

- 后台启动 

  ```
  ssserver -p 443 -k password -m aes-256-cfb -d start
  ```

  或：

  ```
  ssserver -p 443 -k password -m aes-256-cfb --user nobody -d start
  ```

  带配置文件运行：

  ```
  ssserver -c /etc/shadowsocks.json -d start
  ```

- 停止 

  ```
  ssserver -d stop
  ```

  或：

  ```
  ssserver -c /etc/shadowsocks.json -d stop 
  ```

- 查看日志 

  ```
  less /var/log/shadowsocks.log
  ```

  或：

  ```
  tail -f -n 10 /var/log/shadowsocks.log    //查看最近100条日志
  ```

  

### 五、问题解决

- 如果 shadowsocks.json 配置文件中 server 字段配成公网IP，而不是 `0.0.0.0`，会报如下错误：

  `socket.error: [Errno 99] Cannot assign requested address`

- 记得开放相应的端口。

### 六、客户端

- [Windows](https://github.com/shadowsocks/shadowsocks/wiki/Ports-and-Clients#windows) / [OS X](https://github.com/shadowsocks/shadowsocks-iOS/wiki/Shadowsocks-for-OSX-Help)
- [Android](https://github.com/shadowsocks/shadowsocks/wiki/Ports-and-Clients#android) / [iOS](https://github.com/shadowsocks/shadowsocks-iOS/wiki/Help)
- [OpenWRT](https://github.com/shadowsocks/openwrt-shadowsocks)



**参考：**

https://github.com/shadowsocks/shadowsocks/blob/master/README.md
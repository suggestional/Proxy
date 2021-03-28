## ShadowSocks 不完全入门指北

### 服务器端配置（Linux Ubuntu）

首先需要一台服务器，这里使用的是腾讯云的服务器，假定服务器 ip 地址为：121.5.255.7（事实上并不是）

首先通过 ssh 远程登陆服务器：在命令行输入 `ssh username@serverIP`，将 `username` 替换为服务器的登陆用户名，通常是 `root`，根据不同的服务商会有所不同；`serverIP` 就是服务器的公网 IP，此处就是 `121.5.255.7`。所以实际上输入的指令为：`ssh root@121.5.255.7`。

之后会弹出密码输入界面，输入用户名对应的密码即可，输入密码时不会明文显示。

登陆完成后，输入指令 `sudo apt-get install shadowsocks`，下载 `shadowsocks`，如弹出是否安装的询问，输入 `y` 即可继续。

安装完成后，可以通过 `ssserver -h` 查看配置参数的具体意义，需要用到的参数，如图所示。

![help](src/help.jpg)

配置代理通常不需要用到上述列举的所有参数，下面是一个较为常用的指令模板：

`ssserver -p port -k password -m method --user username -d start`

将 `port` 替换为开放代理的端口号，`password` 替换为连接代理的密码，`method` 替换为代理密码的加密方式名称，`username` 替换为登录服务器的用户名，即可正常启动。`-d start` 表示后台运行，如果不需要后台运行，只要输入 `ssserver -p port -k password -m method` 即可。

实际上的指令：`ssserver -p 54434 -k testpasswd -m rc4-md5 --user root -d start`

需要注意，如果运营商有防火墙设置，需要在防火墙的设置中，将配置中的代理端口号打开，否则可能会配置失败。

另外，如果启动代理服务端程序后，发现有 `[Errno 13] Permission denied: '/var/run/shadowsocks.pid'`和`[Errno 13] Permission denied: '/var/log/shadowsocks.log'` 的报错，只需输入下面两条指令。

`sudo chown username:username /var/run/shadowsocks.pid`

`sudo chown username:username /var/log/shadowsocks.log`

将指令中的 `username` 替换为登录服务器的用户名。

实际指令：

`sudo chown root:root /var/run/shadowsocks.pid`

`sudo chown root:root /var/log/shadowsocks.log`

完成后再次尝试启动服务，如果没有报错，即为成功，如图所示。

![started](src/started.jpg)

到此为止，服务器端的配置就已经全部结束了，接下来是客户端的配置。



### 客户端

#### Windows

下载[Windows 客户端](https://github.com/shadowsocks/shadowsocks-windows/releases/download/4.4.0.0/Shadowsocks-4.4.0.185.zip)，并解压缩文件夹，打开 `Shadowsocks.exe` 文件，可以看到如图界面

![Shadowsocks](src/Shadowsocks.jpg)

将刚才配置好的一系列参数填入，只需填写服务器地址，服务器端口，密码，加密四项即可，其他不用修改。服务器地址为服务器的公网 IP，服务器端口为刚才设置的代理端口，加密方式通过下拉框选择。配置完成之后点击`应用`或`确定`按钮。

右键桌面右下角的小飞机图标，如图所示，颜色可能与图片不同。

![icon](src/icon.jpg)

`系统代理`选择全局模式，`服务器`选择刚才配置的服务器，如果不另外设置，名称应是`服务器 IP:端口号`

打开浏览器，搜索 `ip`，查询自己的外网 IP 地址，如果代理配置成功，应当显示为自己的服务器 IP 地址，不连接代理时，应该是本地网络的 IP 地址，如图所示。

![connected](src/connected.jpg)

![not connected](src/not_connected.jpg)
## 1. 3-1 快捷键

|   作用   |    快捷键    |           描述            |          补充          |
| :----: | :-------: | :---------------------: | :------------------: |
|  强制停止  | ctrl + c  |    强制停止程序执行、错误命令的输入等    |                      |
|   退出   | ctrl + d  |   退出账户的登录或者有些程序的专属页面    |                      |
| 历史命令搜索 | `history` |      查看历史输入过的命令列表       |                      |
| 历史命令搜索 |  `!命令前缀`  | 以`!+命令前缀`，自动执行匹配前缀的历史命令 |      模糊搜索，不一定准确      |
| 历史命令搜索 |  ctrl+r   |       输入内容匹配历史命令        | 回车执行匹配到的命令;左右方向键获取命令 |
|  光标移动  |  ctrl+a   |        光标跳到命令开头         |                      |
|  光标移动  |  ctrl+e   |        光标调到命令结尾         |                      |
|  光标移动  | ctrl+方向左键 |       光标向左移动一个单词        |                      |
|  光标移动  | ctrl+方向右键 |       光标向右移动一个单词        |                      |
|   清屏   |  ctrl+l   |         清空终端内容          |                      |
|   清屏   |  `clear`  |         清空终端内容          |                      |

## 2. 3-2 systemctl 命令

作用：Linux的服务管理

语法：

```shell
# 启动服务
system start 服务名
# 关闭服务
system stop 服务名
# 查看服务状态
system status 服务名
# 将服务加入到白名单，也就是开机自启
system enable 服务名
# 将服务从白名单中去除，也就是取消开机自启
system disable 服务名
```

内置服务：

|       服务名        | 服务描述  |
| :--------------: | :---: |
| `NetworkManager` | 主网络服务 |
|    `network`     | 副网络服务 |
|   `firewalld`    | 防火墙服务 |
|      `sshd`      | ssh服务 |

> 对于第三方软件没有自动注册的，可以通过手动注册，将其摄入到服务管理的范围内

在archlinux中的相关学习[[archlinux_服务#1 服务-systemd]]

## 3. 3-3 软链接

![[Linux_1_命令#1-12 文件链接]]

## 4. 3-4 日期和时区

### 4.1. date命令

作用：查看系统时间

语法:

```shell
date [-d] [+格式化字符串]
```

`[options]`:

+ `-d --date==STRING`：按照给定字符串显示日期，一般用于日期计算。其支持的时间标记为：
	+ `year`
	+ `month`
	+ `day`
	+ `hour`
	+ `minute`
	+ `second`

示例

```shell
# 显示后一天的时间
date -d "+1 day"
# 可以和格式化字符串一起使用
date -d "+1 day" "+%Y-%m-%d"
# 显示前一个月的时间
date -d "-1 month"
# 可以和格式化字符串一起使用
date -d "-1 month" "+%Y-%m-%d"
```

参数：

+ 格式化字符串：通过特定的字符串标记，来控制显示日期格式
	+ `%Y`：年
	+ `%y`：年份最后两位数字
	+ `%m`(小写)：月份
	+ `%d`：日
	+ `%H`：小时
	+ `%M`(大写)：分钟
	+ `%S`(大写)：秒
	+ `%s`(小写)：从`1970-01-01`自今的秒数

>[!note] 帮助
>可以通过`date --help`查看其具体的信息

>[!tip] 注意
>`-d`在help信息中有一句话为`not now`，也就是说，如果只有`-d`以及格式化字符串，而没有中间的描述字符串，则命令会报错，如：
>```shell
># 这句话会报错
>date -d "%Y%m%d"
>```
>或者说，`-d`操作需要参数，然后格式化字符串并非是其参数，其参数为描述字符串，如`+1 day`

### 4.2. 修改时区

步骤：

```shell
# 1 删除旧时区信息
sudo rm -f /etc/localtime
# 2 建立软链接到上海的时区信息
sudo ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

![[archlinux_system_time|在archlinux上系统时间相关操作如下]]

## 5. 3-5 IP地址和主机名字

### 5.1. 3-5-1 IP地址

IPv4

格式: `a.b.c.d`

+ abcd表示0-255之间数字

[[archlinux_服务#3-1 静态 ip]]

特殊ip地址

`127.0.0.1`：指代本机

`0.0.0.0`：

+ 可以用来指代本机
+ 端口绑定中确定绑定关系
+ 在IP地址限制中，代表所有IP，比如放行规则设置为`0.0.0.0`，就是允许任意IP访问

### 5.2. 3-5-2 主机名

![[archlinux_服务#4 主机名]]

### 5.3. 3-5-3 域名解析

计算机根据域名访问网址步骤：

![[计算机根据域名访问网站步骤|1000]]

1. 查看本机hosts文件
2. 访问DNS服务器，有名的DNS服务器如下：
	1. `114.114.114.114`
	2. `8.8.8.8`

> 配置主机名映射：配置主机hosts文件，将ip地址和主机名对写入即可

## 6. 3-6 配置静态 IP

![[archlinux_服务#ip地址 `ip address`]]

## 7. 3-7 网络传输

### 7.1. 3-7-1 ping

作用：检查网络服务是否联通

语法：

```shell
ping [-c num] ip或主机名
```

`[options]`：

+ `[-c num]`：设置ping次数，如果不使用`-c`将会无限发送ping包，使用`ctrl+c`停止

参数：

+ ip或主机名：ping包发送的网站，可以向DNS服务器发送测试主机是否联网

### 7.2. 3-7-2 wget

![[软件-下载器-wget]]

### 7.3. 3-7-3 curl

![[软件-下载器-curl]]

## 8. 3-8 端口

### 8.1. 3-8-1 基础知识

设备和外界通讯交流的出入口。分为：

+ 物理端口：可见的端口，又叫接口，如USB接口
+ 虚拟端口：不可见的端口，操作系统与外部交互使用

> 通过IP地址索引到具体的计算机，通过端口号索引到具体的程序

Linux系统具有65535个端口，分为三类：

+ 公认端口：1-1023，供系统内置或知名程序预留使用，请勿占用
	+ SSH：22
	+ HTTPS：443
+ 注册端口：1024-49151，通常可以随意使用，用于松散的绑定一些程序 \\ 服务
+ 动态端口：49152-65535，通常不会固定绑定程序，而是当程序对外进行网络链接时，临时使用

### 8.2. 3-8-2 查看端口占用

#### 8.2.1. nmap 命令

```shell
# 查看本机开放端口
nmap 127.0.0.1
```

#### 8.2.2. netstat

查看指定端口占用情况

![[archlinux_服务#5 端口号]]

#### 8.2.3. lsof

与网络服务相关常用命令

```shell
# 查看ipv4网络文件
lsof -i 4
# 获得所有在指定端口号上打开的文件
# 其中，端口号可以是具体端口号，也可以是范围如`1-1024`
lsof -i:端口号
# 可以指定TCP/UDP协议
lsof -i TCP
lsof -i UDP
# 可以指定TCP协议以及端口号
lsof -i TCP:端口号
```

## 9. 3-9 进程管理

### 9.1. 3-9-1 概念

进程的概念

+ 进程是程序的**一次执行过程**
+ 进程是一个程序及其数据在处理机上顺序执行发生的活动
+ 进程是具有独立功能的程序在一个数据集合上运行的过程，是系统资源分配和调度的一个独立单元

特征：

+ 动态性：进程的最基本特征。
	+ 进程是程序的一次执行
	+ 有创建、活动、暂停、终止等过程
	+ 具有一定的生命周期，是动态地产生、变化和消亡的过程
+ 并发性：引入进程的目的
+ 独立性：进程是操作系统资源分配的最小单元，具有PCB
+ 异步性：进程以不可预知的速度向前推进
+ 结构性：进程包含程序段、数据段以及进程控制块三部分

进程和程序的概念最大的区别，一个是动态的，一个是静态的

每个进程都有进程 ID

### 9.2. 3-9-2 PS 命令

作用：查看进程信息

语法：

```shell
ps [options] [--help]
```

`[options]`：

+ `[-A, -e]`：显示全部进程
+ `[-f]`：以完全格式化的形式展示信息
+ `[-u]`：指定用户，参数为用户名

全部信息（从左到右）：

+ UID：user ID
+ PID：process ID
+ PPID：父进程 ID
+ C：CPU占有率
+ STIME：start time
+ TTY：启动进程的终端序号，非终端启动则显示`?`
+ TIME：进程占用CPU时间
+ CMD：进程对应名称或启动路径或启动命令

常用命令：

```shell
# 查看全部信息
ps -ef
ps -A
# 查看指定进程信息
ps -ef | grep 进程关键字
# 查看对应用户进程
ps -u 用户名
```

> 通常配合`grep`对进程信息过滤

### 9.3. 3-9-3 kill 命令

[kill 帮助手册](https://man.archlinux.org/man/kill.1.en#:~:text=The%20command%20kill%20sends%20the%20specified%20signal%20to,for%20this%20signal%20is%20to%20terminate%20the%20process.)

作用：向进程或进程组发送相应的信号

> 1. 默认优先发送`TERM`信号，该信号为终止程序信号，进程会调用析构函数，从而`Terminated`
> 2. `TERM`信号没有起到作用时，会发送`KILL`信号，然后从外部强制终止程序，从而`Killed`

语法：

```shell
kill [-9] 进程ID
```

+ `[-9]`：强制关闭进程，该选项将强制发送`KILL`信号，而不是`TERM`信号

## 10. 3-10 主机状态监控

### 10.1. 3-10-1 查看系统资源占用-top命令

![[命令 - TOP#1 作用]]

![[命令 - TOP#2 语法]]

![[命令 - TOP#4 快捷键]]

### 10.2. 3-10-2 磁盘信息监控-df命令

![[命令 - iostat]]

### 10.3. 3-10-3 网络状态监控-sar命令

![[命令 - sar]]

## 11. 3-11 环境变量

### 11.1. 3-11-1 基础信息

[[archlinux_环境变量]]

### 11.2. 3-11-2 自定义环境变量PATH

通过将命令加入到PATH中，以获取在任何目录中执行的能力

```shell
# `$PATH`获取PATH原来的路径
export PATH=$PATH:新命令绝对路径
```

## 12. 3-12 压缩和解压缩

[[软件-压缩-tar]]

[[软件-压缩-ZIP]]

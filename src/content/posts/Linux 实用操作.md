---
title: Linux 实用操作
published: 2026-04-20
updated: 2026-04-20
description: '了解 Linux实用操作'
tags: [Linux, 学习笔记]
category: 'Linux'
draft: true
---

## 各类小技巧快捷键

### 强制停止`ctrl + c`

- 强制停止某些程序的运行
- 命令输入错误也可以通过该快捷键退出当前输入

### 退出或登出`ctrl + d`

- 退出当前账户的登录
- 退出某些特定程序的专属界面
- **不能用于退出vi/vim**

### 历史命令搜索

1. `history` ：查看历史输入过的命令
2. `！命令前缀` ：自动执行上一次匹配前缀的命令
3. `ctrl + r` ：输入内容去匹配历史命令
   - 回车键可以直接执行
   - 键盘左右键，可以得到此命令(不执行)

### 光标移动快捷键

1. `ctrl + a` ：跳到命令开头
2. `ctrl + e` ：跳到命令结尾
3. `ctrl + 键盘左键` ：向左跳一个单词
4. `ctrl + 键盘右键` ：向右跳一个单词

### 清屏

1. `ctrl + l` ：清空终端内容
2. `clear` ：作用同上

## 软件安装

### CentOS系统——yum命令

1. yum:RPM包软件管理器，用于自动化安装配置Linux软件，并可以自动解决依赖问题
2. 语法：`yum [-y] [install | remove | search] 软件名称`
   - -y选项：自动确认，无需手动确认安装或卸载过程
   - install 安装，remove 卸载，search 搜索
   - **需要root权限，需要联网**

### Ubantu系统——apt命令

- 语法：`apt [-y] [install | remove | search] 软件名称`
  - -y选项：自动确认，无需手动确认安装或卸载过程
  - install 安装，remove 卸载，search 搜索
  - **需要root权限，需要联网**

## systemctl命令控制软件启动关闭

1. 能够被systemctl管理的软件，一般也称之为：服务
2. 语法：`systemctl start | stop | status | enable | disable 服务名`
   - start启动，stop关闭，status查看状态，enable开启开机自启，disable关闭开机自启
3. 系统内置的服务举例：
   - NetworkManager：主网络服务
   - network：副网络服务
   - firewalld：防火墙服务
   - sshd,ssh服务
4. 除了内置的服务以外，部分第三方软件安装后也可以用systemctl进行控制

## 软链接

1. 软链接可以将文件、文件夹链接到其他位置
2. 链接只是一个指向，并不是物理移动，类似于Windows系统中的“快捷方式”
3. 语法：`ln -s 参数1 参数2`
   - -s选项：创建软链接
   - 参数1：被链接的文件或文件夹
   - 参数2：要链接去的目的地

## 日期和时区

### date命令

1. 作用：在命令行中查看系统的时间
2. 语法：`date [-d] [+格式化字符串]`
   - -d选项：按照给定的字符串显示日期，一般用于日期计算
   - 格式化字符串：通过特定的字符串标记，来控制显示的日期格式
     - %Y 年
     - %y 年份后两位数字
     - %m 月份
     - %d 日
     - %H 小时
     - %M 分钟
     - %S 秒
     - %s 自1970-01-01 00:00:00 UTC到现在的秒数
   - 举例：
     - 按照2026-01-01的格式显示日期：`date +%Y-%m-%d`
     - 按照2026-01-01 10:00:00 的格式显示日期：`date "+%Y-%m-%d %H:%M:%S"`

### date命令用于日期加减

- `date -d "+1 day" +%Y%m%d` 显示后一天的日期
- `date -d "-1 day" +%Y%m%d` 显示前一天的日期
- `date -d "-1 month" +%Y%m%d` 显示上一月的日期
- `date -d "+1 month" +%Y%m%d` 显示下一月的日期
- `date -d "-1 year" +%Y%m%d` 显示前一年的日期
- `date -d "+1 year" +%Y%m%d` 显示下一年的日期

### 修改Linux时区

1. 使用root权限，执行如下命令
   `rm -f /etc/localtime`
   `sudo ln -s /usr/share/zoneinfo/Asia/Shanghai  /etc/localtime`
   (将系统自带的localtime文件删除，并将/usr/share/zoneinfo/Asia/Shanghai链接为localtime文件即可)

### ntp程序校准系统时间

#### 自动校准

1. 安装ntp：`yum -y install ntp`
2. 启动并设置开机自启
   `systemctl start ntpd`
   `systemctl enable ntpd`
3. ntpd启动后会定期联网校准系统时间

#### 手动校准(**需要root权限**)

- `ntpdate -u ntp.aliyun.com`

## ip地址和主机名

### ip地址

- `ifconfig` 查看本机ip地址
- 如无法使用ifconfig命令，可以安装：`yum -y install net-tools`
- 特殊ip地址
  - 127.0.0.1，这个ip地址用于指代本机
  - 0.0.0.0
    - 可以用于指代本机
    - 可以在端口绑定中用来确定绑定关系
    - 在一些IP地址限制中，表示所有IP的意思，如放行规则设置为0.0.0.0，表示允许任意IP访问

### 主机名

1. 查看主机名：`hostname`
2. 修改主机名(**需root**)：`hostnamectl set-hostname 主机名`

## 网络请求和下载

### ping命令

1. 作用：检查指定的网络服务器是否是可联通状态
2. 语法：`ping [-c num] ip或主机名`
   - -c选项：检查的次数，不使用则无限次数持续检查
   - 参数：ip或主机名，被检查的服务器的ip地址或主机名地址

### wget命令

1. 作用：是非交互的文件下载器，可以在命令行内下载网络文件
2. 语法：`wget [-b] url`
   - -b选项：可选，后台下载，会将日志写入当前工作目录的wget-log文件
   - url参数：下载链接
3. 通过tail命令可以监控后台下载进度：`tail -f wget-log`
4. 无论下载是否完成，都会生成要下载的文件，如果下载未完成，请及时清理未完成的不可用文件

### curl命令

1. 作用：发送http请求，可用于下载文件、获取信息等
2. 语法：`curl [-O] url`
   - -O选项：用于下载文件，当url是下载链接时，可以使用此选项保存文件
   - url参数：要发起请求的网络地址

## 端口

- 是设备与外界通讯交流的出入口。可以分为物理端口和虚拟端口

### Linux端口

1. 支持65535个端口，分为3类
   - 公认端口：1~1023,通常用于一些系统内置或知名程序的预留使用，如SSH服务的22端口，HTTPS服务的443端口(非特殊需要，不要占用这个范围的端口)
   - 注册端口：1024~49151，通常可以随意使用，用于松散的绑定一些程序/服务
   - 动态端口：49152~65535，通常不会固定绑定程序，而是对程序对外进行网络链接时，用于临时使用

### 查看端口占用

#### nmap查看端口占用

1. 安装nmap：`yum -y install nmap`
2. 语法：`nmap 被查看的IP地址`

#### netstat查看指定端口占用

1. 安装netstat：`yum -y install net-tools`
2. 语法：`netstat -anp | grep 端口号`

```shell
[sakura@localhost ~]$ netstat -anp | grep 111
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
tcp        0      0 0.0.0.0:111             0.0.0.0:*               LISTEN      -
tcp6       0      0 :::111                  :::*                    LISTEN      -
udp        0      0 0.0.0.0:111             0.0.0.0:*                           -
udp6       0      0 :::111                  :::*                                -
unix  3      [ ]         STREAM     CONNECTED     33111    -                    @/tmp/dbus-90Vst45C
```

## 进程管理

### 进程

1. 程序运行在操作系统中，是被操作系统所管理的
2. 为管理运行的程序，每一个程序在运行时，都会被操作系统注册为系统中的一个进程并为每个进程都分配一个独有的进程ID(进程号)

### ps命令查看进程

1. 语法：`ps [-e -f]`
   - -e选项：显示出全部的进程
   - -f选项：以完全格式化的形式展示信息(展示全部信息)
   - 一般来说，固定用法是：`ps -ef` 列出全部进程的全部信息
   - 命令输出的信息格式从左到右分别是：
     - UID：进程所属的用户ID
     - PID：进程的进程号ID
     - PPID：进程的父ID(启动此进程的其他进程)
     - C：此进程的CPU占用率(百分比)
     - STIME：进程的启动时间
     - TTY：启动此进程的终端序号，若显示？，表示非终端启动
     - TIME：进程占用的CPU时间
     - CMD：进程对应的名称或启动路径或启动命令

```shell
[sakura@localhost ~]$ ps -ef
UID         PID   PPID  C STIME TTY          TIME CMD
root          1      0  0 02:34 ?        00:00:02 /usr/lib/systemd/systemd --switched-roo
root          2      0  0 02:34 ?        00:00:00 [kthreadd]
root          4      2  0 02:34 ?        00:00:00 [kworker/0:0H]
root          6      2  0 02:34 ?        00:00:00 [ksoftirqd/0]
```

2. 配合管道符可以实现查看指定进程
   - `ps -ef | grep 进程名称/进程号/进程ID`

### kill命令关闭进程

1. 语法：`kill [-9] 进程ID`
   - -9选项：表示强制关闭进程

## 主机状态监控

### top命令查看系统资源占用

1. 语法：`top`
2. 默认每5秒刷新一次，按q或ctrl + c退出

### 磁盘信息监控

#### df命令

1. 查看硬盘的使用情况
2. 语法：`df [-h]`
   - -h选项：以更加人性化的单位显示

```shell
[sakura@localhost ~]$ df -h
Filesystem      Size  Used Avail Use% Mounted on
devtmpfs        471M     0  471M   0% /dev
tmpfs           487M     0  487M   0% /dev/shm
tmpfs           487M  8.5M  478M   2% /run
tmpfs           487M     0  487M   0% /sys/fs/cgroup
/dev/sda3        18G  4.2G   14G  24% /
/dev/sda1       297M  152M  145M  52% /boot
tmpfs            98M   24K   98M   1% /run/user/1000
```

#### iostat命令

1. 作用：查看CPU、磁盘的相关信息
2. 语法：`iostat [-x] [num1] [num2]`
   - -x选项：显示更多信息
   - num1参数：数字，刷新间隔
   - num2参数：数字，刷新次数

```shell
[sakura@localhost ~]$ iostat -x
Linux 3.10.0-1160.el7.x86_64 (localhost.localdomain) 	04/20/2026 	_x86_64_	(1 CPU)

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.65    0.00    0.32    0.02    0.00   99.01

Device:         rrqm/s   wrqm/s     r/s     w/s    rkB/s    wkB/s avgrq-sz avgqu-sz   await r_await w_await  svctm  %util
sda               0.04     0.95    2.33    0.32   125.87     6.79   100.22     0.00    0.78    0.79    0.66   0.28   0.07

//rKB/s：每秒发送到设备的读取请求数
//wKB/s：每秒发送到设备的写入请求数
//%util：磁盘利用率
```

### 网络状态监控

#### sar命令

1. 作用：查看网络的相关统计(sar命令非常复杂，这里仅简单用于统计网络)
2. 语法：`sar -n DEV num1 num2`
   - -n选项：查看网络
   - DEV选项：查看网络接口
   - num1参数：数字，刷新间隔(不填就查看一次结束)
   - num2参数：数字，刷新次数(不填无限次数)

## 环境变量

- 环境变量是操作系统在运行的时候，记录的一些关键信息，用以辅助系统运行
- 环境变量是一种KeyValue型结构，即名称和值

### env命令

1. 作用：查看当前系统中的环境变量
2. 语法：`env`

### 环境变量：PATH

```shell
[sakura@localhost ~]$ env | grep PATH
PATH=/usr/local/bin:/usr/local/sbin:/usr/bin:/usr/sbin:/bin:/sbin:/home/sakura/.local/bin:/home/sakura/bin
WINDOWPATH=1
```

- PATH记录了系统执行任何命令的搜索路径，路径之间以：隔开
- 当执行任何命令，都会按照顺序，从上述路径中搜索要执行的程序的本体。比如执行cd命令，就从第三个目录/usr/bin中搜索到了cd命令，并执行

### $符号

1. 作用：取“变量”的值
2. 用法：`echo $PATH`

```shell
[sakura@localhost ~]$ echo $PATH
/usr/local/bin:/usr/local/sbin:/usr/bin:/usr/sbin:/bin:/sbin:/home/sakura/.local/bin:/home/sakura/bin

//当和其他内容混合在一起的时候，可以通过{}来标注的变量是谁
[sakura@localhost ~]$ echo ${PATH}ABC
/usr/local/bin:/usr/local/sbin:/usr/bin:/usr/sbin:/bin:/sbin:/home/sakura/.local/bin:/home/sakura/binABC
```

### 自行设置环境变量

1. 临时设置：`export 变量名=变量值`
2. 永久生效
   - 针对当前用户生效，配置在当前的用户的 ~/bashrc 文件中
   - 针对所有用户生效，配置在系统的 /etc/profile 文件中
   - 通过语法: `source 配置文件`，进行立刻生效

## Linux文件的上传和下载

### 利用FinalShell工具

- 略

### rz，rs命令

1. 安装：`yum -y install lrzsz`
2. rz命令
   - 作用：实现文件上传
   - 语法：`rz`
3. sz命令
   - 作用：实现文件下载
   - 语法：`sz 要下载的文件`
   - 文件会自动下载到桌面的 fsdownload 文件夹中

## Linux文件的压缩和解压

### Linux常见的压缩格式

- tar格式：归档文件，简单的将文件整合到一个文件内，无压缩效果
- gzip格式：gzip文件，不仅能整合到一个文件内，同时还有压缩效果

### tar命令

- 语法：`tar [-c -v -x -f -z -C] 参数1 参数2 ... 参数N`
  - -c：创建压缩文件，用于压缩模式
  - -v：显示压缩、解压过程，用于查看进度
  - -x：解压模式
  - -f：要创建的文件，或要解压的文件
  - -z：gzip模式，不使用-z就是普通tarball格式
  - -C：选择解压的目的地，用于解压模式
- 注意
  - **-z选项：如果使用的话，一般处于选项位的第一个**
  - **-f选项：必须在选项位的最后一个**
  - **-C选项：单独使用，和解压所需的其他参数分开**

#### tar压缩

- 常用的压缩组合命令
  - `tar -cvf test.tar 1.txt 2.txt 3.txt`
    - 将1.txt，2.txt，3.txt压缩到test.tar文件内
  - `tar -zcvf test.tar.gz 1.txt 2.txt 3.txt`
    - 将1.txt，2.txt，3.txt压缩到test.tar文件内，使用gzip模式

#### tar解压

- 常用的解压组合命令
  - `tar -xvf test.tar`
    - 解压test.tar，将文件解压至当前目录
  - `tar -xvf test.tar -C /home/sakura`
    - 解压test.tar，将文件解压至指定目录(/home/sakura)
  - `tar -zxvf test.tar.gz -C /home/sakura`
    - 以gzip模式解压test.tar.gz，将文件解压至指定目录(/home/sakura)

### zip、unzip命令

#### zip命令压缩文件

1. 语法：`zip [-r] 参数1 参数2 ... 参数N`
   - -r：被压缩的包含文件夹时，需要使用-r
2. 示例
   - `zip test.zip a.txt b.txt c.txt`
     - 将a.txt，b.txt，c.txt压缩到test.zip文件内
   - `zip -r test.zip test a.txt`
     - 将test文件夹和a.txt文件，压缩到test.zip文件内

#### unzip命令解压文件

1. 语法：`unzip [-d] 参数`
   - -d：指定要解压到的位置
   - 参数：被解压的zip压缩包文件
2. 示例
   - `unzip test.zip` 将test.zip解压到当前目录
   - `unzip test.zip -d /home/sakua` 将test.zip解压到指定文件夹内(/home/sakura)

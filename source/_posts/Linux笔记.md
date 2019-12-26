## Linux笔记

本篇博客用于记录在Linux学习过程中遇到的问题、常用的工具等。

<!--more-->

### 1.Ubuntu下pip和sudo pip路径指向修改

**查看现有pip路径指向**

```shell
root@sxj-VirtualBox:/home/sxj# pip -V
pip 1.5.1 from /usr/lib/python2.7/dist-packages/pip (python 2.7)
```

 **pip位置修改** 

```shell
#打开bash文件修改
sudo vim ~/.bashrc
#在最后一行添加如下命令：
alias pip=/usr/local/bin/pip
#更新bash文件
source ~/.bashrc
```

 **sudo pip位置修改** 

```shell
#修改etc目录
sudo vim /etc/profile
#在最后一行添加如下命令：
alias pip=/usr/local/bin/pip
#保存退出后，使用如下更新命令
source /etc/profile
```

参考： https://blog.csdn.net/mecong/article/details/90453111 

### 2.Ubuntu14更换apt-get源

```python
# 1.备份
cp /etc/apt/sources.list /etc/apt/sources.list.bak
# 2.将如下写入source.list
deb http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse
# 3.更新apt-get
apt-get update
```

### 3.Ubuntu更换pip源

**pip国内的一些镜像**

```http
 阿里云         http://mirrors.aliyun.com/pypi/simple/ 
 中国科技大学    https://pypi.mirrors.ustc.edu.cn/simple/ 
 豆瓣(douban)   http://pypi.douban.com/simple/ 
 清华大学        https://pypi.tuna.tsinghua.edu.cn/simple/ 
 中国科学技术大学 http://pypi.mirrors.ustc.edu.cn/simple/
```

**修改源方法：**

**临时使用：** 
可以在使用pip的时候在后面加上-i参数，指定pip源

```shell
pip install scrapy -i https://pypi.tuna.tsinghua.edu.cn/simple
```

**永久修改：** 
  	  **linux:** 
修改 ~/.pip/pip.conf (没有就创建一个)， 内容如下：

```shell
[global] index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```

### 4.MTR网络诊断工具简单使用

 [MTR](http://www.bitwizard.nl/mtr/)是一个功能强大的工具，使管理员能够诊断和隔离网络错误，并向上游提供商提供网络状态报告。MTR表示的演进`traceroute`通过提供更大的数据样本，好像增强命令`traceroute`与`ping`输出 

**安装MTR**

**Ubuntu**

```shell
apt install mtr-tiny
```

**Centos**

```
yum install mtr
```

**使用**

> mtr google.com可实时查看当前网络状况
>
> -r 标志生成报告（为–report所写） 
>
> -w 选项标志使用长版本的主机名，您可以看到每个跳的完整主机名（–report-wide的缩写）
>
> -c 选项标志设置报告中发送和记录的数据包数量。当不使用时，默认值通常为 10，但是对于更快的间隔，您可能希望将其设置为 50 或 100.报告可能需要较长时间才能完成
>
> -i 选项标志以更快的速率运行报告，以显示只能在网络拥塞期间发生的数据包丢失。该标志指示 MTR 每n秒发送一个数据包。默认值为1秒，因此将其设置为十分之一秒（0.1，0.2等）通常是有帮助的

**读懂 MTR 报告**

```shell
[root@centos7 ~]# mtr -r google.com
Start: Thu Dec 26 09:32:31 2019
HOST: centos7                     Loss%   Snt   Last   Avg  Best  Wrst StDev
  1.|-- 100.96.0.1                 0.0%    10   52.5  36.8   9.8  52.5  18.5
  2.|-- 10.35.5.73                 0.0%    10   52.3  42.6   6.3  52.4  16.8
  3.|-- 10.38.6.105                0.0%    10   52.4  47.6   2.7  57.9  16.0
  4.|-- 10.38.5.38                70.0%    10   52.4  59.0  52.4  71.9  11.2
  5.|-- ???                       100.0    10    0.0   0.0   0.0   0.0   0.0
  6.|-- ???                       100.0    10    0.0   0.0   0.0   0.0   0.0
  7.|-- 10.38.5.25                 0.0%    10   53.0  42.5   7.3  56.0  17.9
  8.|-- 10.38.6.190                0.0%    10    1.6   2.1   1.6   5.4   1.0
  9.|-- 10.1.1.137                 0.0%    10    2.1   3.1   2.1   8.9   2.2
 10.|-- 10.1.0.113                 0.0%    10    2.3   2.3   2.2   2.6   0.0
 11.|-- 61.48.56.209              10.0%    10   38.1  28.9   5.7  55.8  18.5
 12.|-- 123.126.0.157              0.0%    10    5.9   5.9   4.3   7.7   1.1
 13.|-- 202.96.12.61               0.0%    10    8.3   7.4   4.1  10.2   1.9
 14.|-- ???                       100.0    10    0.0   0.0   0.0   0.0   0.0
```

返回结果各列数据说明：

第一列: 显示的是 IP 地址或本机域名，这点和 traceroute 很像

第二列: **Loss%** 到达此节点的数据包丢包率，显示的每个对应 IP 的丢包率。

第三列: **Snt**:100 设置发送数据包的数量，默认值是 10 通过参数 -c 来自定义数量。

第四列: **Last** 显示的最近一次的返回时延

第五列: **Avg** 平均值这个应该是发送 ping 包的平均时延

第六列: **Best** 最好或者说时延最低的

第七列: **Wrst** 最差或者说时延最大的

第八列: **StDev** 是标准偏差

### 5.netstat简介

 **`netstat`**是一个基于[命令行界面](https://zh.wikipedia.org/wiki/命令行界面)的[网络](https://zh.wikipedia.org/wiki/计算机网络)[实用工具](https://zh.wikipedia.org/wiki/实用程序)，可显示当前的网络状态，包括[传输控制协议](https://zh.wikipedia.org/wiki/传输控制协议)层的连线状况、[路由表](https://zh.wikipedia.org/wiki/路由表)、[网络接口](https://zh.wikipedia.org/wiki/网络接口)状态和[网络协议](https://zh.wikipedia.org/wiki/网络协议)的统计信息等。

**常见参数**

> -a (all)显示所有选项，默认不显示LISTEN相关
>
> -t (tcp)仅显示tcp相关选项
>
> -u (udp)仅显示udp相关选项
>
> -n 拒绝显示别名，能显示数字的全部转化成数字。
>
> -l 仅列出有在 Listen (监听) 的服務状态
>
> -p 显示建立相关链接的程序名
>
> -r 显示路由信息，路由表
>
> -e 显示扩展信息，例如uid等
>
> -s 按各个协议进行统计
>
> -c 每隔一个固定时间，执行该netstat命令。 

**示例**

![](netstat.png)

**Proto**： 协议（TCP/UDP）

**Local Address** ：本地地址: 端口

**Foreign Address**：外部地址: 端口　

**State**:　内部地址与外部地址的连接状态 




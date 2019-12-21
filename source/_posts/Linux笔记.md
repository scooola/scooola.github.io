---
title: Linux笔记
date: 2019-12-22 00:42:47
tags: Linux
---

## Linux笔记

本片博客用于记录在Linux学习过程中遇到的小问题及其解决办法。

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

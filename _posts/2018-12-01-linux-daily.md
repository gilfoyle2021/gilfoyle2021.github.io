---
title: Linux 命令笔记
subtitle: 每天一点点，linux命令使用
tag: [linux,shell]
layout: post
---

​	每天一点点，总结学习linux命令。

### 1. sysctl

​	`sysctl`命令被用于**在内核运行时动态地修改内核的运行参数**，可用的内核参数在目录`/proc/sys`中。

​	它包含一些TCP/ip堆栈和虚拟内存系统的高级选项，这可以让有经验的管理员提高引人注目的系统性能。用`sysctl`可以读取设置超过五百个系统变量。

```shell
[root@docker1 sys]# ls
abi  crypto  debug  dev  fs  kernel  net  user  vm
[root@docker1 sys]# cd net
[root@docker1 net]# ls
bridge  core  ipv4  ipv6  netfilter  nf_conntrack_max  unix
[root@docker1 net]# cd bridge/
[root@docker1 bridge]# ls
bridge-nf-call-arptables  bridge-nf-call-iptables        bridge-nf-filter-vlan-tagged
bridge-nf-call-ip6tables  bridge-nf-filter-pppoe-tagged  bridge-nf-pass-vlan-input-dev
[root@docker1 bridge]#
```

#### 使用说明：

```shell
[root@docker1 bridge]# sysctl

Usage:
 sysctl [options] [variable[=value] ...]

Options:
  -a, --all            display all variables
  -A                   alias of -a
  -X                   alias of -a
      --deprecated     include deprecated parameters to listing
  -b, --binary         print value without new line
  -e, --ignore         ignore unknown variables errors
  -N, --names          print variable names without values
  -n, --values         print only values of a variables
  -p, --load[=<file>]  read values from file
  -f                   alias of -p
      --system         read values from all system directories
  -r, --pattern <expression>
                       select setting that match expression
  -q, --quiet          do not echo variable set
  -w, --write          enable writing a value to variable
  -o                   does nothing
  -x                   does nothing
  -d                   alias of -h

 -h, --help     display this help and exit
 -V, --version  output version information and exit

For more details see sysctl(8).
```

**实例:**

查看所有可读变量：

`sysctl -a` 读一个指定的变量，

例如kern.maxproc：

`sysctl kern.maxproc kern.maxproc`: 1044 要设置一个指定的变量，

直接用`variable=value`这样的语法：

`sysctl kern.maxfiles=5000 kern.maxfiles: 2088 -> 5000`



### 2. ssh-gen

`ssh`无密码登录，自然要用到`Linux`的基础命令`ssh`及`scp`.

##### 本机自身实现无密码登录：

**生成公钥、私钥对**

```shell
ssh-keygen
```

进入到生成密钥文件夹中，默认在用户的家目录下面，一个隐藏的.ssh文件夹中。

```shell
 cd /home/hzq/.ssh/
```

查看是否有“`authorized_keys`”文件，如果有，直接将公钥追加到“**authorized_keys**”文件中，如果没有，创建“`authorized_keys`”文件，并修改权限为“600”

```shell
 touch authorized_keys
 chmod 600 authorized_keys 
```

追加公钥到“`authorized_keys`”文件中

```shell
 cat id_rsa.pub >> authorized_keys 
```

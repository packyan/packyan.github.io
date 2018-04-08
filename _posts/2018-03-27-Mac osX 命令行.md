---
layout: post
title: MAC OSX 命令行
date: 2018-3-27
categories: blog
tags: [命令行,OSX,terminal]
description: 最近在os X下做Cpp Primer的练习，需要用到一点os X的命令行，基本上和Linux差不多的，都是Unix like操作系统
---

最近在os X下做Cpp Primer的练习，需要用到一点os X的命令行，基本上和Linux差不多的，都是Unix like操作系统

标签（空格分隔）： 命令行 OSX MAC terminal

---

pwd　　　　　　当前工作目录 

cd（不加参数）　　进root

cd（folder）　　进入文件夹

cd ..　　　　　　上级目录

cd ~　　　　　　返回root

cd -　　　　　　返回上一个访问的目录

rm 文件名 　　　　删除

cat 文件名(|less)　　在终端下查看文件

ls　　　　　　　　列出目录下所有文件

cp 文件名 目标目录　　将文件拷贝到目标目录下

~代表root　　如：~/Document/CPP2/

mkdiv　　　　　　新建文件夹

g++ 源文件名　　　　编译源文件，产生a.out

./文件名　　　　　　运行  例如：./a.out < 输入文件名 > 输出文件名

control+d　　　　　中断a.out运行

nano 　　　　　　编写脚本语言　　ctrl+o存储

nano ....sh　　　　打开

bash ....sh　　　　运行脚本

echo "...$i..."　　　输出语句



那么目前我们的 Linux (以 CentOS 5.x 为例) 有多少我们可以使用的 shells 呢？ 你可以检查一下 /etc/shells 这个文件，至少就有底下这几个可以用的 shells：

/bin/sh (已经被 /bin/bash 所取代)
/bin/bash (就是 Linux 默认的 shell)
/bin/ksh (Kornshell 由 AT&T Bell lab. 发展出来的，兼容于 bash)
/bin/tcsh (整合 C Shell ，提供更多的功能)
/bin/csh (已经被 /bin/tcsh 所取代)
/bin/zsh (基于 ksh 发展出来的，功能更强大的 shell)

bash是shell的一种，linux现在默认的shell就是bash。在使用ubuntu 10.4.1开发android的时候，shell也要改成bash。
mac os默认的shell也是bash，打开终端，默认就是bash

如果在终端中输入指令bash:
taylors-Mac-mini:~ taylor$ bash
bash-3.2$ 
然后在bash-3.2$ xxxxxxx
下输入的指令，其实就相当于 bash ....sh　　　　运行脚本
退出bash的脚本模式直接exit就回到了用户状态
错误的理解：在用户状态下输入bash就是进入bash，其实开打终端就是bash了，并不需要再输入bash进入，在bash-3.2$ 状态下输入的指令其实是bash脚本。





﻿---
layout: post
title: Mac OS X Sublime 教程
date: 2018-3-29
categories: blog
tags: [Mac,sublime]
description: os X系统中sublime的相关配置
---
os X系统中sublime的相关配置

标签（空格分隔）： sublime Mac

---
1. OS X 底部预留空间问题
配置文件里添加 
`"scroll_past_end": true`
2. 新标签页打开文件：`"open_files_in_new_window": false`
3. c++11 build-system

```
{
"cmd": ["clang++", "${file}","-std=c++11", "-stdlib=libc++", "-o", "${file_path}/${file_base_name}"],
"file_regex": "^(..[^:]*):([0-9]+):?([0-9]+)?:? (.*)$",
"working_dir": "${file_path}",
"selector": "source.c, source.c++",
 
"variants":
[
{
"name": "Run",
"cmd": ["bash", "-c", "clang++ '${file}' -std=c++11 -stdlib=libc++ -o '${file_path}/${file_base_name}' && '${file_path}/${file_base_name}'"]
}
]
}
```


打开终端版本 加上`open -a Terminal.app`


```
{
"cmd": ["clang++", "${file}","-std=c++11", "-stdlib=libc++", "-o", "${file_path}/${file_base_name}"],
"file_regex": "^(..[^:]*):([0-9]+):?([0-9]+)?:? (.*)$",
"working_dir": "${file_path}",
"selector": "source.c, source.c++",
 
"variants":
[
{
"name": "Run",
"cmd": ["bash", "-c", "clang++ '${file}' -std=c++11 -stdlib=libc++ -o '${file_path}/${file_base_name}' && open -a Terminal.app '${file_path}/${file_base_name}'"]
}
]
}
```

mac sublime3 3156 LICENSE
原创 2017年12月08日 09:38:50 500
下载地址：https://www.sublimetext.com/3dev

Sublime Text 3.x (after Build 3156)

```
—– BEGIN LICENSE —–
Packy, NanJing University of Science and Technology
50 User License
EA7E-1068832
86C49532 8F829C68 2ED18D56 162664F2
8B934F0C EB60A7FE 81D7D5EF BB8F1673
F67D69C7 C5E21B19 42E7EFBD D9C2BBC1
CEBA4697 535E29CA 0D2D0D4D ACE548CE
07815DC7 BDE3901E D5D198E4 BC1677C0
46097A55 29BCE0C9 72A358E8 CEFEEFB5
24CEB623 D7232749 F2515349 FB675F93
C55635A7 B1E32AB0 3D055979 041E0359
—— END LICENSE ——
—– BEGIN LICENSE —–
Esri, Inc
19 User License
EA7E-853424
BBB0D927 00F6E4AB 0AFFEFEA 82C4A3E5
4F9A99D1 7C0475AB 5A708861 6C81D74E
4BBA1F56 877CE0E2 88126328 486D8600
8A0A85D7 49671882 49969D92 312F27A7
5CCAE55B D711D4E2 9069DE55 B510370C
9499567E 61D548BA FB260403 711DFE24
1BAFDA6D 1F52E6B1 B728EF5B A40CCD4D
FD716AF5 1760D208 0E05F26E 22660950
—— END LICENSE ——
—– BEGIN LICENSE —–
Dylan Tittel
Single User License
EA7E-898127
CD1A23C0 E8687245 18A89BFA 7CF52C10
A20AA536 9E1A57C7 D33839DC 6613C428
91E9C7FF F4702893 251C6403 27A10818
6F48AC9C BB66843D C1D60DEA B952C2D0
40019115 E5278B04 95EBB709 D6B1C5B9
67CA940A 88701851 32910570 57AD5B96
263A8906 AFBFAB46 52F6706F CACFC74E
DFAD4752 D10E58B6 744B90F2 13AF2B9C
—— END LICENSE ——
—– BEGIN LICENSE —–
Country Rebel
Single User License
EA7E-993095
19176BCE 3FF86EA0 3CE86508 6BE4DCA7
9F74D761 4D0CAD8B E4033008 43FC73F3
1C01F6DD C4829BE9 E7830578 A4823ADC
61B224F1 DC93C458 ABAAAE0F 925C32D4
04A83C36 813FF6C8 9877942C 4418F99C
2F15E5B8 544EDB80 D9A86985 4CBBA6A8
998DE3E4 7FB33E15 6CD30357 6DC96CEA
ECB1BC4E D8010D5A 77BA86C8 BA7F76CC
—— END LICENSE ——

```

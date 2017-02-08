---
title: 每天一个 Linux 命令（2）： grep
date: 2017-02-08 16:51:00
tags: Linux
categories: How-To
---

grep 是比较常用的一个命令，用来搜索文件或标准输出中的匹配模式的内容，默认打印出匹配的行。
<!-- more -->

```
Usage: grep [OPTION]... PATTERN [FILE]...

--help                    Print a usage message briefly summarizing these command-line options and the bug-reporting address, then exit.
-i, --ignore-case         Ignore  case  distinctions  in  both  the PATTERN and the input files.  (-i is specified by POSIX.)
-c, --count               Suppress normal output; instead print a count of matching lines for each input file.
                          With the -v,  --invert-match option (see below), count non-matching lines.  (-c is specified by POSIX.)
-l, --files-with-matches  Suppress normal output; instead print the name of each input file from which output would normally have
                          been printed.  The scanning will stop on the first match.  (-l  is  specified by POSIX.)
```

用过的选项也就上面几个。
举个例子吧，查找 test.txt 文件中 linux 出现的次数，忽略大小写。
```
grep -ic linux test.txt
```
grep 也支持正则表达式
```
grep -ic lin.x test.txt
```
效果和第一个一样。

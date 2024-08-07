---
title: tar命令常用方式
date: 2024-08-07 23:00:56
tags:
typora-root-url: tar命令常用方式
---

## **tar：常用的文件打包和压缩工具**

当然还有其他的工具比如zip、unzip等，这里只讲tar其他大家用到自行百度原理都是相同的。

```Shell
tar [选项] [压缩文件名] [文件或目录...]
tar [选项] [压缩文件名] [文件或目录...]
```

常用的选项包括：

- `-c`：创建压缩文件
- `-x`：提取压缩文件
- `-z`：使用 `gzip` 算法压缩文件
- `-j`：使用 `bzip2` 算法压缩文件
- `-J`：使用 `xz` 算法压缩文件
- `-f`：指定归档文件的名称
- `-v`：显示详细的处理信息

`-z -j -J`这三种参数如何选择？如果对时间比较敏感，可以使用 `-z` 参数进行 `gzip` 压缩，虽然压缩比较低，但速度较快。而如果您对压缩比较重要，可以选择 `-j` 参数进行 `bzip2` 压缩，尽管速度较慢，但压缩比较高，如果您对压缩率要求非常高，可以选择 `-J` 参数进行 `xz` 压缩需要更长的时间。

下面提供几个常见的后缀名和对应的压缩和解压命令，大家不用太过纠结命令组合，以后看见相应的后缀直接用对应的命令就行。

1. `.tar.gz 或 .tgz：`

```Shell
压缩：tar -czf package.tar.gz file1.txt file2.txt directory    
解压缩：tar -xzf package.tar.gz 
```

1. `.tar.bz2 或 .tbz2：`

```Shell
压缩：tar -cjf package.tar.bz2 file1.txt file2.txt directory
解压缩：tar -xjf package.tar.bz2     
```

1. `.tar.xz：`

```Shell
压缩：tar -cJf package.tar.xz file1.txt file2.txt directory
解压缩：tar -xJf package.tar.xz
```

演示

![img](d6a840dc-40b9-42a0-a089-8a194346e3b3.gif)

还有一个特殊命令我们经常会看到`-C`是大写C他的作用是指定`tar` 命令执行时的工作目录

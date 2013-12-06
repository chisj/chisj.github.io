---
layout: post
title: "APUE源码编译方法"
date: 2013-12-06 16:17:01 +0800
comments: true
categories: 
---

拿书上的第一段源码作为例子：

    #include "apue.h"
    #include <dirent.h>

    int main(int argc, char *argv[]) {
    	DIR *dp;
    	struct dirent *dirp;

    	if (argc != 2) {
        	err_quit("usage:ls direntory_name");
    	}
    	if ((dp = opendir(argv[1])) == NULL)
		err_sys("can't open %s", argv[1]);
    	while ((dirp =  readdir(dp)) != NULL)
		printf("%s\n", dirp->d_name);

    	closedir(dp);
    	exit(0);
	}

源码里的apue.h并没有实现err_quit函数，因为实现在apue源码包的lib里。

先下载apue源码包：http://download.csdn.net/detail/i2c_rs485/4614061  (第二版使用，不保证更新)

然后：

1. 解压文件到apue.2e目录

2. 修改相应平台的文件,我使用的是mac,所以修改Make.defines.macos
你修改的只需要这一行WKDIR=/home/your_dir/apue2e_src/apue.2e,改成自己的目录路径

3. cd到apue.2e目录的lib目录下， 执行make -f macos.mk，之后你会在lib目录下面找到libapue.a这个文件.
现在,你可以把它拷贝到任何地方,在编写例子的时候,你就可以

4. 拷贝apue2e_src/apue.2e/include/apue.h和apue2e_src/apue.2e/lib/libapue.a
到你的源代码目录。

5. 使用gcc -o hello hello.c libapue.a来编译你的源代码

使用上面的方法编译第一段源码：

    gcc -o myls myls.c libapue.a 

运行：

	unit_1 ihome$ ./myls /Volumes
	.
	..
	BOOTCAMP
	File
	MacSys

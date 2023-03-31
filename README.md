# study-note
### 2023.3.16 

抛出问题:
  * plt表和got表有什么区别
  * 32位栈和64位栈有什么区别，描述一下他们调用函数时所进行的行为
  * 32位栈的利用和64位栈的利用有什么区别
  * Bss段是什么
  * 什么情况会导致栈溢出，写出常见危险函数

### 2023.3.17

对照着问题稍微能给出了答案,感觉基础还是太薄弱了,明天把练习的环境搭起来然后抓紧时间打基础

### 2023.3.18 

花了半天时间在Ubuntu搭了环境起来,下午看了一下http协议版本的区别

### 2023.3.19 

研究了一下web路由和url的关系,感觉可以看一下goahead的websRouteRequest(),顺便有几个问题:
  * 这个route是对照goahead的route.txt来建立的吗?
  * URI,URL,URN概念,这三个是什么关系,在goahead里是什么情况

![图片](https://user-images.githubusercontent.com/102180824/226181358-c852416a-fbb6-4c11-83bb-ecebe3ee5e4d.png)

贴个图到时候学到堆回来拷打自己

### 2023.3.20

基本了解了goahead的web模块的工作方式,URL和URI的关系也理清了,最直接的感觉是URI像是绝对路径而URL是相对路径

### 2023.3.21 

找了一个cgi介绍的 https://zhuanlan.zhihu.com/p/25013398 对cgi有个一个大概印象了

### 2023.3.22 

继续看cgi,下午整ppt

顺便解决了之前愚蠢的问题:heap应该就是通常说的堆,chunk就是堆内的块,被分隔开的空白的内存空间

### 2023.3.23

了解了一下qemu-user和qemu-system
顺便有几个问题:
 * 有没有可能程序引用了一个动态库,但是我只拿到可执行文件,没拿到此动态库,所以无法查看该程序引用的函数?
 * 如果我现在已经验证了一个程序存在命令执行的漏洞,在它自身不存在恶意程序的情况下我应该如何获取shell?
 * 我是否已经完全理解了cgi?或许我应该上手试试
 * 我应该如何能做到较快的认识到哪些函数存在漏洞?
 希望最后这个问题能因为经验而解决吧


### 2023.3.24 

看到了两篇文章感觉很有意思,贴一下:

 https://zhuanlan.zhihu.com/p/245070099
 
 https://zhuanlan.zhihu.com/p/26271959

### 2023.3.25

解决了前天的1个问题:
若存在命令执行漏洞,但无恶意程序,我可以:
1.自己在web服务器上传一个恶意程序,使用wget下载下来
2.直接通过命令行写一段恶意代码到一个文件中,然后运行此程序(可能需要靶机有对应环境)
3.或许利用ssh(可能默认开启或不存在需搭配其他漏洞)
此处是想起XiaoMi AX3600 1.0.17版本可通过/cgi-bin/来开启ssh


### 2023.3.26

上午做文件
下午做了几个NKCTF的题,感觉卷不动CTF这个方向了,但是有机会还是要学PWN和RE,不是为了CTF,只是一种乐趣,就像喜欢打游戏一样,即使可能做不出题但还是喜欢解题的那个过程

明天等书到了看一下
 
### 2023.3.27  

看了下程序员的自我修养第一章,出乎意料的搞通了很多之前没怎么学牢的知识点,比如分段分页的具体原理,进程线程的概念,不懂原理时总感觉一个知识点或一道题有些地方是我不知道的,比如参数为什么会出现在哪些寄存器而不是直接入栈(64位ROP不理解为什么控制rdi,2023-3-2),或者为什么本身只需要十个字节空间的变量esp却向下移了12字节,后续还是要补csapp,明天开始尝试搭建qemu环境准备复现漏洞

的确是有点会的太少想得太多了,太浮躁了,总是怕时间太短事情太多,我应该沉下心来


### 2023.3.28 

费了不少功夫算是装上qemu了,也下载到了DIR-815的固件,应该需要从bin文件提出来,明天又是满课,寄

### 2023.3.29

被binwalk提取bin文件卡住了,看看应该怎么分离出来,搜到的都是直接binwalk -e的



今天顺便研究了下关于开辟栈的时候会开辟多大空间的问题,发现了一个比较有意思的点:

```C
#include <stdio.h>

int funA();
int funB();
int funC();
int funD();

int main() {
	int a = funA();
	printf("%d\n", a);
	int b = funB();
	printf("%d\n", b);
	int c = funC();
	printf("%d\n", c);
	int d = funD();
	printf("%d\n", d);
	return 0;
}
int funA() {
	char a[10] = {'0', '1', '2', '3', '4', '5', '6', '7', '8', '9'};
	return sizeof(a);
}
int funB() {
	int b[10] = {'0', '1', '2', '3', '4', '5', '6', '7', '8', '9'};
	return sizeof(b);
}
int funC() {
	char c[] = "helloworld";
	return sizeof(c);
}
int funD() {
	char d[17] = {'0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12', '13', '14', '15', '16'};
	return sizeof(d);
}
```

执行时得到的在内存中的大小是

![image-20230329163644698](https://tharsis.oss-cn-beijing.aliyuncs.com/image-20230329163644698.png)

当使用IDA查看时得到的是rsp分别向下开辟了16,48,16,32,都是16的倍数,不知道是不是和内存对齐有关

![image-20230329163914181](https://tharsis.oss-cn-beijing.aliyuncs.com/image-20230329163914181.png)

![image-20230329163924946](https://tharsis.oss-cn-beijing.aliyuncs.com/image-20230329163924946.png)

![image-20230329163937534](https://tharsis.oss-cn-beijing.aliyuncs.com/image-20230329163937534.png)

![image-20230329163950239](https://tharsis.oss-cn-beijing.aliyuncs.com/image-20230329163950239.png)

### 2023.3.30
通过IDA看cgibin源码,目前跟踪到了sess_get_uid的位置

![image-20230331195724361](https://tharsis.oss-cn-beijing.aliyuncs.com/image-20230331195724361.png)

### 2023.3.31

继续分析完剩余代码,明天开始看如何写这段shellcode

![image-20230331220502416](https://tharsis.oss-cn-beijing.aliyuncs.com/image-20230331220502416.png)

![image-20230331220941078](https://tharsis.oss-cn-beijing.aliyuncs.com/image-20230331220941078.png)

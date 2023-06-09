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

### 2023.3.20

基本了解了goahead的web模块的工作方式,URL和URI的关系也理清了,最直接的感觉是URI像是绝对路径而URL是相对路径

### 2023.3.21 

找了一个cgi介绍的文章 对cgi有个一个大概印象了

### 2023.3.22 

继续看cgi,顺便解决了之前愚蠢的问题:heap应该就是通常说的堆,chunk就是堆内的块,被分隔开的空白的内存空间

### 2023.3.23

了解了一下qemu-user和qemu-system
顺便有几个问题:
 * 有没有可能程序引用了一个动态库,但是我只拿到可执行文件,没拿到此动态库,所以无法查看该程序引用的函数?
 * 如果我现在已经验证了一个程序存在命令执行的漏洞,在它自身不存在恶意程序的情况下我应该如何获取shell?
 * 我是否已经完全理解了cgi?或许我应该上手试试
 * 我应该如何能做到较快的认识到哪些函数存在漏洞?
 希望最后这个问题能因为经验而解决吧


### 2023.3.24 

看到了两篇文章感觉很有意思,对iot有了点了解


### 2023.3.25

解决了前天的1个问题:
若存在命令执行漏洞,但无恶意程序,我可以:
1.自己在web服务器上传一个恶意程序,使用wget下载下来
2.直接通过命令行写一段恶意代码到一个文件中,然后运行此程序(可能需要靶机有对应环境)
3.或许利用ssh(可能默认开启或不存在需搭配其他漏洞)
此处是想起XiaoMi AX3600 1.0.17版本可通过/cgi-bin/来开启ssh


### 2023.3.26

上午做文件
下午做了几个NKCTF的题,感觉有点吃力,有机会还是要补PWN和RE

明天等书到了看一下
 
### 2023.3.27  

看了下程序员的自我修养第一章,出乎意料的搞通了很多之前没怎么学牢的知识点,比如分段分页的具体原理,进程线程的概念,不懂原理时总感觉一个知识点或一道题有些地方是我不知道的,比如参数为什么会出现在哪些寄存器而不是直接入栈(64位ROP不理解为什么控制rdi,2023-3-2),或者为什么本身只需要十个字节空间的变量esp却向下移了12字节,后续还是要补csapp,明天开始尝试搭建qemu环境准备复现漏洞

的确是有点会的太少想得太多了,太浮躁了,总是怕时间太短事情太多,我应该沉下心来

### 2023.3.28 

费了不少功夫算是装上qemu了,也下载到了DIR-815的固件,应该需要从bin文件提出来,明天又是满课,寄

### 2023.3.29

被binwalk提取bin文件卡住了,看看应该怎么分离出来,搜到的都是直接binwalk -e的

今天顺便研究了下关于开辟栈的时候会开辟多大空间的问题,发现rsp开辟新空间时分别向下开辟了16,48,16,32,都是16的倍数,不知道是不是和内存对齐有关

### 2023.3.30
通过IDA看cgibin源码,目前跟踪到了sess_get_uid的位置

### 2023.3.31

继续分析完剩余代码,明天开始看如何写这段shellcode

### 2023.4.1-2023.4.4

电脑电源线寄了,只能被迫看书了,能理清很多知识点,可惜看到的有点晚

### 2023.4.5

终于是给送来线了,被卡了好久进度

研究了会怎么用qemu,需要模拟32位小端mips

### 2023.4.6

一直在搭qemu-mipsel的环境,明天应该可以直接挂上这个环境了

目前是通过scp 传到qemu,然后在宿主机用ssh+ chroot ./squashfs-root /+cp目录

### 2023.4.7

大致复现完了,除了调试,明天继续

### 2023.4.8

目前已经知道的三(其实是四)种方式:

1.直接写一个文件然后在环境变量HTTP_COOKIE的uid=后面插入cyclic生成的值,上传到qemu执行
2.qemu-mipsel-static本地调试
3.qemu-system跑起来httpd服务,抓包再发包

4.gdb_server开启远程调试,gdb或者gdb(目前就没成功过)

明天试试第三种

### 2023.4.9

终于调完拿到shell了,完工,明天写完报告

突然发现下载qemu-system环境文件的网站不能访问了,没事得多拍快照

### 2023.4.10

讲完如何复现的了.DIR815算结束

### 2023.4.11

开始补一点IOT基础知识,顺便看一下libc相关的知识

### 2023.4.12

翻了一下书,计划一下目标:

4月底之前复现三个以上的IOT漏洞,顺便学习一下基础

### 2023.4.13 

开始从头看CSAPP第一章第二章,明天做做lab

### 2023.4.14

继续看第二章,希望这周末之前看完前三章

### 2023.4.15

今天刚做出来data lab的第一个题,感觉真的难

### 2023.4.16

看完整数运算,最近几天时间都拆的太散了

### 2023.4.17

看完浮点数运算,明天写完data lab

### 2023.4.18

跟着wp抄完data lab了

明天可以看第三章了

### 2023.4.19

开始CSAPP第三章,顺便重新搭了环境

### 2023.4.20

第三章看完一半了,明天看完感觉问题不大

### 2023.4.21

卡在3.8,感觉这些东西还是一个个试试比较合适,明天看到3.12应该没什么问题

### 2023.4.22

对前面的东西感觉了解的不详细,重新从视频第五集开始看对前面的再复习了一下

### 2023.4.23

上午上课 晚上开始做bomb lab

### 2023.4.24

bomblab到第二个了
### 2023.4.25

bomb第二个过了,明天继续分析

### 2023.4.26

写点python复习复习

### 2023.4.27-2023.4.28

运动会 没法正常按进度推任务

### 2023.4.29-2023.5.3

五一小休,感觉自己好像懒狗

### 2023.5.4

根据重点速刷csapp第三章,顺便研究了下aircrack-ng

### 2023.5.5

根据wifipumpkin3又写了个小脚本,明天刷完bomb lab

### 2023.5.6

bomb lab做完 看完csapp第四章,最近准备比赛了需要停几天

### 2023.5.7-2023.5.12

未来几天得准备取证方向的学习,12号比赛需要用

### 2023.5.13

白学了好几天取证,比赛是一道没出.全是中职的东西
明天重新再补一次csapp的第三章吧

### 2023.5.14

对照自己学的东西写了个程序,动手重新调了一遍程序调用,感觉现在更明确了

### 2023.5.15

attack_lab 模块1

### 2023.5.16

attack_lab 模块2

### 2023.5.17

写写python脚本

### 2023.5.18

还在写脚本

### 2023.5.19-2023.5.20

完全实现通过json传参调用脚本以及json返回

要说最大收获大概率是学会了怎么通过线程解决问题

### 2023.5.21

投了一下chamd5被拒了hhh,意料之中,还是水平太菜了

### 2023.5.22

今天一天忙完基础的看了看section的内容,已经初步有了个对源码映射到程序文件的了解了

### 2023.5.23

下午双选会感觉企业都很一般...怎么还有个实习期不给钱的

### 2023.5.24

看了看测试esp的程序,没啥头绪

### 2023.5.25

attack lab

### 2023.5.26

彻底翻完第三章,第四章重新看了几遍

### 2023.5.27

彻底拆清楚程序和内存的一些内容了,感觉之前好多理解全是错的,果然很多东西都需要系统学习

### 2023.5.28

搭WR740环境中...

### 2023.5.29

重新看csapp的第四章的执行顺序

### 2023.5.30

csapp第四章快看完了

### 2023.5.31

开始第五章了,attack好久没做了,明天写一下

### 2023.6.1

attack lab对照wp刷完了,第五章一半了

### 2023.6.2

第五章看完了,等着单独研究一下流水线和预测惩罚

### 2023.6.3

队内集训,顺便整理整理之前学的东西

https://zhuanlan.zhihu.com/p/413889872

### 2023.6.4

上午还得去开讲座,接下来一个月得准备考试了,能真正学东西的时间更少哩

### 2023.6.5

csapp第六章

### 2023.6.6

csapp 6.2

### 2023.6.7

csapp 6.4

### 2023.6.8

第六章结束,明天开始csapp第七章

### 2023.6.9

感觉这一章和程序员自我修养的第三章蛮像的应该看起来会容易很多

### 2023.6.10

csapp看到重定位了

### 2023.6.11

已经看到7.8章了,明天还得学毛概,准备刷题库试试

### 2023.6.12

这个周就开始考试了,估计进度会更没以前快了,希望能暑假前看完csapp吧

看完就刷刷pwn题吧


### 2023.6.13 

接下来一周都是考试,时间太不够用了



### 2023.6.18

终于考完第一轮了,csapp看到7.10了

### 2023.6.19

最近刷一下学习通和之前的作业,csapp到7.11了

### 2023.6.20

明天上完半天课放假,趁这个期间抓紧时间投投简历

### 2023.6.21- 2023.6.24

赶了三天大作业,下周开始考试了,先中断一下之前的计划,明天先投一下简历

### 2023.7.8

最近处理完学校和其他方面的事情,继续开始csapp

目前来看对于GOT和PLT表的理解是这样的

```
程序首次运行时调用了一个动态库的外部函数,而此时程序不知道这个函数的地址,它会去尝试读取GOT,而GOT此时也没有此函数地址,所以会指向PLT中对应此函数的条目
当程序执行到PLT的对应条目时,PLT stub会尝试调用动态链接器来解析函数的地址,动态链接器负责查找动态库中被调用的函数的地址
一旦函数被解析,PLT stub会更新GOT中的此函数的条目,下一次再次调用此函数时,程序可以直接从GOT读取此函数的地址
最后,PLT stub会跳转到此函数的地址,进行执行
```

### 2023.7.9

 csapp 8.2 对于进程的内容有了解了

### 2023.7.10

看了下nvram的内容,下午复现一下cve-2017-17215

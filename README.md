# study-note
2023.3.16 抛出问题:
  * plt表和got表有什么区别
  * 32位栈和64位栈有什么区别，描述一下他们调用函数时所进行的行为
  * 32位栈的利用和64位栈的利用有什么区别
  * Bss段是什么
  * 什么情况会导致栈溢出，写出常见危险函数

2023.3.17 对照着问题稍微能给出了答案,感觉基础还是太薄弱了,明天把练习的环境搭起来然后抓紧时间打基础

2023.3.18 花了半天时间在Ubuntu搭了环境起来,下午看了一下http协议版本的区别

2023.3.19 研究了一下web路由和url的关系,感觉可以看一下goahead的websRouteRequest(),顺便有几个问题:
  * 这个route是对照goahead的route.txt来建立的吗?
  * URI,URL,URN概念,这三个是什么关系,在goahead里是什么情况

![图片](https://user-images.githubusercontent.com/102180824/226181358-c852416a-fbb6-4c11-83bb-ecebe3ee5e4d.png)

贴个图到时候学到堆回来拷打自己

2023.3.20 基本了解了goahead的web模块的工作方式,URL和URI的关系也理清了,最直接的感觉是URI像是绝对路径而URL是相对路径

2023.3.21 找了一个cgi介绍的 https://zhuanlan.zhihu.com/p/25013398 对cgi有个一个大概印象了

2023.3.22 继续看cgi,下午整ppt

2023.3.23 了解了一下qemu-user和qemu-system
顺便有几个问题:
 * 有没有可能程序引用了一个动态库,但是我只拿到可执行文件,没拿到此动态库,所以无法查看该程序引用的函数?
 * 如果我现在已经验证了一个程序存在命令执行的漏洞,在它自身不存在恶意程序的情况下我应该如何获取shell?
 * 我是否已经完全理解了cgi?或许我应该上手试试
 * 我应该如何能做到较快的认识到哪些函数存在漏洞?
 希望最后这个问题能因为经验而解决吧


2023.3.24 看到了两篇文章感觉很有意思,贴一下:
 https://zhuanlan.zhihu.com/p/245070099
 https://zhuanlan.zhihu.com/p/26271959

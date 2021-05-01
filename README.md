# how_to_debug_redis_by_vscode_with_own_test
如何自己写测试用例来调试redis代码,进行学习


```

2021-05-01,9点51

2021-05-01,9点51

自己写测试debug redis代码:
错误:
/usr/lib/gcc/x86_64-redhat-linux/4.8.5/../../../../lib64/crt1.o: In function `_start':
(.text+0x20): undefined reference to `main'
collect2: error: ld returned 1 exit status

好像是必须写一个main函数才能编译.

哇成功了!~

举一个例子:
我要调试zipmap.c代码

1.找到他所有的依赖.这个文件比较好, 只依赖zmalloc.c
2.看他的代码.发现最后有一行为了方便我们用已经写好的.
	#ifdef ZIPMAP_TEST_MAIN
	下面就是main函数了.记住gcc一定要有main函数才能编译成功.
	所以我们再这样上面加上这个定义即可.打开下面的函数.
	写上这个#define ZIPMAP_TEST_MAIN   启动测试.
3. cd 进入src目录 然后gcc zmalloc.c   zipmap.c -g直接陈宫
4. ./a.out 直接看到结果
5.配置 .vscode/launch.json 里面:  "program": "${workspaceFolder}/src/a.out",
6.断点打到zipmap.c里面main函数里面第一行.
7.运行debug即可.直接进来了!!!!!!!这回可以精细的玩redis了.




8. 如何开启宏定义:https://blog.csdn.net/zhangjs0322/article/details/39666889


9.所以总结一句话.上面的gcc命令应该为这个: 
gcc zmalloc.c   zipmap.c -g -g3 -gdwarf-2
就完美了.


```

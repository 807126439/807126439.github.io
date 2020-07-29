---
title: arthas指令心得
date: 2020-07-30 00:16:31
tags:
---
## WEBCONSOLE 端口

<http://127.0.0.1:3658/>

## 基础命令

- help——查看命令帮助信息

- [cat](https://alibaba.github.io/arthas/cat.html)——打印文件内容，和linux里的cat命令类似

   cat /src/aaa.txt

- [echo](https://alibaba.github.io/arthas/echo.html)–打印参数，和linux里的echo命令类似

- [grep](https://alibaba.github.io/arthas/grep.html)——匹配查找，和linux里的grep命令类似

- [tee](https://alibaba.github.io/arthas/tee.html)——复制标准输入到标准输出和指定的文件，和linux里的tee命令类似

- [pwd](https://alibaba.github.io/arthas/pwd.html)——返回当前的工作目录，和linux命令类似

- cls——清空当前屏幕区域
<!-- more -->
- session——查看当前会话的信息

- [reset](https://alibaba.github.io/arthas/reset.html)——重置增强类，将被 Arthas 增强过的类全部还原，Arthas 服务端关闭时会重置所有增强过的类

- version——输出当前目标 Java 进程所加载的 Arthas 版本号

- history——打印命令历史

- quit——退出当前 Arthas 客户端，其他 Arthas 客户端不受影响

- stop——关闭 Arthas 服务端，所有 Arthas 客户端全部退出

- [keymap](https://alibaba.github.io/arthas/keymap.html)——Arthas快捷键列表及自定义快捷键



## jvm相关

- [dashboard](https://alibaba.github.io/arthas/dashboard.html)——当前系统的实时数据面板
- [thread](https://alibaba.github.io/arthas/thread.html)——查看当前 JVM 的线程堆栈信息
- [jvm](https://alibaba.github.io/arthas/jvm.html)——查看当前 JVM 的信息
- [sysprop](https://alibaba.github.io/arthas/sysprop.html)——查看和修改JVM的系统属性
- [sysenv](https://alibaba.github.io/arthas/sysenv.html)——查看JVM的环境变量
- [vmoption](https://alibaba.github.io/arthas/vmoption.html)——查看和修改JVM里诊断相关的option
- [perfcounter](https://alibaba.github.io/arthas/perfcounter.html)——查看当前 JVM 的Perf Counter信息
- [logger](https://alibaba.github.io/arthas/logger.html)——查看和修改logger
- [getstatic](https://alibaba.github.io/arthas/getstatic.html)——查看类的静态属性
- [ognl](https://alibaba.github.io/arthas/ognl.html)——执行ognl表达式
- [mbean](https://alibaba.github.io/arthas/mbean.html)——查看 Mbean 的信息
- [heapdump](https://alibaba.github.io/arthas/heapdump.html)——dump java heap, 类似jmap命令的heap dump功能

***dashboard***
线程，内存，GC，环境总览
***thread***
查看线程相关：最忙线程，阻塞线程，线程栈，可以设置采样时间
***ognl***
@[类全名（包括包路径）]@[方法名 |  值名]，例如：
@java.lang.String@format('foo %s', 'bar')
或@tutorial.MyConstant@APP_NAME；



## class/classloader相关

- [sc](https://alibaba.github.io/arthas/sc.html)——查看JVM已加载的类信息
- [sm](https://alibaba.github.io/arthas/sm.html)——查看已加载类的方法信息
- [jad](https://alibaba.github.io/arthas/jad.html)——反编译指定已加载类的源码
- [mc](https://alibaba.github.io/arthas/mc.html)——内存编译器，内存编译`.java`文件为`.class`文件
- [redefine](https://alibaba.github.io/arthas/redefine.html)——加载外部的`.class`文件，redefine到JVM里
- [dump](https://alibaba.github.io/arthas/dump.html)——dump 已加载类的 byte code 到特定目录
- [classloader](https://alibaba.github.io/arthas/classloader.html)——查看classloader的继承树，urls，类加载信息，使用classloader去getResource



***sc***

“Search-Class” 的简写,可查看类的属性和模糊匹配，例：sc demo.*

**sm**

“Search-Method” 的简写,查看当前类的方法，父类不能看，例子：sm java.lang.String

**jad**

反编译

**mc**

编译java文件，配合redefine热更新，`-c`参数指定classloader，`-d`命令指定输出目录

**redefine**

限制：不能加field或者方法，`jad`/`watch`/`trace`/`monitor`/`tt`等命令会冲突

**classloader**

可以用classloader加载类



## monitor/watch/trace相关

> 请注意，这些命令，都通过字节码增强技术来实现的，会在指定类的方法中插入一些切面来实现数据统计和观测，因此在线上、预发使用时，请尽量明确需要观测的类、方法以及条件，诊断结束要执行 `stop` 或将增强过的类执行 `reset` 命令。

- [monitor](https://alibaba.github.io/arthas/monitor.html)——方法执行监控
- [watch](https://alibaba.github.io/arthas/watch.html)——方法执行数据观测
- [trace](https://alibaba.github.io/arthas/trace.html)——方法内部调用路径，并输出方法路径上的每个节点上耗时
- [stack](https://alibaba.github.io/arthas/stack.html)——输出当前方法被调用的调用路径
- [tt](https://alibaba.github.io/arthas/tt.html)——方法执行数据的时空隧道，记录下指定方法每次调用的入参和返回信息，并能对这些不同的时间下调用进行观测

**monitor**

监控方法调用，次数，成功率等，可以设置调用周期

**watch**

	$ watch demo.MathGame primeFactors "{params[0],target}" "params[0]<0"
	
	类名 方法名 观察内容 条件内容
	
	-b 调用前 -e调用异常后 -s方法返回后 -f方法结束  （调用位置可以设置多个） -x 显示深度 -n 执行次数
	
	ognl 参数：params：入参，可能会被改变	returnObj：返回对象	target：当前对象	throwExp：异常
	
				#cost：耗时	target.XXX:获取对象的属性

**trace**

查看方法调用链路，方法用时，可以动态多层trace

**stack**

向上查看，方法被哪里调用了

**tt**

`tt -t` 类路径 方法名 ognl过滤条件

开始记录 -n 记录次数

`tt -l` 列出数据 `tt -s` 搜索过滤数据 `tt -i 编号` 显示编号数据的详细信息

`$ tt -i 1004 -p` 重调用数据`--replay-times` 指定 调用次数，通过 `--replay-interval` 指定多次调用间隔(单位ms, 默认1000ms)



### 任务

```
trace Test t & &符号创建后台任务
```

`jobs` 查看任务

bg <任务ID>  fg<任务ID> 任务启动并在后台/前台执行

- ‘ctrl + z’：将任务暂停。通过`jbos`查看任务状态将会变为Stopped，通过`bg <job-id>`或者`fg <job-id>`可让任务重新开始执行
- ‘ctrl + c’：停止任务



### 使用示例：

1.查找CPU占用过高的原因

thread 获取占用线程

```
tt -t com.google.common.collect.HashBiMap seekByKey -n 100
```

监听查看调用频率是否过高

```
$ tt -t org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter invokeHandlerMethod -n 10
```

获取springContex对象，可以获取所有bean

```
tt -i 1000 -w 'target.getApplicationContext().getBean("oaInfoManager").userCache.entrySet()‘
```

获取bean对象的属性，是否有循环引用



Jps查看运行的java程序名

--select arthas-demo 直接attach进程

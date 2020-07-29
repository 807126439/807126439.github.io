---
title: arthas使用笔记
date: 2020-07-30 00:15:07
tags:
---
## arthas使用笔记

### 1.安装
 使用`arthas-boot`（推荐）
下载`arthas-boot.jar`，然后用`java -jar`的方式启动：
```
curl -O https://alibaba.github.io/arthas/arthas-boot.jar
java -jar arthas-boot.jar
```
<!-- more -->
 使用`as.sh`
 ```bash
curl -L https://alibaba.github.io/arthas/install.sh | sh
 ```

打印帮助信息：

```
java -jar arthas-boot.jar -h
```

 通过Cloud Toolkit插件使用Arthas一键诊断远程服务器(另外研究)

### 2.快速入门

**Demo示例**

```
curl -O https://alibaba.github.io/arthas/arthas-demo.jar
java -jar arthas-demo.jar
```

`arthas-demo`是一个简单的程序，每隔一秒生成一个随机数，再执行质因数分解，并打印出分解结果。

**启动arthas**

```
curl -O https://alibaba.github.io/arthas/arthas-boot.jar
java -jar arthas-boot.jar
```

**选择应用Java进程**

**查看dashboard**,输入[dashboard](https://alibaba.github.io/arthas/dashboard.html)，按`回车/enter`，会展示当前进程的信息，按`ctrl+c`可以中断执行。

**通过thread命令来获取到`arthas-demo`进程的Main Class**

`thread 1`会打印线程ID 1的栈，通常是main函数的线程。

```
$ thread 1 | grep 'main('
    at demo.MathGame.main(MathGame.java:17)
```

**通过jad来反编译Main Class  jad demo.MathGame**

**watch 通过[watch](https://alibaba.github.io/arthas/watch.html)命令来查看`demo.MathGame primeFactors`函数的返回值**

**推出**,如果只是退出当前的连接，可以用`quit`或者`exit`命令保持连接,stop完全推出

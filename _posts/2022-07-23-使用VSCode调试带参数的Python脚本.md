---
layout:     post
title:      "使用VSCode调试带参数的Python脚本"
subtitle:   ""
date:       2022-07-23
author:     "Pcon"
header-img: "img/post-bg-py.jpg"
tags:
    - VScode
---

## 使用VSCode调试带参数的Python脚本

### 1 问题描述

linux系统上执行带参数的python程序直接添加-arg xxx即可。但在VSCode调试模式（Debug）下该执行方式不可行。那么是否有办法在VSCode上调试带参数的python脚本呢？

这里提供三种方案：

- 2.1 方案1 pbd命令的Debug
- 2.2 方案2 在launch.json中设置参数的Debug
- 2.3 方案3 终端命令行中写入参数的Debug

### 2  解决方案

#### 2.1 方案1 pbd命令的Debug

终端窗口执行命令

```python
python -m pdb xxx.py
```

开启Debug模式，在断点处暂停，可输入以下命令：

```bash
h：（help）帮助
w：（where）打印当前执行堆栈
d：（down）执行跳转到在当前堆栈的深一层（个人没觉得有什么用处）
u：（up）执行跳转到当前堆栈的上一层
b：（break）添加断点
	b 列出当前所有断点，和断点执行到统计次数
	b line_no：当前脚本的line_no行添加断点
	b filename:line_no：脚本filename的line_no行添加断点
	b function：在函数function的第一条可执行语句处添加断点
tbreak：（temporary break）临时断点
	在第一次执行到这个断点之后，就自动删除这个断点，用法和b一样
cl：（clear）清除断点
	cl 清除所有断点
	cl bpnumber1 bpnumber2… 清除断点号为bpnumber1,bpnumber2…的断点
	cl lineno 清除当前脚本lineno行的断点
	cl filename:line_no 清除脚本filename的line_no行的断点
disable：停用断点，参数为bpnumber，和cl的区别是，断点依然存在，只是不启用
enable：激活断点，参数为bpnumber
s：（step）执行下一条命令
	如果本句是函数调用，则s会执行到函数的第一句
n：（next）执行下一条语句
	如果本句是函数调用，则执行函数，接着执行当前执行语句的下一条。
r：（return）执行当前运行函数到结束
c：（continue）继续执行，直到遇到下一条断点
l：（list）列出源码
	l 列出当前执行语句周围11条代码
	l first 列出first行周围11条代码
	l first second 列出first–second范围的代码，如果second<first，second将被解析为行数
a：（args）列出当前执行函数的函数
p expression：（print）输出expression的值
pp expression：好看一点的p expression
run：重新启动debug，相当于restart
q：（quit）退出debug
j lineno：（jump）设置下条执行的语句函数
	只能在堆栈的最底层跳转，向后重新执行，向前可直接执行到行号
unt：（until）执行到下一行（跳出循环），或者当前堆栈结束
condition bpnumber conditon，给断点设置条件，当参数condition返回True的时候bpnumber断点有效，否则bpnumber断点无效
```

**Note：**
`1：直接输入Enter，会执行上一条命令；`
`2：输入PDB不认识的命令，PDB会把他当做Python语句在当前环境下执行；`

但这种方式不依赖VSCode，并且是在命令行中调试，并不方便。

#### 2.2 方案2 在launch.json中设置参数的Debug

**Step1：**VSCode菜单栏-运行-打开配置，出现launch.json文件。

**Step2：**在configurations中添加”args”参数形式如下：

```bash
{
      "version": "0.2.0",
      "configurations": [
            {
                  "name": "Python: Current File"
                  "type": "python",
                  "request": "launch",
                  "program": "${file}",
                  "console": "integratedTerminal",
                  "args": [
                        "arg1", "xxx",
                        "arg2", "xxx",
                   ]
            }
}
```

**Step3：**添加完成后，设置断点，按F5执行即可开始调试。

该方式需要预先写入参数，在实际项目中参数数量和名称可能常常发生变化，这种方式在调整时还不够灵活，效率不够高。

#### 2.3 方案3 终端命令行中写入参数的Debug

**Step1：**安装debugpy库，`pip install debugpy`

**Step2：**打开本地终端，找到一个未占用的端口号。输入`netstat -a` State显示为LISTEN即为未占用，记该端口号为xxxx

```
关于State的说明：
- LISTEN: The socket is listening for incoming connections. Foreign address is not relevant for this line
- ESTABLISHED: The socket has an established connection. Foreign address in the address of the remote end point of the socket.
- CLOSE_WAIT: The remote end has shut down, waiting for the socket to close.
```

**Step3：**修改launch.json中内容为：

```
{
	"version": "0.2.0",
	"configurations": [
		{
			"name": "Python: Attach",
			"type": "python",
			"request": "attach",
			"connect": {
				"host": "localhost",
				"port": xxxx
			}
		}
	]
}
```

其中，xxxx表示使用的端口号。

**Step4：**假设run 该Python脚本的命令为：`python xxx.py -arg1 ARG1 -arg2 ARG2`。即脚本有两个参数输入。在进行调试之前，在VSCode终端命令行中键入：

```
python -m debugpy --listen xxxx --wait-for-client xxx.py -arg1 ARG1 -arg2 ARG2
```

注意这里监听的端口xxxx即为上一步找到的未占用端口（这里输入的端口号需要和launch.json中设置的端口号一致）


执行上述命令，终端处于执行中，没有任何返回。接下来在程序中设置断点，按下F5键，即可进入VSCode的调试模式。调试方式与不带参数的情况相同。

### Ref:

https://blog.csdn.net/weixin_37251044/article/details/107471459

https://www.cnblogs.com/JavicxhloWong/p/14356814.html（重要参考，感谢）

https://blog.csdn.net/qq_37837061/article/details/124561317
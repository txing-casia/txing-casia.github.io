---
layout:     post
title:      "在python中调用MATLAB的方法"
subtitle:   "速度肯定是没有只用一个软件来得快了啊"
date:       2018-08-15
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Python
    - MATLAB
---

# 在python中调用MATLAB的方法

网上有些是让你安装mlab库来实现调用的，这个方法已经淘汰不用了，这里介绍比较新的方法。

## Step 1: install MATLAB Engine API for Python

官网给出的方案：

[install MATLAB Engine API for Python](http://ww2.mathworks.cn/help/matlab/matlab_external/install-the-matlab-engine-for-python.html?ue)

在MATLAB中输入：

```matlab
matlabroot
```

得到返回的位置

打开anaconda，输入下面语句进行安装

```python
cd matlabroot\extern\engines\python
python setup.py install
```

注意要将matlabroot替换为之前返回的位置

## Step 2: Call User Script and Function from Python 

官网给出的方案：

[Call User Script and Function from Python](http://cn.mathworks.com/help/matlab/matlab_external/call-user-script-and-function-from-python.html)

###.m脚本的调用

.m文件与.py文件需要放在同一目录下。在test.m文件中输入：

```matlab
b=2;
h=3;
a=b*h
```

在H.py文件中输入：

```python
import matlab.engine
eng = matlab.engine.start_matlab()
eng.test(nargout=0)
```

其中指定 `nargout=0`。尽管脚本会打印输出，但它不会向 Python 返回任何输出参数。

### 自定义函数的调用

在time.m中输入：

```matlab
function a = time(b,h)
	a = b*h;
end
```

在H.py中输入：

```python
import matlab.engine
eng = matlab.engine.start_matlab()
ret = eng.time(1.0,5.0)
print(ret)
```

可以实现函数的调用。由于函数仅返回一个输出参数，因此无需指定 `nargout`。